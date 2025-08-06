```csharp
using System;
using System.Security.Cryptography;

public static class Pbkdf2Hasher
{
    private const int SaltSize = 16; // 128-bit salt
    private const int HashSize = 32; // 256-bit hash
    private const int Iterations = 310_000; // OWASP recommends ~600k for SHA256

    public static (string hash, byte[] salt) HashPassword(string password)
    {
        byte[] salt = RandomNumberGenerator.GetBytes(SaltSize);
        byte[] hash = Rfc2898DeriveBytes.Pbkdf2(
            password,
            salt,
            Iterations,
            HashAlgorithmName.SHA256,
            HashSize);

        return (Convert.ToBase64String(hash), salt);
    }

    public static bool Verify(string password, string storedHashBase64, byte[] storedSalt)
    {
        byte[] hash = Rfc2898DeriveBytes.Pbkdf2(
            password,
            storedSalt,
            Iterations,
            HashAlgorithmName.SHA256,
            HashSize);

        return CryptographicOperations.FixedTimeEquals(hash, Convert.FromBase64String(storedHashBase64));
    }
}
```

### Using Pepper

Using Pepper (additional to Salt):
```csharp
string pepper = GetSecretPepper(); // securely stored, not in DB
var (hash, salt) = Pbkdf2Hasher.HashPassword(password + pepper);
```

> [!important]
> Pepper is stored as an application-wide secret, rather than a per-password secret.
> The pepper should be stored inside an **Hardware Security Module** (**HSM**) like a **Trusted Platform Module** (**TPM**), or configuration.
> It is not to be stored in the database!
