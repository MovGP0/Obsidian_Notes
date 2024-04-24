Perform a binary search between two commits to find the culprit of an issue.
```powershell
git bisect start
git bisect bad [bad_commit_hash]
git bisect good [good_commit_hash]
git bisect skip [not_building_hash]
git bisect reset
```

## Example

Bug is between commit `0d54bdb4c4` and commit `d8672753bf`
```powershell
# start bisect mode
git bisect start

# define known bad state
git bisect bad `d8672753bf`

# define known good state
git bisect good `0d54bdb4c4`

# git will select commit in the middle
git bisect good # if current commit is good
git bisect bad # if current commit has bug

# rinse and repreat
```
