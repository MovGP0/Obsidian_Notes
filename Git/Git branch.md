Create a branch and checkout the newly created branch
```bash
git branch feature/foobar
git checkout feature/foobar
```

Create a branch during an checkout
```bash
git checkout -b feature/foobar
```

List the git branches
```bash
git branch
```

List local and remote branches
```bash
git branch --all
```

List the git branches with information of the last commit
```bash
git branch -vv
git branch -vv --all
```

Delete a branch
```bash
git branch -d 'feature/foobar'
```