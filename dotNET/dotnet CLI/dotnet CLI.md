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

## Common `dotnet` commands

| Command             | Description                                                             |
|---------------------|-------------------------------------------------------------------------|
| `dotnet new`        | Initializes a new project. Can specify templates like `console`, `web`, etc. |
| `dotnet restore`    | Restores the dependencies and tools of a project.                       |
| `dotnet build`      | Builds a project and all of its dependencies.                           |
| `dotnet run`        | Builds and runs the application.                                        |
| `dotnet publish`    | Publishes a .NET application or DLL in a distribution-friendly format.   |
| `dotnet pack`       | Creates a NuGet package from the project.                               |
| `dotnet push`       | Pushes a package to a package source.                                   |
| `dotnet test`       | Runs the tests in a project.                                            |
| `dotnet add`        | Adds a package or reference to the project.                             |
| `dotnet remove`     | Removes a package or reference from the project.                        |
| `dotnet list`       | Lists packages or references for a project.                             |
| `dotnet clean`      | Cleans the build artifacts of a project.                                |
| `dotnet sln`        | Manages solution files and the projects they contain.                   |
| `dotnet migrate`    | (Deprecated in newer versions) Migrates a valid .NET Core project to a .NET SDK-style project. |
| `dotnet user-secrets` | Manages user-level secrets. Used typically in development to avoid storing app secrets in code. |
| `dotnet watch`     | Starts a file watcher that re-runs a specified dotnet command (like `build` or `test`) when source files change. |
