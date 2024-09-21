Object-oriented (structured) terminal implemented in [[Rust]].

## Quick Guide

### Commands

| Command | Description                                                                      |
| ------- | -------------------------------------------------------------------------------- |
| help    | get help on built-in commands                                                    |
| tutor   | start Nu tutorial                                                                |
| ls      | List files and directories                                                       |
| ps      | List processes                                                                   |
| sys     | System info                                                                      |
| open    | load a file content as object; supports common file types like `json` and `toml` |
| cat     | load the file content as string                                                  |

### [Filters](https://www.nushell.sh/commands/categories/filters.html)

| Pipe    | Description                   |
| ------- | ----------------------------- |
| [each](https://www.nushell.sh/commands/docs/each.html)    | Foreach loop                  |
| [where](https://www.nushell.sh/commands/docs/where.html)   | Filter                        |
| [reverse](https://www.nushell.sh/commands/docs/reverse.html) | Reverse object order          |
| [sort-by](https://www.nushell.sh/commands/docs/sort-by.html) | Sort objects by some property |

### [Viewers](https://www.nushell.sh/commands/categories/viewers.html)

| Format   | Description                             |
| -------- | --------------------------------------- |
| table    | Format output in collapsed table format |
| table -e | Format output in expanded table format  |
| columns  | Format output in column format          |
| grid     | Format output in a pipe-separated list  |

### [Formats](https://www.nushell.sh/commands/categories/formats.html)

| Parse     | Format  | Description        |
| --------- | ------- | ------------------ |
| from xml  | to xml  | Handle XML files   |
| from json | to json | Handle JSON files  |
| from xlsx | to xlsx | Handle Excel files |
| from csv  | to csv  | Handle CSV files   |

```shell
open --raw 'some.xlsx' | from xlsx --sheets ['Sheet1', 'Sheet2']
```

## Examples

List files and directories
```shell
ls
ls **/app.json # recursive search
```

Sort and filter objects
```shell
ls | sort-by modified | reverse | where type == 'dir'
ls | where name =~ '(bin|obj)' # include with regex search
ls | where name !~ node_modules # exculde with regex search
```

Convert output to specific file format
```shell
ls | where type == 'file' | to toml
ls | where type == 'file' | where size > 2KB | to json
```

Manage processes
```shell
ps | where mem > 100MiB
```

Foreach loop
```shell
ls **/tsconfig.json | where name !~ node_modules | each { |t| ( open $t.name | get compilerOptions -i ) }
```

Get the value of a property
```shell
sys | get host # get the host property
sys | get host.os_version # get the value of the host.os_version property
```

Load data from HTTP
```shell
http get 'https://some-url.com/foobar.json' | from json
http get 'https://some-url.com/foobar.html' | query web -q 'h2' # get the <h2> elements of the html page
```

## Resources

- [NuShell Manual](https://nushell.sh/)
- [NuShell GitHub](https://github.com/nushell/nushell)
