---
title: dolt with .NET
---
### Check if dolt is installed

```csharp
public static bool IsDoltInstalled()
{
    try
    {
        var startInfo = new ProcessStartInfo("dolt", "version")
        {
            RedirectStandardOutput = true,
            RedirectStandardError = true,
            UseShellExecute = false,
            CreateNoWindow = true
        };
        using var process = Process.Start(startInfo);
        process.WaitForExit();

        // ExitCode == 0 indicates the command executed (Dolt is present)
        return process.ExitCode == 0;
    }
    catch (Win32Exception)
    {
        // Thrown if "dolt" is not found on PATH
        return false;
    }
}
```

### Check if dolt is installed using the `PATH` variable

> [!Warning]
> This only confirms if the binary is present; does not check if it can start

```csharp
public static bool IsDoltOnPath()
{
    var pathEnv = Environment.GetEnvironmentVariable("PATH") ?? "";
    var paths = pathEnv.Split(Path.PathSeparator, StringSplitOptions.RemoveEmptyEntries);

    string fileName = OperatingSystem.IsWindows() ? "dolt.exe" : "dolt";

    return paths.Any(dir =>
    {
        try
        {
            var fullPath = Path.Combine(dir, fileName);
            return File.Exists(fullPath);
        }
        catch
        {
            // Skip invalid paths or permission issues
            return false;
        }
    });
}
```

### NuGet Packages

```sh
# for connection
dotnet add package MySqlConnector

# ORM mapping
dotnet add package Dapper

# for configuration
dotnet add package Microsoft.Extensions.Configuration
dotnet add package Microsoft.Extensions.Configuration.Json
dotnet add package Microsoft.Extensions.Configuration.EnvironmentVariables
dotnet add package Microsoft.Extensions.Configuration.CommandLine

# for the app builder
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;
```

Configure using `appsettings.json`
```json
{
  "Dolt": {
    "Host": "127.0.0.1",
    "Port": 3306,
    "DataDir": "."
  },
  "Server": {
    "MaxAttempts": 10,
    "DelayMs": 500
  }
}
```

Setting classes:
```cs
public sealed class DoltSettings
{
	public string Host { get; set; } = "127.0.0.1";
	public int Port { get; set; } = 3306;
	public string DataDir { get; set; } = Directory.GetCurrentDirectory();
}

public sealed class ServerSettings
{
	public int DelayMs { get; set; } = 500;
	public int MaxAttempts { get; set; } = 10;
}
```

In `Program.cs`
```cs
static void Main(string[] args)
{
    HostApplicationBuilder builder = Host.CreateApplicationBuilder(args);

	// Build configuration from JSON, env vars, and CLI args
	builder.Configuration
		.SetBasePath(Directory.GetCurrentDirectory())
		.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
		.AddEnvironmentVariables(prefix: "MYAPP_")
		.AddCommandLine(args);

	// Bind to strongly typed settings
	builder.Services.Configure<DoltSettings>(builder.Configuration.GetSection("Dolt"));
    builder.Services.Configure<ServerSettings>(builder.Configuration.GetSection("Server"));

	builder.Services.AddScoped<DoltServerManager>();
	IHost app = builder.Build();

	var manager = app.Services.GetRequiredService<DoltServerManager>();
	manager.EnsureServerRunning();
}
```

```cs
public sealed class DoltServerManager
{
	readonly string _host;
	readonly int _port;
	readonly string _dataDir;
	readonly int _maxAttempts;
	readonly int _delayMs;
	readonly string _connStr;

	public DoltServerManager(DoltSettings doltSettings, ServerSettings serverSettings)
	{
		_host = doltSettings.Host;
		_port = doltSettings.Port;
		_dataDir = doltSettings.DataDir;
		_maxAttempts = serverSettings.MaxAttempts;
		_delayMs = serverSettings.DelayMs;

		var dbName = Path.GetFileName(_dataDir).Replace('-', '_');
		_connStr = $"Server={_host};Port={_port};User Id=root;Database={dbName};";
	}

	public void EnsureServerRunning()
	{
		try
		{
			using var conn = new MySqlConnection(_connStr);
			conn.Open();
			Console.WriteLine("Connected to Dolt SQL server.");
		}
		catch (MySqlException)
		{
			Console.WriteLine("Cannot connect; starting Dolt SQL server...");
			StartServer();
			WaitForServerReady();
		}
	}

	void StartServer()
	{
		var psi = new ProcessStartInfo("dolt",
			$"sql-server --host={_host} --port={_port} --data-dir=\"{_dataDir}\"")
		{
			UseShellExecute = false,
			RedirectStandardOutput = true,
			RedirectStandardError = true,
			CreateNoWindow = true
		};
		Process.Start(psi);
	}

	void WaitForServerReady()
	{
		for (int attempt = 1; attempt <= _maxAttempts; attempt++)
		{
			try
			{
				using var conn = new MySqlConnection(_connStr);
				conn.Open();
				Console.WriteLine("Dolt server is ready.");
				return;
			}
			catch
			{
				Console.WriteLine($"Waiting for server... ({attempt}/{_maxAttempts})");
				Thread.Sleep(_delayMs);
			}
		}

		throw new InvalidOperationException($"Dolt SQL server failed to start after {_maxAttempts} attempts.");
	}
}
```
