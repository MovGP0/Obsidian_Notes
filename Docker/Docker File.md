| Command    | Description                                                     |
| ---------- | --------------------------------------------------------------- |
| FROM       | docker base image                                               |
| WORKDIR    | changes the current work directory in the container (like `cd`) |
| COPY       | Copies files from the host system into the container            |
| RUN        | Execute a command in the docker container                       |
| EXPOSE     | Opens a network port in the container                           |
| ENV        | Sets an environment variable in the container                   |
| VOLUME     | Creates a docker volumne to be used in the container            |
| ENTRYPOINT | Command to run when the container starts                        |

## Example
```
dotnet restore
dotnet publish --configuration Release --os linux --arch x64 --output D:\ExampleApp\publish\
```

```dockerfile
FROM ubuntu/dotnet-aspnet:edge
COPY D:/ExampleApp/publish/ /app
WORKDIR /app
EXPOSE 80/tcp
ENTRYPOINT ["dotnet", "ExampleApp.dll"]
```

Use [[docker build]] to build image using the docker file.