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

- `docker-compose stop`... Does what you think it does. Does not remove containers.

- `docker-compose rm` stops then removes all containers.

Note that if you modify the Dockerfile, `docker-compose up` will not recreate the image. You need to run `docker-compose build` if you need to update the image.

### 25. Extra Learning: Things to Watch out When Working with Docker Containers

Extra article: <https://www.level-up.one/things-watch-working-docker-containers/>

## Section 4: Docker Networking

### 26. Introduction to Docker Networking 

- Docker uses the networking from the host OS to give containers networking
- Once Docker Daemon is installed, `docker0` interface is created on the host. It is used to bridge outside network to the internal containers.
- Each container connects to `docker0` through an individual container network interface.

![](/images/2021-05-12-docker-crash-course/docker-networking.png)

There are 4 types of networks.

1. Closed/None Network
2. Bridge Network
3. Host Network
4. Overlay Network

Run `docker network ls`, which should bring up 3 preinstalled networks.

### 27. None Network

No access to the outside world. This adds a container with a network stack that has no container interface.

To make a closed container, add `--net none` to a `docker run` command:

    docker run -d --net none busybox sleep 1000

Then `docker exec -it <id> sh` and try to `ping 8.8.8.8` - it will fail. `ifconfig` only reports `lo` (loopback) as an adapter.

- Provides max level of network protection
- Bad if network connection is required
- Ideal where network access is not necessary

### 28. Bridge Network

Default type of network in Docker containers.

All containers in bridge network are connected to each other, and can also connect to the outside world.

Docker makes a default bridge network called "bridge" when the docker daemon is first installed.

Run

    docker network ls

    docker network inspect bridge

To view details about the "bridge" network.

Subnet range is 172.17.0.0 to 172.17.255.255

Run

    docker run -d --name container_1 busybox sleep 1000
    docker run -d --name container_2 busybox sleep 1000

We don't specify `--net` as `bridge` is default.

Then run

    docker exec -it container_1 ifconfig

To get `container_1`'s ip address.

And then run

    docker ps

To get `container_2`'s ID.

And then run

    docker exec -it container_2 sh

    ping <container_1_ip>

To see if the bridge network works.

Output:

```
┌─[vagrant@vagrant-virtualbox]─[~/Github/henryfbp.github.io]
└──╼ $    docker network ls
NETWORK ID     NAME                DRIVER    SCOPE
f4bbec1ad501   bridge              bridge    local
6990fcf18be5   host                host      local
09d23bc21e33   my_bridge_network   bridge    local
84049b5950fc   none                null      local
┌─[vagrant@vagrant-virtualbox]─[~/Github/henryfbp.github.io]
└──╼ $    docker network inspect my_bridge_network
[
    {
        "Name": "my_bridge_network",
        "Id": "09d23bc21e33707d9a3385d07c6c3ddb7494dda233be8be791633a13fcefcb61",
        "Created": "2021-05-26T18:49:04.675528224-05:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.18.0.0/16",
                    "Gateway": "172.18.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {},
        "Options": {},
        "Labels": {}
    }
]
```

You should be able to ping the other container. This works because the hostname is set to the container name.

#### Separate Bridged Networks cannot communicate

By default, different bridged networks cannot contact eachother. Let's demo this.

Create a new bridge network:

    docker network create --driver bridge my_bridge_network

    docker network ls

    docker network inspect my_bridge_network

Then create a docker box using that bridge network:

    docker run -d --name container_3 --net my_bridge_network busybox sleep 100000

And check the IP of container_3:

```
vagrant@vagrant-virtualbox ~/G/henryfbp.github.io (master)> docker exec -it container_3 ifconfig | grep inet | grep 172
          inet addr:172.18.0.2  Bcast:172.18.255.255  Mask:255.255.0.0
```

And then get the IP of container_1:

```
vagrant@vagrant-virtualbox ~/G/henryfbp.github.io (master)> docker exec -it container_1 ifconfig | grep inet | grep 172
          inet addr:172.17.0.2  Bcast:172.17.255.255  Mask:255.255.0.0
```

And then try to ping container_1 from container_3:

```
vagrant@vagrant-virtualbox ~/G/henryfbp.github.io (master)> docker exec -it container_3 ping 172.17.0.2
PING 172.17.0.2 (172.17.0.2): 56 data bytes
^C
--- 172.17.0.2 ping statistics ---
26 packets transmitted, 0 packets received, 100% packet loss
```

It fails because different bridge networks are isolated from eachother in Docker.

But, docker has a feature that lets us connect a container to another network.

This is done via `docker network connect`.

#### docker network connect

Let's connect `container_3` to our original bridge network.

```
vagrant@vagrant-virtualbox ~/G/henryfbp.github.io (master) [1]> docker network ls
NETWORK ID     NAME                DRIVER    SCOPE
f4bbec1ad501   bridge              bridge    local
6990fcf18be5   host                host      local
09d23bc21e33   my_bridge_network   bridge    local
84049b5950fc   none                null      local
```

`container_{1,2}` are both on `bridge`. Only `container_3` is connected to `my_bridge_network`.

We can attach `bridge` to `container_3` by running:

    docker network connect bridge container_3

And we can see `eth1` interface gets added below:

```
vagrant@vagrant-virtualbox ~/G/henryfbp.github.io (master)> docker exec -it container_3 ifconfig
eth0      Link encap:Ethernet  HWaddr 02:42:AC:12:00:02  
          inet addr:172.18.0.2  Bcast:172.18.255.255  Mask:255.255.0.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:9 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:806 (806.0 B)  TX bytes:0 (0.0 B)

eth1      Link encap:Ethernet  HWaddr 02:42:AC:11:00:02  
          inet addr:172.17.0.2  Bcast:172.17.255.255  Mask:255.255.0.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:8 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:736 (736.0 B)  TX bytes:0 (0.0 B)

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)
```

And we can ping `container_1` now.

```
vagrant@vagrant-virtualbox ~/G/henryfbp.github.io (master)> docker exec -it container_1 ifconfig | grep inet | grep 172
          inet addr:172.17.0.3  Bcast:172.17.255.255  Mask:255.255.0.0
vagrant@vagrant-virtualbox ~/G/henryfbp.github.io (master)> docker exec -it container_3 ping 172.17.0.3
PING 172.17.0.3 (172.17.0.3): 56 data bytes
64 bytes from 172.17.0.3: seq=0 ttl=64 time=0.083 ms
^C
--- 172.17.0.3 ping statistics ---
1 packets transmitted, 1 packets received, 0% packet loss
round-trip min/avg/max = 0.083/0.083/0.083 ms
```

Now let's disconnect `container_3` from the `bridge` network.

    docker network disconnect bridge container_3

Running `docker exec -it container_3 ifconfig` will show only one `eth` interface now instead of two.

#### Recap - Bridge Network

- In a bridge network, containers have access to 2 interfaces:
    - Loopback (`lo`)
    - Private interface connected to the bridge network of the host (`eth0`)
        - This is used to connect to the outside network
- All containers in the same bridge network can communicate with eachother
- Containers from different bridge networks can't communicate with eachother
- Reduces the level of network isolation in favor of better outside connectivity
- Most suitable where you want to set up a relatively small network on a single host

### 29. Host Network and Overlay Network

#### Host Network

- The least protected network model. Adds a container on the host's network stack.
- Containers on the host stack have full access to the host's interface.
- Called an "Open Container".

To create open containers, you use `--net host`:

    docker run -d --name container_4 --net host busybox sleep 100000

And net info:

    docker exec -it container_4 ifconfig

Results in you seeeing all network interfaces from the host machine:

```
vagrant@vagrant-virtualbox ~/G/henryfbp.github.io (master)> docker exec -it container_4 ifconfig
br-09d23bc21e33 Link encap:Ethernet  HWaddr 02:42:DF:FC:78:40  
          inet addr:172.18.0.1  Bcast:172.18.255.255  Mask:255.255.0.0
          inet6 addr: fe80::42:dfff:fefc:7840/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:32 errors:0 dropped:0 overruns:0 frame:0
          TX packets:13 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:2410 (2.3 KiB)  TX bytes:1206 (1.1 KiB)

docker0   Link encap:Ethernet  HWaddr 02:42:5A:24:9D:D0  
          inet addr:172.17.0.1  Bcast:172.17.255.255  Mask:255.255.0.0
          inet6 addr: fe80::42:5aff:fe24:9dd0/64 Scope:Link
          UP BROADCAST MULTICAST  MTU:1500  Metric:1
          RX packets:9 errors:0 dropped:0 overruns:0 frame:0
          TX packets:11 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:310 (310.0 B)  TX bytes:1122 (1.0 KiB)

eth0      Link encap:Ethernet  HWaddr 08:00:27:6B:C9:D9  
          inet addr:10.0.2.15  Bcast:10.0.2.255  Mask:255.255.255.0
          inet6 addr: fe80::248:e902:5932:8fac/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:273702 errors:0 dropped:0 overruns:0 frame:0
          TX packets:70138 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:397121663 (378.7 MiB)  TX bytes:5173643 (4.9 MiB)

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:3813 errors:0 dropped:0 overruns:0 frame:0
          TX packets:3813 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:12294647 (11.7 MiB)  TX bytes:12294647 (11.7 MiB)

veth4017061 Link encap:Ethernet  HWaddr 9A:A8:54:D0:87:64  
          inet6 addr: fe80::98a8:54ff:fed0:8764/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:18 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:0 (0.0 B)  TX bytes:1436 (1.4 KiB)
```

- Minimum network security level
- No isolation on this type of container, so it leaves the container unprotected
- However, containers running in the host network stack see a higher level of performance than those traversing `docker0` bridge and iptables port mappings.

You should only use this network model if you have a valid reason.

#### Overlay Network

All the other network models, none, bridge, and host, can only be deployed on single machine.

- The overlay network supports multi-host networking.
- Overlay networks require pre-existing conditions before they can be created.
    - Running Docker engine in Swarm mode.
    - A key-value store like consul
- Most services are deployed across multiple hosts, so the overlay network is widely used in production.
- The latest docker swarm mode will create the overlay network automatically

### 30. D3: Text Lecture: Overlay Network

See <https://docs.docker.com/engine/userguide/networking/overlay-standalone-swarm/#create-a-swarm-cluster>

### 31. Define Container Networks with Docker Compose

By default, Docker Compose sets up a single network for services. Each container joins the default network and is reachable by other containers on that network.

In the repo, <https://github.com/henryfbp/dockerapp/>, run this:

    git checkout v0.4

Then run `docker-compose up -d`.

```
vagrant@vagrant-virtualbox ~/G/dockerapp ((v0.4))> docker-compose up -d
Creating network "dockerapp_default" with the default driver
Creating dockerapp_redis_1 ... done
Creating dockerapp_dockerapp_1 ... done
```

You can see `Creating network (...)`.

The prefix of the default network name comes from the current working directory.

And the default network type is 'bridge'. You can check with `docker network ls`.

When we run `docker-compose down`, the network gets removed.

You can modify the `docker-compose.yml` file to look like the one below:

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
    networks:
      - my_net

networks:
  my_net:
    driver: bridge
```

See `networks:` item both under `redis:` node, and in the root YAML object.

Next, save the file and run `docker-compose up -d`.

Note the new network.

```
vagrant@vagrant-virtualbox ~/G/dockerapp ((v0.4))> docker-compose up -d
Creating network "dockerapp_my_net" with driver "bridge"
Creating network "dockerapp_default" with the default driver
Creating dockerapp_redis_1 ... done
Creating dockerapp_dockerapp_1 ... done
```

There are even more complex network topologies that can be created. Example.

Here, two custom networks, front and back, are defined.

3 services, `proxy`, `app`, and `db`. The `db` service is on the `back` network. The `proxy` service is isolated from `db`. Only `app` can talk to both.

This provides network isolation between services. These are popular things to do on multi tiered applications.

```yaml
version: '2'

services:
  proxy:
    build: ./proxy
    networks:
      - front
  app:
    build: ./app
    networks:
      - front
      - back
  db:
    image: postgres
    networks:
      - back

networks:
  front: # use custom driver
    driver: custom-driver-1
  back: # use custom driver w/ opts
    driver: custom-driver-2
    driver_opts:
      foo: "1"
      bar: "2"

```

## Section 5: Create a Continuous Integration Pipeline

### 32. Write and Run Unit Tests inside Containers

#### Unit tests will...

- Test basic functionality of app code, ideally w/o reliance on external services
- Run as quickly as possible so that developers can iterate faster w/o waiting for test results

#### Run it already!

They're already written.

Clone <https://github.com/henryfbp/dockerapp/> and inside the `dockerapp/` folder, run `git checkout v0.5`.

See `/app/test.py`:

```py
import unittest
import app

class TestDockerapp(unittest.TestCase):

    def setUp(self):
        self.app = app.app.test_client()

    def test_save_value(self):
        response = self.app.post('/', data=dict(submit='save', key='2', cache_value='two'))
        assert response.status_code == 200
        assert b'2' in response.data
        assert b'two' in response.data

    def test_load_value(self):
        self.app.post('/', data=dict(submit='save', key='2', cache_value='two'))
        response = self.app.post('/', data=dict(submit='load', key='2'))
        assert response.status_code == 200
        assert b'2' in response.data
        assert b'two' in response.data

if __name__=='__main__':
    unittest.main()
```

This is a simple test file that will make sure the app can save/load a key value pair.

Run `docker-compose up -d --build` to rebuild all the containers.

Then, run `docker-compose run dockerapp python test.py` to run the test suite.

### 33. Introduction to Continuous Integration

- CI lets software engineers test isolated changes when they are added to a larger code base
- Goal of CI is to provide rapid feedback to immediately eliminate defects introduced into codebase

#### Typical CI Pipeline w/o Docker

1.  Dev checks out source code
2.  Dev makes changes, adds features, etc
3.  Dev checks in source code to central repo (commits)
4.  A CI server triggers a build that does:
    1.  Check out the latest version
    2.  Build the code
    3.  Run unit tests
    4.  Create a build artifact (package and archive, assign a build label)

#### CI process with Docker involved

The CI server builds the docker image after it builds the application.

The app goes inside the image, and the CI server pushes the image to a docker registry.

You can then pull the newly-built image from the registry.

We'll set up a CI workflow using GitHub as the central repo, and CircleCI as the CI server.

GitHub lets you host public repos for free, and CircleCI is a hosted CI provider that lets you run concurrent builds for free.

You need a GitHub and CircleCI account.

(next part of the lecture is just setting up SSH/GPG keys, use Google if you need to)

### 34. Text Direction: Introduction to Continuous Integration

URL of the Github account to fork:

https://github.com/jleetutorial/dockerapp

Checking for existing SSH keys:

https://help.github.com/articles/checking-for-existing-ssh-keys/

Generating a new SSH key and adding it to the ssh-agent:

https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/

Adding a new SSH key to your GitHub account:

https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/

### 35. Link CircleCI with Github Account for Setting up a CI Workflow

`.circleci/` folder is used to store the code that enables the CI workflow.

<https://github.com/HenryFBP/dockerapp/blob/branch-v0.6/.circleci/config.yml>

You can clone the repo and the run `git checkout v0.6`.

-   1. Version number.
-   2. Jobs key. Every config file needs a build job.
-   4. A working directory for the rest of the job.
-   5-6. Specifying the primary container images for this job, for docker to use.

    In CircleCI, all tests run under containers.

    What we want is a docker image that has Git, and we can get these by using `docker:17.05.0-ce-git` as an image name.
-   7. All of our steps to run in the job.
-   8. The `checkout` step will check out our code from Github to our working directory.
-   9. The `setup_remote_docker` step will create a separate docker image for each build, for security purposes.
-   10-14. Install `pip` and `docker-compose`.
-   15-19. Finally, bring up docker containers and run tests.

Now, link CircleCI with your Github account. You can do that here:

<https://app.circleci.com/>

Create a new project and use the existing `.circleci/` folder.

It will fail to log into docker hub. This is because the build is running against the `master` branch, which includes publishing to Docker Hub.

But we only want to run the `v0.6 branch`.

Next, we're going to make a new branch from `v0.6` and make some dummy changes to see what CircleCI does.

We expect to see a new build automatically run, and it should be green.

This will be green because branch `v0.6` does not contain the 'publish to Docker Hub' logic.

    git checkout v0.6

    git checkout -b test-ci

    touch dummy.txt

    git add dummy.txt

    git commit -m "dummy commit"

    git push

![](/images/2021-05-12-docker-crash-course/circleci-1.png)

### 36. Push Docker Images To DockerHub from CircleCI

Here, we publish a fully built image to Docker Hub if CI is green.

![](/images/2021-05-12-docker-crash-course/complete-ci.png)

Make a Docker Hub account.

Next, we'll link CircleCI with Docker Hub.

Log into CircleCI and click the build dashboard (upper left).

Then click the "dockerapp" repo.

Click "Project Settings".

Click "Environment Variables".

Set these env vars:

    DOCKER_HUB_USER_ID : your user id
    DOCKER_HUB_PWD : your password
    DOCKER_HUB_EMAIL : your email

Next step is to add instructions to CircleCI yaml file.

Make sure it's a forked repo under your Github account, so that you can push to your remote.

Run:

    git checkout v0.6

    git checkout -b circle_ci_publish

And add this key under `steps` to your .circleci/config.yml file:

We should also tag images with commit hashes and other relevant information.

Let's use these 2 tags:

1. Commit hash of the source code
2. Latest 

Note: The name is `dockerapp_dockerapp` because the first `dockerapp` is the name of the working directory, and the 2nd one is the service name we define in the `compose.yml` file.

Note2: $CIRCLE_SHA1 is the current git commit hash.

```yml
      - deploy:
          name: Publish application Docker image
          command: |
              docker login -e $DOCKER_HUB_EMAIL -u $DOCKER_HUB_USER_ID -p $DOCKER_HUB_PWD

              docker tag dockerapp_dockerapp $DOCKER_HUB_USER_ID/dockerapp:$CIRCLE_SHA1
              docker tag dockerapp_dockerapp $DOCKER_HUB_USER_ID/dockerapp:latest

              docker push $DOCKER_HUB_USER_ID/dockerapp:$CIRCLE_SHA1
              docker push $DOCKER_HUB_USER_ID/dockerapp:latest
```

Then commit the changes and push to the `circle_ci_publish` branch.

    git add .circleci/
    git commit -m 'push to docker hub'
    git push -u origin circle_ci_publish

Now view CircleCI.

<https://hub.docker.com/r/henryfbp/dockerapp/tags?page=1&ordering=last_updated>

### 37. Trouble Shooting: Push Docker Images to Docker Hub

If you are able to run docker login, but encountered the following unauthorized: authentication required error while running docker push  

    [root@terry ~]# docker login --username=1972
    Password: 
    Login Succeeded
    [root@terry ~]# 
    [root@terry ~]# docker push asiye_yigit_tutorial/debian:1.01
    The push refers to a repository [docker.io/asiye_yigit_tutorial/debian]
    29303f03b719: Preparing 
    77a77cd4826d: Preparing 
    fe4c16cbf7a4: Preparing 
    unauthorized: authentication require

Solution:

Creating the repository on Docker hub before running docker push.

Take a look at 

http://stackoverflow.com/questions/36663742/docker-unauthorized-authentication-required-upon-push-with-successful-login

## Section 6: Deploy Docker Containers in Production

### 38. Introduction to Running Docker Containers in Production

#### Opinions about Running Docker in Prod

- Many docker engineers are confident that a distributed web app can be deployed at scale using docker, and have incorporated docker into their production environment.
- However, there are still some people who are reluctant to use Docker in production as they think Docker workflow is too complex/unstable for real life use cases.

So...is Docker production-ready?

It's very disruptive (good thing) to software dev, ops, system architecture, testing, and compliance practices.

But Docker itself is very young.

#### Concerns about Running Docker in Prod

- There are still some missing pieces about Docker around data persistence, networking, security, and identity management
- The ecosystem of supporting Dockerized applications in production like tools/monitoring/logging are still not ready yet

#### Companies already running Docker in production

- Spotify
- Yelp
- Bedify(?)

#### Why run Docker Containers inside VMs?

The most popular way to run Docker Containers is currently inside virtual machines.

- Addresses security concerns
- The isolation Docker provides is not as robust as the segregation that Docker provides
- Without hardware level isolation, all containers share kernel resources.
    - If 1 container can monopolize resources, it can starve others in a DoS.

Most of the popular container service engines, like Google Container Engine and Amazon EC2 Container Service, still use VMs internally.

#### Docker Machine

The simplest way to provision new VMs and run containers on top of them is by using Docker Machine.

It can

- Provision new VMs
- Install Docker Engine
- Configure Docker client with remote docker machines

Local example:

We can provision a VM on our local machine using Docker Machine.

To do that, we use VirtualBox.

Docker Machine also provides drivers for DigitalOcean, Amazon AWS, Google App Ocean, etc; so that you can use Docker Machine to provision VMs in the cloud.

Running containers in the cloud is quite different from running containers on your local box.

### 39. Register Digital Ocean Account for Deploying Containerized Applications

Make a DO account.

Use a promo code from retailmenot if you want free codes.

Generate an API token, read-write.

### 40. Deploy Docker Application to the Cloud with Docker Machine

Deploy time!

If you don't have docker machine installed, google it and RTFM ;)

<https://docs.docker.com/machine/install-machine/>

Run

    docker-machine ls

To test it.

You can run

    docker-machine create --driver digitalocean --digitalocean-access-token=<TOKEN GOES HERE> dockerapp-machine

![](/images/2021-05-12-docker-crash-course/docker-machine-DO.png)

Console:

```
vagrant@vagrant-virtualbox ~/G/henryfbp.github.io (master)> docker-machine create --driver digitalocean --digitalocean-access-token=b8d4facaa8291b55aa5fae1f8901a7f8675ac34a04f11f11a35364501e50e8d2 dockerapp-machine

Running pre-create checks...
Creating machine...
(dockerapp-machine) Creating SSH key...
(dockerapp-machine) Creating Digital Ocean droplet...
(dockerapp-machine) Waiting for IP address to be assigned to the Droplet...
Waiting for machine to be running, this may take a few minutes...
Detecting operating system of created instance...
Waiting for SSH to be available...
Detecting the provisioner...
Provisioning with ubuntu(systemd)...
Installing Docker...
Copying certs to the local machine directory...
Copying certs to the remote machine...
Setting Docker configuration on the remote daemon...
```

Note: This is not working for me.

Note2: Started working after SSHing into the box in DigitalOcean and just running `docker`.

Now `docker-machine ls` gives this.

```
vagrant@vagrant-virtualbox ~/G/henryfbp.github.io (master)> docker-machine ls
NAME                ACTIVE   DRIVER         STATE     URL                        SWARM   DOCKER     ERRORS
dockerapp-machine   -        digitalocean   Running   tcp://104.131.47.74:2376           v20.10.7   
```

And we can run `docker-machine env dockerapp-machine` to get the commands that we want to run on our remote machine.

```
vagrant@vagrant-virtualbox ~/G/henryfbp.github.io (master)> docker-machine env dockerapp-machine
set -gx DOCKER_TLS_VERIFY "1";
set -gx DOCKER_HOST "tcp://104.131.47.74:2376";
set -gx DOCKER_CERT_PATH "/home/vagrant/.docker/machine/machines/dockerapp-machine";
set -gx DOCKER_MACHINE_NAME "dockerapp-machine";
# Run this command to configure your shell: 
# eval (docker-machine env dockerapp-machine)
```

So we run `eval (docker-machine env dockerapp-machine)` (if using `fish` shell).

Now let's run `docker info` to see info about system-wide VMs.

```
$ docker info 
(...)
 Operating System: Ubuntu 16.04.7 LTS
 OSType: linux
 Architecture: x86_64
 CPUs: 1
 Total Memory: 992.1MiB
(...)
```

We're going to now make a copy of `docker-compose.yml` into `prod.yml`.

This should be done in a fork of this repo: <https://github.com/henryfbp/dockerapp/>

    cp docker-compose.yml prod.yml

And in `prod.yml`, we change it to this:

(see <https://hub.docker.com/r/henryfbp/dockerapp>)

```yml

version: "3.0"
services:
  dockerapp:
    image: henryfbp/dockerapp
    ports:
      - "5000:5000"
    depends_on:
      - redis
  redis:
    image: redis:3.2.0
```

This file is available in `master` branch (`git checkout master`).

Now we can run:

    docker-compose -f prod.yml up -d

This deploys all services defined in `prod.yml` to the remote VM.

Assuming we ran `eval (docker-machine env dockerapp-machine)` in the previous step, we can now run `docker-machine ls` to see the status.

You need to run 

    eval (docker-machine env dockerapp-machine)

BEFORE

    docker-compose -f prod.yml up -d

because it sets up required environment variables for deployment that happens when you run `docker-compose ...`.

```
vagrant@vagrant-virtualbox ~/G/dockerapp (master)> docker-machine ls
NAME                ACTIVE   DRIVER         STATE     URL                        SWARM   DOCKER     ERRORS
dockerapp-machine   *        digitalocean   Running   tcp://104.131.47.74:2376           v20.10.7   
```

And visit <http://104.131.47.74:5000>.

![](/images/2021-05-12-docker-crash-course/docker-machine-deployed-ip.png)

See <https://docs.docker.com/machine/drivers/digital-ocean/> for more info on options for DigitalOcean as a specific docker-machine driver.

### 41. Text Direction: Deploy Docker Application to the Cloud with Docker Machine

TODO

### 42. Introduction to Docker Swarm and Set up Swarm Cluster

### 43. Deploy Docker App Services to the Cloud via Docker Swarm

### 44. Extra learning Material: Dockers Monitoring Tools

## Section 7: Additional Learning Materials

### 45. What is new in Docker 17.06

### 46. Docker's Native support for Kubernetes

### 47. Future Learning

### 48. Text Lecture: Future Learning

### 49. Coupons to Our Other Courses