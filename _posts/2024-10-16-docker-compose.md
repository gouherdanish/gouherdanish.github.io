---
layout: post
title: "Handling multi-containered deployments with Docker Compose"
date: 2024-10-16

tags: ["Software Development"]
---

Docker compose provides a declarative syntax which helps manage deployments efficiently

---
### Background of existing App

- Previously, in [this](https://gouherdanish.github.io/2024/10/12/mongodb.html) article, we saw how database persistance helps us capture user-related data which improves overall user experience
- However, the process that we followed was very manual. We had to setup each container using terminal one-by-one which is not a production-ready workflow practice

---

### Introducing Docker Compose

- Instead of launching different containers one by one, we can write a Docker Compose file which provides the details of every containerized service at one place
- Moreover we do not need to create a docker network explicitly since Docker Compose creates it for us and launches all the services in the same network

---

### Creating Docker Compose File

- We can use any text editor to create a docker compose yaml file
- We just need to make sure of the proper arguments and indentation
- Below we deploy MongoDB and Mongo Express UI as containers using yaml file

```
# mongo.yaml

version: '3'                                  <-- version of docker compose (deprecated now)
services:
  mongodb:                                    <-- container name
    image: mongo                              <-- image name (from Dockerhub)
    ports:
      - 27017:27017                           <-- port binding (host:container)
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

---

### Launching Docker Containers

- Using docker compose file, it is very to launch all the docker containers in one go
- Below we run the containers in detached mode which will not print the detailed logs

```
>>> docker-compose -f mongo.yaml up -d
[+] Running 2/2
 ✔ Container urban_flooding-mongoui-1  Started                                                                                      
 ✔ Container urban_flooding-mongodb-1  Started 
```

- We can confirm that containers are created properly by listing them

```
>>> docker ps
CONTAINER ID   IMAGE           COMMAND                  CREATED         STATUS         PORTS                      NAMES
caa3d986c3aa   mongo           "docker-entrypoint.s…"   2 minutes ago   Up 2 minutes   0.0.0.0:27017->27017/tcp   urban_flooding-mongodb-1
5016a8c16f5f   mongo-express   "/sbin/tini -- /dock…"   2 minutes ago   Up 2 minutes   0.0.0.0:8081->8081/tcp     urban_flooding-mongoui-1
```

- If we want to see the logs, we can look inside each container logs itself as shown below

```
docker logs 
...
mongodb-1  | {"t":{"$date":"2024-10-18T00:48:32.776+00:00"},"s":"I",  "c":"NETWORK",  "id":6788700, "ctx":"conn2","msg":"Received first command on ingress connection since session start or auth handshake","attr":{"elapsedMillis":4}}
mongoui-1  | Mongo Express server listening at http://0.0.0.0:8081
...
```

- If we go over to http://0.0.0.0:8081 in our browser, we will see something like this

<img src="{{site.url}}/images/mongo/start.png">

Note:
- Mongo has restarted and previous data has gone away
- This happens because MongoDB was running inside a container which does not have any disk storage
- We will use Docker Volumes to save data in a separate blog

---
### Stopping Docker Containers
- We can stop docker containers very easily as well using docker compose in one go
- Note that it removes the network that it created as well which makes it very clean

```
>>> docker-compose -f mongo.yaml down

[+] Running 3/3
 ✔ Container urban_flooding-mongoui-1  Removed
 ✔ Container urban_flooding-mongodb-1  Removed
 ✔ Network urban_flooding_default      Removed
```

---

### Conclusion

- We introduced Docker Compose and saw how it helps with multi-containered app deployment
- Docker Compose makes handling multiple containers very easily with a declarative syntax in one place