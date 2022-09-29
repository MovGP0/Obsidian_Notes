# Deployment Image Servicing and Management (DISM)
DISM needs to be executed as Administrator.

| Command                                                                                                  | Description                                       |
| -------------------------------------------------------------------------------------------------------- | ------------------------------------------------- |
| `dism /Online /Cleanup-Image /StartComponentCleanup`                                                     |                                                   |
| `dism /Online /Cleanup-Image /RestoreHealth`                                                             |                                                   |
| `dism /Online /Cleanup-Image /AnalyzeComponentStore`                                                     | Analyze WinSXS Folder                             |
| `dism /Get-WimInfo /WimFile:*:sources/install.wim`                                                       | Get information of `install.wim` file             |
| `dism /Get-WimInfo /WimFile:*:sources/install.esd`                                                       | Get information of `install.esd` file             |
| `dism /Online /Cleanup-Image /RestoreHealth /Source:WIM:*:\sources\install.wim:IndexNumber /LimitAccess` | Use `install.wim` file from mounted windows image |
| `dism /Online /Cleanup-Image /RestoreHealth /Source:ESD:*:\sources\install.esd:IndexNumber /LimitAccess` | Use `install.esd` file from mounted windows image |

## Sources
* [DISM Cheat Sheet](https://cheatography.com/jandreacola/cheat-sheets/dism-deployment-image-servicing-and-management/)