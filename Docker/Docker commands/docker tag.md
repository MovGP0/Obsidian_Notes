## Examples
Adding tags to a docker image
```powershell
docker tag 'myimage' 'myimage:latest'
docker tag 'myimage' 'mynamespace/myimage:latest'
```

Publish to a remote registry
```powershell
docker tag 'myimage:latest' 'myregistry.example.com/myimage:staging'
docker push 'myregistry.example.com/myimage:staging'
```

List tags of a docker image
```powershell
docker image inspect 'myimage'
```
