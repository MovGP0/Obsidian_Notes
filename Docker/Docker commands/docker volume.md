# docker volume

Declare a directory in the docker config file
```dockerfile
VOLUME /var/lib/mysql
```

Create a docker volume
```powershell
docker volume create --name 'mysqldata'
```

Mount the volume using [[docker run]]/[[docker create]]
```powershell
docker run `
    --name 'mysql' `
    --volume 'mysqldata:/var/lib/mysql' `
    --env 'MYSQL_ROOT_PASSWORD=Pa$$w0rd' `
    --env 'bind-address=0.0.0.0' `
    'mysql:8.0.0'
```

| Command              | Description             |
| -------------------- | ----------------------- |
| docker volume create | Create a volume         |
| docker volume ls     | List docker volumes     |
| docker volume rm     | Removes a docker volume |

## See also
- [Docker Volume reference](https://docs.docker.com/engine/reference/volume/)