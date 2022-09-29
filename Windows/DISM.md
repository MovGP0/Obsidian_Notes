# Deployment Image Servicing and Management (DISM)
DISM needs to be executed as Administrator.

| Command                                                                                                  | Description                                       |
| -------------------------------------------------------------------------------------------------------- | ------------------------------------------------- |
| `dism /Online /Cleanup-Image /StartComponentCleanup`                                                     |                                                   |
| `dism /Online /Cleanup-Image /RestoreHealth`                                                             |                                                   |
| `dism /Online /Cleanup-Image /AnalyzeComponentStore`                                                     | Analyze WinSXS Folder                             |
| `dism /Get-WimInfo /WimFile:*:sources/install.wim`                                                       | Get information of `install.wim` file             |
| `dism /Get-WimInfo /WimFile:*:sources/install.esd`                                                       | Get information of `install.esd` file             |
| `dism /Online /Cleanup-Image /RestoreHealth /Source:D:\sources\install.wim:2 /LimitAccess` | Use `install.wim` file from mounted windows image |
| `dism /Online /Cleanup-Image /RestoreHealth /Source:D:\sources\install.esd:2 /LimitAccess` | Use `install.esd` file from mounted windows image |

## Cleanup
* Check Windows version
```powershell
winver
```
* Download Windows Image from [Windows Eval Center](https://www.microsoft.com/en-us/evalcenter)
* Mount the `.iso` image
* Get the available images
```powershell
dism /Get-WimInfo /WimFile:D:sources/install.wim
```
* Start repair installation
```powershell
dism /Online /Cleanup-Image /RestoreHealth /Source:D:\sources\install.wim:2
```
## Sources
* [DISM Cheat Sheet](https://cheatography.com/jandreacola/cheat-sheets/dism-deployment-image-servicing-and-management/)