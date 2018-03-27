# Docker Intro & Setup
## Docker Editions - *store.docker.com*
Docker is no longer just a "container runtime" and it moves really fast so things are changing quickly. Docker CE (Community Edition) is the version we will focus on in this course and it is the free, open-source toolkit that is free.  Docker EE (Enterprise Edition) is the paid, enterprise version which we will NOT be using in this course. It is typically for better support and extra features more likely used in large scale production environments.

### Install Types

There are three major types of installs:
1. **Direct**  
    *Installed directly onto a supported operating system, like linux, raspberry pi, windows server, etc...*
1. **Mac/Win**  
    *Docker for Mac/Windows uses a small VM to run docker in, since it is not supported directly on these OS's.*
1. **Cloud**  
    *Docker for AWS/Azure/Google usually come with features specific to these platforms.*

**_Note:_** *I installed "Docker for Mac" from the docker store for this training.*

**Stable vs Edge**  
Edge (beta) is released monthly and is only supported for a month. Stable is released quarterly and it rolls in three months of Edge features.

## After Installation
After installing "Docker for Mac", I went into the preferences by clicking on the docker logo in the top toolbar and clicking preferences.  Here you can change VM options like RAM, CPU count, Storage space, etc... After that, I opened up terminal and ran the following commands for further information:
```bash
# shows the Docker version information
$ docker version

# display system-wide information
$ docker info

# list running containers
$ docker ps  

# list images locally stored with Docker Engine
$ docker images 
```

### Docker shell completion
We also installed the 'docker' extension in vscode as well as zsh completion. The following are the steps to set-up bash completion on macOS:

1. Make sure bash completion is installed by running the following brew command:
    ```
    $ brew install bash-completion
    ```
2. In Zsh, the completion system takes care of things. To activate completion for Docker commands, these files need to be copied or symlinked to your Zsh `site-functions/` directory. For example, if you installed Zsh via Homebrew:
    ```
    $ etc=/Applications/Docker.app/Contents/Resources/etc
    $ ln -s $etc/docker.zsh-completion /usr/local/share/zsh/site-functions/_docker
    $ ln -s $etc/docker-machine.zsh-completion /usr/local/share/zsh/site-functions/_docker-machine
    $ ln -s $etc/docker-compose.zsh-completion /usr/local/share/zsh/site-functions/_docker-compose
    ```

    Reference:  
    https://docs.docker.com/machine/completion/

