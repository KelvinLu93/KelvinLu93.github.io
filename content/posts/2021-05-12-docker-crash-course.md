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

TODO

## Section 2: Working with Docker Images
TODO

## Section 3: Create Containerized Web Applications
TODO

## Section 4: Docker Networking
TODO

## Section 5: Create a Continuous Integration Pipeline
TODO

## Section 6: Deploy Docker Containers in Production
TODO

## Section 7: Additional Learning Materials
TODO
