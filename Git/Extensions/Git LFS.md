## Installation

Windows:
```bash
winget install -e --id GitHub.GitLFS
```

MacOS:
```bash
brew install git-lfs
```

Ubuntu:
```bash
sudo apt-get install git-lfs
```

## Initialize Git LFS

In a given Repository, use
```bash
git lfs install
```

## Track binary files

```bash
git lfs track "*.jpg"
```

In `.gitattributes` file:
```
*.psd filter=lfs diff=lfs merge=lfs -text
*.mp4 filter=lfs diff=lfs merge=lfs -text
*.jpg filter=lfs diff=lfs merge=lfs -text
*.zip filter=lfs diff=lfs merge=lfs -text
*.pdf filter=lfs diff=lfs merge=lfs -text
```

## Status
```bash
git lfs status
```

## Fetch file

Replaces the pointer file with the binary file:
```bash
git lfs fetch --include "path/to/image.jpg"
```

