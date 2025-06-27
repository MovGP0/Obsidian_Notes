**Note:** Git VFS has been replaced by [[Git Scalar - Microsoft Git]].

Git VFS only downloads files when accessed. This is helpful for large repositories, when not all projects are required.

## Installation

```bash
winget install -e --id Microsoft.VFSforGit
```

## Cloning a Git Repository

```bash
gvfs clone https://server/some/repository.git D:\Repository
```

## Mount

```bash
gvfs mount D:\Repository
```

## Prefetching

But there is the possibility to download specific files and projects by prefetching to improve performance:

```bash
gvfs prefetch --files '*.cs'
```

## Monitoring

```bash
gvfs status
```

## Diagnosis

```bash
gvfs diagnose
```

## Status

```bash
gvfs status
```

## Unmount

```bash
gvfs unmount D:\Repository
```

## References

* [GitHub: VFSForGit](https://github.com/microsoft/VFSForGit)