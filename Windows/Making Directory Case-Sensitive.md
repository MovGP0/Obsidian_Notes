To make a directory in Windows case-sensitve, use the following command:
```shell
fsutil.exe file setCaseSensitiveInfo d:\Dev enable
```

Also consider the git setting:
```shell
git config core.ignorecase false
```

## Case-Insensitive Directories on Linux (`casefold`)

On Linux, the Kernel needs to support case-insensitive folders. Check if the following file exists:
```bash
cat /sys/fs/ext4/features/casefold
```

The file system needs to be created with support of the `casefold` feature:
```bash
mkfs.ext4 -O casefold /dev/sdX
```

Check if the file system supports the `casefold` feature:
```bash
dumpe2fs -h /dev/sdX | grep 'Filesystem features'
```

Create an empty folder
```shell
mkdir /mnt/your_directory
```

Set the `+F` attribute to enable `casefold`
```bash
chattr +F /mnt/your_directory
```

Verify that `casefold` is active
```bash
lsattr /mnt/your_directory
```

## Notes

- When using a JetBrains IDE, make sure to open the Action `Registry` and disable the setting `ide.new.project.model.index.case.sensitivity`. Then use `File`/`Invalidate Caches...` to clear all caches and restart the IDE.