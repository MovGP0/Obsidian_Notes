Run the tests, but only show failing tests
```powershell
dotnet test --logger "console;verbosity=minimal"
```

Run the test and write the output to a trace file
```powershell
dotnet test --logger "trx;LogFileName=TestResults.trx"
```

Full Example
```powershell
dotnet test 'c:\path\to\MyProject.csproj' `
		--configuration "Debug" ` # build the project with the 'debug' build target
		--runtime 'win-x64' `
		--property:ParallelizeTestCollections=false ` # setting additional properties
		--no-restore ` # prevents git restore
		--blame ` # records the test that have been in progress when the test host crashes
		--logger "trx;LogFileName=MyProject.trx" ` # log the output to a trace file for later analysis
		--tl:auto; # use the improved terminal logger format when appropriate
```

Get list of failed unit tests
```powershell
$testOutput = dotnet test --no-build --verbosity normal --logger "console;verbosity=minimal";
$matches = [regex]::Matches($testOutput, '^.*Tests.*\[FAIL\]$');

$failedTests = @();
foreach ($match in $matches) {
	$failedTests += $match;
};

Write-Host $failedTests;
```

## See also

- [dotnet test](https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-test)
