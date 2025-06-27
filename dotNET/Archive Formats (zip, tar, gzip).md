
## TAR Achive

**Requires .NET 7+**

Pack a TAR archive. The achive is then packed using GZip.
```csharp
using TarWriter writer = new(ms, TarEntryFormat.Pax, leaveOpen: true);

foreach (var file in new System.IO.DirectoryInfo(folder).GetFiles("*.xlsx"))
{
	writer.WriteEntry(fileName: file.FullName, entryName: file.Name);
}

using FileStream tarstream = File.Create(tarFile);
using GZipStream compressor = new(tarstream, CompressionMode.Compress);

using MemoryStream ms = new();
ms.Seek(0, SeekOrigin.Begin);
ms.CopyTo(compressor);
ms.Close();
```

Unpack a TAR achive
```csharp
using FileStream compressedStream = File.OpenRead(@"/path/to/file.tar.gz");
using GZipStream decompressor = new(compressedStream, CompressionMode.Decompress);
TarFile.ExtractToDirectory(source: decompressor, destinationDirectoryName: @"/path/to/target/directory", overwriteFiles: true);
```

Unpack specific files from TAR achive
```csharp
using FileStream fs = File.OpenRead(@"/path/to/file.tar.gz");
using GZipStream decompressor = new(fs, CompressionMode.Decompress);
using TarReader reader = new(decompressor, leaveOpen: false);

TarEntry? entry;
int count = 0;
while ((entry = reader.GetNextEntry(copyData: true)) != null)
{
    if (entry.Name.EndsWith(".xlsx"))
    {
	    string destFileName = Path.Join(folder3, entry.Name);
	    entry.ExtractToFile(destFileName, overwrite: true);
    }
}
```