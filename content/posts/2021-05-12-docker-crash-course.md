---
layout: post
title: "Docker Crash Course Notes"
date: 2021-05-12
categories: [programming, docker, devops]
toc: true
---

## The course

The course is below.

<https://www.udemy.com/course/docker-tutorial-for-devops-run-docker-containers/>

My work for this course can be found here.

<https://github.com/HenryFBP/docker-crash-course>

## Section 1: Get Started with Docker

### 1. Course Overview

See course.

### 2. Support

See course.

### 3. Support

See course.

### 4. Slides

See course.

### 5. Intro to virtualization

Docker is only one implementation of containerization.

Before virtualization, the OS is installed on a physical machine, and apps run on the OS. Each machine only ran 1 app.

This is a problem as you need 1 machine per app, and this is wasteful and wastes CPU and RAM as they are underutilized.

Also, this slows deployment time and makes migration hard.

Next up, hypervisors run multiple OSes on top of one host OS.

This is cost efficient and easy to scale, but kernel resources get duplicated as you need 1 kernel per guest OS.

One more issue is that VM portability is difficult as you have different guest OSes.

Finally, containerization is a process where only 1 operating system runs - Docker is an implementation of container-based virtualization. The kernel does not get copied, there is only 1 kernel (Container Engine) with containerization.

By running apps in different VMs, we achieve runtime isolation.

This means we can use JRE7, 8, 9, 11, etc; without introducing any conflicts.

Container-based virtualization is more cost-efficient.

### 6. Docker's Client-Server architecture

Docker uses a client-server architecture, and the docker daemon is the server.

The user interacts through the daemon using `docker build`, `docker run`, etc.

There are 2 types of docker clients:

- CLI
- Kitematic, a Docker client with a GUI

The docker daemon is also called "Docker Engine" or "Docker Server".

On OSX/Windows, the Docker daemon runs in a Linux VM.

### 7. Install Docker

...

### 8. Install Docker Toolbox

...

### 9. Important Concepts of Docker Technology

#### Images

- Read-only templates used to create containers
- Created with `docker build` command by us or other users
- Can be large
    - Intended to be made of other images
- Stored in a Docker registry

#### Containers

- metaphor: If an image is a Java class, then a container is an instance of that class
- Lightweight and portable encapsulations of an environment
- Created from images
- Runnable
- Has binaries and dependencies on filesystem

#### Registries

- Where images are stored.
- You can self host or use DockerHub, docker's public registry

#### Repositories

- Inside a registry, images are stored in repos
- A collection of different docker images with the same name, but different tags/versions

#### Docker Hub

A public docker registry with a LOT of images you can use.

https://hub.docker.com/

It's recommended to use official images instead of non-official images.

### 10. Hello World Docker Container

We'll create a container from `busybox` image.

<https://hub.docker.com/_/busybox>

It's a very small image.

In Tags, we can see 1.24 as our version.

Run `docker images` to see what images you have pulled.

    vagrant@vagrant-virtualbox ~/G/henryfbp.github.io (master)> docker images
    REPOSITORY   TAG       IMAGE ID   CREATED   SIZE

None.

Run `docker run busybox:1.24 echo "hello world"` to run the specific tagged box with a command.

We can also run the container in interactive mode with `-i` and `-t`.

`-i` starts an interactive container.

`-t` creates a pseudo-TTY that attaches `stdin` and `stdout`.

    docker run -i -t busybox:1.24 

Note that the container's filesystem is lost when the container shuts down. If you made a file in the terminal, it would not be kept.

### 11. Deep Dive into Docker Containers

Most cases, you want to run containers in the background.

Use `-d` for detatched mode.

Example:

    docker run -d busybox:1.24 sleep 1000

This docker container will sleep for 1000 seconds.

Run `docker ps` to see it running. You can run `docker ps -a` to see all previous commands.

---

If you want to delete the container when it exists, you add `--rm`.

Example:

    docker run --rm busybox:1.24 sleep 1

Now, if you run `docker ps -a` you will NOT see the container that ran `sleep 1`.

---

You can also specify a name of a container with `--name`.

Example:

    docker run --name hello_world busybox:1.24

Running `docker ps -a` will show you the name.

---

Another useful command is `docker inspect`. This will show you low level info on an image.

Start a container in detached mode with `docker run -d busybox:1.24 sleep 999`, and you get a long container ID from that command.

Then run `docker inspect 13eedc4b42499231873f6926620d148ee219146c78b4727e08d0798d426a7f8d` to get a JSON array.

You get the address, memory info, image ID, log path, etc.

### 12. Docker Port Mapping and Docker Logs Command

We'll use Tomcat.

https://hub.docker.com/_/tomcat

Tomcat runs on port 8080 by default.

But Docker lets you expose container ports (VM) onto host ports by using `-p` option.

Example: Tomcat runs on port 8080 in container, and is exposed onto 8888 in the host:

    docker run -it -d -p 8888:8080 tomcat:8.0

The format is `-p host_port:container_port`.

This means you can later visit http://localhost:8888 in your browser, in the host OS. If you're not using Linux, scroll up to see the host IP and use that instead of `localhost`.

You can later run `docker logs container_id` to see the logs from a container.

### 13. Deep Dive into Logging

<https://www.level-up.one/deep-dive-into-docker-logging>

## Section 2: Working with Docker Images

### 14. Docker Image Layers

Docker images are a stack of layers. Example:


    Description     Type
    --------------------------
    writable        Container
    add apache      Image
    add emacs       Image
    Kernel          bootfs

You can check the full set of layers making up an image by running:

    docker history busybox:1.24

Gives us:

    IMAGE          CREATED       CREATED BY                                      SIZE      COMMENT
    47bcc53f74dc   5 years ago   /bin/sh -c #(nop) CMD ["sh"]                    0B        
    <missing>      5 years ago   /bin/sh -c #(nop) ADD file:47ca6e777c36a4cff…   1.11MB    

The base layer adds a file, and the next just runs `sh`.

When you make a new container, you add a new writable layer on top of the existing stack of layers.

This layer is often called the "writable container layer".

All changes made to the running container will be writen to this thin writable container layer.

The main difference between a container and an image is the top writable layer exists in the container.

When the container is deleted, the writable layer also gets deleted, but the image is unchanged.

This means multiple containers can share the same image.

### 15. Build Docker images by using `docker commit`

2 ways to build a docker image:

1. Commit changes made in a Docker container.
2. Write a `Dockerfile`.

Steps for type 1:

1. Spin up a container from a base image
2. Install a package
3. Commit changes that were made

Let's do it!

<https://hub.docker.com/_/debian>

    docker run -it debian:jessie

    ls

    git

Git's not installed.

    apt-get update && apt-get install -y git

    clear

    git

Now we can `exit` and commit our container as a new docker image.

    exit

    docker commit container_id repository_name:tag

For me this was:

    docker commit 3b6f832d9588 henryfbp/debian:1.0.0

Now if we run `docker images`, we can see our new image:

    vagrant@vagrant-virtualbox ~/G/henryfbp.github.io (master)> docker images
    REPOSITORY        TAG       IMAGE ID       CREATED          SIZE
    henryfbp/debian   1.0.0     53ad0d3f3c1a   51 seconds ago   224MB
    debian            jessie    3aaeab7a4777   6 weeks ago      129MB
    busybox           1.24      47bcc53f74dc   5 years ago      1.11MB

We can even start a container from our new image.

    docker run -it henryfbp/debian:1.0.0

    ls

    git

### 16. Build Docker Images by making a `Dockerfile`

A Dockerfile is a text document that contains all the steps to assemble an image.

Each instruction adds a layer.

See <https://github.com/HenryFBP/docker-crash-course/tree/master/dockerfile-example>.

Run `docker build henryfbp/debian .` in the same dir as the Dockerfile.

- Docker build takes the path (`.`) to the build context as an argument.
- When the build starts, the docker client packs all the files in the build context path into a tarball and transfer that file to the daemon.
- By default, docker searches for `Dockerfile` in the root of the build context path.
- If your `Dockerfile` doesn't sit in the root of the build context path, you can use `-f` to change the Dockerfile path.

Output:

    vagrant@vagrant-virtualbox ~/G/d/dockerfile-example (master) [1]> docker build -t henryfbp/debian .
    Sending build context to Docker daemon  2.048kB
    Step 1/4 : FROM debian:jessie
    ---> 3aaeab7a4777
    Step 2/4 : RUN apt-get update
    ---> Running in 2e3b76aea9b3
    Get:1 http://security.debian.org jessie/updates InRelease [44.9 kB]
    
    ...

    update-alternatives: using /usr/bin/vim.basic to provide /usr/bin/ex (ex) in auto mode
    update-alternatives: using /usr/bin/vim.basic to provide /usr/bin/editor (editor) in auto mode
    Processing triggers for libc-bin (2.19-18+deb8u10) ...
    Removing intermediate container 69eb836d88b3
    ---> 3490c50cf88b
    Successfully built 3490c50cf88b
    Successfully tagged henryfbp/debian:latest

The reason we see `Removing intermediate container 69eb836d88b3` is because the Docker daemon runs each instruction in a different container.

Once it writes changes to an image, and commits it, the container used to run the old instruction is removed. This is repeated. Containers are ephemeral and only used once in this scenario. Images are persistent and read only.

### 17. Dockerfile in-depth

#### Chain RUN instructions

- Each RUN command executes the command on the top writable layer of the container, and then commit the container as a new image.
- The new image is used for the next step in the Dockerfile. So each RUN instruction will create a new image layer.
- It is recommended to chain RUN instructions in Dockerfile to reduce the number of image layers created.

Example Dockerfile:

https://github.com/HenryFBP/docker-crash-course/tree/master/dockerfile-in-depth

```
FROM debian:jessie
RUN apt-get update && apt-get install -y \ 
  git \
  vim
```

#### CMD Instructions

- This specifies what command to run when the container starts up.
- If not specified in the Dockerfile, Docker will use the default command defined in the base image.
- The CMD instruction doesn't run when building the image. Only when the container starts up.
- You can use EXEC form (preferred) or use shell form.

```
FROM debian:jessie
RUN apt-get update && apt-get install -y \ 
  git \
  python \
  vim

CMD ["python", "-c", "print('hello from python!')"]
```

Output:

```
vagrant@vagrant-virtualbox ~/G/d/dockerfile-in-depth (master)> docker build -t henryfbp/debian .
Sending build context to Docker daemon  2.048kB
Step 1/3 : FROM debian:jessie
 ---> 3aaeab7a4777
Step 2/3 : RUN apt-get update && apt-get install -y   git   python   vim
 ---> Using cache
 ---> d1b8c2ab08bb
Step 3/3 : CMD ["python", "-c", "print('hello from python!')"]
 ---> Using cache
 ---> 3707fcb27c55
Successfully built 3707fcb27c55
```

Then we can run `docker run 3707fcb27c55`:

```
vagrant@vagrant-virtualbox ~/G/d/dockerfile-in-depth (master)> docker run 3707fcb27c55
hello from python!
```

You can also overwrite the `CMD` instruction like this:

    docker run 3707fcb27c55 echo "hello from sh!"

#### Docker Cache

- Each time docker executes an instruction, it builds a new image layer
- If the instruction doesn't change, docker will just reuse an existing layer.

Example - "Using cache" can be seen:

```
vagrant@vagrant-virtualbox ~/G/d/dockerfile-in-depth (master)> docker build -t henryfbp/debian .
Sending build context to Docker daemon  2.048kB
Step 1/3 : FROM debian:jessie
 ---> 3aaeab7a4777
Step 2/3 : RUN apt-get update && apt-get install -y   git   python   vim
 ---> Using cache
 ---> d1b8c2ab08bb
Step 3/3 : CMD ["python", "-c", "print('hello from python!')"]
 ---> Using cache
 ---> 3707fcb27c55
Successfully built 3707fcb27c55
```

This can speed up build time a lot. But it may cause issues if used too much.

#### Aggressive Caching

Changing this:

```
FROM ubuntu:14.04
RUN apt-get update
RUN apt-get install -y git
```

To this:

```
FROM ubuntu:14.04
RUN apt-get update
RUN apt-get install -y git curl
```

Means the first 2 lines are reused.

This means `apt-get update` would not be run, which may result in you getting an out of date version of `git` and `curl`.

One solution: Chain instructions.

```
FROM ubuntu:14.04
RUN apt-get update && apt-get install -y \
    git \ 
    curl
```

This ensures that whenever `apt-get install` is modified, `apt-get update` gets run again as well.

You could also add `--no-cache=true`.

    docker build -t henryfbp/debian . --no-cache=true

#### COPY instruction

- Copies files to the container.

Example:

```
FROM debian:jessie
RUN apt-get update && apt-get install -y \ 
  git \
  python \
  vim

COPY foo.txt /src/foo.txt

RUN cat /src/foo.txt

CMD ["python", "-c", "print('hello from python!')"]
```


#### ADD instruction

- Similar to COPY
- ADD can do more magic
- It can download files from the internet
- Also can unpack compressed files.
- COPY is preferred to ADD, as COPY is more transparent. Use COPY unless you need ADD.

### 18. Push Docker images to Docker Hub

Use <https://hub.docker.com/> and make a Docker Hub account.

Mine is <https://hub.docker.com/u/henryfbp>.

I made a repo here:

https://hub.docker.com/repository/docker/henryfbp/betterdebian

I'm going to go to `docker-crash-course/dockerfile-in-depth` from this repo:

https://github.com/HenryFBP/docker-crash-course/

And run `docker build -t henryfbp/debian:1.0.1 .`

Then run `docker images` to get its image ID. It should be at the top.

To rename our image, run

    docker tag 7bbb4d211d9b henryfbp/betterdebian:1.0.0

If we didn't add `:1.0.0`, docker would have just used "latest" as the default tag, which is sloppy and should be avoided.

Images tagged 'latest' will not be automatically updated when a newer version of the image is pushed to the repo. Avoid using latest tag. Avoid pulling latest tag. Specify tags every time.

Then run `docker images` again. You should see the tagged image.

    vagrant@vagrant-virtualbox ~/G/d/dockerfile-in-depth (master)> docker images
    REPOSITORY              TAG       IMAGE ID       CREATED       SIZE
    henryfbp/betterdebian   1.0.0     7bbb4d211d9b   4 hours ago   280MB
    henryfbp/debian         1.0.1     7bbb4d211d9b   4 hours ago   280MB
    henryfbp/debian         latest    7bbb4d211d9b   4 hours ago   280MB
    henryfbp/debian         1.0.0     53ad0d3f3c1a   5 hours ago   224MB
    tomcat                  9.0       c0e850d7b9bb   2 weeks ago   667MB
    debian                  jessie    3aaeab7a4777   6 weeks ago   129MB

The next step is to push `henryfbp/betterdebian:1.0.0` to docker hub.

You need to use `docker login --username=henryfbp` and then provide your password first.

Then run `docker push henryfbp/betterdebian:1.0.0`.

    vagrant@vagrant-virtualbox ~> docker push henryfbp/betterdebian:1.0.0
    The push refers to repository [docker.io/henryfbp/betterdebian]
    4084515d3265: Pushed 
    de0ab1eee188: Pushed 
    fac8b84e323e: Mounted from library/debian 
    1.0.0: digest: sha256:93f342ed49915cef585c0756143e72b166466fd48f7a13f8afa2c5927a017eea size: 948

You can see it at https://hub.docker.com/repository/docker/henryfbp/betterdebian

## Section 3: Create Containerized Web Applications

### 19. Containerize a Simple Hello World Web Application

1. Run `git clone -b v0.1 https://github.com/henryfbp/dockerapp/` to clone a simple dockerized application.

`-b v0.1` tells git to clone the repo and move the HEAD to the branch `v0.1`, which is an early version of the app.

Inside the working directory, you can see there's just 2 files: 

    app/app.py
    Dockerfile

`app.py` is a very simple flask app.

Inside Dockerfile:

```
FROM python:3.5
RUN pip install Flask==0.11.1
RUN useradd -ms /bin/bash admin
USER admin
WORKDIR /app
COPY app /app
CMD ["python", "app.py"] 
```

1. Use base image `python:3.5`
2. Install Flask using `pip`
3. Create a new user, `admin`, who has a default shell of `/bin/bash`.
4. All commands Docker runs are now run by the newly created `admin` user.
   
   Note: It's a good idea to avoid running processes in Docker as a root user to avoid privilege escalation.
5. Set the current working directory to `/app` for all `COPY`, `ADD`, etc. instructions.
6. Copy a folder, `app`, on the parent, to the container's filesystem root `/app`.
7. Start the webserver.

Let's build it!

    docker build -t dockerapp:v0.1 .

And get the newly-built image ID.

    docker images

    REPOSITORY              TAG       IMAGE ID       CREATED              SIZE
    dockerapp               v0.1      b6069a89ed4c   About a minute ago   881MB
    henryfbp/betterdebian   1.0.0     7bbb4d211d9b   2 days ago           280MB


And then start a container from the image.

    docker run -d -p 5000:5000 b6069a89ed4c

And now, visit <http://localhost:5000/>

Next, run `docker ps` to view the list of running containers, and to get the container ID:

    vagrant@vagrant-virtualbox ~> docker ps
    CONTAINER ID   IMAGE          COMMAND           CREATED         STATUS         PORTS                    NAMES
    1698ac6cd3ca   b6069a89ed4c   "python app.py"   2 minutes ago   Up 2 minutes   0.0.0.0:5000->5000/tcp   elastic_ritchie

Then, we're going to enter the container with a shell.

    docker exec -it 1698ac6cd3ca bash

    admin@1698ac6cd3ca:/app$ ls
    app.py

If we run `ps axu`, we see this:

    admin@1698ac6cd3ca:~$ ps axu
    USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
    admin          1  0.2  0.3  35640 26888 ?        Ss   17:43   0:01 python app.py
    admin          8  0.1  0.0   5756  3704 pts/0    Ss   17:48   0:00 bash
    admin         19  0.0  0.0   9396  3096 pts/0    R+   17:50   0:00 ps axu

The webserver is still running in the background, while we are in the shell. Note the user running it is `admin`.

### 20. Text Direction: Containerize a Hello World Web Application

...

### 21. Implement a Simple Key-value Lookup Service

Inside your `dockerapp` folder, run:

    git stash && git checkout v0.2

To change the HEAD to a newer version of the docker web app example.

Now rebuild the docker image with:

    docker build -t dockerapp:v0.2

Output:

    vagrant@vagrant-virtualbox ~/G/dockerapp ((v0.2)) [1]> docker build -t dockerapp:v0.2 .
    Sending build context to Docker daemon  86.53kB
    Step 1/7 : FROM python:3.5
    ---> 3687eb5ea744
    Step 2/7 : RUN pip install Flask==0.11.1
    ---> Using cache
    ---> 2406c93d981a
    Step 3/7 : RUN useradd -ms /bin/bash admin
    ---> Using cache
    ---> dfeb1e7906d5
    Step 4/7 : USER admin
    ---> Using cache
    ---> 664ea1b9ff07
    Step 5/7 : WORKDIR /app
    ---> Using cache
    ---> 52fdf671467c
    Step 6/7 : COPY app /app
    ---> 8663f068c28a
    Step 7/7 : CMD ["python", "app.py"]
    ---> Running in ee06eec84720
    Removing intermediate container ee06eec84720
    ---> 9ab9b6586dd5
    Successfully built 9ab9b6586dd5
    Successfully tagged dockerapp:v0.2

Note that only step 6 and 7 get executed, the rest of the steps are simply reused.

Make sure to stop your previous container next - use `docker ps` and `docker stop <container_id>`.

Next, start the updated dockerapp container with:

    docker run -d -p 5000:5000 dockerapp:v0.2

And visit <http://localhost:5000/>. Play around with the app a bit.

...

Now we'll introduce Redis, a memory cache, to take the role of the database in this application.

I made a new branch called `add-redis` to track my work in this section.

See next section.

### 22. Create Docker Container Links

Container links are essentially secure LAN links. The links depend on container names.

Run this to get a Redis container running:

    docker run -d --name redis redis:3.2.0

Then `docker ps`:

    vagrant@vagrant-virtualbox ~/G/dockerapp (add-redis)> docker ps
    CONTAINER ID   IMAGE            COMMAND                  CREATED          STATUS          PORTS                    NAMES
    4e369b5bdb17   redis:3.2.0      "docker-entrypoint.s…"   4 seconds ago    Up 1 second     6379/tcp                 redis
    38847d7d8da3   dockerapp:v0.2   "python app.py"          25 minutes ago   Up 25 minutes   0.0.0.0:5000->5000/tcp   focused_wing

Then build your dockerapp again -- Using the branch `add-redis` (`git checkout add-redis`) and bumping the version as the dockerapp now uses redis.

    docker build -t dockerapp:v0.3 .

Stop `dockerapp:v0.2` with `docker ps` and `docker stop <container_id>` if it's running.

Then start an instance of `dockerapp:v0.3`, except with `--link redis` added:

    docker run -d -p 5000:5000 --link redis dockerapp:v0.3

My understanding is these are just put on some virtual subnet together automagically.

Now visit <http://localhost:5000/> to make sure everything is working.

Running `docker ps`, we can see both boxes running:

    vagrant@vagrant-virtualbox ~/G/dockerapp (add-redis)> docker ps
    CONTAINER ID   IMAGE            COMMAND                  CREATED          STATUS          PORTS                    NAMES
    600048574ea5   dockerapp:v0.3   "python app.py"          4 minutes ago    Up 3 minutes    0.0.0.0:5000->5000/tcp   cool_faraday
    4e369b5bdb17   redis:3.2.0      "docker-entrypoint.s…"   13 minutes ago   Up 13 minutes   6379/tcp                 redis

Run `docker exec -it 600048574ea5 bash` to enter the Python container.

Then run `more /etc/hosts` to view the initial hostname setup.

    admin@600048574ea5:/app$ more /etc/hosts
    127.0.0.1	localhost
    ::1	localhost ip6-localhost ip6-loopback
    fe00::0	ip6-localnet
    ff00::0	ip6-mcastprefix
    ff02::1	ip6-allnodes
    ff02::2	ip6-allrouters
    172.17.0.3	redis 4e369b5bdb17
    172.17.0.2	600048574ea5

Note how `redis` gets assigned a static IP address.

You can run `docker inspect 4e369b5bdb17 | grep -i ip` to confirm that `172.17.0.3` is indeed the IP address of the redis container.

Log into the python container again with `docker exec -it 600048574ea5 bash`, and run `ping redis`.

It works!

```
vagrant@vagrant-virtualbox ~/G/dockerapp (add-redis)> docker exec -it 600048574ea5 bash
admin@600048574ea5:/app$ ping redis
PING redis (172.17.0.3) 56(84) bytes of data.
64 bytes from redis (172.17.0.3): icmp_seq=1 ttl=64 time=1.20 ms
64 bytes from redis (172.17.0.3): icmp_seq=2 ttl=64 time=0.180 ms
64 bytes from redis (172.17.0.3): icmp_seq=3 ttl=64 time=0.163 ms
64 bytes from redis (172.17.0.3): icmp_seq=4 ttl=64 time=0.202 ms
64 bytes from redis (172.17.0.3): icmp_seq=5 ttl=64 time=0.120 ms
^C
--- redis ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 58ms
rtt min/avg/max/mdev = 0.120/0.373/1.202/0.415 ms
```

#### Summary

The main use for docker container links is for building applications with microservice architecture -- We can run many independent components in different containers.

Docker creates a secure tunnel between the containers that only exposes a minimum of necessary ports.

### 23. Automate Current Workflow with Docker Compose

Phew, thank god. I was afraid we wouldn't cover it. I love docker-compose.

#### Why Docker Compose?

- We need to manually start up 2 containers and manually link them every time.
- Doing this for 20 containers or a lot of times is just a waste of time and effort.
- Think of not using `Pipenv` and having to type `pip install some-crap` 20 times. It's just a waste of time, it can be automated and streamlined.

Example `docker-compose.yml` file, which can be found in the `add-redis` branch (<https://github.com/HenryFBP/dockerapp/tree/add-redis>):

```yaml
version: '3'
services:
    dockerapp:
        build: .
        ports:
            - "5000:5000"
        depends_on:
            - redis
    redis:
        image: redis:3.2.0
```

Note that line 4 uses a `Dockerfile` in the same directory as the `docker-compose.yml` file.

As for line 9, you may note the lack of a `build` key. This is because all containers defined in `docker-compose.yml` can either have a base `"image"` command, or a `"build"` instruction. One or the other.

You may note we don't need a link. This is because from version 2 and onwards, it's automatically done between containers managed by Docker Compose.

Make sure no ports are open with `docker ps` and `docker stop`, and then you can just run `docker-compose up`. That's it. Then go to <http://localhost:5000/> and enjoy the fancy console output.

#### Summary

Docker Compose eliminates a lot of extra work and makes working with Docker containers a lot easier.

### 24. Deep Dive into Docker Compose Workflow

If you don't have the docker-compose example set up, run this in my 'dockerapp' repo.

    git stash && git checkout v0.4

You can run `docker-compose up -d` to not have your terminal hijacked by Docker Compose.

You can also run `docker-compose ps` to view information on the running containers managed by `docker-compose` for a specific `docker-compose.yml` file:

    vagrant@vagrant-virtualbox ~/G/dockerapp (add-redis)> docker-compose ps
            Name                       Command               State           Ports         
    ---------------------------------------------------------------------------------------
    dockerapp_dockerapp_1   python app.py                    Up      0.0.0.0:5000->5000/tcp
    dockerapp_redis_1       docker-entrypoint.sh redis ...   Up      6379/tcp              

- `docker-compose logs`. 
    - Add `-f` to follow log output. Like `tail`.
    - Add a container name to only view that container's logs - i.e. `docker-compose logs dockerapp`

TODO

### 25. Extra Learning: Things to Watch out When Working with Docker Containers

## Section 4: Docker Networking
TODO

## Section 5: Create a Continuous Integration Pipeline
TODO

## Section 6: Deploy Docker Containers in Production
TODO

## Section 7: Additional Learning Materials
TODO
