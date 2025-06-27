# docker compose
Docker compose defines a YAML file that describes how to compose a system of multiple docker images, containers, and networks.

## Quick reference

| Command              | Description                                                  |
| -------------------- | ------------------------------------------------------------ |
| docker-compose build | Builds the YAML file                                         |
| docker-compose up    | Runs the containers, networks, and volumes in docker         |
| docker-compose stop  | Stops the containers in docker                               |
| docker-compose down  | Stops the containers, networks, and volumes in docker        |
| docker-compose scale | Changes the number of containers running for a given service |
| docker-compose ps    | Lists the running containers for a service                   |

## Examples

Compose application and run all required services
```powershell
docker compose --file 'D:\myproject\docker-compose.yaml' --project-name 'my-project' build
docker compose up
```

Run only specific services. Also starts dependent services
```powershell
docker compose up 'service'
docker compose up 'service1' 'service2'
```

Scale service to 4 instances
```powershell
docker compose scale 'service=4'
```

List services
```powershell
docker compose ps
```

Stop all services
```powershell
docker compose stop --timeout 20
```

Remove all services and volumes
```powershell
docker compose down --timeout 20 --volumes
```

## docker-compose YAML-File

See also: 
- [compose file specification](https://docs.docker.com/compose/compose-file/)
- [compose file schema](https://github.com/docker/compose/blob/master/compose/config/compose_spec.json)
- [YAML](https://yaml.org/spec/1.2.2/)

| Attribute                                        | Description                                                   |
| ------------------------------------------------ | ------------------------------------------------------------- |
| version                                          | Version of the docker-compose file format                     |
| volumes                                          | Volumes for permanent data storage                            |
| networks                                         | Networks used in the application                              |
| services                                         | List of services to create                                    |
| services/{service}/image                         | Use existing dokcer image for the service                     |
| services/{service}/build                         | Build a new image using [[docker build]]                      |
| services/{service}/build/context                 | Directory with the dockerfile                                 |
| services/{service}/build/dockerfile              | Name of the dockerfile                                        |
| services/{service}/volumes                       | Where to mount a volume for data storage                      |
| services/{service}/networks                      | Which networks should the service be connected with           |
| services/{service}/environment                   | Set environment variables                                     |
| services/{service}/depends_on                    | Which services should be started before this service starts   |
| services/{service}/links                         | Auto-wire load-balancer to service instances                  |
| services/{service}/deploy/mode                   | Set to `global` when service needs to be on every worker node |
| services/{service}/deploy/replicas               | Number of instances of the service                            |
| services/{service}/deploy/placement              | Where the containers will be hosted                           |
| services/{service}/deploy/placement/contstraints | constraints for where the containers will be hosted           |

### YAML-File Example

```yaml
version: "3"
volumes:
	productdata:
networks:
	frontend:
	backend:
services:
	mysql:
		image: "mysql:8.0.0"
		volumes:
			-productdata:/var/lib/mysql
		networks:
			-backend
		environment:
			-MYSQL_ROOT_PASSWORD=mysecret
			-bind-address=0.0.0.0
		deploy:
		    replicas: 1
		    placement:
			    constraints:
				    -node.hostname==dbhost
	dbinit:
		build:
			context: .
			dockerfile: Dockerfile
		networks:
			-backend
		environment:
			-INITDB=true
			-DBHOST=mysql
		depends_on:
			-mysql
	mvc:
		build:
		context: .
		dockerfile: Dockerfile
		networks:
			-backend
			-frontend
		environment:
			-DBHOST=mysql
		depends_on:
			-mysql
		deploy:
		    mode: replicated
			replicas: 5
			placement:
				constraints:
					-node.labels.type==mvc
	loadbalancer:
	    image: dockercloud/haproxy:latest
	    ports:
	        -80:80
	    links:
	        -mvc
	    volumes:
	        -/var/run/docker.sock:/var/run/docker.sock
	    networks:
	        -frontend
```

## See also
- [Docker Compose](https://docs.docker.com/compose/)