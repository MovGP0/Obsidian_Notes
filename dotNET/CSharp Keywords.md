## checked / unchecked

```csharp
int maxValue = int.MaxValue;

checked
{
	try
	{
		int overflow = maxValue + 1;
	}
	catch (OverflowException)
	{
		// ...
	}
}

unchecked
{
	int overflow = maxValue + 1; // No exception is thrown.
}
```

## extern

Used to declare a method that is implemented externally, typically using a C++ DLL (called P/Invoke in .NET).
```csharp
using System.Runtime.InteropServices;

class Program
{
    [DllImport("User32.dll", CharSet = CharSet.Unicode)]
    public static extern int MessageBox(IntPtr h, string m, string c, int type);

    static void Main()
    {
        MessageBox(new IntPtr(0), "Hello, World!", "Extern Example", 0);
    }
}
```

## fixed

Used to prevent the garbage collector from relocating a movable variable. It's used in an unsafe context when working with pointers to access a managed object's memory.
```csharp
int[] arr = new int[5] { 1, 2, 3, 4, 5 };

fixed (int* ptr = arr)
{
	for (int i = 0; i < 5; i++)
	{
		Console.WriteLine("Element {0}: {1}", i, *(ptr + i));
	}
}
```

In `.csproj` file
```xml
<AllowUnsafeBlocks>true</AllowUnsafeBlocks>
```

## stackalloc

allocate memory on the stack in an unsafe context
```csharp
const int size = 5;
int* intArray = stackalloc int[size];

for (int i = 0; i < size; i++)
{
	intArray[i] = i * 2;
}

for (int i = 0; i < size; i++)
{
	Console.WriteLine("Element {0}: {1}", i, intArray[i]);
}
```

In `.csproj` file
```xml
<AllowUnsafeBlocks>true</AllowUnsafeBlocks>
```
