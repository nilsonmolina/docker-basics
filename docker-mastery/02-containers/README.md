# Docker Containers
## Check versions of Docker Client & Engine
`$ docker version`  
*returns the version of your client and the server, also called the engine.*

`$ docker info`  
*shows most configuration values for the engine.*

## Image vs. Container
`Image`  
*An image is the application we want to run. It consists of the binaries and libraries that make up your application.*

`Container`  
*A Container is a running instance of an image.  You can have many containers running off the same image.*

## Docker Command Line Structure
The typical docker command line structure is as follows:  
`docker <command> <sub-command> (options)`  

**Typical Commands:**
```docker
# List all running containers
$ docker container ls

# List all containers (stopped, running, etc...)
$ docker container ls -a

# Stop a container
$ docker container stop <container>

# Remove a container
$ docker container rm <container>

# Forcefully remove a container (needed if it is running)
$ docker container rm -f <container>

# Show logs of container running in background
$ docker container logs <container>

# Run a container
$ docker container run <image>

# Run a container in background
$ docker container run --detach <image>

# Run a container w/ name, in background, on a specified port
$ docker container run --name webserver --detach --publish 8080:80 <image>

# Show the process list in one container
$ docker container top <container>

# Show performance stats for all containers
$ docker container stats
```



# Starting an Nginx Web Server
Below is a command to start an nginx server and a few steps of what it actually does:

```
$ docker container run --publish 8080:80 nginx
```
1. Look for the image locally in image cache. If not found, it tries a remote image repo and downloads it - *(defaults to Docker Hub)*
1. Creates a new container from that image
1. Gives it a virtual IP on a private network inside docker engine
1. Opens port 8080 on the host and forwards to port 80 in container
1. Starts container by using the CMD in the image Dockerfile

**_Note:_** *The container will be running on the foreground of our command line.*

To have it run in the background and give it a name:
```
$ docker container run --publish 80:80 --detach --name webhost nginx
```

If you want to see the logs of a container running in the background, you can use the following command:
```
docker container logs <container-name>
```

Now with the docker running, we can run the following command to see the list of running containers:
```
$ docker container ls
```

If we wanted to stop the container, we could use the following command:
```
$ docker container stop <container-id>
```
**_Note:_** *for the container id, you only have to put the first few characters, enough to make it unique.*

At this point if you run the `docker container ls` command, nothing would shown.  However, if you run the following command, **two** containers will be shown:
```
$ docker container ls -a
```

The reason why two containers are shown in the last section, is because we used the `docker container run` command twice.  That's because using that run sub-command always starts a new container, while using `docker container start` will simply start an existing stopped one.
```
$ docker container start <container-id>
```

To remove containers, we simply use the following command:
```
$ docker container rm <container-id> <container-id> ...
```
**_Note:_** *An error is thrown if you try to remove a running container, but you can force it with the `-f` flag.*


