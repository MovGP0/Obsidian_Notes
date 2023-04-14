Restore NuGet files
```powershell
dotnet restore './MySolution.sln' --interactive --configfile './NuGet.config'
```

Build a solution
```powershell
dotnet build './MySolution.sln' --configuration 'Debug'
```

Publish a solution
```powershell
dotnet publish './MySolution.sln' --configuration 'Release' --output './Publish/' --runtime 'win81-x64' --self-contained
```
