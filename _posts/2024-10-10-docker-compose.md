---
layout: post
title: "Docker: Handling multi-container apps with Docker Compose"
date: 2024-10-10
tags: ["Software Development"]
---

Docker compose provides a declarative syntax which helps manage deployments efficiently

---
### Background

- Previously, in [this](https://gouherdanish.github.io/2024/09/25/low-lying-areas-mapping.html) blog, we created a simple app to locate low-lying areas in Bangalore
- Also, in [this](https://gouherdanish.github.io/2024/10/07/dockerfile.html) article, we created a Dockerfile for the same and built its image and ran it locally as a container

### New Requirements
- Let's say, we get new requirements to add couple of features into our app
    - Keep a log for the search history in our app so that we know how many times each villages are searched by users
    - Keep a log for the last searched village in our app so that the user should have to search for his village if the page is refreshed
---

### Database Setup
- The obvious solution to this is that we need to use a database
- We can use MongoDB for this since
    - Most users will be searching for their villages and thus DB writes will be more. MongoDB is optimized for write-heavy needs
    - MongoDB is easy to use with its Mongo Express UI
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

**Step 2 - Run Containers**
- Start MongoDB and MongoUI containers using `docker run` command
- Make sure to run both in same network so that the container can communicate natively

```
docker run -d \                                                                     
-p 27017:27017 \ 
-e MONGO_INITDB_ROOT_USERNAME=admin \
-e MONGO_INITDB_ROOT_PASSWORD=password \
--network mongo-network \
--name mongodb \
mongo

docker run -d \                                                                     
-p 8081:8081 \
--name mongoui \
--network mongo-network \
-e ME_CONFIG_MONGODB_SERVER=mongodb \
-e ME_CONFIG_MONGODB_ADMINUSERNAME=admin \
-e ME_CONFIG_MONGODB_ADMINPASSWORD=password \
mongo-express
```

**Step 3 - Check containers**
- Both containers should be running

```
docker ps
CONTAINER ID   IMAGE             COMMAND                  CREATED          STATUS          PORTS                      NAMES
2325792727d9   mongo-express     "/sbin/tini -- /dock…"   2 days ago       Up 2 days       0.0.0.0:8081->8081/tcp     mongoui
e29bf436c061   mongo             "docker-entrypoint.s…"   2 days ago       Up 2 days       0.0.0.0:27017->27017/tcp   mongodb
```

**Step 4 - Check Mongo UI**
- Open 0.0.0.0:8081 on browser for Mongo Express UI

<img src="{{site.url}}/images/mongoui_0.png">

**Step 5 - Create Document and Collection on MongoUI**

<img src="{{site.url}}/images/mongoui.png">

<img src="{{site.url}}/images/mongoui_1.png">

---

### 

