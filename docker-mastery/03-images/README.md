# Docker Images
## What is in an image (and what isn't)?
A docker `image` is an ordered collection of root filesystem changes and the corresponding execution parameters for use within a container runtime. An image has the following: 
- App binaries and dependencies.
- Metadata about the image data and how to run the image.
- Not a complete OS. No kernel, kernel modules (e.g. drivers), because the host actually provides these things. It is NOT booting up a full OS like a typical VM.
- Can be as small as one file (your app binary) like a golang static binary or as big as an ubuntu distro with apt, Apache, PHP, and more installed. 

**Docker Hub** - *hub.docker.com*  
This is essentially like github for docker and it is the default remote location for remote docker pulls. You can login to your docker hub by first creating a free account on their website and then using `docker login`. You can just as easily logout by using the `docker logout` command.

**Image Layers**  - [official docs](https://docs.docker.com/storage/storagedriver/#images-and-layers)  
An image isn't just a big blob of data that comes and goes in one huge chunk. Instead, they're a series of changes on the file system and metadata about those changes. They are designed using the union file system concept of making layers about the changes. We can use the `docker image history <image:tag>` command to show layers of changes made to an image. These layers can be built atop shared bases which is much more efficient. Using the `docker image inspect <image:tag>` command will give us all the metadata on the image.
- Images are made up of file system changes (layers) and metadata.
- Each layer is uniquely identified and only stored once on a host.
- This saves storage space on host and transfer time on push/pull.
- A container is just a single read/write layer on top of an image.
- `history` and `inspect` commands can teach us how an image was made.

**Image Tags and Pushing to Docker Hub**  
If you were to run `docker image pull nginx` and then `docker image pull nginx:mainline`, you'd notice that the second pull would finish almost immediately. If you then run `docker image ls` and look at the "IMAGE ID" of both **"nginx:latest"** and **"nginx:mainline"**, you would notice that they are actually the same image. That is because although they have a different tags, in this case, they are still the same image. We can even retag an existing image to have our own custom tag using the following command:
```bash
$ docker image tag nginx nilsonmolina/nginx
```

You can then push this image up to docker hub however, you must first login to your docker hub account and then use the push command:
```bash
# Log into docker hub
$ docker login

# Push "customized" image to docker hub
$ docker image push nilsonmolina/nginx
```

We can also add different tags to our customized image in the same way `docker image tag nginx:latest nilsonmolina/nginx:testing`, and if you run the `docker image ls` command again, you will also see that this uses the exact same "Image ID".  If you push this newly tagged image, you will see a "Layer already exists" response, which is because the layers of the first uploaded image are exactly the same as this ones (so beautifully optimized!). 

## Building Images: Dockerfile Basics
The dockerfile included is from the official 'nginx' Docker Hub Repo and has plenty of comments to explain each of the commands.  Below I have included extra information that may be useful:

- All images must have a **FROM** and they are usually a minimal distribution like *"alpine"*.

-  We set environment variables using **ENV**, and they are the main way we set keys and values for container building and for running containers.

- The reason we use the "&&" in the **RUN** command, is to chain them and make sure that all the commands are fit into one single layer.  Thats because each stanza is a layer itself.

- The proper way of logging in containers, is to NOT log to a logfile, but instead log to "stdout" & "stderr", and docker will handle the rest.

- The **Expose** command will expose those ports on the docker virtual network, but it won't actually open them unless you use the `-p` flag when running the container. 

- **CMD** is a required parameter that is the final command that will be run every time you launch a new container from an image (or restart a stopped container).

**Hands On:**



**Typical Commands:**
```bash
# List all images 
$ docker image ls

# Show layers of changes made in an image
$ docker image history <image:tag>

# Returns JSON metadata about the image
$ docker image inspect <image:tag>

# Pull an image from Docker Hub
$ docker image pull <image>

# Re-tag existing docker image into a custom image
$ docker image tag <source-image>:<tag> <target-image>:<tag>

# Login to Docker Hub
$ docker login

# Uploads changed layers to an image registry (default: Docker Hub)
$ docker image push <image>

# Build a dockerfile
$ docker build -t testnode .
```