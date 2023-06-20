Create the image file
```powershell
New-VHD -Path 'C:\vhdx.vhdx' -Fixed -SizeBytes 10GB
New-VHD -Path 'C:\vhdx.vhdx' -Dynamic -SizeBytes 10GB
```

Get the disk number
```powershell
Get-Disk
```

Initialize the disk inside the VHDX file
```powershell
Initialize-Disk -Number 1 -PartitionStyle GPT -PassThru `
    | New-Partition -AssignDriveLetter -UseMaximumSize `
    | Format-Volume -FileSystem ReFS -Confirm:$false
```

Mount the image
```powershell
Mount-DiskImage -ImagePath 'C:\vhdx.vhdx'
```
