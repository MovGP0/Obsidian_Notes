### Docker Specific Properties

```xml
<DockerDefaultTargetOS>Windows</DockerDefaultTargetOS>
<TargetLatestRuntimePatch>true</TargetLatestRuntimePatch>
<DockerfileContext>.</DockerfileContext>
<DockerfileFile>MyDockerfile</DockerfileFile>
<DockerfileBuildArguments>-t contoso/front-end:v2.0</DockerfileBuildArguments>
<DockerfileRunArguments>-v $(MSBuildProjectDirectory)/host-folder:/container-folder:ro</DockerfileRunArguments>
<DockerComposeProjectPath Condition="'$(Configuration)'=='Release'">..\docker-compose.dcproj</DockerComposeProjectPath>
```

### References

- [Container Tools build properties](https://learn.microsoft.com/en-us/visualstudio/containers/container-msbuild-properties)
