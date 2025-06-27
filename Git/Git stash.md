Add the current changes to the Stash stack
```bash
git stash
git stash push -m "itemname"
```

List the current items on the Stash
```bash
git stash list
```
```
stash@{0}: On main: itemname
stash@{1}: On main: another-itemname
```

Get changes from the stash and remove the stash item
```bash
git stash pop
git stash pop stash@{1}
git stash pop --index 1
```

Get changes from the stash and keep the stash item
```bash
git stash apply
git stash apply stash@{1}
git stash apply --index 1
```

Delete last stash item without applying
```bash
git stash drop
```

Delete all stash items
```bash
git stash clear
```