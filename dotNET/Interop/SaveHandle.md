In .NET, `SafeHandle` is an abstract class that provides a reliable way to handle unmanaged resources by encapsulating operating system handles.

It ensures that resources are released appropriately, even in the presence of exceptions, by implementing a critical finalizer that guarantees the release of the handle when the garbage collector reclaims the `SafeHandle` object.

This approach helps prevent resource leaks and enhances the reliability of applications that interact with unmanaged resources.

# `SafeFileHandle`

Wraps a file handle.

**Example** 
```csharp
using System;
using System.IO;
using System.Runtime.InteropServices;
using Microsoft.Win32.SafeHandles;

class Program
{
	[DllImport("kernel32.dll", SetLastError = true, CharSet = CharSet.Unicode)]
	private static extern SafeFileHandle CreateFile(
		string lpFileName,
		uint dwDesiredAccess,
		uint dwShareMode,
		IntPtr lpSecurityAttributes,
		uint dwCreationDisposition,
		uint dwFlagsAndAttributes,
		IntPtr hTemplateFile);

const uint GENERIC_READ = 0x80000000;
const uint OPEN_EXISTING = 3;

static void Main()
{
	using (SafeFileHandle handle = CreateFile(
		"example.txt",
		GENERIC_READ,
		0,
		IntPtr.Zero,
		OPEN_EXISTING,
		0,
		IntPtr.Zero))
	{
		if (handle.IsInvalid)
		{
			throw new System.ComponentModel.Win32Exception(Marshal.GetLastWin32Error());
		}

		// Use the file handle as needed
	} // handle.Dispose() is called automatically here
}
```

## `SafeWaitHandle`

Wraps a wait handle, such as those used for synchronization primitives.

**Example**
```csharp
using System;
using System.Runtime.InteropServices;
using Microsoft.Win32.SafeHandles;
    
    class Program
    {
	    [DllImport("kernel32.dll", SetLastError = true)]
	    private static extern SafeWaitHandle CreateEvent(
		    IntPtr lpEventAttributes,
		    bool bManualReset,
		    bool bInitialState,
		    string lpName);

    static void Main()
    {
        using (SafeWaitHandle waitHandle = CreateEvent(
            IntPtr.Zero,
            false,
            false,
            null))
        {
            if (waitHandle.IsInvalid)
            {
                throw new System.ComponentModel.Win32Exception(Marshal.GetLastWin32Error());
            }
    
            // Use the wait handle as needed
        } // waitHandle.Dispose() is called automatically here
    }
}
 ```
 
## `SafeProcessHandle`

Wraps a process handle.

**Example**
```csharp
using System;
using System.ComponentModel;
using System.Runtime.InteropServices;
using Microsoft.Win32.SafeHandles;
    
class Program
{
	[DllImport("kernel32.dll", SetLastError = true)]
	private static extern SafeProcessHandle OpenProcess(
		uint processAccess,
		bool bInheritHandle,
		uint processId);

    const uint PROCESS_QUERY_INFORMATION = 0x0400;
    
    static void Main()
    {
        uint processId = 1234; // Example process ID
        using (SafeProcessHandle processHandle = OpenProcess(
            PROCESS_QUERY_INFORMATION,
            false,
            processId))
        {
            if (processHandle.IsInvalid)
            {
                throw new Win32Exception(Marshal.GetLastWin32Error());
            }
    
            // Use the process handle as needed
        } // processHandle.Dispose() is called automatically here
    }
}
```

## `SafeRegistryHandle` 

Wraps a handle to a registry key.

**Example**
```csharp
using System;
using Microsoft.Win32;
using Microsoft.Win32.SafeHandles;
    
class Program
{
	static void Main()
	{
		using (RegistryKey key = Registry.CurrentUser.OpenSubKey("Software\Example"))
		{
			if (key != null)
			{
				SafeRegistryHandle handle = key.Handle; // Use the registry handle as needed
			}
		} // key.Dispose() is called automatically here
	}
}
```

## `SafeMemoryMappedFileHandle`

Wraps a handle to a memory-mapped file.

**Example**
```csharp
using System;
using System.IO.MemoryMappedFiles;
using Microsoft.Win32.SafeHandles;
    
class Program
{
	static void Main()
	{
		using (MemoryMappedFile mmf = MemoryMappedFile.CreateNew("testmap", 1024))
		{
			SafeMemoryMappedFileHandle handle = mmf.SafeMemoryMappedFileHandle;
			// Use the memory-mapped file handle as needed
		} // mmf.Dispose() is called automatically here
	}
}
```

## `SafeMemoryMappedViewHandle`

Wraps a handle to a view of a memory-mapped file.

 **Example**
```csharp
using System;
using System.IO.MemoryMappedFiles;
using Microsoft.Win32.SafeHandles;

class Program
{
	static void Main()
	{
		using (MemoryMappedFile mmf = MemoryMappedFile.CreateNew("testmap", 1024))
		{
			using (MemoryMappedViewAccessor accessor = mmf.CreateViewAccessor())
			{
				SafeMemoryMappedViewHandle handle = accessor.SafeMemoryMappedViewHandle;
				// Use the memory-mapped view handle as needed
			}
		} // accessor.Dispose() and mmf.Dispose() are called automatically here
	}
}    
```

## `SafePipeHandle`

Wraps a handle to a named or anonymous pipe.

**Example**
```csharp
using System;
using System.IO.Pipes;
using Microsoft.Win32.SafeHandles;

class Program
{
    static void Main()
    {
        // Create an anonymous pipe server stream
        using (var pipeServer = new AnonymousPipeServerStream(PipeDirection.Out))
        {
            // Retrieve the SafePipeHandle from the server stream
            SafePipeHandle serverHandle = pipeServer.SafePipeHandle;

            // Use the handle as needed
            // For demonstration, we'll write a simple message to the pipe
            using (var writer = new StreamWriter(pipeServer))
            {
                writer.AutoFlush = true;
                writer.WriteLine("Hello from the pipe server!");
            }

            // The SafePipeHandle will be released when the pipeServer is disposed
        } // pipeServer.Dispose() is called automatically here
    }
}
```

## `SafeNCryptHandle`

Provides a safe handle that can be used by Cryptography Next Generation (CNG) objects.

**Example**
```csharp
using System;
using System.Runtime.InteropServices;
using Microsoft.Win32.SafeHandles;

class Program
{
	[DllImport("ncrypt.dll", SetLastError = true)]
	private static extern int NCryptOpenStorageProvider(
		out SafeNCryptHandle phProvider,
		string pszProviderName,
		int dwFlags);

	static void Main()
	{
		SafeNCryptHandle providerHandle;
		int status = NCryptOpenStorageProvider(out providerHandle, null, 0);
	
		if (status != 0)
		{
			throw new Exception($"NCryptOpenStorageProvider failed with error code {status}");
		}
	
		// Use the provider handle as needed
	
		// Ensure the handle is released
		providerHandle.Dispose();
	}
}
```

## `SafeNCryptKeyHandle`

Represents a safe handle for a key (`NCRYPT_KEY_HANDLE`) in CNG.

**Example**
```csharp
using System;
using System.Runtime.InteropServices;
using Microsoft.Win32.SafeHandles;

class Program
{
	[DllImport("ncrypt.dll", SetLastError = true)]
	private static extern int NCryptOpenKey(
		SafeNCryptHandle hProvider,
		out SafeNCryptKeyHandle phKey,
		string pszKeyName,
		int dwLegacyKeySpec,
		int dwFlags);
	
	static void Main()
	{
		SafeNCryptHandle providerHandle = /* obtain provider handle */;
		SafeNCryptKeyHandle keyHandle;
		int status = NCryptOpenKey(providerHandle, out keyHandle, "MyKey", 0, 0);

		if (status != 0)
		{
			throw new Exception($"NCryptOpenKey failed with error code {status}");
		}
		
		// Use the key handle as needed

		// Ensure the handle is released
		keyHandle.Dispose();
		providerHandle.Dispose();
	}
}
 ```
 
### `SafeNCryptProviderHandle`

Represents a safe handle for a key storage provider (`NCRYPT_PROV_HANDLE`) in CNG.

**Example**
```csharp
using System;
using System.Runtime.InteropServices;
using Microsoft.Win32.SafeHandles;

class Program
{
	[DllImport("ncrypt.dll", SetLastError = true)]
	private static extern int NCryptOpenStorageProvider(
		out SafeNCryptProviderHandle phProvider,
		string pszProviderName,
		int dwFlags);

	static void Main()
	{
		SafeNCryptProviderHandle providerHandle;
		int status = NCryptOpenStorageProvider(
			out providerHandle,
			"Microsoft Software Key Storage Provider",
			0);
	
		if (status != 0)
		{
			throw new Exception($"NCryptOpenStorageProvider failed with error code {status}");
		}
	
		// Use the provider handle as needed
	
		// Ensure the handle is released
	
		providerHandle.Dispose();
	}
}
```

## `SafeNCryptSecretHandle`

Represents a safe handle for a secret agreement value (`NCRYPT_SECRET_HANDLE`) in CNG.

```csharp
using System;
using System.Runtime.InteropServices;
using Microsoft.Win32.SafeHandles;

class Program
{
	[DllImport("ncrypt.dll", SetLastError = true)]
	private static extern int NCryptSecretAgreement(
		SafeNCryptKeyHandle hPrivKey,
		SafeNCryptKeyHandle hPubKey,
		out SafeNCryptSecretHandle phSecret,
		int dwFlags);

	static void Main()
	{
		SafeNCryptKeyHandle privateKeyHandle = /* obtain private key handle */;
		SafeNCryptKeyHandle publicKeyHandle = /* obtain public key handle */;
		int status = NCryptSecretAgreement(
			privateKeyHandle,
			publicKeyHandle,
			out SafeNCryptSecretHandle secretHandle,
			0);

		if (status != 0)
		{
			throw new Exception($"NCryptSecretAgreement failed with error code {status}");
		}

		// Use the secret handle as needed

		// Ensure the handles are released
		secretHandle.Dispose();
		privateKeyHandle.Dispose();
		publicKeyHandle.Dispose();
	}
}
```

## `SafeX509ChainHandle`

Provides a wrapper class that represents the handle of an X.509 chain object.

**Example**
```csharp
using System;
using System.Runtime.InteropServices;
using Microsoft.Win32.SafeHandles;

class Program
{
	[DllImport("crypt32.dll", SetLastError = true)]
	private static extern SafeX509ChainHandle CertDuplicateCertificateChain(
		SafeX509ChainHandle pChainContext);

	static void Main()
	{
		SafeX509ChainHandle originalChainHandle = /* obtain original chain handle */;
		SafeX509ChainHandle duplicateChainHandle = CertDuplicateCertificateChain(originalChainHandle);

		if (duplicateChainHandle == null || duplicateChainHandle.IsInvalid)
		{
			throw new Exception("CertDuplicateCertificateChain failed.");
		}

		// Use the duplicate chain handle as needed

		// Ensure the handles are released
		duplicateChainHandle.Dispose();
		originalChainHandle.Dispose();
	}
}
```

## Implementing a custom SaveHandle

1. Derive from `SafeHandle` or its Subclasses
    - Derive from `SafeHandleZeroOrMinusOneIsInvalid` if an invalid handle is represented by `0` or `-1`.
    - Derive from `SafeHandleMinusOneIsInvalid` if an invalid handle is represented by `-1`.
2. Implement the `IsInvalid` Property
	- Override the `IsInvalid` property to specify when the handle is considered invalid.
3. Override the `ReleaseHandle` Method
	- Provide the logic to release the unmanaged resource in the `ReleaseHandle` method. This method is called by the runtime to free the handle when it's no longer needed.

**Example**
```csharp
using System;
using System.Runtime.InteropServices;
using System.Security;
using System.Runtime.ConstrainedExecution;

public sealed class MySafeHandle : SafeHandleZeroOrMinusOneIsInvalid
{
    // Constructor that sets the handle and indicates ownership
    public MySafeHandle(IntPtr handle, bool ownsHandle)
        : base(ownsHandle)
    {
        SetHandle(handle);
    }

    // Override IsInvalid to determine when the handle is invalid
    public override bool IsInvalid
    {
        get { return handle == IntPtr.Zero || handle == new IntPtr(-1); }
    }

    // Override ReleaseHandle to execute code required to free the handle
    [ReliabilityContract(Consistency.WillNotCorruptState, Cer.Success)]
    protected override bool ReleaseHandle()
    {
        // Call the appropriate method to release the handle
        // For example, if the handle is a file handle:
        return CloseHandle(handle);
    }

    // P/Invoke declaration for releasing the handle
    [DllImport("kernel32.dll", SetLastError = true)]
    [SuppressUnmanagedCodeSecurity]
    private static extern bool CloseHandle(IntPtr hObject);
}
```

**Usage**
```csharp
using System;
using System.Runtime.InteropServices;

class Program
{
    static void Main()
    {
        // Obtain an unmanaged handle through P/Invoke or other means
        IntPtr unmanagedHandle = GetUnmanagedResource();

        // Wrap the unmanaged handle in your custom SafeHandle
        using (MySafeHandle safeHandle = new MySafeHandle(unmanagedHandle, ownsHandle: true))
        {
            if (safeHandle.IsInvalid)
            {
                throw new InvalidOperationException("Failed to acquire a valid handle.");
            }

            // Use the safeHandle as needed
            // The handle will be released automatically when safeHandle is disposed
        }
    }

    // Example method to obtain an unmanaged resource
    private static IntPtr GetUnmanagedResource()
    {
        // Implementation to acquire the unmanaged resource
        // For demonstration purposes, returning an invalid handle
        return IntPtr.Zero;
    }
}
```
## References

- [C# I wish I knew: SafeHandle](https://blog.benoitblanchon.fr/safehandle/)
- [Microsoft.Win32.SafeHandles Namespace](https://learn.microsoft.com/en-us/dotnet/api/microsoft.win32.safehandles)
