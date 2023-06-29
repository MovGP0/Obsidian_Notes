## Pin to Quick Access

1. right click the **Recycle Bin** on the desktop
2. choose **Pin to Quick Access**

## Add to "This PC"

Add the entry
```powershell
New-Item -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\MyComputer\NameSpace\{645FF040-5081-101B-9F08-00AA002F954E}"
```

Undo
```powershell
Remove-Item -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\MyComputer\NameSpace\{645FF040-5081-101B-9F08-00AA002F954E}"
```
