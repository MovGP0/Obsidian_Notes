# PowerShell DSC Local Configuration Manager
Responsible for scheduling and executing `*.mof` files.

Manage using:
- `Get-DscLocalConfigurationManager`
- `Set-DscLocalConfigurationManager`

## Important properties

| Property                         | Description                                                                                                    |
| -------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| `AllowModuleOverwrite`           | Allow/Disallow overriding DSC modules                                                                          |
| `ConfigurationMode`              | Mode of when and how to apply configuration changes (`ApplyOnly`, `ApplyAndMonitor`, or `ApplyAndAutoCorrect`) |
| `ConfigurationModeFrequencyMins` | How often to check for configuration drift                                                                     |
| `RebootNodeIfNeeded`             | Reboot the server if needed                                                                                    |
| `RefreshFrequencyMins`           | How often to check pull server updates                                                                         |
| `RefreshMode`                    | `Push`, or `Pull`                                                                                              |

## Set LCM using DSC Configuration
```powershell
Configuration Foobar
{
    Import-Resource LocalConfigurationManager;
    Target localhost
    {
        LocalConfigurationManager
        {
            ConfigurationMode = 'ApplyAndCorrect'
            ConfigurationModeFrequencyMins = 120
            RefreshMode = 'Push'
            RebootIfNeeded = $true
        }
    }
}
```
