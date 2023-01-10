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
| ARG        | Parameters for the script.                                      |

## Example

```
dotnet restore
dotnet publish --configuration Release --os linux --arch x64 --output D:\ExampleApp\publish\
```

```dockerfile
FROM ubuntu/dotnet-aspnet:edge
COPY D:/ExampleApp/publish/ /app
WORKDIR /app
EXPOSE 80/tcp 443/tcp 443/udp
ENTRYPOINT ["dotnet", "ExampleApp.dll"]
```

Use [[docker build]] to build image using the docker file.

## Parameters

Use the `ARG VARIABLENAME=DEFAULTVALUE` keyword for declaring variables. Use `$VARIABLENAME` to use the value.
```dockerfile
FROM alpine
ARG MY_VAR='hello world'
RUN echo $MY_VAR
```
Use `docker build --build-arg VARIABLENAME=VALUE dockerfile` to override the default. 