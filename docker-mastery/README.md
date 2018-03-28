# docker-mastery
**Udemy Course:** *Docker Mastery: The Complete Toolset From a Docker Captain*

**Course Author:** *Bret Fisher*

**Start Date:** *March 25, 2018*

# Cheatsheet

**Typical Docker Commands:**
```bash
# List all running containers
$ docker container ls

# List all containers (stopped, running, etc...)
$ docker container ls -a

# Run a container
$ docker container run <image>

# Run a container in background
#   can use -d in place of --detach
$ docker container run --detach <image>

# Run a container w/ name, in background, on a specified port
#   can use -d in place of --detach 
#   can use -p in place of --publish (8080:80 -> host:container)
$ docker container run --name webserver --detach --publish 8080:80 <image>

# Start a stopped container
$ docker container start <container>

# Stop a container
$ docker container stop <container>...

# Remove a container
$ docker container rm <container>...

# Forcefully remove a container (needed if it is running)
$ docker container rm -f <container>...

# Show logs of container running in background
$ docker container logs <container>

# Show the process list in one container
$ docker container top <container>

# Show performance stats for all containers
$ docker container stats

# Get more information about a container
$ docker container inspect <container>

# Run a container in shell environment (sets default command to bash)
$ docker container run -it <image> bash

# Run a container, open it's bash, and remove it once bash closes
$ docker container run --rm -it <image> bash

# Get a shell environment of a running docker container
$ docker container exec -it <container> bash

# Quick port check 
$ docker container port <container>

# Show docker networks
$ docker network ls

# Inspect a network
$ docker network inspect <network>

# Create a docker virtual network 
$ docker network create <name>

# Attach a network to container
$ docker network connect <network> <container> 

# Detach a network from container
$ docker network disconnect <network> <container>

```