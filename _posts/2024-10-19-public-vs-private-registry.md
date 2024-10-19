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
- We can definitely do this one by one on terminal, but that won't be a production workflow practice. So, we need a way to do all this in one shot

--- 

### Introducing Docker Compose

- In [this](https://gouherdanish.github.io/2024/10/16/docker-compose.html) article, we introduced Docker Compose and saw how it helps us launch multiple containers in same network when we created a docker compose yaml file which pulled MongoDB and Mongo Express images from DockerHub public registry and then launched these as containers in one go. 
- In this article, we will see how we can include working with Private registry in the same Docker Compose yaml file so that our app has everything it needs in one place and can be launched in one shot

---
### Modifying Deployment file

- 

```
version: '3'
services:
  flood-app:
    image: ghcr.io/gouherdanish/flood-image:1.0  <-- Takes this image from Private Registry
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