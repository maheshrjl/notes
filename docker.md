# 🚀 Docker

`docker version` - Docker client/engine version

`docker info` - Show docker engine configuration (info about containers & images)

### Docker CLI structure

* Old: `docker <command> (options)`
* New: `docker <command> <sub-command> (options)`

### Isolation mode for windows containers

* `process isolation`: multiple container instances run concurrently on a given host sharing the same kernel with the host as well as each other. Same as how Linux containers run.
* `Hyper-V isolation`: Offers enhanced security and broader compatibility between host and container versions. With Hyper-V isolation, multiple container instances run concurrently on a host; however, each container runs inside of a highly optimized virtual machine and effectively gets its own kernel. The presence of the virtual machine provides hardware-level isolation between each container as well as the container host.

### Container Management

#### Starting & Restarting container

`docker container run --publish 80:80 --detach --name weserver nginx` - Start a nginx container

`docker restart <container name>` - Restart a container

`docker container start -ai <name> <command>` - Start existing container

#### Getting a shell inside a container

* `docker container run -it` - Start a new container interactively (container will stop when the shell/command is terminated)
*   `docker exec -it <container name> /bin/bash` - execute a bash shell in the container

    `docker exec -it <container name> <command>` - execute any command inside the container

{% hint style="info" %}
Container run as long as the command that was executed on startup runs. Eg: If bash shell was executed on startup, the container will stop when the shell is terminated.
{% endhint %}

#### List containers

&#x20;`docker container ls`- List running containers (new command)

`docker ps` - List running containers (old command)

`docker container ls -a`- List all containers

#### Viewing Logs

* View Logs for a container: `docker container logs name`
* Follow container logs: `docker container logs name -f`
* View logs help: `docker container logs --help`

#### View configuration info & utilization:

* View processes running inside a container: `docker container top`
* details of container config: `docker container inspect name`
* performance stats of containers(stream): `docker container stats`
* `docker system df` - Display information about disk space being used by your containers.

#### Docker Cleanup

* To clean an unused/dangling image: `Docker image prune`
* To remove an image that is not used in a container: `Docker image prune -a`
* To prune the entire system: `Docker system prune`
* To kill all running containers: `Docker kill $ (docker ps -q)`
* To delete all stopped containers: `docker rm $(docker ps -a -q)`
* To delete all images: `docker rmi $(docker images -q)`

{% hint style="info" %}
A running container cannot be removed unless stopped or using `docker` container rm -f name
{% endhint %}

### Docker Images

Image contains applications binaries, dependencies for the app along with metadata about how to run it

#### VIew Image details/layers

* List downloaded images: `docker image ls`
* View history of image layers: `docker image history nginx:latest`
* Inspecting a docker image: `docker image inspect nginx`

#### Tagging images

* Give a new tag to a image: `docker image tag <source-img:tag> <taget-image:tag>`

#### Uploading images

* Upload image to docker hub: `docker image push <img-name>`

#### Cleaning up images

* Clean up dangling images: `docker image prune`
* Clean all images that are not in use: `docker image prune -a`
* Clean up everything: `docker system prune`

### Docker Networking

#### Basic docker Networking commands

* Show networks: `docker network ls`
* Inspect a network: `docker network inspect`
* Create a network: `docker network create --driver`
* Attach a network to a container: `docker network connect`
* Detach a network from container: `docker network disconnect`

## Storage in Docker

### Data Volumes:

Creating a named volume at runtime

* `-v` : Specify new/existing volumes on docker run

```shell
docker container run -d --name mysqlnamedvol -e MYSQL_ALLOW_EMPTY_PASSWORD=TRUE -v mysql-db:/var/lib/mysql mysql
```

{% hint style="info" %}
Custom drivers & driver options can be specified only if docker volume is created with the volume command.&#x20;

Eg: docker volume create&#x20;
{% endhint %}

### Bind Mounts:&#x20;

Maps the host files/directories to a container files/directory i.e. Two locations pointing to the same files.

> Bind mounts cannot be used in **Dockerfile**, must be used at container run

{% hint style="info" %}
Bind mount starts as / whereas volume mount starts without
{% endhint %}

Bind Mount Eg:

* Mac/Linux: `... run -v /Users/mhs/stuff:/path/container`
* Windows: `... run -v //c/Users/mhs/stuff:/path/container`
