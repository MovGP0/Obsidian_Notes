## Sign a file

Load certificate from a file
```csharp
var certPath = "ServerCertificate.pfx";
var password = "secret";
var cert = new X509Certificate2(certPath, password);
```

Get the private key
```csharp
var rsa = (RSACryptoServiceProvider)cert.PrivateKey;
```

Compute the hash of the file to sign
```csharp
var filePath = "FileToSign.txt";
var data = File.ReadAllBytes(filePath);
SHA256Managed sha256 = new SHA256Managed();
var hash = sha256.ComputeHash(data);
```

Sign the hash value
```csharp
byte[] signature = rsa.SignHash(hash, CryptoConfig.MapNameToOID("SHA256"));
```

Verify the signature
```csharp
bool isValid = rsa.VerifyHash(hash, CryptoConfig.MapNameToOID("SHA256"), signature);
```

### Alternatives

Store signature as metadata of a file
```csharp
using DSOFile;

byte[] signature = ...; // The byte array of the signature
var odp = new OleDocumentProperties();
odp.Open(filePath, false, DSOFile.dsoFileOpenOptions.dsoOptionDefault);
odp.CustomProperties.Add("Signature", ref signatureValue);
odp.Save();
```

#### Use SignTool for signing a binary

Sign a `.exe` or `.dll` file using SignTool
```powershell
signtool.exe sign `
    /f "Certificate.pfx" `
    /p "secret" `
    /fd SHA256 `
    /tr 'http://timestamp.digicert.com' `
    /td SHA256 `
    "FileToSign.exe"
```

#### Use `Set-AuthenticodeSignature` to sign a binary

Sing a `.exe` or `.dll` file using [[PowerShell]]
```powershell
$cert = Get-PfxCertificate -FilePath "Certificate.pfx"

Set-AuthenticodeSignature `
    -FilePath "FileToSign.exe" `
    -Certificate $cert `
    -TimestampServer http://timestamp.digicert.com `
    -HashAlgorithm SHA256
```

## See also

- [[Self-Signed TLS Zertifikate]]
