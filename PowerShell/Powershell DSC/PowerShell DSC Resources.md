# PowerShell DSC Resources
Resources are responsible to bring the system into a specific state.

## Base Resources
List all resources using `Get-DscResource `

| Resource         | Description                                            |
| ---------------- | ------------------------------------------------------ |
| `Service`        | Start/Stop Windows Service                             |
| `Script`         | PowerShell code block that executes on the target host |
| `User`           | Create/Delete/Modify local users                       |
| `WindowsProcess` | Execute an program with specific parameters            |
| `WindowsFeature` | Add/Remove Windows Feature                             |
| `Registry`       | Add/Remove/Modify Registry entries                     |
| `Environment`    | Add/Remove/Modify Environment Variables                |
| `Archive`        | Operations on compressed files                         |
| `Group`          | Add/Remove/Modify membership on local groups and users |
| `Package`        | Install software using `*.msi` or `*.exe` installers   |
| `Log`            | Log output for diagnostics and troubleshooting         |

## Implement custom DSC Resources
- [Writing a custom DSC resource with MOF](https://learn.microsoft.com/en-us/powershell/dsc/resources/authoringresourcemof?view=dsc-1.1)
- [Writing a custom DSC resource with PowerShell classes](https://learn.microsoft.com/en-us/powershell/dsc/resources/authoringresourceclass?view=dsc-1.1)
- [Composite resources - Using a DSC configuration as a resource](https://learn.microsoft.com/en-us/powershell/dsc/resources/authoringresourcecomposite?view=dsc-1.1)
- [Authoring a DSC resource in C#](https://learn.microsoft.com/en-us/powershell/dsc/resources/authoringresourcemofcs?view=dsc-1.1)
- [Using the Resource Designer tool](https://learn.microsoft.com/en-us/powershell/dsc/resources/authoringresourcemofdesigner?view=dsc-1.1)


## Example DSC files
### Ensure file is present
```powershell
configuration ExampleConfiguration
{
    # (optional) ensure resource is imported
    Import-DSCResource -Name File;
    Node localhost
    {
	    File ExampleFile
	    {
	        Ensure = 'present'
	        Type = 'File'
	        DestinationPath = 'C:\Foobar.txt'
	        Contents = "content of the file";
	    }
    }
}
```
### Ensure WindowsFeature is installed
```powershell
configuration ExampleConfiguration
{
    # (optional) ensure resource is imported
    Import-DSCResource -Name WindowsFeature;
     
    Node localhost
    {
	    WindowsFeature SomeFeature
	    {
	        Ensure = 'Present'
	        Name = 'NameOfTheFeature'
	    }
    }
}
```
## Ensure Windows Service is running
```powershell
Import-DSCResource -Name Service;
Node localhost
{
    Service "Spooler:Running"
    {
	    Name = "Spooler"
	    State = "Running"
	}
	Service "DHCP:Running"
	{
		Name = "DHCP"
		State = "Running"
	}
}
```