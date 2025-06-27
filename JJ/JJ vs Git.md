## Installation

```sh
winget install --id jj-vcs.jj -e
```

## Basic Setup & Remote Management

| Task                 | `git` Command                 | `jj` Command                                          |
| -------------------- | ----------------------------- | ----------------------------------------------------- |
| Initialize a repo    | `git init`                    | `jj git init [--colocate]`                            |
| Clone a repo         | `git clone <src> <dst>`       | `jj git clone <src> <dst> [--remote <name>]`          |
| Fetch updates        | `git fetch [remote]`          | `jj git fetch [--remote <name>]`                      |
| Push everything      | `git push --all [remote]`     | `jj git push --all [--remote <name>]`                 |
| Push a single branch | `git push <remote> <branch>`  | `jj git push --bookmark <bookmark> [--remote <name>]` |
| Add remote           | `git remote add <name> <url>` | `jj git remote add <name> <url>`                      |

## Working Copy & Commits

| Task                     | `git` Command               | `jj` Command              |
| ------------------------ | --------------------------- | ------------------------- |
| View status              | `git status`                | `jj st`                   |
| See diff                 | `git diff HEAD`             | `jj diff`                 |
| Diff arbitrary revisions | `git diff A B`              | `jj diff --from A --to B` |
| Finish change / commit   | `git commit -a`             | `jj commit`               |
| Log history              | `git log --oneline --graph` | `jj log -r ::@`           |
| List files               | `git ls-files`              | `jj file list`            |

## Branching, Stashing, Rewriting

| Task               | `git` Command                      | `jj` Command                                                  |
| ------------------ | ---------------------------------- | ------------------------------------------------------------- |
| Reset hard/discard | `git reset --hard`                 | `jj abandon` (abandon change) or `jj restore <paths>`         |
| Amend last commit  | `git commit --amend`               | `jj squash` (to merge change into parent)                     |
| Interactive amend  | `git add -p`, `git commit --amend` | `jj squash -i`                                                |
| Stash              | `git stash`                        | `jj new @-` (creates a new change; edit later with `jj edit`) |
| Checkout/switch    | `git switch -c topic main`         | `jj new main`                                                 |

## Merging & Rebasing

| Task                               | `git` Command                | `jj` Command                                         |
| ---------------------------------- | ---------------------------- | ---------------------------------------------------- |
| Merge branch                       | `git merge A`                | `jj new @ A` (creates a new change with two parents) |
| Rebase branch onto another         | `git rebase B A`             | `jj rebase -b A -d B`                                |
| Rebase change & descendants onto B | `git rebase --onto B A^ ...` | `jj rebase -s A -d B`                                |
| Reorder interactive rebase         | `git rebase -i A`            | `jj rebase -r C --before B`                          |

## Navigation & Logs

| Task                      | `git` Command             | `jj` Command                              |
| ------------------------- | ------------------------- | ----------------------------------------- |
| Show details              | `git show <rev>`          | `jj show <revision>`                      |
| Create bookmarks/branches | `git branch <name> <rev>` | `jj bookmark create <name> -r <revision>` |
| List branches             | `git branch`              | `jj bookmark list` or `jj b l`            |

## Advanced Workflow

- Modify commit message: use `jj describe` instead of amend or rebase (`jj describe -r <rev> -m "msg"`)
- No staging area—working copy is a commit. To split hunks, use `jj split` or amend via `jj squash`
 
### Workflow Tips

- use `jj edit <change>` for editing a change and `jj new` for branching.
- `jj describe` and `jj new` replace what Git handles via commit, amend, interactive-rebase, stash, and index.
