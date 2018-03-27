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

# Start a container in shell environment (sets default command to bash)
$ docker container run -it <image> bash

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
docker container logs <container>
```

Now with the docker running, we can run the following command to see the list of running containers:
```
$ docker container ls
```

If we wanted to stop the container, we could use the following command:
```
$ docker container stop <container>
```

At this point if you run the `docker container ls` command, nothing would shown.  However, if you run the following command, **two** containers will be shown:
```
$ docker container ls -a
```

The reason why two containers are shown in the last section, is because we used the `docker container run` command twice.  That's because using that run sub-command always starts a new container, while using `docker container start` will simply start an existing stopped one.
```
$ docker container start <container>
```

To remove containers, we simply use the following command:
```
$ docker container rm <container>...
```
**_Note:_** *An error is thrown if you try to remove a running container, but you can force it with the `-f` flag.*



# Getting a Shell inside Containers
We can add a few flags to our run command to get a shell environment of within our docker container:
```bash
# start a new container interactively
$ docker container run -it

# run additional command in existing container
$ docker container exec -it
```
**Flags:**
- `-t`  
    **pseudo-tty -** *simulates a real terminal, like what SSH does.*
- `-i`  
    **interactive -** *Keep the session open to receive terminal input.*

To test this out, we spun up a new nginx server using the following command:
```bash
$ docker container run -it --name <image> nginx bash
# running 'bash' with '-it' gives you a terminal inside the running container
```
Notice the `bash` command passed at the end. Every container runs with a default command (for nginx its to run the nginx program itself), but you can override that by passing a command after the IMAGE ('nginx' in this case). So by running the `bash` command with the `-it` flags, it will give you a terminal inside the container. 

You can exit the terminal using the `exit` terminal command, but this will stop the container. That is because **_a docker container only runs as long as the command that runs on startup runs._** 

If we want to start up that container again, but within the terminal, we can use the following command:
```bash
$ docker container start -ai <image>
# notice that instead of -it, the start command requires -ai
```
**Flags:**
- `-a`  
    **attach -** *attach STDOUT/STDERR and forward signals*
- `-i`  
    **interactive -** *Keep the session open to receive terminal input.*

**_Note:_** *once you exit, it will close again, because `bash` is still the default command.*

By using the `docker container exec` command, we can run additional processes in a running container.  If we use the following command, we can get into the shell of a running container temporarily:
```
$ docker container exec -it <container> bash
```

### Linux Alpine
Linux Alpine is a very small and efficient linux distro.  By performing `docker pull alpine` and then `docker image ls`, you can see how much smaller alpine is to other images (just 4 MB).  If we were to run the `docker container exec -it alpine bash` it would fail, because bash is not installed on alpine, but by swapping bash out for sh, it will run just fine.

# Docker Network Concepts
When you start a container, in the background, you're connecting to a particular Docker network and by default, that is the "bridge" private virtual network. Each virtual network routes through NAT firewall on host IP, so that your containers can get out the internet. All containers on the same virtual network can talk to each other without `-p`. A best practice is to create a new virtual network for each app (ex. netowrk "my_web_app" for mysql and php containers). In most cases, the defaults work well, but it is also easy to swap out parts to customize it. As you will see, Docker follows a **"Batteries Included, but Removable"** motto, where the defaults work well, but are also easy to swap out parts to customize.  

**Overview:**
- Each container is connected to a private virtual network, which is "bridge" by default.
- Each virtual network routes through NAT firewall on host IP.
- All containers on the same virtual network can talk to each other without `-p`.
- Best practice is to create a new virtual network for each app.
    - network 'my_web_api' for mysql and apache containers.
    - network 'my_api' for mongo and nodejs containers.
- "Batteries Included, But Removable" - *defaults work well, but are also easy to swap out and customize.*
    - Make new virtual networks
    - Attach containers to more than one virtual networks (or none)
    - Skip virtual networks and use host IP (--net=host)
    - Use different Docker network drivers to gain new abilities
    - and much, much more...

## Hands On Practice with Docker Networking
In the following example, we ran an nginx container with a name of webhost and host port 8080, container port 80. 
```
$ docker container run -p 8080:80 --name webhost -d nginx
47f3d2c63c41427ee4750de1a1adc80aacc8d0ed46c3dcd71063107a9a420d5a
```
We then checked the port the container was using:
```
$ docker container port webhost
80/tcp -> 0.0.0.0:80
```
After that, we got the IP of the container:
```
$ docker container inspect --format '{{ .NetworkSettings.IPAddress }}' webhost
172.17.0.2
```
We then checked the IP address of the host and as you can see, they are different:
```
$ ifconfig en0
en0: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
	ether 80:e6:50:01:82:26
	inet6 fe80::c94:ba02:52ea:ecd5%en0 prefixlen 64 secured scopeid 0x5
	inet 10.10.180.226 netmask 0xffff0000 broadcast 10.10.255.255
	nd6 options=201<PERFORMNUD,DAD>
	media: autoselect
	status: active
```
## Docker Networks: CLI Management
There are three docker networks by default:

1. `'bridge'` - the default docker virtual network, which is NAT'ed behind the host IP. 

1. `'host'` - a special docker network that gains performance by skipping virtual networks and attaches directly to the host's interface, but sacrifices the security of the container model.

1. `'none'` - removes the eth0 interface and only leaves you with localhost interface in the container. 
 
To see all of the networks that have been created, use the following command:
```
$ docker network ls
```

You can spawn a new virtual network for you to attach containers to using the following command:
```
$ docker network create <name>
```
**_Note:_** *This will create a new network using the 'bridge' driver*

To attach a container to a network, you can do use the command shown below. This dynamically creates a NIC in a container on an existing virtual network.
```
$ docker network connect <network> <container> 
```

You can dynamically remove a NIC from a container on a specific virtual network:
```
$ docker network connect <network> <container>
```

**Docker Networks: Default Security**
- Create your apps so frontend/backend sit on the same docker network.
- Their inter-communication never leaves host
- All externally exposed ports closed by default
- You must manually expose via `-p`, which is better default security!

**_Note:_** *This gets even better later with Swarm and Overlay networks*

## Docker Networks: DNS
When using Docker networks, you need to remember one concept **forget IP's**!  Because of how dynamic docker and it's containers are, static IP's and using IP's for talking to containers is an anti-pattern. Do your best to avoid it and instead use Docker DNS Naming.

**Docker DNS** *- Docker daemon has a built-in DNS server that containers use by default.*  

**DNS Default Names** *- Docker defaults the hostname to the container's name, but you can also set aliases.*

**Hands On:**
```
$ docker-basics git:(master) ✗ docker container run -d --name server01 --network my_app_net nginx:alpine
ba00a1282c2fcd990d14f16c50abc3c05f29691fd98a6a028feb48661ee397d2

$ docker-basics git:(master) ✗ docker container run -d --name server02 --network my_app_net nginx:alpine
11a8ac78f6b02a94fd633256773784257721f7cafff8fafc60961261a7e0a513

$ docker-basics git:(master) ✗ docker container exec -it server01 ping server02
PING server02 (172.18.0.4): 56 data bytes
64 bytes from 172.18.0.4: seq=0 ttl=64 time=0.394 ms
64 bytes from 172.18.0.4: seq=1 ttl=64 time=0.120 ms
64 bytes from 172.18.0.4: seq=2 ttl=64 time=0.117 ms
64 bytes from 172.18.0.4: seq=3 ttl=64 time=0.118 ms
64 bytes from 172.18.0.4: seq=4 ttl=64 time=0.319 ms
^C
--- server02 ping statistics ---
5 packets transmitted, 5 packets received, 0% packet loss
round-trip min/avg/max = 0.117/0.213/0.394 ms
```

**Recap:**
- Containers shouldn't rely on IP's for inter-communication.

- DNS Names is built-in if you use custom networks and is the way you should set up inter-communication.

- Custom networks is recommended, since the default 'bridge' network does NOT have DNS Names built-in.

