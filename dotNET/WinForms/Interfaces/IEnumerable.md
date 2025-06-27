Provides enumerations via an enumerator. Can return multiple enumerators to support different enumerations at the same time.

Important Properties of Enumerator:
- `bool MoveNext()`
- `T Current`
- `void Reset()`

## How to avoid Eager Enumeration

Avoid eager enumeration in LINQ methods. Using `yield` causes the .NET compiler to create a state-machine to solve the problem.

```csharp
public static class MyExtensions
{
    public static IEnumerable<Person> WithAgeAbove(this IEnumerable<Person> source, int age)
    {
        foreach (var person in source)
        {
            if (person.Age > age)
            {
                yield return person;
            }
        }
    }
}
```

In older C# versions one can implement a custom enumerator instead
```csharp
public static class MyExtensions
{
    public static IEnumerable<Person> WithAgeAbove(this IEnumerable<Person> source, int age)
    {
        return new AgeAboveEnumerator(source.GetEnumerator(), age);
    }

    private class AgeAboveEnumerator : IEnumerator<Person>
    {
        private IEnumerator<Person> _sourceEnumerator;
        private int _age;
        private Person _current;

        public AgeAboveEnumerator(IEnumerator<Person> sourceEnumerator, int age)
        {
            _sourceEnumerator = sourceEnumerator;
            _age = age;
        }

        public Person Current => _current;

        object IEnumerator.Current => _current;

        public bool MoveNext()
        {
            while (_sourceEnumerator.MoveNext())
            {
                var person = _sourceEnumerator.Current;
                if (person.Age > _age)
                {
                    _current = person;
                    return true;
                }
            }

            return false;
        }

        public void Reset()
        {
            _sourceEnumerator.Reset();
            _current = default(Person);
        }

        public void Dispose()
        {
            _sourceEnumerator.Dispose();
        }
    }
}
```

## Notes

- Avoid multiple enumerations of an `IEnumerable`. Cache the enumeration result.

- Enumerating an `IEnumerable` using `.ToArray()` (or similar LINQ methods) will first crate an `List<T>` internally, because the size of the result is unknown before the enumeration is finished. Using `.ToList()` can result in better performance, but returning an `List` (ie. instead of `ReadonlyCollection<T>` or `Array<T>`) is usually not the optimal data type for upstream data access.
