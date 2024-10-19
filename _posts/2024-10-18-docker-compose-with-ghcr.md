---
layout: post
title: "Deploying App using Docker Compose and Container Registry"
date: 2024-10-18

tags: ["Software Development"]
---

A container registry is a repository—or collection of repositories—used to store and access container images

---

### Background of existing App

- Previously, in [this](https://gouherdanish.github.io/2024/10/16/docker-compose.html) article, we created a deployment yaml which pulled MongoDB and Mongo Express images from DockerHub and then launched these as containers in one go. 
- Also, in [this](https://gouherdanish.github.io/2024/10/18/container-registry.html) article, we used Github Container Registry (GHCR) to store our own created docker images
- In order to enable our app to communicate with

--- 

### Introducing Container Registry

- 

---
### Modifying Deployment file


```
version: '3'
services:
  flood-app:
    image: ghcr.io/gouherdanish/flood-image:1.0
    ports: 
      - 8501:8501
  mongodb:
    image: mongo
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

### Conclusion

- We introduced Container Registry and saw how it can be used to store docker images in a production setting
- Specifically, we set up Github Container Registry and pushed a docker image there