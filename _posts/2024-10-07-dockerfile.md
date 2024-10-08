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

```
FROM continuumio/miniconda3:4.11.0
```

- Every Dockerfile must have a base image on which it is based. 
- We must specify base image at the start of a Dockerfile as shown above
- 
