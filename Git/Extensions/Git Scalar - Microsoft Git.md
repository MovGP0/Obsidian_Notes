## Installation

```bash
winget install --id microsoft.git
```

## Clone Repository

```bash
scalar clone <repository-url>
```

## Register existing Repository

```bash
scalar register <repository-path>
```

## Execute Maintenance Tasks

```bash
scalar maintenance run
```

## Submodules

Clone Submodules
```bash
scalar clone --recurse-submodules <repository-url>
```

Update Submodules
```bash
git submodule update --init --recursive
```

## Subtrees

Add Subtree
```bash
git subtree add --prefix=<directory> <repository-url> <branch>
```

Update Subtree
```bash
git subtree pull --prefix=<directory> <repository-url> <branch>
```

## References

* [GitHub: Microsoft Git](https://github.com/microsoft/git)