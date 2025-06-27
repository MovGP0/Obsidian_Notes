## Get assembly information

```csharp
var assembly = Assembly.Load(new AssemblyName() { Name = "mscorlib", Version = new Version(4, 0, 0, 0) });
var assembly = Assembly.Load("mscorlib, Version=4.0.0.0");
var assembly = Assembly.LoadFrom(@"file:///C:/Windows/Microsoft.NET/Framework/v4.0.30319/mscorlib.dll");
var assembly = Assembly.GetExecutingAssembly();
```

## Get type information

```csharp
var type = Type.GetType("System.Random", true);
var type = typeof(System.Random);
var type = new System.Random().GetType();
var type = assembly.GetType("System.Random");
```

Generic types

```csharp
var type = typeof(System.Lazy<>);
var type = assembly.GetType("System.Lazy`1");
```

Closed generic type

```csharp
var closedType = typeof(System.Lazy<int>);
var closedType = typeof(System.Lazy<>).MakeGenericType(typeof(int));
```

Use the following methods to get detailed member information:
- `GetProperty(...)`
- `GetField(...)`
- `GetEvent(...)`

Create an instance of the given type (must be a closed type when generic)

```csharp
// call default constructor
ObjectHandle? instance = Activator.CreateInstance(type);

// call constructor with params
ObjectHandle? instance = Activator.CreateInstance(type, [parmeter1, parmeter2, ...]);

// cast instance to specific type
MyType myObject = (MyType)instance.Unwrap();
```

## Get method information

```csharp
var method = type.GetMethod("MethodName");
var method = type.GetMethod("MethodName", new Type[] { typeof(int), typeof(string) });
var method = type.GetMethod("MethodName", BindingFlags.Instance | BindingFlags.Public, null, new Type[] { typeof(int), typeof(string) }, null);
```

Get parameters from method information

```csharp
var parameter = method.GetParameters();
```

Get method attributes

```csharp
var attributes = method.GetCustomAttributes();
var attributes = method.GetCustomAttributes(typeof(MyAttribute), true);
bool hasAttribute = method.IsDefined(typeof(MyAttribute), true);
```

Invoke method on object instance

```csharp
Type type = typeof(MyType);
MethodInfo? method = type.GetMethod("MethodName", /*...*/);
ObjectHandle? instance = Activator.CreateInstance(type, /*...*/);
var result = instance.Invoke(method, /*...*/);
```
