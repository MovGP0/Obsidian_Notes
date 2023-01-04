# docker exec

Executes a command in a container. 

## Examples

Execute a single command
```powershell
docker exec 'my-container' 'cat /bin/myapp/appsettings.json'
```

Run interactive shell
```powershell
docker exec  --interactive --tty 'my-container' 'bin/bash'
```
```bash
cat /bin/myapp/appsettings.json
exit
```

Create image from changed container
```powershell
docker exec 'my-container' 'apt-get update; apt-get upgrade -y'
docker commit 'my-container' 'mynamespace/mycontainer:latest' 
```

## See also
- [docker exec](https://docs.docker.com/engine/reference/commandline/exec/)