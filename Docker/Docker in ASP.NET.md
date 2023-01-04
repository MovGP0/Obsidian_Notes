Create a new Project and add the following attributes:

| Attribute          | Description                                                                |
| ------------------ | -------------------------------------------------------------------------- |
| RuntimeIdentifier  | Ensures the code will run on the given runtime                             |
| ContainerImageName | The name of the docker container. Will use the assembly name if not given. |
| ContainerBaseImage | Base image for the docker host.                                            |
| Version            | Version tag of the assembly/container.                                     |

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
  <PropertyGroup>
    <TargetFramework>net7.0</TargetFramework>
    <Nullable>enable</Nullable>
    <ImplicitUsings>enable</ImplicitUsings>
    <RuntimeIdentifier>linux-x64</RuntimeIdentifier>
    <ContainerImageName>docker-net-example</ContainerImageName>
    <ContainerBaseImage>mcr.microsoft.com/dotnet/aspnet:7.0-alpine</ContainerBaseImage>
    <Version>1.0.0</Version>
  </PropertyGroup>
  
  <ItemGroup>
    <PackageReference Include="Microsoft.NET.Build.Containers" Version="0.2.7" />
    <PackageReference Include="Swashbuckle.AspNetCore" Version="6.4.0" />
  </ItemGroup>
</Project>
```

Publish using using the `PublishContainer` task.
```powershell
dotnet publish --os linux --arch x64 /t:PublishContainer -c Release
```

## Deploy/Test locally
Check if the image is published in docker.
```powershell
docker image ls
```
Run the container
```powershell
docker run --name docker-net-example -p 8080:80 -d docker-net-example:1.0.0
```
List running images
```powershell
docker ps
```

## Publish to docker image registry
```powershell
docker tag
docker push
```

## Weblinks
- [Announcing built-in container support for the .NET SDK](https://devblogs.microsoft.com/dotnet/announcing-builtin-container-support-for-the-dotnet-sdk/)