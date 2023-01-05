# docker service

Manages docker services. See [[docker swarm]] for details.

## Example

Create a new service
```powershell
docker service create ` # create a new service
--name mysql ` # name of the service
--mount 'type=volume,source=productdata,destination=/var/lib/mysql' ` # docker volume named 'productdata' mapped to dir
--constraint "node.hostname == dbhost" ` # run only on docker host named 'dbhost' 
--constraint "node.role == worker" # only on worker|manager nodes
--constraint "node.labels.type==db" # only on docker nodes tagged with 'db'
--replicas 1 ` # singleton instance for database
--network 'backend' ` # connect to SDN; network can span swarm
--publish 3306:3306 ` # open port to host system (shoud be closed on backend services when not needed)
--env 'MYSQL_ROOT_PASSWORD=Pa$$w0rd' `
--env 'bind-address=0.0.0.0' `
mysql:8.0.0
```

Open port of service to host system
```powershell
docker service update --publish-add 3306:3306 'mysql'
```

Remove port mapping
```powershell
docker service update --publish-rm 3306:3306 'mysql'
```

List docker services
```powershell
docker service ls
```

Active tasks for a given service
```powershell
docker service ps 'appname'
```

Remove/delete service
```powershell
docker service rm 'appname'
```

Scaling service
```powershell
docker service scale 'appname=3'
```

Shutdown service
```powershell
docker service scale 'appname=0'
```

Redistribute service to workers, when new workers have been added
```powershell
docker service update --force 'appname'
```