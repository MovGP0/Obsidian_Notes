```sh
dotnet add package Konscious.Security.Cryptography.Argon2
```

```csharp
using Konscious.Security.Cryptography;
using System;
using System.Text;
using System.Security.Cryptography;

public static class Argon2Hasher
{
    public static byte[] GenerateSalt(int length = 16)
    {
        byte[] salt = new byte[length];
        RandomNumberGenerator.Fill(salt);
        return salt;
    }

    public static byte[] Hash(string password, byte[] salt)
    {
        var pwBytes = Encoding.UTF8.GetBytes(password);
        using var argon = new Argon2id(pwBytes);
        argon.Salt = salt;
        argon.DegreeOfParallelism = 4;
        argon.MemorySize = 64 * 1024; // 64 MB
        argon.Iterations = 2;
        return argon.GetBytes(32);
    }
}
```

```csharp
byte[] salt = Argon2Hasher.GenerateSalt();
byte[] hash = Argon2Hasher.Hash(password, salt);
```

### With Pepper

Argon2 supports a `secret` parameter, but bindings often don’t expose it. Instead you can do:
- **Preferred**: `argon2(secret: pepper)` when library supports it  
- If not available, prepend: `argon2(HMAC_SHA256(pepper, password))`

```csharp
var keyed = HMACSHA256.HashData(Encoding.UTF8.GetBytes(pepper), Encoding.UTF8.GetBytes(password));
byte[] hash = Argon2Hasher.Hash(Convert.ToBase64String(keyed), salt);
```
