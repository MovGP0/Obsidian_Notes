Configure default Git editor
```powershell
git config --global core.editor "code" --wait
```

Edit global config
```powershell
git config --global -e
```

Configure `.gitconfig`
```ini
[credential "http://10.1.1.204:8080"]
    provider = generic
[user]
    name = Johann Dirry
    email = johann.dirry@microsea.at
[core]
    editor = code --wait
    autocrlf = true
[diff]
    tool = default-difftool
[difftool "default-difftool"]
    cmd = code --wait --diff $LOCAL $REMOTE
[merge]
    tool = code
[mergetool "code"]
    cmd = code --wait --merge $REMOTE $LOCAL $BASE $MERGED
```