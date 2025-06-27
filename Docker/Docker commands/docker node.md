# docker node

## Examples

List all the nodes in a [[docker swarm]] on a manager node
```powershell
docker node ls
```

Label worker nodes
```powershell
docker node update --label-add 'type=frontend' 'worker1'
docker node update --label-add 'type=frontend' 'worker2'
docker node update --label-add 'type=frontend' 'worker3'
docker node update --label-add 'type=database' 'worker4'
docker node update --label-add 'type=database' 'worker5'
```

Shutdown worker node / stop all containers
```powershell
docker node update --availability 'drain' 'worker2'
```

Keep containers running, but do not add new containers
```powershell
docker node update --availability 'pause' 'worker2'
```

Assign and run containers
```powershell
docker node update --availability 'active' 'worker2'
```

## See also

- [docker node](https://docs.docker.com/engine/reference/commandline/node/)