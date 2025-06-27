Subtrees are integrated into the main repository.
The subtree's history is merged into the main repository's history.

Subtrees do not track specific commits separately. All changes are part of the main repository.
Cloning a repository with subtrees is straightforward; no additional steps are required. The subtree is part of the main repository.
The history of the subtree is combined with the main repository, making it easier to work with a unified history.


## Add a subtree

```bash
git subtree add --prefix=<directory> <repository-url> <branch>
```

## Update subtree

```bash
git subtree pull --prefix=<directory> <repository-url> <branch>
```

## Push changes to subtree

```bash
git subtree push --prefix=<directory> <repository-url> <branch>
```
