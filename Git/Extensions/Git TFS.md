## Code

[git-tfs Github](https://github.com/git-tfs/git-tfs)

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
git tfs clone 'https://tfs.mydomain.com/Workspace/' '$/Project/Source'
```

Output log
```powershell
git log --all --oneline --decorate --graph > log.txt
```
