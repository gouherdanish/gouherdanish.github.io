---
layout: post
title: "Docker Concepts and Usage"
date: 2024-09-28
tags: ["Software Development"]
---

_"But it works on my machine" - SDE_

---
### Introduction

- Docker is a way to package application with all the necessary dependencies and configuration. (Isolation)
- The packaged artifact can be shared among different teams and it will work seamlessly (Portability)
- This makes development and deployment process more efficient

---
### Background

**Before Docker**
- Before Docker came, the software development process was tedious and inefficient
- Let's say developer1 developed a service with necessary dependencies installed in his local machine.
- If he wants to hand over the code to other developer/tester, the other developer needs to install the dependencies again which can have many bottlenecks
    - The dependencies versions may not match
    - Different developers may have different OS for which the installation process will be different.
    - There are multiple steps during installation and there might be chances of errors
- During delpoyment, the developer used to hand over the code to Devops team with instructions on how to setup the service directly on the server.
    - This can also have dependency version conflicts
    - Multiple back-on-forth can happen due to misinterpretation of instructions

**After Docker**
- Now, developers can build a docker image and push to a container repository and share the location to other team
- Other developers can just pull the image from the repository and run on their machine without any issues

---
### Can we run docker images on any OS ?
- Short answer is No
- Docker images don't have their own OS and therefore run on host OS.
    - Mac OS is powered by Unix Kernel
    - Windows is powered by Windows Kernel
- However, Docker Engine is built on Linux Kernel, so docker images can't directly run on OS other than Linux.
    - We can use Docker Desktop in Mac and Windows to escape this
    - Docker creates its own Linux VM on top of Mac/Windows OS

### Docker vs VM
Virtualization
- Docker virtualizes just the application layer of the OS 
- VM virtualizes both the kernel and application layer (complete virtualization of OS)

Boot Time / Speed
- Starting/stopping a container is very easy and fast
- Starting VM takes some time as they need to load the entire OS to start

Size / Performance
- Docker images are very lightweight (MBs) so we can run many containers on a laptop
- VMs are more resource-intensive and heavywieght (GBs) so we can't run many VMs on one laptop

Compatibility
- Docker containers are limited to Linux only
- VMs of any OS can run on any host OS using virtualization softwares e.g. Parallels, VMWare Fusion


---
### Image vs Container

- A Docker image is the package that we create of the code and its dependencies
    - Images can be created afresh from a Dockerfile using `docker build ...`
    - Images can also be pulled from public/private repository (dockerhub) using `docker pull ...`
- When we run a docker image, then it creates a container environment
    - Image is just the artifact lying around, it's not running
    - Container is when we run the image on a machine using `docker run ...`

---
### Docker Commands

**List images**

```
docker images

REPOSITORY                 TAG       IMAGE ID       CREATED         SIZE
redis                      latest    345f80b79535   2 months ago    140MB
input-validations          latest    97b2280e4635   2 months ago    2.28GB
<none>                     <none>    57eeb062e00f   3 months ago    2.28GB
docker/welcome-to-docker   latest    648f93a1ba7d   10 months ago   19MB
```

**List Containers**

```
docker ps -a

CONTAINER ID   IMAGE                             COMMAND                  CREATED          STATUS                      PORTS                    NAMES
f7e74675aef3   redis                             "docker-entrypoint.s…"   12 minutes ago   Up 12 minutes               0.0.0.0:6000->6379/tcp   stoic_almeida
a61ac128421c   redis                             "docker-entrypoint.s…"   29 minutes ago   Exited (0) 13 minutes ago                            romantic_brahmagupta

```

**Pull an image**

```
docker pull redis:latest
docker pull postgres:9.6
docker pull postgres:10.10
```

Note - We can run multiple version of the image on the machine without any conflicts because both the running in their own isolated environments

**Build an image**

```
cd folder-containing-Dockerfile
docker build -t my_image:1.0 .
```

**Run Image**
- Runs the image and forms a container with container id and a name
- port binding is necessary to allow connecting to container from host machine

```
docker run redis                            # run an image to form container
docker run -d --name redis_latest redis     # run in detached mode and name the container
docker run -p 6000:6379 -d redis            # bind host port 6000 to container port 6379       
```

**Stop Container**

```
docker stop <container-id>
```

**Start Container**

```
docker start <container-id>
```

**Deleting Container**

```
docker rm <container-id>
```

**Deleting Image**

```
docker rmi <image-id>
```

**Going inside a docker container**

```
docker exec -it <container-id> /bin/bash    # goes inside the container and opens interactive shell
```

---
### Summary

- Docker is a containerization platform that uses OS-level virtualization to package software applications and their dependencies into reusable units called containers
- Docker containers can be run on any host with Docker or an equivalent container runtime installed, whether locally on your laptop or in a remote cloud.

---
### Conclusion

- We introduced Docker and saw how it helps us navigate software development and deployment phases safely and efficiently.
- We also went through some useful commands to interact with docker engine.

