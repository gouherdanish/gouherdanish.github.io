---
layout: post
title: "Docker: Building your first Docker Image"
date: 2024-10-07
tags: ["Software Development"]
---

Containerization is crucial for software development and deployment

---
### Introduction

- Getting your app to work on someone else's machine is the key idea behind containerization
- Docker provides the flexibility to build image of our app and thereafter launch the app inside containers on any system.

---
### Create Dockerfile

- Previously, in [this](https://gouherdanish.github.io/2024/09/25/low-lying-areas-mapping.html) blog, we have created a simple app to locate low-lying areas in Bangalore
- In order to deploy this app in a container, we have to fist create a Docker image. 
- Let's first create a Dockerfile and then use it to build image.

```
# Dockerfile
FROM continuumio/miniconda3:4.11.0

SHELL ["/bin/bash", "-c"]

RUN conda install geopandas==0.14.2

ENV PROJ_LIB=/opt/conda/share/proj

RUN mkdir -p /home/app

WORKDIR /home/app

COPY requirements.txt ./

RUN conda install -c conda-forge --file requirements.txt

COPY data data/

COPY src src/

WORKDIR /home/app/src

EXPOSE 8501

CMD ["streamlit","run","main.py","--server.address=0.0.0.0","--server.port=8501"]
```

---
### Concepts

**Base Image**

- Every Dockerfile must have a base image which we must specify at the start of a Dockerfile 
- We can use FROM directive to specify base image as shown below

```
FROM continuumio/miniconda3:4.11.0
```

Note: Although _creating a Base image_ is never required for standard development, we still can create if we so need
- Using `FROM scratch` directive which directs the docker engine to build fresh image and not pull from Dockerhub
- Using `docker import` on the tarball of required File System libraries and binaries

**Install specific Libraries**

- We can use RUN command followed by `conda install` command which tells Docker to install the package inside container
- Runtime environment variables can be set using ENV command
```
RUN conda install geopandas==0.14.2

ENV PROJ_LIB=/opt/conda/share/proj
```

**Copy files from Host to container**

_Create new Directory_
- We can use RUN command followed by `mkdir` command to create a new directory inside container
- `-p` flag is used to create parent folder if not present

```
RUN mkdir -p /home/app
```

_Change working directory_
- We can use WORKDIR command for this as follows

```
WORKDIR /home/app
```

_Copy files from Host to container_

- We can't use RUN command here as RUN command is executed inside the container
- We must use COPY command in this case
```
COPY requirements.txt ./
```