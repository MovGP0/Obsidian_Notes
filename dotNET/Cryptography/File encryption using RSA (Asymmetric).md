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
