# Docker notes

- [Docker notes](#docker-notes)
  - [Install Docker desktop](#install-docker-desktop)
  - [Basic operations](#basic-operations)
    - [Run a container](#run-a-container)
    - [Redirect a port](#redirect-a-port)
    - [Bash inside a running container](#bash-inside-a-running-container)
    - [Run an image and Bash inside by overwriting the entry point](#run-an-image-and-bash-inside-by-overwriting-the-entry-point)
    - [Stop a container](#stop-a-container)
    - [Remove containers, images and build cache](#remove-containers-images-and-build-cache)
    - [List the local containers](#list-the-local-containers)
  - [Docker hub](#docker-hub)
    - [Download a docker hub container image](#download-a-docker-hub-container-image)
  - [Dockerfile](#dockerfile)
    - [FROM](#from)
    - [RUN](#run)
    - [ADD](#add)
    - [EXPOSE](#expose)
    - [VOLUME](#volume)
    - [CMD](#cmd)
    - [COPY vs ADD](#copy-vs-add)
  - [Issues](#issues)
    - [No time sync](#no-time-sync)

## Install Docker desktop

Download the Windows (or MacOS) installer. Run the installer. On Windows accept the activation of *hyper-v*. If like me you were unfortunately using *VirtualBox*, it will not work anymore with *hyper-V* activated. Then you might switch to *VMWare* or use *Docker toolbox*. I did not choose the second option for the Docker team advise against it as it is provided for compatibility with old Windows versions.

Start *Docker Desktop* by running the executable `Docker Desktop.exe` (click the desktop icon or go to the main menu). Now containers from [Docker hub](https://www.docker.com/products/docker-hub) can be run from a terminal with the command `docker run`.
```
> docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
1b930d010525: Pull complete
Digest: sha256:451ce787d12369c5df2a32c85e5a03d52cbcef6eb3586dd03075f3034f10adcd
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```
The container is identified by the *digest* `sha256` hash value.

## Basic operations

### Run a container
```
> docker run image_name
```
An image may be run with a specific command. Here the container's folder `logs/` is listed:
```
> docker run image_name ls logs/
```

### Redirect a port

A port of the host machine may be redirected to a port of the container. This can be useful for accessing an HTTP server like *nginx* installed in the container. The following command redirects port 8080 to the default port 80 of an nginx HTTP server installed in the container *ningx* provided by [Docker hub](https://www.docker.com/products/docker-hub).
```
> docker run -d -p 8080:80 nginx
```
Option `-d` stands for detach so that the container is run by a daemon process. Option `-p` redirects port 8080 to port 80.

### Bash inside a running container

When running a container the hash of the container is printed:
<pre class="hljs"><code><div>>Digest: sha256:<span style="color:#33ce58">451ce787d12369c5df2a32c85e5a03d52cbcef6eb3586dd03075f3034f10adcd</span></div></code></pre>
or get short hash from `docker ps`.

The container can be explored with a fully functional `bash`:

<pre class="hljs"><code><div>> docker exec -ti 451ce787d12369c5df2a32c85e5a03d52cbcef6eb3586dd03075f3034f10ad <span style="color:#33ce58">bash</span> 
root@886e1dee4699:/#</div></code></pre>

`-t`, `--tty`  allocates a pseudo-TTY. `-i` keeps STDIN open even if not attached.

### Run an image and Bash inside by overwriting the entry point
Get the short hash of the image with `docker image ls`. Create a new container by specifying `bash` command as entry point with the option `--entrypoint`.
```
docker run -it --entrypoint bash 87a38b0c6e60
```

### Stop a container

Use command `docker stop` with its hash value:
```
> docker stop 451ce787d12369c5df2a32c85e5a03d52cbcef6eb3586dd03075f3034f10ad
```

### Remove containers, images and build cache

Remove a container with command `docker rm` and the ID of the container:
```
> docker rm 451ce787d12369c5df2a32c85e5a03d52cbcef6eb3586dd03075f3034f10ad
```
Remove an image with `docker rmi`.

For removing:
- all stopped containers
- all networks not used by at least one container
- all dangling images
- all dangling build cache
```
> docker system prune
```

For removing everything including non-dangling images and build cache:
```
> docker system prune -a
```

### List the local containers

The command `docker ps` lists the containers which are running on the machine:
```
> docker run -d -p 8080:80 nginx
98854e73ea71d81c999f55e149721672afedd7aa7d347126e0e672547431ca24

> docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                  NAMES
98854e73ea71        nginx               "nginx -g 'daemon of…"   7 seconds ago       Up 6 seconds        0.0.0.0:8080->80/tcp   goofy_shannon
```

For listing *all* the local container images use:
```
docker images -a
```
The option `-a` stands for all.

## Docker hub

### Download a docker hub container image

This operation downloads a container image container without running it.
```
docker pull <container's name>
```

## Dockerfile

Docker can build images automatically by reading the instructions from a Dockerfile. A Dockerfile is a text document that contains all the commands a user could call on the command line to assemble an image. Using `docker build` users can create an automated build that executes several command-line instructions in succession.

[Dockerfile reference](https://docs.docker.com/engine/reference/builder/).

### FROM
```
FROM <image>[:<tag>]
```

The `FROM` instruction initializes a new build stage and sets the *Base Image* (an image that has no parent) for subsequent instructions. As such, a valid `Dockerfile` must start with a `FROM` instruction. There can only be one `FROM` instruction in a Dockerfile.

Example:
```Dockerfile
FROM debian:9
```

### RUN
Run a command a shell, which by default is `/bin/sh -c` on Linux or `cmd /S /C` on Windows.

### ADD
```Dockerfile
ADD [--chown=<user>:<group>] <src>... <dest>
```
The `ADD` instruction copies new files, directories or remote file URLs from `<src>` and adds them to the filesystem of the image at the path `<dest>`.

### EXPOSE
```Dockerfile
EXPOSE <port> [<port>/<protocol>...]
```
Specification of the listening port `<port>` with an optional associated protocol `<protocol>`. The Docker container listens to this port at runtime.

### VOLUME
```Dockerfile
VOLUME ["/data"]
```
This path is shared with the host. The `VOLUME` instruction creates a mount point with the specified name and marks it as holding externally mounted volumes from native host or other containers.
Volumes can be listed with `docker volume ls` and removed with `docker volume rm`.

Specify the mountpoint on the host machine while running the container. Here the image named `docker-image-name` is run with a shared volume which mount point on the host machine is `C:/Users/flaudanum/Documents/DEV/docker-data` and path in the container is `/app/logs`:
```
docker run -v C:/Users/flaudanum/Documents/DEV/docker-data:/app/logs docker-image-name
```

### CMD
Shell-form syntax:
```
CMD command param1 param2
```
When used in the shell format, the `CMD` instruction sets the command to be executed when running the image. In the shell form, the <command> will execute in `/bin/sh -c`.

### COPY vs ADD

(From a blog at [takacsmark.com](https://takacsmark.com/dockerfile-tutorial-by-example-dockerfile-best-practices-2018/))

Both `ADD` and `COPY` are designed to add directories and files to your Docker image in the form of `ADD <src>... <dest>` or `COPY <src>... <dest>`. Most resources, including myself, suggest to use `COPY`.

The reason behind this is that `ADD` has extra features compared to `COPY` that make `ADD` more unpredictable and a bit over-designed. `ADD` can pull files from url sources, which `COPY` cannot. `ADD` can also extract compressed files assuming it can recognize and handle the format. You cannot extract archives with `COPY`.

The `ADD` instruction was added to Docker first, and `COPY` was added later to provide a straightforward, rock solid solution for copying files and directories into your container’s file system.

If you want to pull files from the web into your image I would suggest to use `RUN` and `curl` and uncompress your files with `RUN` and commands you would use on the command line.


## Issues
<style>
  .issue_section {
    text-decoration: underline;
    font-weight: 700;
  }
</style>

### No time sync
<span class="issue_section">Description:</span><br/>
This issue may happen with Linux containers on Windows. When running a container the time is not synchronized with the host.

See the [open issue](https://github.com/docker/for-win/issues/5803) at GitHub Docker's repository.

<span class="issue_section">Solution:</span><br/>
Restart *Docker desktop*. This issue should be corrected soon (now: 03-09-2020).
