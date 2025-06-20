## Standards

- RFC 1760: [[#S/KEY]]
- RFC 2289: [[#One-Time Password (OTP)]]
- RFC 4226: [[#HMAC-based One-Time Password (HOTP)]]
- RFC 6238: [[#Time-based One-Time Password (TOTP)]]

## S/KEY

- Protocol is based on MD4 hashes

```sh
dotnet add package BouncyCastle.NetCoreSdk
```
```csharp
using System;
using System.Text;
using Org.BouncyCastle.Crypto.Digests;

public static class SKeyGenerator
{
    /// <summary>
    /// Generates the nth S/KEY one‐time password (64 bits) as hex.
    /// </summary>
    public static string Generate(string passphrase, string seed, int iterations)
    {
        // 1. Initial input: ASCII(seed + passphrase)
        byte[] input = Encoding.ASCII.GetBytes(seed + passphrase);
        // 2. MD4 hashing
        var digest = new MD4Digest();
        byte[] hash = new byte[digest.GetDigestSize()];
        digest.BlockUpdate(input, 0, input.Length);
        digest.DoFinal(hash, 0);
        // 3. Iterate hashing (count−1 more times)
        for (int i = 1; i < iterations; i++)
        {
            digest.Reset();
            digest.BlockUpdate(hash, 0, hash.Length);
            digest.DoFinal(hash, 0);
        }
        // 4. Fold 128 bits → 64 bits by XOR’ing halves
        byte[] folded = new byte[8];
        for (int i = 0; i < 8; i++)
            folded[i] = (byte)(hash[i] ^ hash[i + 8]);
        // 5. Return as hex
        return BitConverter.ToString(folded).Replace("-", "").ToLowerInvariant();
    }
}
```

## One-Time Password (OTP)

- Protocol is based on MD5/SHA-1 hashes

```sh
dotnet add package BouncyCastle.NetCoreSdk
```
```csharp
using System;
using System.Linq;
using System.Text;
using Org.BouncyCastle.Crypto;
using Org.BouncyCastle.Crypto.Digests;

public static class Rfc2289Otp
{
    public static byte[] Generate(
        string passphrase,
        string seed,
        int count,
        string hashAlg = "md5")
    {
        // Combine seed + passphrase
        byte[] data = Encoding.ASCII.GetBytes(seed + passphrase);
        // Choose digest
        IDigest digest = hashAlg.ToLower() switch
        {
            "md4"  => new MD4Digest(),
            "md5"  => new MD5Digest(),
            "sha1" => new Sha1Digest(),
            _      => throw new ArgumentException($"Unsupported hash: {hashAlg}")
        };
        // Initial digest
        byte[] result = new byte[digest.GetDigestSize()];
        digest.BlockUpdate(data, 0, data.Length);
        digest.DoFinal(result, 0);
        // Iterate
        for (int i = 1; i < count; i++)
        {
            digest.Reset();
            digest.BlockUpdate(result, 0, result.Length);
            digest.DoFinal(result, 0);
        }
        // Return first 8 bytes (64 bits)
        return result.Take(8).ToArray();
    }
}
```

## HMAC-based One-Time Password (HOTP)

- Hash-based Message Authentication Code (HMAC)
- uses a shared secret for authentication

**OTP.NET**
```sh
dotnet add package Otp.NET
```
```csharp
using OtpNet;

public class HotpExample
{
    public void Demo()
    {
        // 1. Generate or supply a shared secret (20 bytes recommended)
        byte[] secret = KeyGeneration.GenerateRandomKey(20);

        // 2. Create HOTP instance
        var hotp = new Hotp(secret);

        // 3. Compute HOTP value for a moving counter
        long counter = 123456L;
        string code = hotp.ComputeHOTP(counter);

        // 4. Verify later
        bool isValid = hotp.VerifyHOTP(code, counter);
        Console.WriteLine($"HOTP={code} valid={isValid}");
    }
}
```

**Yort.OTP**
```sh
Install-Package Yort.Otp
```
```csharp
using Yort.Otp;

public class YortExample
{
    public void Demo()
    {
        byte[] secret = Base32Encoding.ToBytes("JBSWY3DPEHPK3PXP"); 

        // HOTP
        var hotp = new Hotp(secret);
        string hcode = hotp.Generate(42);
        bool hValid = hotp.Validate(hcode, 42);
    }
}
```

## Time-based One-Time Password (TOTP)

- uses a shared secret for authentication

**OTP.NET**
```sh
dotnet add package Otp.NET
```
```csharp
using System;
using OtpNet;

public class TotpExample
{
    public void Demo()
    {
        // 1. Shared secret (same as HOTP)
        byte[] secret = KeyGeneration.GenerateRandomKey(20);

        // 2. Create TOTP instance (default 30 s step, SHA-1)
        var totp = new Totp(secret);

        // 3. Compute current TOTP code
        string code = totp.ComputeTotp();  // e.g. "492039"

        // 4. Verify with an optional time‐step window
        bool valid = totp.VerifyTotp(code, out long timeStepMatched, new VerificationWindow(previous:1, future:1));
        Console.WriteLine($"TOTP={code} valid={valid} at step={timeStepMatched}");
    }
}
```

**Yort.OTP**
```sh
Install-Package Yort.Otp
```
```csharp
using Yort.Otp;

public class YortExample
{
    public void Demo()
    {
        byte[] secret = Base32Encoding.ToBytes("JBSWY3DPEHPK3PXP"); 

        // TOTP
        var totp = new Totp(secret);
        string tcode = totp.Generate();
        bool tValid = totp.Validate(tcode, out long deviation);
    }
}
```
