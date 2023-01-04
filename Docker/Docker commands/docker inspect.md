# docker inspect

Views the configuration of an container

## Examples

Get image-ID
```powershell
docker container ls --all --format "table {{.ID}}\t{{.Image}}"
```

Inspect image using image-ID
```powershell
docker inspect 'imageid'
```

## See also
- [docker inspect](https://docs.docker.com/engine/reference/commandline/inspect/)