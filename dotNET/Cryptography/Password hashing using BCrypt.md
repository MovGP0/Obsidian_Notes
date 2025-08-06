```csharp
using BCrypt.Net;

string hashed = BCrypt.HashPassword(password, workFactor: 12);

bool valid = BCrypt.Verify(password, hashed);
```

### With Pepper
```csharp
string pepper = GetSecretPepper();
string hashed = BCrypt.HashPassword(salt + password + pepper, 12);
bool valid = BCrypt.Verify(salt + providedPassword + pepper, hashed);
```
