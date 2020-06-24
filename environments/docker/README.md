# Docker Notes

The below instructions will show you the workflow to make a Plex Docker container and host it.

Source: https://www.youtube.com/watch?v=fqMOX6JJhGo

## Commands

Using the Docker command gives you the following options in order.

docker [command] [OPTIONS] [IMAGE] [COMMAND] [ARG...]

### General

check docker version

`docker version`

### Creating containers

Create a new Dockerfile, but don't run it.

`docker container create my-image`

or pull an image from a registry and cache it your local machine

`docker pull linuxserver/plex`

create a new container and either automatically pull a hosted image from Docker.com or use an image that was previously cached on your local machine.

`docker run ubuntu /bin/bash`

Note that a docker container must have a script to run, or it will stop automatically.

Create a new docker container with a specific version of an image. IF you don't select, Docker with select `:latest`

`docker run ubuntu:5.0`

### Configure setup

Note that adding `-i` in docker run will attach the container to your local command line in interactive mode, meaning that you can actually control the container from terminal.

You can also provide `-t` to ensure all prompts from the application file you are running are linked to the terminal. You can short hand this like this: `-it`.

Example: `docker run -i -t linuxserver/plex`

Note that adding `-d` in docker run will detach the container from your local terminal, all together. If you forget this, the container will run with a non-interactive terminal. Use Ctrl+C to stop the container and exit the terminal.

to reattach to the container that was run in detached mode use...

`docker attach my-container`

### Managing Containers

list all running containers 

`docker ps`

also list previously activated, but currently stopped containers. 

`docker ps -a`

inspect a single container for additional details.

`docker inspect my-container`

### End Containers

stop a container

`docker stop my-container`

delete a container

`docker rm`

### Images

list all cached images to be used by newly created docker containers

`docker images`

remove an cached image from your local machine

make sure to stop all containers using that image before deleting it.

`docker rmi my-name`

### Ports

control which internal port the container is broadcasting on, note that this port would have to be mapped to an external port on your host machine.

Use `ip 80:5000` to map the external port `80` to internal Docker container port `5000`

### Persist Data

To get containers to have their data persist after being stopped, they need to have their filesystems mapped to the host file system.

Use the `-v` command followed by the two file systems, starting with the host eg. `/home/persist-dir:/home/container-dir`

<!-- Not actually sure that the -v is required. -->

### Logs

get logs from an active detached container.

`docker logs my-container`

### Controlling Containers

execute a command on a docker container

`docker exec my-container linux-command`

### Environment variables

`docker run -e my-variable=true`

<!-- Need to add information about where to place these variables in application code -->

## Building a Docker Image

Create a file called `Dockerfile` at the root of your project.

Use `docker build Dockerfile my-container` to build a enw container from that image. 

If, during the set up of a docker image, a command fails, you can fix the issue and use `docker build` again to contineu where you left off.