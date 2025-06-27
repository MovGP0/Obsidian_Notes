## AllocHGlobal / FreeHGlobal

`AllocHGlobal` allocates memory from the unmanaged global heap.
`FreeHGlobal` - Frees memory allocated with `AllocHGlobal`.

```csharp
using System.Runtime.InteropServices;

static unsafe void Main()
{
	int size = 5;
	IntPtr memoryBlock = Marshal.AllocHGlobal(size * sizeof(int));
	int* intPtr = (int*)memoryBlock;

	for (int i = 0; i < size; i++)
	{
		intPtr[i] = i * 2;
		Console.WriteLine("Element {0}: {1}", i, intPtr[i]);
	}

	Marshal.FreeHGlobal(memoryBlock);
}
```

## AllocCoTaskMem / FreeCoTaskMem

`AllocCoTaskMem` - Allocates memory from the COM task memory allocator.
`FreeCoTaskMem` - Frees memory allocated with `AllocCoTaskMem`.

```csharp
using System;
using System.Runtime.InteropServices;

int[] managedArray = new int[] { 1, 2, 3, 4, 5 };
int size = Marshal.SizeOf<int>() * managedArray.Length;

// Allocate memory in the unmanaged COM task memory allocator
IntPtr unmanagedArray = Marshal.AllocCoTaskMem(size);

// Copy the data from the managed array to the unmanaged memory
Marshal.Copy(managedArray, 0, unmanagedArray, managedArray.Length);

// Use the unmanaged memory (e.g., pass it to an unmanaged function)
// ...

// Deallocate the unmanaged memory
Marshal.FreeCoTaskMem(unmanagedArray);
```

## Copy

Provides several overloaded methods for copying data between unmanaged memory blocks and managed arrays.
```csharp
byte[] byteArray = new byte[] { 1, 2, 3, 4, 5 };
IntPtr unmanagedArray = Marshal.AllocHGlobal(byteArray.Length);
Marshal.Copy(byteArray, 0, unmanagedArray, byteArray.Length);
```

## PtrToStructure

Marshals data from an unmanaged block of memory to a managed object.
```csharp
[StructLayout(LayoutKind.Sequential)]
struct MyStruct
{
	public int A;
	public int B;
}

IntPtr unmanagedStruct = /* some unmanaged memory containing the structure */; 
MyStruct myStruct = Marshal.PtrToStructure<MyStruct>(unmanagedStruct);
```

## StructureToPtr

Marshals data from a managed object to an unmanaged block of memory.
```csharp
MyStruct managedStruct = new MyStruct { A = 10, B = 20 };
IntPtr unmanagedStruct = Marshal.AllocHGlobal(Marshal.SizeOf(managedStruct));
Marshal.StructureToPtr(managedStruct, unmanagedStruct, false);
```

## GetFunctionPointerForDelegate

Converts a delegate to a function pointer callable from unmanaged code.
```csharp
[UnmanagedFunctionPointer(CallingConvention.Cdecl)]
delegate int MyFunction(int a, int b);

MyFunction func = (a, b) => a + b;
IntPtr funcPtr = Marshal.GetFunctionPointerForDelegate(func);
```

## GetDelegateForFunctionPointer

Converts a function pointer to a delegate.
```csharp
MyFunction managedDelegate = Marshal.GetDelegateForFunctionPointer<MyFunction>(funcPtr);
```
