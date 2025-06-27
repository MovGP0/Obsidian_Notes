# docker logs

Gets the console output from a container.

## Example

Current console output from a given image
```powershell
docker logs 'imagename'
```

Live console output from a given image (until `Ctrl+C` is pressed)
```powershell
docker logs 'imagename' --follow
```

## See also
- [docker logs](https://docs.docker.com/engine/reference/commandline/logs/)