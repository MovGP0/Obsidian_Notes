---
title: Branch Management
---
## Create Branch
```sql
-- switch to a specific repository (optional)
USE 'repo_name';

CALL DOLT_BRANCH('feature/xyz');
-- more SQL commands here
```

## Check-Out Branch
```sql
-- switch to a specific repository (optional)
USE 'repo_name';

-- checkout the branch
CALL DOLT_CHECKOUT('feature/xyz');

-- more SQL commands here
```

## Merging
```sql
-- checkout the target branch
CALL DOLT_BRANCH('main');

-- Merge another branch into current branch:
CALL DOLT_MERGE('feature/xyz');
```

## Rebasing

### using SQL

Start the rebase
```sql
-- interactive rebase onto main
CALL DOLT_REBASE('-i', 'main');
```

This action creates a `dolt_rebase` table which contains the `pick`, `reword`, `drop`, `fixup` operations.

Continue or abort the rebase
```sql
-- continue with rebase
CALL DOLT_REBASE('--continue');

-- cancel the rebase
CALL DOLT_REBASE('--abort');
```

### using the CLI

```sh
# start the interactive rebase onto main
dolt rebase -i main

# continue with rebase
dolt rebase --continue

# cancel the rebase
dolt rebase --abort
```

## Commit history

### using SQL

```sql
SELECT commit_hash, message, commit_date
FROM dolt_log
ORDER BY commit_date DESC;
```

### using the CLI
```
dolt log
```

## Cherry-Picking

### using SQL
```sql
CALL DOLT_CHERRY_PICK('<commit_hash_or_ref>');
```

### using the CLI
```sh
dolt cherry-pick 'commit_hash_or_ref'
```

## Delete a Branch

### using SQL
```sql
CALL DOLT_BRANCH('-d', 'branchName');
```
### using the CLI
```sh
dolt branch -d 'branchName'
```
