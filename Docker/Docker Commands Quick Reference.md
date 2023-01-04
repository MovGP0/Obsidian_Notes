## Docker Commands Quick Reference

### Container lifecycle commands

| Command            | Description                             |
| ------------------ | --------------------------------------- |
| [[docker create]]  | Creates a new container from an image   |
| [[docker start]]   | Starts a new container from an image    |
| [[docker run]]     | Combines `docker create` + `docker run` |
| [[docker stop]]    | Stops a container                       |
| [[docker pause]]   | Pauses a container                      |
| [[docker restart]] | `docker stop` + `docker start`          |
| [[docker rm]]      | Removes an container                    |

### Container management commands

| Command            | Description                                                          |
| ------------------ | -------------------------------------------------------------------- |
| [[docker ps]]      | Lists docker containers                                              |
| [[docker logs]]    | Prints the log output of an container                                |
| [[docker inspect]] | View the configuration of an container                               |
| [[docker diff]]    | Inspect changes in a container                                       |
| [[docker volume]]  | Manages docker volumes                                               |
| [[docker network]] | Defines software-defined-networks                                    |
| [[docker stack]]   | Deploys systems of images from a docker-compose file as a single app |

### Image management commands

| Command           | Description                                    |
| ----------------- | ---------------------------------------------- |
| [[docker images]] | Lists the docker images on the system          |
| [[docker pull]]   | Downloads an image from the registry           |
| [[docker rmi]]    | Removes docker image from the system           |
| [[docker login]]  | Authenticate/auhorize with a docker repository |
| [[docker push]]   | Publishes a docker image to the registry       |
| [[docker logout]] | De-Authenticate with the docker repository     |

### Build docker images commands

| Command            | Description                                 |
| ------------------ | ------------------------------------------- |
| [[docker build]]   | Processes a docker file to create an image  |
| [[docker exec]]    | Executes a command in an container          |
| [[docker tag]]     | Names an docker image.                      |
| [[docker compose]] | Composes a system of multiple docker images |

### [[docker swarm]] commands

| Command                 | Description                                               |
| ----------------------- | --------------------------------------------------------- |
| [[docker swarm]] init       | Create a new docker swarm                                 |
| [[docker swarm]] join       | Join a worker node to a swarm                             |
| [[docker node]] ls          | List all nodes in the swarm                               |
| [[docker node]] update      | Updates the configuration of a node in the swarm          |
| [[docker service]] create   | Manually starts a service on the swarm                    |
| [[docker service]] update   | Updates the configuratin of a service in the swarm        |
| [[docker service]] scale    | Change the number of cotainer running for a service       |
| [[docker service]] ls       | Lists the services running on a swarm                     |
| [[docker service]] ps       | Lists the containers running for a service                |
| [[docker stack]] deploy | Deploys an application described in a docker-compose file |
| [[docker stack]] rm     | Removes the services described in a docker-compose file   |

## Create a docker container

- Select a base image from the docker repository (ie. [Docker Hub](https://hub.docker.com/))
- Prepare the app binaries that should be in the docker image
- Write a [[Docker File]] that sets up the image
- Use [[docker build]] to create the new image
- Use [[docker create]] to create an container 
- Use [[docker run]] to start the image
- Test if everything is working
- Use [[docker stop]] to stop the image

## Update a docker container

- Use [[docker exec]] to make changes in the running container
- Use [[docker commit]] to create a new image from the changed container

## Publish a docker image

- Use [[docker tag]] to add tags to the image
- Use [[docker login]] to authenticate with the docker hub
- Use [[docker push]] to publish the docker image to the docker hub
- Use [[docker logout]] to de-authenticate with the docker hub
- Use [[docker rmi]] to remove obsolete images from the system

## See also
- [Docker CLI reference](https://docs.docker.com/engine/reference/run/)