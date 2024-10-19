---
layout: post
title: "Working with Public and Private Container Registry"
date: 2024-10-19

tags: ["Software Development"]
---

A container registry is a repository or collection of repositories used to store and access container images

---

### Background

- Any app can utilize images stored across public and private container registries
  - Previously in [this](https://gouherdanish.github.io/2024/10/12/mongodb.html) article, we used `mongo` and `mongo-express` images from Dockerhub which is a public registry
  - Also, in [this](https://gouherdanish.github.io/2024/10/18/container-registry.html) article, we used Github Container Registry (GHCR) to store our own created docker images
- If we need to run the app, we need to pull these images from these registries and launch these as containers in the same Docker network which will enable our app to communicate seamlessly across these container
- As shown in We can definitely do this one by one on terminal, but that won't be a production workflow practice. So, we need a way to do all this in one shot

--- 

### Introducing Docker Compose

- In [this](https://gouherdanish.github.io/2024/10/16/docker-compose.html) article, we introduced Docker Compose and saw how it helps us launch multiple containers in same network when we created a docker compose yaml file which pulled MongoDB and Mongo Express images from DockerHub public registry and then launched these as containers in one go. 
- Similarly, we can also pull image from Private Registry as well and use it as we need
- Here is the architecture diagram for this setup

<img src="{{site.url}}/images/ghcr/arch1.png">

- In this article, we will see how we will explore how we can work with Public and Private registry in the same Docker Compose yaml file so that our app has everything it needs in one place and can be launched in one shot

---
### Modifying Yaml file

- As seen previously, we already have defined two services `mongodb` and `mongoui` in the Docker Compose yaml file which are pulled from Dockerhub
- Below, we add one more service `flood-app` whose image will be pulled from Private Registry GHCR.
- We define appropriate port mapping to enable the services to communicate with each other

```
# app.yaml

version: '3'
services:
  flood-app:
    image: ghcr.io/gouherdanish/flood-image:1.0   <-- For Private Registry images, we need to enter full address
    ports: 
      - 8501:8501
  mongodb:
    image: mongo                                  <-- Dockerhub images can be referred by image names as well 
    ports:
      - 27017:27017
    environment:
      - MONGO_INITDB_ROOT_USERNAME=admin
      - MONGO_INITDB_ROOT_PASSWORD=password
  mongoui:
    image: mongo-express
    ports:
      - 8081:8081
    environment:
      - ME_CONFIG_MONGODB_ADMINUSERNAME=admin
      - ME_CONFIG_MONGODB_ADMINPASSWORD=password
      - ME_CONFIG_MONGODB_SERVER=mongodb
```

- If we look at Github packages, we can see that our image has been published.
<img src="{{site.url}}/images/ghcr/push.png">

---
### Deploying the App

- We use docker-compose utility to deploy our app along with the Mongo database server and Mongo Express UI in one shot
- This creates all the containers in same network as shown below which ensures seamless communication between the services

```
>>> docker-compose -f app.yaml up -d
[+] Running 4/4
 ✔ Network urban_flooding_default        Created
 ✔ Container urban_flooding-flood-app-1  Started
 ✔ Container urban_flooding-mongodb-1    Started
 ✔ Container urban_flooding-mongoui-1    Started
```

---
### Checking our App

- Our app is hosted locally on port 8501
- If we paste the url http://0.0.0.0:8501 in the browser, we can see the app is working fine
<img src="{{site.url}}/images/mongo/start.png">

---
### Stopping the App

**Stop the App Only**
- If we just need to stop the app and not the database servers, we can stop the individual container the app is running in.

```
>>> docker ps
CONTAINER ID   IMAGE                                  COMMAND                  CREATED          STATUS          PORTS                      NAMES
24f9c21177a8   mongo-express                          "/sbin/tini -- /dock…"   58 seconds ago   Up 58 seconds   0.0.0.0:8081->8081/tcp     urban_flooding-mongoui-1
901a8d0c8f42   mongo                                  "docker-entrypoint.s…"   58 seconds ago   Up 58 seconds   0.0.0.0:27017->27017/tcp   urban_flooding-mongodb-1
adce32e96e52   ghcr.io/gouherdanish/flood-image:1.0   "streamlit run main.…"   58 seconds ago   Up 58 seconds   0.0.0.0:8501->8501/tcp     urban_flooding-flood-app-1

>>> docker stop adce32e96e52
>>> docker rm adce32e96e52
```

**Stop the full setup**
- If we need to stop the app and all associated containers, we can down the full setup as follows

```
>>> docker-compose -f app.yaml down
[+] Running 4/3
 ✔ Container urban_flooding-mongodb-1    Removed
 ✔ Container urban_flooding-mongoui-1    Removed
 ✔ Container urban_flooding-flood-app-1  Removed
 ✔ Network urban_flooding_default        Removed
```

Note:
- Whenever we restart the database container, all the data stored in the database so far will be gone since the container does not have data persistance

---
### Conclusion

- We explored working with Public and Private container registry
- We used Docker compose to deploy our app very easily