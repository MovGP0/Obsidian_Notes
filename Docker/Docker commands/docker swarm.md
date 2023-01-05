# docker swarm

Used for running applications/services on a cluster of multiple servers.

## Definitions

| Definition  | Description                                                                                       |
| ----------- | ------------------------------------------------------------------------------------------------- |
| Service     | Group of identical images                                                                         |
| Node        | Worker machine in Docker Swarm. physical or virtual machine. manager node or worker node          |
| Manager     | Node responsible for orchestrating nodes in a swarm                                               |
| Worker      | Node that runs a application-specific service (ie. database, frontend, load-balancer)             |
| Stack       | Group of interconnected services that share dependencies, as defined by a [[docker compose]] file |
| Application | Group of services for a specific task                                                             |

## Examples

Create a swarm manager
```powershell
docker swarm init
```
The output gives instructions to add additional machines to the swarm.

List worker token
```powershell
docker swarm join-token worker
```

List manager token
```powershell
docker swarm join-token manager
```

Join additional machine an existing swarm
```powershell
docker swarm join --token 'TOKEN' 172.16.0.5:2377
```

Change join-tokens / invalidate old join-token
```powershell
docker swarm join-token --rotate
```

Leave a swarm
```powershell
docker swarm leave --force
```

## See also
- [[docker node]]