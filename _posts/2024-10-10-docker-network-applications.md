---
layout: post
title: "Docker: Handling multi-containered apps with Docker Network"
date: 2024-10-10
tags: ["Software Development"]
---

Docker compose provides a declarative syntax which helps manage deployments efficiently

---
### Background

- Previously, in [this](https://gouherdanish.github.io/2024/09/25/low-lying-areas-mapping.html) blog, we created a simple app to locate low-lying areas in Bangalore
- Also, in [this](https://gouherdanish.github.io/2024/10/07/dockerfile.html) article, we created a Dockerfile for the same and built its image and ran it locally as a container

---
### New Requirements
- Let's say, we get new requirements to add couple of features into our app
    - Keep a log for the search history in our app so that we know how many times each villages are searched by users
    - Keep a log for the last searched village in our app so that the user should have to search for his village if the page is refreshed

---
### Database to the Rescue
- The obvious solution to this is to use a database
- We can use MongoDB for this since most users will be searching for their villages and thus DB writes will be more. MongoDB is optimized for write-heavy needs
- Also, MongoDB is easy to use with its Mongo Express UI

---
### Database Setup
- MongoDB can be setup using Docker very easily

**Step 1 - Pull images**
- Pull `mongo` and `mongo-express` images from Dockerhub

```
docker pull mongo
docker pull mongo-express
```

**Step 2 - Create new Docker Network**

```
docker network create mongo-network
```

**Step 3 - Run Containers**
- Start MongoDB container using `docker run` command

```
docker run -d \                                                                     
-p 27017:27017 \ 
-e MONGO_INITDB_ROOT_USERNAME=admin \
-e MONGO_INITDB_ROOT_PASSWORD=password \
--network mongo-network \
--name mongodb \
mongo
```

- Next, start Mongo Express container
```
docker run -d \                                                                     
-p 8081:8081 \
--name mongoui \
--network mongo-network \
-e ME_CONFIG_MONGODB_SERVER=mongodb \
-e ME_CONFIG_MONGODB_ADMINUSERNAME=admin \
-e ME_CONFIG_MONGODB_ADMINPASSWORD=password \
mongo-express
```

Note
- Make sure to run both in same network so that the container can communicate natively

**Step 4 - Check containers**
- Both containers should be running

```
docker ps
CONTAINER ID   IMAGE             COMMAND                  CREATED          STATUS          PORTS                      NAMES
2325792727d9   mongo-express     "/sbin/tini -- /dock…"   2 days ago       Up 2 days       0.0.0.0:8081->8081/tcp     mongoui
e29bf436c061   mongo             "docker-entrypoint.s…"   2 days ago       Up 2 days       0.0.0.0:27017->27017/tcp   mongodb
```

**Step 5 - Check Mongo UI**
- Open 0.0.0.0:8081 on browser for Mongo Express UI

<img src="{{site.url}}/images/docker/mongoui_0.png">

**Step 6 - Create Document and Collection on MongoUI**

<img src="{{site.url}}/images/docker/mongoui.png">

<img src="{{site.url}}/images/docker/mongoui_1.png">

---

### Implemention
- After the Database has been setup, now it is time to implement the new features in our app
- We can implement the above requirements using MongoDB Crud Operations
- The implementation logic is given below

**Logic**
- Find the village whose last searched flag is True and use that for display instead of default village
    - At start, when database has no entries available, select default village having index 0
- Upsert the current village into the collection by incrementing its search counter by 1 and setting its last searched flag to True
- Update the last searched flag to False for all other villages in the collection 

For detailed implementation, please refer
[Urban Flooding App](https://github.com/gouherdanish/urban_flooding)

---

### Testing

- The docker image needs to be rebuilt since we have implemented new features in our app now
- We can follow below steps to launch new container 

**Stop Container**
- stop running container
- remove the container
- remove the image

```
docker stop flood-ctnr
docker rm flood-ctnr
docker rmi flood-image:1.0
```

**Build Image**
- build docker image and tag appropriately

```
docker build -t flood-image:1.0 .
```

**Launch Container**
- launch a docker container for our app with appropriate port mapping
- If we launch the container in the same network as mongodb

```
docker run -d -p 8003:8501 --network mongo-network --name flood-ctnr flood-image:1.0
```

