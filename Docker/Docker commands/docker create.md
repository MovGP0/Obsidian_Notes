# docker create

Creates a new docker container from an image.

| Attribute     | Description                                          |
| ------------- | ---------------------------------------------------- |
| --env, -e     | Sets environment variables                           |
| --name        | Assignes a name to the container                     |
| --network     | Assignes the container to a software-defined-network |
| --publish, -p | Maps a host port to a container port                 |
| --rm          | Remove a container automatically when stoped         |
| --volume, -v  | Mount a docker volume as directory in the container  |

## Examples
```powershell
docker create `
--name 'my-container' `
--volume 'volumename:/path/in/container' `
--env 'VAR_NAME1=value1' `
--env 'VAR_NAME2=value2' `
--publish 8080:80 ` # map host port to port in image
--entrypoint '/bin/myapp' `
--network backend `
'namespace/imagename:version'
```

## Important hints

- Port publishing should only be done for frontend services. The frontend communicates with the backend using the software-defined-network (SDN).
- Each container requires a unique port on the host system.
- Use [[docker start]] to start the container. Use [[docker stop]] to stop the container.
- You can also use [[docker run]], which combines [[docker create]] with [[docker start]].

## See also
- [docker create](https://docs.docker.com/engine/reference/commandline/create/)