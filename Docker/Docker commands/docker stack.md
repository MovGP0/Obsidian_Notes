Deploys systems of images from a docker-compose file as a single app: 
```powershell
docker stack deploy --compose-file my-compose-file.yaml myapp
```
The application name will be used as a prefix for all images.
