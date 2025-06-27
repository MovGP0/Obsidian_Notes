```csharp
public static class DependencyInjection
{
	public static IServiceCollection AddSystemIO(this IServiceCollection services)
	{
		services.AddTransient<IDirectory, DirectoryWrapper>();
		services.AddTransient<IFile, FileWrapper>();
		services.AddTransient<IPath, PathWrapper>();
		return services;
	}
}
```

### Directory Wrapper

```csharp
namespace System.IO;

public interface IDirectory
{
	DirectoryInfo CreateDirectory(string path);
	bool Exists(string path);
	string[] GetFiles(string path);
	string[] GetFiles(string path, string searchPattern);
	void Delete(string path);
}

public sealed class DirectoryWrapper : IDirectory
{
	public DirectoryInfo CreateDirectory(string path) => Directory.CreateDirectory(path);
	public bool Exists(string path) => Directory.Exists(path);
	public string[] GetFiles(string path) => Directory.GetFiles(path);
	public string[] GetFiles(string path, string searchPattern) => Directory.GetFiles(path, searchPattern);
	public void Delete(string path) => Directory.Delete(path);
}
```

### File Wrapper

```csharp
namespace System.IO;

public interface IFile
{
	bool Exists(string path);
	void Copy(string sourceFileName, string destFileName);
	void Delete(string path);
	FileStream Create(string path, int bufferSize, FileOptions options);
	DateTime GetLastAccessTime(string path);
	DateTime GetLastWriteTime(string path);
	DateTime GetCreationTime(string path);
	DateTime GetLastAccessTimeUtc(string path);
	DateTime GetLastWriteTimeUtc(string path);
	DateTime GetCreationTimeUtc(string path);
	byte[] ReadAllBytes(string path);
	void Move(string sourceFileName, string destFileName);
	string ReadAllText(string path);
	string ReadAllText(string path, Encoding encoding);
	Task<string> ReadAllTextAsync(string path, CancellationToken cancellationToken);
	Task<string> ReadAllTextAsync(string path, Encoding encoding, CancellationToken cancellationToken);
	FileStream Open(string path, FileMode mode);
	FileStream Open(string path, FileMode mode, FileAccess access);
	FileStream Open(string path, FileMode mode, FileAccess access, FileShare share);
	FileStream OpenRead(string path);
	StreamReader OpenText(string path);
	FileStream OpenWrite(string path);
	IEnumerable<string> ReadLines(string path);
	IEnumerable<string> ReadLines(string path, Encoding encoding);
	Task<string[]> ReadAllLinesAsync(string path, CancellationToken cancellationToken);
	Task<string[]> ReadAllLinesAsync(string path, Encoding encoding, CancellationToken cancellationToken);
}

public sealed class FileWrapper : IFile
{
	public bool Exists(string path) => File.Exists(path);
	public void Copy(string sourceFileName, string destFileName) => File.Copy(sourceFileName, destFileName);
	public void Delete(string path) => File.Delete(path);
	public FileStream Create(string path, int bufferSize, FileOptions options) => File.Create(path, bufferSize, options);
	public DateTime GetLastAccessTime(string path) => File.GetLastAccessTime(path);
	public DateTime GetLastWriteTime(string path) => File.GetLastWriteTime(path);
	public DateTime GetCreationTime(string path) => File.GetCreationTime(path);
	public byte[] ReadAllBytes(string path) => File.ReadAllBytes(path);
	public string ReadAllText(string path) => File.ReadAllText(path);
	public string ReadAllText(string path, Encoding encoding) => File.ReadAllText(path, encoding);
	public void Move(string sourceFileName, string destFileName) => File.Move(sourceFileName, destFileName);
	public DateTime GetLastAccessTimeUtc(string path) => File.GetLastAccessTimeUtc(path);
	public DateTime GetLastWriteTimeUtc(string path) => File.GetLastWriteTimeUtc(path);
	public DateTime GetCreationTimeUtc(string path) => File.GetCreationTimeUtc(path);
	public Task<string> ReadAllTextAsync(string path, CancellationToken cancellationToken)  => File.ReadAllTextAsync(path, cancellationToken);
	public Task<string> ReadAllTextAsync(string path, Encoding encoding, CancellationToken cancellationToken) => File.ReadAllTextAsync(path, encoding, cancellationToken);
	public FileStream Open(string path, FileMode mode) => File.Open(path, mode);
	public FileStream Open(string path, FileMode mode, FileAccess access) => File.Open(path, mode, access);
	public FileStream Open(string path, FileMode mode, FileAccess access, FileShare share) => File.Open(path, mode, access, share);
	public FileStream OpenRead(string path) => File.OpenRead(path);
	public StreamReader OpenText(string path) => File.OpenText(path);
	public FileStream OpenWrite(string path) => File.OpenWrite(path);
	public IEnumerable<string> ReadLines(string path) => File.ReadLines(path);
	public IEnumerable<string> ReadLines(string path, Encoding encoding) => File.ReadLines(path, encoding);
	public Task<string[]> ReadAllLinesAsync(string path, CancellationToken cancellationToken) => File.ReadAllLinesAsync(path, cancellationToken);
	public Task<string[]> ReadAllLinesAsync(string path, Encoding encoding, CancellationToken cancellationToken) => File.ReadAllLinesAsync(path, encoding, cancellationToken);
}
```

### PathWrapper

```csharp
namespace System.IO;

public interface IPath
{
	string GetTempPath();
	string GetFileName(string path);
	string Combine(string path1, string path2);
	string Combine(string path1, string path2, string path3);
	string Combine(string path1, string path2, string path3, string path4);
	string Combine(params string[] paths);
	string GetTempFileName();
	string GetExtension(string path);
	string GetFileNameWithoutExtension(string path);
}

public sealed class PathWrapper : IPath
{
	public string GetTempPath() => Path.GetTempPath();
	public string GetFileName(string path) => Path.GetFileName(path);
	public string Combine(string path1, string path2) => Path.Combine(path1, path2);
	public string Combine(string path1, string path2, string path3) => Path.Combine(path1, path2, path3);
	public string Combine(string path1, string path2, string path3, string path4) => Path.Combine(path1, path2, path3, path4);
	public string Combine(params string[] paths) => Path.Combine(paths);
	public string GetTempFileName() => Path.GetTempFileName();
	public string GetExtension(string path) => Path.GetExtension(path);
	public string GetFileNameWithoutExtension(string path) => Path.GetFileNameWithoutExtension(path);
}
```