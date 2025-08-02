---
title: .NET Cryptography
---
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

Sing a `.exe` or `.dll` file using [[PowerShell]]
```powershell
$cert = Get-PfxCertificate -FilePath "Certificate.pfx"

Set-AuthenticodeSignature `
    -FilePath "FileToSign.exe" `
    -Certificate $cert `
    -TimestampServer http://timestamp.digicert.com `
    -HashAlgorithm SHA256
```

*See also:* [[Self-Signed TLS Zertifikate]]

## File encryption and decryption

### Using Data Protection API (DPAPI)

```csharp
var filePath = "File.txt";
File.Encrypt(filePath);
```

```csharp
File.Decrypt(filePath);
```

### Symmetric encryption using AES

Crate an AES key
```csharp
// Generate a secret key and an IV for AES encryption
using (Aes aes = Aes.Create())
{
    byte[] key = aes.Key; // The secret key
    byte[] iv = aes.IV; // The initialization vector
}
```

Encrypt a file using AES
```csharp
// Encrypt a file using Aes.CreateEncryptor
using (var inputFileStream = new FileStream(@"FileToEncrypt.txt", FileMode.Open))
using (var outputFileStream = new FileStream(@"FileToEncrypt.aes", FileMode.Create))
using (var aes = Aes.Create())
{
    aes.Key = key;
    aes.IV = iv;
    var encryptor = aes.CreateEncryptor(aes.Key, aes.IV);
    using (var cryptoStream = new CryptoStream(outputFileStream, encryptor, CryptoStreamMode.Write))
    {
        inputFileStream.CopyTo(cryptoStream);
    }
}
```

Decrypt a file using AES
```csharp
using (var inputFileStream = new FileStream(@"FileToDecrypt.aes", FileMode.Open))
using (var outputFileStream = new FileStream(@"FileToDecrypt.txt", FileMode.Create))
using (var aes = Aes.Create())
{
    aes.Key = key;
    aes.IV = iv;
    var decryptor = aes.CreateDecryptor(aes.Key, aes.IV);
    using (var cryptoStream = new CryptoStream(inputFileStream, decryptor, CryptoStreamMode.Read))
    {
        cryptoStream.CopyTo(outputFileStream);
    }
}
```

### Assymmetric encryption using RSA

Load the RSA certificate from a `.p12` or `.pfx` file:
```csharp
var filePath = "Certificate.p12";
var cert = new X509Certificate2(filePath, password);
```

```csharp
var rsaPublic = (RSACryptoServiceProvider)cert.PublicKey.Key;
var rsaPrivate = (RSACryptoServiceProvider)cert.PrivateKey;
```

Encrypt data using RSA public key
```csharp
byte[] dataToEncrypt = Encoding.UTF8.GetBytes("Hello world");
byte[] encryptedData = rsaPublic.Encrypt(dataToEncrypt, false);
```

Decrypt data using RSA private key
```csharp
byte[] dataToDecrypt = encryptedData;
byte[] decryptedData = rsaPrivate.Decrypt(dataToDecrypt, false);
string decryptedText = Encoding.UTF8.GetString(decryptedData);
```

## Manage Certificates in the X.509 Certificate Store

- On Linux, this uses `/usr/share/.mono/certs` or `/etc/ssl/certs`
- On Windows, the certificate store is used

Get a certificate store
```csharp
using System.Security.Cryptography.X509Certificates;
using var store = new X509Store(StoreName.My, StoreLocation.CurrentUser);
```

Add a certificate to the store
```csharp
try
{
	store.Open(OpenFlags.ReadWrite);
	store.Add(cert);
}
finally
{
    store.Close();
}
```

Find a certificate
```csharp
try
{
	store.Open(OpenFlags.ReadOnly);
	var foundCerts = store.Certificates
	    .Find(X509FindType.FindBySubjectName, "SubjectName", false);
	var certificate = foundCerts[0];
}
finally
{
    store.Close();
}
```

Delete a certificate
```csharp
try
{
	store.Open(OpenFlags.ReadWrite);
	store.Remove(certificate);
	store.RemoveRange(certificates);
}
finally
{
    store.Close();
}
```
