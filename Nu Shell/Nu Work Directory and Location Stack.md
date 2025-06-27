## Current Directory

Get the present work directory (PWD):
```shell
$env.PWD
echo $env.PWD
```
There is an alias command:
```shell
pwd
```

Get the parent directory
```shell
$env.PWD | path dirname
```

Get the current directory name
```shell
$env.PWD | path basename
```

### Script specific directories

Current script location:
```shell
$env.CURRENT_FILE
```

Script directory:
```shell
$env.FILE_PWD
```

## Location Stack

View the current location stack:
```nushell
dirs
```

Push a directory onto the stack:
```nushell
pushd <directory>
```

Pop a directory from the stack:
```nushell
popd
```

Rotate the stack:
```nushell
dirs --rotate <number>
```
This rotates the stack by the specified number of positions. Positive numbers rotate up, negative numbers rotate down.

Clear the stack:
```nushell
dirs --clear
```
This clears the entire directory stack, except for the current directory.

Add a directory to the stack without changing the current directory:
```nushell
pushd --no-change <directory>
```
This adds the specified directory to the stack without changing the current working directory.

View the stack as a list:

```nushell
dirs --list
```
This displays the directory stack as a list, with each directory on a separate line.

Change to a specific directory in the stack:
```nushell
cd (dirs | nth <index>)
```
This changes to the directory at the specified index in the stack.
