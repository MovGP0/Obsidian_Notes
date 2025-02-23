## Git Config Command Manual

### Overview

The `git config` command lets you read and write Git configuration values. These values control your identity, editor, aliases, diff/merge tools, and many other Git behaviors.

Git configuration is stored in plain-text files at three levels:
- **Local:** Specific to a repository (`.git/config`).
- **Global:** User-specific, affecting all repositories (typically `~/.gitconfig`).
- **System:** System-wide settings (e.g. `/etc/gitconfig`).

*Note: Local settings override global ones, and global settings override system settings.*

### Basic Usage

#### Reading a Value

To read a configuration value, simply provide the key:
```bash
git config user.email
```
This returns the value of `user.email` (if set).

#### Setting a Value

To set a configuration value, use:
```bash
git config [--local | --global | --system] <key> "<value>"
```

### Configuration Scopes

- **Local (`--local`):**  
  Applied only to the current repository.  
  *Stored in:* `.git/config`

- **Global (`--global`):**  
  Applies to all repositories for the current user.  
  *Stored in:* `~/.gitconfig`

- **System (`--system`):**  
  Applies to every user on the machine.  
  *Stored in:* `/etc/gitconfig` (or similar on your OS)

*Tip: If no scope is specified, Git defaults to local configuration.*

### Listing and Editing Configurations

List all configuration settings:
  ```bash
  git config --list
  ```

Edit global configuration file:
```bash
git config --global --edit
```

### Advanced Options

Appending values: For options that accept multiple values, you can append:
  ```bash
  git config --global --add alias.lg "log --graph --oneline --all"
  ```

Removing a configuration:
  ```bash
  git config --global --unset alias.lg
  ```

### Best Practices

- **Set your identity globally** to ensure your commits are properly attributed.
- **Use local configuration** for project-specific settings.
- **Review your settings** occasionally with `git config --list` to ensure they match your preferences.
- **Edit configuration files directly** if necessary, but using the `git config` command helps prevent syntax errors.

## See also

**Configure basic settings**
- [[Git config Username and Email]]
- [[Git config Command Alias]]
- [[Git config Visual Studio Code]]

**Configure security settings**
- [[Git config Credential Manager]]
- [[Git config Ignore Self-Signed Certificate]]

**Configure advanced settings**
- [[Git config Example]]

**Official Documentation**
- [git-config](https://git-scm.com/docs/git-config)