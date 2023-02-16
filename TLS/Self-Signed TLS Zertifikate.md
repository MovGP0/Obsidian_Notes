## Create Root Certificate

```powershell
using namespace System.Security.Cryptography.X509Certificates;

Import-Module PKI -SkipEditionCheck;

function New-RootCertificate {
	[CmdletBinding()]
	[OutputType([System.IO.FileInfo])]
	param(
		[Parameter(Mandatory = $false)]
		[string]$basePath = $PSScriptRoot,

		# ConvertTo-SecureString -String "urtHeRnATenCoNshIZeRAGuENHOrDIGh" -Force -AsPlainText;
		[Parameter(Mandatory = $true)]
		[System.Security.SecureString]$password
	)
    
	Set-StrictMode -Version Latest;
	$script:ErrorActionPreference = 'Stop';

	$debug = ($PSBoundParameters['Debug'] -eq $true)
	$verbose = ($PSBoundParameters['Verbose'] -eq $true)
	
	[System.Security.Cryptography.X509Certificates.X509Certificate2]$rootCert = New-SelfSignedCertificate -Type Custom -CertStoreLocation 'cert:\LocalMachine\My' -Subject 'FM Consulting Root' -DnsName 'fmconsulting.at' -KeyExportPolicy Exportable -KeyUsage CertSign -KeyUsageProperty All -HashAlgorithm SHA256 -KeyLength 2048 -Debug:$debug -Verbose:$verbose;

	Write-Information 'Exporting Root Certificate';

	[String]$rootCertPath = Join-Path -Path Cert:\LocalMachine\My -ChildPath "$($rootCert.Thumbprint)";
	Export-PfxCertificate -Cert $rootCertPath -FilePath (Join-Path $basePath 'root.pfx') -Password $password -Debug:$debug -Verbose:$verbose; # private key
	Export-Certificate    -Cert $rootCertPath -FilePath (Join-Path $basePath 'root.crt') -Debug:$debug -Verbose:$verbose -Type CERT; # public key
	
	Write-Information $rootCert.GetType();
	return [System.IO.FileInfo]$rootCert;
}
```

## Import Certificate from Certificate Store

```powershell
using namespace System.Security.Cryptography.X509Certificates;

function Import-Certificate {
    [CmdletBinding()]
    param(
        [Parameter(Position = 0, Mandatory = $true)]
        [ValidateNotNull()]
        [ValidateScript( { Test-Path $_ })]
        [strig]$path,

        [Parameter(Position = 1, Mandatory = $true)]
        [ValidateNotNull()]
        [SecureString]$password,

        [Parameter(Position = 2, Mandatory = $false)]
        [StoreName]$storeName = [StoreName]::Root
    )

    process {
        Set-StrictMode -Version Latest;
        $script:ErrorActionPreference = 'Stop';

        $pfx = New-Object X509Certificate2;
        $pfx.Import($path, $password, "Exportable,PersistKeySet");
        $store = New-Object X509Store($storeName, "localmachine");

        try {
            $store.Open("MaxAllowed");
            $store.Add($pfx); 
            $store.Close();
        }
        catch {
            $store.Dispose();
        }
    }
}
```

## Create Code-Signing Certificate

```powershell
using namespace System.Security.Cryptography.X509Certificates;

Import-Module PKI -SkipEditionCheck;

function New-CodeSigningCertificate {
    [CmdletBinding()]
    [OutputType([System.Security.Cryptography.X509Certificates.X509Certificate2])]
    param(
        [Parameter(Position = 0, Mandatory = $true)]
        [string]$appName,

        [Parameter(Position = 1, Mandatory = $false)]
        [string]$basePath = $PSScriptRoot,

        [Parameter(Position = 2, Mandatory = $true)]
        [System.Security.SecureString]$password,

        [Parameter(Position = 3, Mandatory = $true)]
        [Microsoft.CertificateServices.Commands.Certificate]$rootCert
    )

    process {
        Set-StrictMode -Version Latest;
		$script:ErrorActionPreference = 'Stop';

		$debug = ($PSBoundParameters['Debug'] -eq $true)
		$verbose = ($PSBoundParameters['Verbose'] -eq $true)

        Write-Information 'Creating Code Signing Certificate';

        $cert = New-SelfSignedCertificate `
            -Subject "FM Consulting ($appName, CodeSigning)" `
            -Type CodeSigningCert `
            -CertStoreLocation Cert:\LocalMachine\My `
            -Signer $rootCert `
            -KeyUsage DigitalSignature `
            -KeyUsageProperty Sign `
            -KeyExportPolicy Exportable;

        Write-Information 'Exporting Code Signing Certificate';

        [String]$codeSigningCertPath = Join-Path -Path 'Cert:\LocalMachine\My\' -ChildPath "$($cert.Thumbprint)";
        Export-PfxCertificate -Cert $codeSigningCertPath -FilePath (Join-Path $basePath "$appName.pfx") -Password $password -Debug:$debug -Verbose:$verbose; # private key
        Export-Certificate    -Cert $codeSigningCertPath -FilePath (Join-Path $basePath "$appName.crt") -Debug:$debug -Verbose:$verbose; # public key

        return $cert;
    }
}
```

## Create TLS Certificate

```powershell
using namespace System.Security.Cryptography.X509Certificates;
Import-Module PKI -SkipEditionCheck;

function New-TlsCertificate {
    [CmdletBinding()]
    [OutputType([X509Certificate2])]
    param(
        [Parameter(Position = 0, Mandatory = $true)]
        [string]$appName,

        [Parameter(Position = 1, Mandatory = $false)]
        [string]$basePath = $PSScriptRoot,

        [Parameter(Position = 2, Mandatory = $true)]
        [System.Security.SecureString]$password,

        [Parameter(Position = 2, Mandatory = $true)]
        [string[]]$dnsName = @("*.fmconsulting.at","localhost"),

        [Parameter(Position = 3, Mandatory = $true)]
        [X509Certificate2]$rootCert
    )

    process {
        Set-StrictMode -Version Latest;
		$script:ErrorActionPreference = 'Stop';

		$debug = ($PSBoundParameters['Debug'] -eq $true)
		$verbose = ($PSBoundParameters['Verbose'] -eq $true)

        Write-Information 'Creating TLS Certificate';

        [X509Certificate2]$cert = New-SelfSignedCertificate `
			-CertStoreLocation 'Cert:\LocalMachine\My' `
			-Subject "FM Consulting ($appName, TLS)" `
            -DnsName $dnsName `
			-Signer $rootCert `
			-KeyUsage DigitalSignature `
			-KeyUsageProperty All `
			-KeyExportPolicy Exportable `
			-Debug:$debug -Verbose:$verbose | Select-Object -First 1;

        Write-Information 'Exporting TLS Certificate';

        [string]$codeSigningCertPath = Join-Path -Path 'Cert:\LocalMachine\My\' -ChildPath "$($cert.Thumbprint)";
        Export-PfxCertificate -Cert $codeSigningCertPath -FilePath (Join-Path $basePath "$appName.pfx") -Password $password -Debug:$debug -Verbose:$verbose; # private key
        Export-Certificate    -Cert $codeSigningCertPath -FilePath (Join-Path $basePath "$appName.crt") -Debug:$debug -Verbose:$verbose; # public key

        return $cert;
    }
}
```

## Delete Certificates from Store

```powershell
function Remove-Certificates
{
    [CmdletBinding()]
    param()

    process{
        Set-StrictMode -Version Latest;
        $script:ErrorActionPreference = 'Stop';

        Write-Information "Deleting certificates from local machine store";

        Get-ChildItem cert:\LocalMachine\My `
        | Where-Object { $_.Subject -match '.*FM Consulting.*' } `
        | Remove-Item
    }
}
```

## Sign Code

```powershell
using namespace System.Security;
using namespace System.Security.Cryptography.X509Certificates;
Import-Module PKI -SkipEditionCheck;

function Sign-Code {
    param(
        [Parameter(Position = 0, Mandatory = $true)]
        [string]$folder,

        [Parameter(Position = 1, Mandatory = $true)]
        [string]$certPath,

        [Parameter(Position = 2, Mandatory = $true)]
        [SecureString]$password
    )

    process {
        $script:ErrorActionPreference = 'Stop';
        $cert = Get-PfxCertificate -FilePath $certPath -Password $password;
        $files = Get-ChildItem -Path $folder -Include ('*.exe', '*.dll') -Recurse -ErrorAction 'SilentlyContinue';

        foreach ($file in $files) {
            [string]$filePath = $file.PSPath;
            Write-Verbose "Processing file '$filePath'";

            $result = Get-AuthenticodeSignature $file;
        
            $status = $result.Status;
            if ($status -eq 'Valid') {
                Write-Verbose "File $file is signed";
                continue;
            }

            if ($status -eq 'NotSigned') {
                $result = Set-AuthenticodeSignature `
                    -FilePath $file.FullName `
                    -Certificate $cert `
                    -IncludeChain "NotRoot" `
                    -HashAlgorithm "sha256";

                if ($result.Status -eq "Valid") {
                    Write-Verbose "$file is signed"
                }
                else { 
                    $message = $result.StatusMessage;
                    Write-Verbose "$file $message";
                }
            }
        }
        Write-Verbose 'Done';
    }
}
```

## Sign PowerShell Scripts

```powershell
using namespace System.Security;
using namespace System.Security.Cryptography.X509Certificates;
Import-Module PKI -SkipEditionCheck;

function Sign-PowerShellScripts {
    param(
        [Parameter(Position = 0, Mandatory = $false)]
        [string]$folder = $PSScriptRoot,

        [Parameter(Position = 1, Mandatory = $true)]
        [string]$certPath,

        [Parameter(Position = 2, Mandatory = $true)]
        [SecureString]$password
    )
    process {
        Set-StrictMode -Version Latest;
        $script:ErrorActionPreference = 'Stop';
        
        $cert = Get-PfxCertificate -FilePath $certPath -Password $password;

        Write-Information 'Signing PowerShell Scripts';

        $files = Get-ChildItem -Path $folder -Filter @('*.ps*1') -Recurse -ErrorAction 'SilentlyContinue';;

        foreach ($file in $files) {
            Set-AuthenticodeSignature -FilePath $file.PSPath -Certificate $cert -HashAlgorithm sha256 -Debug:$debug -Verbose:$verbose;	
        }
    }
}
```

## Check if user has admin rights

```powershell
using namespace System.Security.Principal;

function Test-Administrator  
{  
    [OutputType([bool])]
    param()
    process {
        [WindowsPrincipal]$user = [WindowsIdentity]::GetCurrent();
        return $user.IsInRole([WindowsBuiltinRole]::Administrator);
    }
}
```

## Example Usage

```powershell
using namespace System.Security.Cryptography.X509Certificates;

Set-StrictMode -Version Latest;
$script:ErrorActionPreference = 'Stop';
$script:WarningPreference = 'Continue';
$script:InformationPreference = 'Continue';
$script:VerbosePreference = 'SilentlyContinue';

. (Join-Path $PsScriptRoot 'New-RootCertificate.ps1')
. (Join-Path $PsScriptRoot 'New-CodeSigningCertificate.ps1')
. (Join-Path $PsScriptRoot 'New-TlsCertificate.ps1')
. (Join-Path $PsScriptRoot 'Test-Administrator.ps1')

if(Test-Administrator -ne $true) {
    Write-Warning 'Script must run with administrator privileges';
}

$rootCertPassword = (ConvertTo-SecureString '...' -AsPlainText -Force);
[System.IO.FileInfo]$rCert = (New-RootCertificate `
    -BasePath $PsScriptRoot `
    -Password $rootCertPassword | select -First 1);

$rootCert = Get-PfxCertificate -FilePath $rCert -Password $rootCertPassword;

New-CodeSigningCertificate `
    -AppName 'CareCenterServices' `
    -BasePath $PsScriptRoot `
    -Password (ConvertTo-SecureString '....' -AsPlainText -Force) `
    -RootCert $rootCert;
```

## See also

- [Public Key Infrastructure Powershell Cmdlets](https://docs.microsoft.com/en-us/powershell/module/pkiclient/)
- [Distribute certificates in Active Directory](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/deployment/distribute-certificates-to-client-computers-by-using-group-policy)
- [Active Directory Group Policy Cmdlets](https://docs.microsoft.com/en-us/powershell/module/grouppolicy/)
