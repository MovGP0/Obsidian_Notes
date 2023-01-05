```bash
ls -l filename
```

```
-rwxr-xr-- 1 OWNERNAME GROUPNAME 956 TIMESTAMP FILENAME
```
0th letter = `d` directoy or `-` file
1st 3 letters = owner permissions
2nd 3 letters = group permissions
3rd 3 letters = all user permissions

## chmod
set file permission
```bash
chmod +w filename
```

remove file permission
```bash
chmod -r filename
```

set `setuid` to allow user to access a file with owner permission
```bash
chmod +s filename
```

set `setgit` to allow user to access a file with group permission
```bash
chmod +g filename
```

## chown
sets owner of file
```bash
sudo chown root filename
```

