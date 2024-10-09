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

### Concepts

**Base Image**

- Every Dockerfile must have a base image which we must specify at the start of a Dockerfile 
- We can use `FROM` directive to specify base image as shown below

```
FROM continuumio/miniconda3:4.11.0
```

Note: 
- Most of the images are based on some version of Linux base image either Debian, Alpine or Ubuntu.
- Although _creating a Base image_ is never required for standard development, we can create one if we need to 
    - Using `FROM scratch` directive which directs the docker engine to build fresh image and not pull from Dockerhub
    - Using `docker import` on the tarball of required File System libraries and binaries

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
- It creates the destination folder if it does not exist

```
COPY requirements.txt ./

COPY data data/

COPY src src/
```

- This will create following folder structure inside the container

```
>>> ls /home/app
data/
src/
requirements.txt
```


```
>>> ls /home/app/src

factory/
segmentation/
app.py
main.py
```

Note:
- We need to be careful while using COPY command as it does not copy the source folder itself like the `mv` command in Linux
- Rather it copies all the contents inside the source folder one by one into the destination folder

```
COPY src .
```

- E.g. Using COPY command as above will just copy all of the contents in the current directory which might not be as expected sometimes

```
>>> ls /home/app
factory/
segmentation/
app.py
main.py
requirements.txt
```

**Install Packages inside container**

_Install one package_
- We can use RUN command followed by `conda install` command which tells Docker to install that package inside the container using conda
- Runtime environment variables can be set using ENV command

```
RUN conda install geopandas==0.14.2

ENV PROJ_LIB=/opt/conda/share/proj
```

_Installing packes from file_
- We can specify `--file` tag followed by the `requirements.txt` file to install relevant packages

```
RUN conda install -c conda-forge --file requirements.txt
```

**Run app**

_Expose port_

- We can use EXPOSE command to expose a port for our app inside the container 

```
EXPOSE 8501
```

Note:
- The EXPOSE instruction inside a Dockerfile does not make the port accessible from outside the container by default. 
- It simply informs Docker that the application inside the container will listen on a specified port.

_Run app inside container_

- We use CMD to denote the entrypoint Linux command
- Always given at the end of a Dockerfile

```
CMD ["streamlit","run","main.py","--server.address=0.0.0.0","--server.port=8501"]
```

Note:
- We could also use RUN to execute the command but there is a difference
- There can be multiple RUN commands but just one CMD command which marks the entrypoint to the application

---
### Create Dockerfile

- Previously, in [this](https://gouherdanish.github.io/2024/09/25/low-lying-areas-mapping.html) blog, we have created a simple app to locate low-lying areas in Bangalore
- Before we deploy this app, we have to first create a Docker image. 
- Using the concepts shown above, let's create a Dockerfile which can be used to build image.

```
# Dockerfile
FROM continuumio/miniconda3:4.11.0

SHELL ["/bin/bash", "-c"]

RUN mkdir -p /home/app

WORKDIR /home/app

COPY data data/

COPY src src/

COPY requirements.txt ./

RUN conda install geopandas==0.14.2

ENV PROJ_LIB=/opt/conda/share/proj

RUN conda install -c conda-forge --file requirements.txt

WORKDIR /home/app/src

EXPOSE 8501

CMD ["streamlit","run","main.py","--server.address=0.0.0.0","--server.port=8501"]
```

---
### Build Image

- we can use `docker build` to create docker image
- `-t` flag is used to provide tag in the format image-name:image-tag
- `.` signifies docker engine to use the Dockerfile placed in the current directory

```
>>> docker build -t urban-flooding:1.0 .
REPOSITORY                 TAG       IMAGE ID       CREATED          SIZE
urban-flooding             1.0       408b7d43c322   11 seconds ago   2.53GB
```

---

### Create container

- we can use `docker run` command to run the image by creating a container
- `-p` flag is used to do port binding
    - Here, We have mapped container port 8501 to the host port 8003
    - Http requests coming on host port 8003 will be port forwarded to container port 8501 and will be served
- `-d` can be used to run the container in detached mode
- `--name` flag is used to provide the name of the container

```
>>> docker run -p 8003:8501 --name floodapp urban-flooding:1.0
Collecting usage statistics. To deactivate, set browser.gatherUsageStats to false.


  You can now view your Streamlit app in your browser.

  URL: http://0.0.0.0:8003
```

---

### Access the app from host

- we can use `curl` do a health check of the app
```
>>> curl -I http://0.0.0.0:8003
HTTP/1.1 200 OK
Server: TornadoServer/6.1
Content-Type: text/html
Date: Wed, 09 Oct 2024 17:48:25 GMT
Accept-Ranges: bytes
Etag: "2c06f1bb433d252a1eb544504956def4a269feb16611ea654d8c1a75495c830ea0b3d387e5cd3ddc0016819f4db0fc85474a0465d138dd293b9d952e5b947140"
Last-Modified: Tue, 01 Oct 2024 21:34:20 GMT
Cache-Control: no-cache
Content-Length: 891
Vary: Accept-Encoding
```

- If we paste the url in the browser, we can see the app is working fine
<img src="{{site.url}}/images/low_lying_areas/landing_page.png"/>

---

### Conclusion

- In this blog, we dockerized our existing streamlit app and deployed inside a container

