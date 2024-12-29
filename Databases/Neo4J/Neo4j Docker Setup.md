Download the image
```powershell
docker pull neo4j:latest
```

Start a New Docker Instance with Data Persistence
```powershell
docker run `
  --name 'neo4j' `
  --publish=7474:7474 --publish=7687:7687 ` # map HTTP and Bolt ports
  --env 'NEO4J_AUTH=neo4j/some_secure_password' ` # initial username ('neo4j') and password.
  # --env 'NEO4J_AUTH=none' # disable authentication
  --volume=d:\some_folder:/data ` # mount 'some_folder' as '/data' drive
  --restart=always `
  -d ` # runs the container in detached mode, allowing it to operate in the background
  neo4j:latest
```

Manage the container
```powershell
docker stop 'neo4j' # stop the container
docker start 'neo4j' # start the container
docker rm 'neo4j' # delete the container
```
