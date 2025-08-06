
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
