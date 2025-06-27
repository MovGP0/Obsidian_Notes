## Edit the user config

Basic setup:
```sh
jj config set --user user.name 'Firstname Lastname'
jj config set --user user.email 'you@example.com'
jj config set --user ui.editor '["code.cmd", "--wait"]'
```

Configure with editor
```sh
jj config edit --user
```

## Basic Configuration
```toml
# the schema for validation of the TOML file
"$schema" = "https://jj-vcs.github.io/jj/latest/config-schema.json"

# configure file monitoring
core.fsmonitor = "watchman"
core.watchman.register-snapshot-trigger = true

# user and email
[user]
name = "Firstname Lastname"
email = "you@example.com"

[ui]
color = "auto" # "always" | "never" | "auto" | "debug"
pager = ":builtin" # app for paging in the CLI (ie. "less -FRX")
paginate = false # false | true
default-command = ["log", "-r", "::", "--limit", "10"] # default jj command to execute when none is given

[aliases]
init = ["git", "init"]
```

## Configure Visual Studio Code

```toml
[ui]
diff-editor = ["code.cmd", "--wait", "$left", "--no-snapshot", "$right"]
editor = ["code.cmd", "--wait"] # for commit messages and `jj edit`
merge-editor = "vscode" # for 3-way merges with `jj resolve`
```

> [!warning]
> Using `code` instead of `code.cmd` might not work

## Scoped Settings

Settings can be scoped to specific scopes:
```toml
# default email address
[user]
email = "foo@example.com"

# use different email for github projects
[[--scope]]
--when.repositories = ["D:/GitHub/**"]
[--scope.user]
email = "bar@gmail.com"
```

> [!warning] 
> Use forward-slashes `/` or double backward-slashes `\\` as path separator.
> Single backward-slashes result in error.

## Further settings to consider

```toml
[ui]
bookmark-list-sort-keys = ["name", …] # sort by name instead of timestamp
show-cryptographic-signatures = true # true | false

[colors]
commit_id = "green" # or { fg="green", bold=true }

[templates]
draft_commit_description = "…"
duplicate_description = "…"
config_list = "builtin_config_list"
log = "builtin_log_compact"
op_log = "builtin_op_log_compact"
show = "builtin_log_detailed"

[template-aliases]
"format_timestamp(ts)" = "…"

[revsets]
log = "…"
revset-aliases = { alias = "revset-expr", … }

[signing]
behavior = "own"
backend = "ssh" # "gnupg" | "ssh"
key = "<ssh-or-gpg-key>"

[git]
fetch = ["origin", "upstream"] # which upstreams to fetch by default
push = "origin" # upstream to push commits to
auto-local-bookmark = true # true | false
auto-local-bookmark-on-clone = true # true | false
abandon-unreachable-commits = true # true | false
push-bookmark-prefix = "push-"
private-commits = "revset-expression"
executable-path = "/path/to/git"

[snapshot]
auto-track = "glob:**/*.cd" # files auto-included
max-new-file-size = "10MiB"
```
