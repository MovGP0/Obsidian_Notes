| Command    | Description                                                                              |
| ---------- | ---------------------------------------------------------------------------------------- |
| FROM       | docker base image                                                                        |
| MAINTAINER | name and contact data of the maintainer                                                  |
| WORKDIR    | changes the current work directory in the container (like `cd`)                          |
| LABEL      | Adds metadata to an image                                                                |
| ADD        | Copies compressed files (.bz2, .tar, .zip, etc.) from the host system into the container |
| COPY       | Copies uncompressed files from the host system into the container                        |
| RUN        | Execute a command while building the docker image                                        |
| EXPOSE     | Opens a network port in the container                                                    |
| ENV        | Sets an environment variable in the container                                            |
| VOLUME     | Creates a docker volumne to be used in the container                                     |
| CMD        | Executes a command when starting the docker container                                    |
| ENTRYPOINT | Command to run when the container starts                                                 |
| ARG        | Parameters for the script.                                                               |

## Example

```
dotnet restore
dotnet publish --configuration Release --os linux --arch x64 --output D:\ExampleApp\publish\
```

```dockerfile
FROM ubuntu/dotnet-aspnet:edge
MAINTAINER Johann Dirry <johann.dirry@gmail.com> 
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

## Labels

OpenContainers specify labels: [image-spec/annotations](https://github.com/opencontainers/image-spec/blob/main/annotations.md)

| Label                                       | Description                                                           |
| ------------------------------------------- | --------------------------------------------------------------------- |
| **org.opencontainers.artifact.created**     | [RFC 3339](https://tools.ietf.org/html/rfc3339#section-5.6) Timestamp |
| **org.opencontainers.artifact.description** | Human-readable short description                                      |
| **org.opencontainers.image.created**        | [RFC 3339](https://tools.ietf.org/html/rfc3339#section-5.6) Timestamp |
| **org.opencontainers.image.authors**        | List of Authors                                                       |
| **org.opencontainers.image.url**            | About/Readme                                                          |
| **org.opencontainers.image.documentation**  | Detailed Documentation                                                |
| **org.opencontainers.image.source**         | URL to SourceCode (ie. DOCKERFILE)                                    |
| **org.opencontainers.image.version**        | SemVer Version                                                        |
| **org.opencontainers.image.revision**       | Source-Control identifier                                             |
| **org.opencontainers.image.vendor**         | Name of the vendor (Person or Company)                                |
| **org.opencontainers.image.licenses**       | License(s) of the distribution                                        |
| **org.opencontainers.image.ref.name**       | Name of the image; see [Image Name Pattern](#Image_Name_Pattern)      |
| **org.opencontainers.image.title**          | Human-readable title for the image                                    |
| **org.opencontainers.image.description**    | Human-readable description for the image                              |
| **org.opencontainers.image.base.digest**    | Digest of the base image                                              |
| **org.opencontainers.image.base.name**      | Name of the base image                                                |

### Image Name Pattern

The property `org.opencontainers.image.ref.name` must follow the Regex `^(?P<ref>(?P<component>[A-Za-z0-9]+)(?P<separator>[/\-._:@+]|--)(?P<alphanum>[A-Za-z0-9]+)*)+$`.

## Custom Labels

Can be used to provide additional data; ie. metric or healthcheck-endpoints. Examples:
| Label            | Description                                 |
| ---------------- | ------------------------------------------- |
| `ci-build`       | URL to the CI pipeline                      |
| `releasenotes`   | Release notes                               |
| `healthz`        | Healthcheck endpoint                        |
| `metrics`        | Metrics endpoint                            |
| `docker.run`     | Example command how to run the image        |
| `k8s.deployment` | Base64-encoded YAML for [[Kubernetes Architecture]] (k8) |

## See also
- [Dockerfile reference](https://docs.docker.com/engine/reference/builder/)