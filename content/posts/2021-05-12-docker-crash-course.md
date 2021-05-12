---
layout: post
title: "Docker Crash Course Notes"
date: 2021-05-12
categories: [programming, docker, devops]
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