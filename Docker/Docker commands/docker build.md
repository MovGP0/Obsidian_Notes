Creates a docker image from a [[Docker File]].

## Examples

Build image from local directory
```powershell
docker build . --tag 'mycompany/exampleapp' --file 'D:\ExampleApp\dockerfile'
```

Build image from Git (using `#Branch:Directory`)
```powershell
docker build 'https://git.mycopmany.com/exampleapp/exampleapp.git#docker:exampleapp' --tag 'mycompany/exampleapp'
```

Build from Tarball
```powershell
docker build 'https://mycompany.com/exampleapp.tar.gz'
```

## See also
- [docker build](https://docs.docker.com/engine/reference/commandline/build/)