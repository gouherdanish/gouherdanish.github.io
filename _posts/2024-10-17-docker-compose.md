---
layout: post
title: "Handling multi-container apps with Docker Compose"
date: 2024-10-17

tags: ["Software Development"]
---

Docker compose provides a declarative syntax which helps manage deployments efficiently

---
### Background

- Previously, in [this](https://gouherdanish.github.io/2024/10/10/mongodb.html) article, we saw how database persistance helps us capture user-related data which improves overall user experience
- However, the process that we followed was very manual. We had to setup each container using terminal one-by-one which is not a production-ready workflow practice

---

### Introducing Docker Compose

- Instead of launching different containers one by one, we can write a Docker Compose file which provides the details of every containerized service at one place

```
# docker-compose.yaml

version: '3'
services:
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
