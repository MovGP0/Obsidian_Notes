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

## See also

- [docker node](https://docs.docker.com/engine/reference/commandline/node/)