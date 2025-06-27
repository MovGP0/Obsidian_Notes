# Create tag

Tag a commit with tag and message
```bash
git tag -a "version.1.2.3" -m "MESSAGE" <COMMITID>
```

Push a given tag to server
```bash
git push origin "version.1.2.3"
```

Push all tags to server
```bash
git push origin --tags
```

## Query Tags

Get the last tag starting with "version"
```bash
git tag -l "version.*" -n 1
```

Get the ID of commit message with the tag
```bash
git rev-list --no-walk 'version.1.2.3'
```

Count number of commits since given commit ID
```bash
git rev-list --count <COMMITID>..HEAD
```
