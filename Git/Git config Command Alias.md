Define an command alias
```powershell
git config --global alias.ac '!git add -A && git commit -m'

# prefixes 'git' command when no '!' is defined
git config --global alias.a 'add .'
```

Usage
```powershell
git ac "Commit message"
git a
```