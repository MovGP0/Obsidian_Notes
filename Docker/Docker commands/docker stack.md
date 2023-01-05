Deploys systems of images from a docker-compose file as a single app. The application name will be used as a prefix for all images.

## Examples

Create a new app from a compose file
```powershell
docker stack deploy --compose-file my-compose-file.yaml 'myapp'
```

Remove tha app and all associated services
```powershell
docker stack rm 'myapp'
```
