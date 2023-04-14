```csharp
using Microsoft.EntityFrameworkCore;  
using MockQueryable.NSubstitute;

public static class QueryableExtensions
{
    public static DbSet<TEntity> BuildMockDbSet<TEntity>(this IEnumerable<TEntity> data) where TEntity : class
    {
        if (data == null) throw new ArgumentNullException(nameof(data));
        return NSubstituteExtensions.BuildMockDbSet(data.AsQueryable());
    }
}
```
