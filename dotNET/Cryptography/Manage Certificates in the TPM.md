> [!note] 
> - Use the TPM **as a Key Storage Provider** via CNG/NCrypt.
> 	- easy, high-level, great for signing/keys that are stored in the TPM
> - Talk to the TPM **directly** via a TPM 2.0 library such as Microsoft’s TSS.NET (`Tpm2Lib`).
> 	- low-level, full control over PCRs, sealing, quotes

## CNG + Microsoft Platform Crypto Provider (TPM KSP)

This lets you create non-exportable RSA/ECC keys inside the TPM and use them through the normal .NET crypto types. It's the simplest way to use the TPM programmatically.

### Create a key
```csharp
// Create a persistent ECDSA-P256 key pair stored in TPM (machine scope)
var provider = new CngProvider("Microsoft Platform Crypto Provider");
var algorithm = new CngAlgorithm("ECDSA_P256");

var parameters = new CngKeyCreationParameters
{
	Provider = provider,
	KeyCreationOptions = CngKeyCreationOptions.OverwriteExistingKey | CngKeyCreationOptions.MachineKey,
	ExportPolicy = CngExportPolicies.None, // non-exportable private key
};

const string keyName = "Contoso-Tpm-ECDSA-P256";
using (var key = CngKey.Create(algorithm, keyName, parameters))
{
	Console.WriteLine($"Created TPM-backed key: {keyName}");
}
```

### Use the key
```csharp
// Open and use the key for signing
using var tpmKey = CngKey.Open(keyName, provider);
using var ecdsa = new ECDsaCng(tpmKey); // operations are performed by the TPM

byte[] data = System.Text.Encoding.UTF8.GetBytes("hello TPM");
byte[] hash = SHA256.HashData(data);
byte[] signature = ecdsa.SignHash(hash);

Console.WriteLine($"Signature length: {signature.Length}");

// Get the public key to distribute
byte[] publicKeyBlob = tpmKey.Export(CngKeyBlobFormat.EccPublicBlob);
Console.WriteLine($"Public key blob bytes: {publicKeyBlob.Length}");
```

### Delete the key

```csharp
var provider = CngProvider.MicrosoftPlatformCryptoProvider; // TPM KSP
var openOptions = isMachine ? CngKeyOpenOptions.MachineKey : CngKeyOpenOptions.None;

using var key = CngKey.Open(keyName, provider, openOptions);
key.Delete(); // removes the named key from the KSP
```

### Notes

- Provider name must be `Microsoft Platform Crypto Provider`.
- Use `CngKeyCreationOptions.MachineKey` if services or multiple users will need the key. Otherwise omit for a user key.
- Set `ExportPolicy = None` to keep the private key non-exportable.
- Works with `RSACng` as well (`CngAlgorithm.Rsa` or `RSA` with CNG handles).
- You can enroll the TPM key in a certificate like any other CNG key (e.g., with `CertificateRequest` + `CreateSigningRequest`).

Attestation of a TPM key (proving it was generated and lives in a real TPM) is available via `NCrypt` properties on the key handle, but requires P/Invoke.

## Issue TPM 2.0 commands with TSS.NET (`Tpm2Lib`)

Use this when you want PCR-bound sealing, quotes (remote attestation), custom hierarchies, policies, NV storage, etc.

```sh
dotnet add package Tpm2Lib
```

```csharp
// Open the real TPM via the Windows TBS driver
using var device = new TbsDevice(); // Requires Windows, TPM 2.0
device.Connect();

var tpm = new Tpm2(device);

// Read some PCRs
PcrSelection pcrs = new PcrSelection(TpmAlgId.Sha256, new uint[] { 7, 11 });
var read = tpm.PcrRead(
	new PcrSelection[] { pcrs },
	out uint pcrUpdateCounter,
	out PcrSelection[] selectionOut,
	out Tpm2bDigest[] pcrValues);

Console.WriteLine($"PCR Update Counter: {pcrUpdateCounter}");
for (int i = 0; i < pcrValues.Length; i++)
	Console.WriteLine($"PCR {selectionOut[0].GetSelectedPcrs()[i]}: {BitConverter.ToString(pcrValues[i].buffer)}");

// Create a primary key in the Storage Hierarchy
var sens = new SensitiveCreate();
var templ = new TpmPublic(
	TpmAlgId.Sha256,
	ObjectAttr.FixedTPM | ObjectAttr.FixedParent | ObjectAttr.SensitiveDataOrigin |
	ObjectAttr.UserWithAuth | ObjectAttr.NoDA | ObjectAttr.Restricted | ObjectAttr.Decrypt,
	null,
	new RsaParms(new SymDefObject(TpmAlgId.Aes, 128, TpmAlgId.Cfb), new NullAsymScheme(), 2048, 0),
	new Tpm2bPublicKeyRsa()
);

tpm[AuthHandle.Owner].CreatePrimary(out TpmHandle primaryHandle, out TpmPublic primaryPub, out _, null, null, templ, sens);
Console.WriteLine("Created primary storage key.");

// Define a policy bound to current PCR values
var pcrDigest = CryptoLib.HashData(TpmAlgId.Sha256, pcrValues[0].buffer, pcrValues[1].buffer);
var policyDigest = CryptoLib.HashData(TpmAlgId.Sha256, AuthValue.FromRandom(0).Auth); // stub init

// Build the policy with a PolicyPCR session
var policy = new PolicyTree(TpmAlgId.Sha256);
policy.AddPolicyPcr(pcrs, null); // PCR policy, values filled at session time

var sess = tpm.StartAuthSessionEx(TpmSe.Policy, TpmAlgId.Sha256);
policy.Execute(tpm, sess, null, new PolicyTree.PolicyTicket());
var finalPolicyDigest = tpm.PolicyGetDigest(sess).buffer;

// Seal a secret under that policy
byte[] secret = System.Text.Encoding.UTF8.GetBytes("super-secret-value");
var sealTemplate = new TpmPublic(
	TpmAlgId.Sha256,
	ObjectAttr.FixedTPM | ObjectAttr.FixedParent | ObjectAttr.UserWithAuth,
	new byte[0],
	new SymDefObject(),
	new Tpm2bDigestKeyedhash() // keyed-hash object for sealing
);
sealTemplate.authPolicy = finalPolicyDigest;

var inSensitive = new SensitiveCreate(new byte[0], secret);
var created = tpm[primaryHandle].Create(sealTemplate, inSensitive, null, new PcrSelection[] { pcrs }, out TpmPublic sealedPub, out TpmPrivate sealedPriv, out _, out _);

// Load and unseal (will succeed only if PCRs still match)
tpm[primaryHandle].Load(out TpmHandle sealedHandle, sealedPriv, sealedPub);
var unsealed = tpm[sealedHandle, sess].Unseal();
Console.WriteLine($"Unsealed: {System.Text.Encoding.UTF8.GetString(unsealed)}");
```

### Notes

- `TbsDevice` talks to the real TPM via Windows TBS. For a simulator, use `TcpTpmDevice("127.0.0.1", 2321)`.
- With TSS.NET you can: make quotes (`tpm.Quote`), bind secrets to PCRs (`PolicyPCR`), use `PolicyOR`, `PolicyAuthorize`, NV indices, etc.
- Works alongside the KSP approach; you can create keys both ways.

## List TPM keys via `NCrypt.dll`

### P/Invoke Code
```csharp
// ncrypt.h P/Invoke
private const string NCRYPT = "ncrypt.dll";
private const int ERROR_SUCCESS = 0x0;
private const int NTE_NO_MORE_ITEMS = unchecked((int)0x8009002A);

[DllImport(NCRYPT, CharSet = CharSet.Unicode)]
private static extern int NCryptOpenStorageProvider(out IntPtr phProvider, string pszProviderName, int dwFlags);

[DllImport(NCRYPT)]
private static extern int NCryptFreeObject(IntPtr hObject);

[DllImport(NCRYPT, CharSet = CharSet.Unicode)]
private static extern int NCryptEnumKeys(
	IntPtr hProvider,
	string pszScope, // null => current scope
	out IntPtr ppKeyName,
	ref IntPtr ppEnumState,
	int dwFlags);

[DllImport(NCRYPT)]
private static extern void NCryptFreeBuffer(IntPtr pvInput);

[StructLayout(LayoutKind.Sequential)]
private struct NCryptKeyName
{
	public IntPtr pszName; // LPWSTR
	public IntPtr pszAlgId; // LPWSTR
	public int dwLegacyKeySpec;
	public int dwFlags; // bit 0 => machine key
}

[DllImport("ncrypt.dll", CharSet = CharSet.Unicode)]
static extern int NCryptOpenKey(
	IntPtr hProvider,
	out IntPtr phKey,
	string pszKeyName,
	int dwLegacyKeySpec,
	int dwFlags);

[DllImport("ncrypt.dll")]
static extern int NCryptDeleteKey(IntPtr hKey, int dwFlags);
```

## Usage
```csharp
public sealed record TpmKeyInfo(string Name, string Algorithm, bool IsMachineKey);

public static IEnumerable<TpmKeyInfo> EnumPlatformKeys()
{
	const string Provider = "Microsoft Platform Crypto Provider"; // TPM KSP
	IntPtr hProv;
	int err = NCryptOpenStorageProvider(out hProv, Provider, 0);
	if (err != ERROR_SUCCESS) throw new Win32Exception(err, "NCryptOpenStorageProvider failed");

	try
	{
		var result = new List<TpmKeyInfo>();
		IntPtr enumState = IntPtr.Zero;

		try
		{
			while (true)
			{
				IntPtr pKeyName;
				int rc = NCryptEnumKeys(hProv, null, out pKeyName, ref enumState, 0);
				if (rc == NTE_NO_MORE_ITEMS) break;
				if (rc != ERROR_SUCCESS) throw new Win32Exception(rc, "NCryptEnumKeys failed");

				try
				{
					var native = Marshal.PtrToStructure<NCryptKeyName>(pKeyName);
					string name = Marshal.PtrToStringUni(native.pszName) ?? "";
					string alg  = Marshal.PtrToStringUni(native.pszAlgId) ?? "";
					bool isMachine = (native.dwFlags & 0x1) != 0;

					result.Add(new TpmKeyInfo(name, alg, isMachine));
				}
				finally
				{
					NCryptFreeBuffer(pKeyName);
				}
			}
		}
		finally
		{
			if (enumState != IntPtr.Zero) NCryptFreeBuffer(enumState);
		}

		return result;
	}
	finally
	{
		NCryptFreeObject(hProv);
	}
}
```

### Delete Keys

```csharp
const string Provider = "Microsoft Platform Crypto Provider";

if (NCryptOpenStorageProvider(out var hProv, Provider, 0) != 0)
	throw new Exception("Open provider failed");

try
{
	if (NCryptOpenKey(hProv, out var hKey, keyName, 0, 0) != 0)
		throw new Exception("Open key failed");

	try
	{
		int rc = NCryptDeleteKey(hKey, 0);

		if (rc != 0)
			throw new Win32Exception(rc, "NCryptDeleteKey failed");
	}
	finally
	{ 
		NCryptFreeObject(hKey);
	}
}
finally
{
	NCryptFreeObject(hProv);
}
```

## Sign a JWT with a TPM-backed ECDSA key

Create the TPM key if it does not exist
```csharp
private const string KeyName = "Contoso-Tpm-ECDSA-P256";

public static void CreateTpmKey()
{
	var provider = CngProvider.MicrosoftPlatformCryptoProvider;
	var algorithm = new CngAlgorithm("ECDSA_P256");

	if (!CngKey.Exists(KeyName, provider, CngKeyOpenOptions.MachineKey))
	{
		var p = new CngKeyCreationParameters
		{
			Provider = provider,
			KeyCreationOptions = CngKeyCreationOptions.MachineKey,
			ExportPolicy = CngExportPolicies.None
		};
		using var _ = CngKey.Create(algorithm, KeyName, p);
		Console.WriteLine($"Created TPM key: {KeyName}");
	}
}
```

Create and sign a [[Json Web Token (JWT)]]
```csharp
public static string CreateJwt()
{
	var provider = CngProvider.MicrosoftPlatformCryptoProvider;
	using var key = CngKey.Open(KeyName, provider, CngKeyOpenOptions.MachineKey);
	using var ecdsa = new ECDsaCng(key); // calls into the TPM for signing

	var securityKey = new ECDsaSecurityKey(ecdsa)
	{
		KeyId = key.Thumbprint?.ToString() ?? KeyName  // kid
	};

	var creds = new SigningCredentials(securityKey, SecurityAlgorithms.EcdsaSha256);

	var descriptor = new SecurityTokenDescriptor
	{
		Issuer = "https://issuer.example",
		Audience = "api://example",
		Subject = new ClaimsIdentity(new[]
		{
			new Claim("sub", "user-123"),
			new Claim("role", "admin")
		}),
		NotBefore = DateTime.UtcNow,
		Expires = DateTime.UtcNow.AddMinutes(5),
		SigningCredentials = creds
	};

	var handler = new JwtSecurityTokenHandler();
	var token = handler.CreateToken(descriptor);   // or CreateEncodedJwt overloads
	return handler.WriteToken(token);
}
```

Verify the JWT
```csharp
// Export public key for verifiers
using var key = CngKey.Open(KeyName, CngProvider.MicrosoftPlatformCryptoProvider, CngKeyOpenOptions.MachineKey);
byte[] publicBlob = key.Export(CngKeyBlobFormat.EccPublicBlob); // public only

// Convert to ECParameters / JWK as needed
using var ecdsa = new ECDsaCng(key);
ECParameters ec = ecdsa.ExportParameters(false);
```
