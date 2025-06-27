## Install

```powershell
choco install gittfs
```

## Examples

List Remote Branches
```powershell
git tfs list-remote-branches 'https://tfs.mydomain.com/Workspace/'
```
```txt
$/Project/Source
|
+- $/Project/Foo
|
+- $/Project/Bar
```

Clone TFSVC Repository
```powershell
git tfs clone 'https://tfs.mydomain.com/Workspace/' '$/Project/Folder'
```

Output log
```powershell
git log --all --oneline --decorate --graph > log.txt
```

Initial Setup
```powershell
git config --global user.name "First Last"
git config --global user.email "first.last@example.com"
choco install gittfs
```

```powershell
git tfs list-remote-branches 'http://10.1.1.204:8080/tfs/Company/'
git tfs clone 'http://10.1.1.204:8080/tfs/Company/' '$/ProjectName/Development' Development
git tfs verify
```

Clone a folder
```powershell
git tfs clone 'http://10.1.1.1:8080/tfs/Company/' '$/ProjectName/Folder' FOLDERNAME --branches=none
```

```powershell
git tfs clone 'http://10.1.1.204:8080/tfs/Company/' '$/ProjectName' ProjectName
git tfs verify
```

Fetch changes
```powershell
git tfs fetch
```

Commit changes with commit message
```powershell
git tfs checkin -m "MESSAGE"
```

Push changes to TFS
```powershell
git tfs checkin
```

## Rename Changeset

```powershell
tf changeset /comment:"This is a new comment." 8675309
```

## Sources

[Github: git-tfs](https://github.com/git-tfs/git-tfs)
