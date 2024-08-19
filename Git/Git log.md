Graphical representation of the git log
```powershell
git log --graph --decorate --oneline
```

Search the log for a given text
```powershell
git log -S "FOOBAR"
```

Custom log formatting
```powershell
git config --global alias.hs "log --pretty='%C(yellow)%h %C(cyan)%cd %Cblue%aN%C(auto)%d %Creset%s' --graph --date=relative --date-order"
git hs
```