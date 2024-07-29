Used to include a separate Git repository within the current repository.

The main repository tracks specific commits (references) of the submodule, not the entire history.
The submodule is essentially a pointer to a particular commit in the submodule repository.

Use [[Git subtree]] when the subdirectory should be part of the current repository.

## Add Submodule

```bash
git submodule add <repository-url> <path>
```

## Initialize Submodule

```bash
git submodule update --init --recursive
```

## Update Submodule

```bash
git submodule update --remote
```
