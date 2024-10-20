---
layout: post
title: "Persisting Data with Docker Volumes"
date: 2024-10-20

tags: ["Software Development"]
---

Docker Volumes enable us to persist data which can be used whenever container restarts

---

### Background

- Previously, in [this](https://gouherdanish.github.io/2024/09/25/low-lying-areas-mapping.html) article, we created a Streamlit App  to locate low-lying areas in Bangalore 
- Later, in [this](https://gouherdanish.github.io/2024/10/12/mongodb.html) article, we enabled database interactions to save user search history data 
- Also, in [this](https://gouherdanish.github.io/2024/10/19/public-vs-private-registry.html) article, we enabled our App to pull images from Public and Private container registry and used Docker Compose to easily deploy our app in one go

---

### Issues in existing App

- Currently the app data is saved in a Virtual Filesystem inside the MongoDB Container
- Since, there is no persistence inside a container, we would lose all the saved data if the container is restarted
- This might be a problem if we need our application to be stateful

---
### Understanding Virtual FileSystem

- MongoDB Container saves Data in Virtual Filesystem which can be accessed as follows

**List docker containers**
``` 
>>> docker ps
CONTAINER ID   IMAGE                                  COMMAND                  CREATED        STATUS        PORTS                      NAMES
24f9c21177a8   mongo-express                          "/sbin/tini -- /dock…"   22 hours ago   Up 22 hours   0.0.0.0:8081->8081/tcp     urban_flooding-mongoui-1
901a8d0c8f42   mongo                                  "docker-entrypoint.s…"   22 hours ago   Up 22 hours   0.0.0.0:27017->27017/tcp   urban_flooding-mongodb-1
adce32e96e52   ghcr.io/gouherdanish/flood-image:1.0   "streamlit run main.…"   22 hours ago   Up 22 hours   0.0.0.0:8501->8501/tcp     urban_flooding-flood-app-1
```

**Go inside MongoDB container**
- Since the mongodb is running in the container `901a8d0c8f42`, we exec inside it as follows
```
>>> docker exec -it 901a8d0c8f42 bash
```

**Check Container FileSystem**
```
root@901a8d0c8f42:/# ls
bin   data  docker-entrypoint-initdb.d  home        lib    mnt  proc  run   srv  tmp  var
boot  dev   etc                         js-yaml.js  media  opt  root  sbin  sys  usr
```

**Check where MongoDB saves data**

- MongoDB saves data in `/data/db` directory inside the container
```
root@901a8d0c8f42:/# ls /data/db   
WiredTiger         collection-0-16809421161523113700.wt  index-1-16809421161523113700.wt  index-9-16909179650290339279.wt
WiredTiger.lock    collection-0-16909179650290339279.wt  index-1-16909179650290339279.wt  journal
WiredTiger.turtle  collection-2-16909179650290339279.wt  index-3-16909179650290339279.wt  mongod.lock
WiredTiger.wt      collection-4-16909179650290339279.wt  index-5-16909179650290339279.wt  sizeStorer.wt
WiredTigerHS.wt    collection-7-16909179650290339279.wt  index-6-16909179650290339279.wt  storage.bson
_mdb_catalog.wt    diagnostic.data                       index-8-16909179650290339279.wt
```

- Similarly other databases use different location to save data inside a container
  - MySQL saves data in `/var/lib/mysql` folder
  - PostgreSQL saves data in `/var/lib/posgresql/data` folder

---

### Understanding Data Persistence

**Check existing app**
- If we open `http://0.0.0.0:8501` in browser, we would see something like this

<img src="{{site.url}}/images/dkrv/existing_app.png">

- The data for this app is saved inside the MongoDB container in a virtual FileSystem as shown above 
- This data is rendered on Mongo Express UI as documents inside a NoSql database collection as follows

<img src="{{site.url}}/images/dkrv/existing_data.png">

- Each document is a dictionary of key-value pairs which represents each village searched on the app and its statistics
<img src="{{site.url}}/images/dkrv/existing_data_point.png">

**Restart the app along with the database**
- We can restart the app using docker-compose as follows

```
>>> docker-compose -f app.yaml down 
[+] Running 4/4
 ✔ Container urban_flooding-flood-app-1  Removed 
 ✔ Container urban_flooding-mongodb-1    Removed 
 ✔ Container urban_flooding-mongoui-1    Removed 
 ✔ Network urban_flooding_default        Removed 

>>> docker-compose -f app.yaml up -d
[+] Running 4/4
 ✔ Network urban_flooding_default        Created
 ✔ Container urban_flooding-mongoui-1    Started
 ✔ Container urban_flooding-flood-app-1  Started
 ✔ Container urban_flooding-mongodb-1    Started 
```

- If we open `http://0.0.0.0:8501` in browser again, we would see this

<img src="{{site.url}}/images/dkrv/restarted_app.png">

- Notice, how all the existing data history of the searched villages has gone away now.
- This lack of data persistence can pose issues in stateful applications

---
### Introducing Docker Volumes

- Docker provides capability to mount a persistence layer in the container 
- Essentially, we plug the physical storage on the host with the virtual filesystem inside the container
- This works in two ways
  - When the app writes new data locally in the container, it gets persisted in the physical storage of the host as well
  - Also when container gets restarted, it looks for previously persisted data in the host and syncs into the container

<img src="{{site.url}}/images/dkrv/docker_volume.png">

---
### Usecase for Docker Volumes

- Whenever we are working with Databases or stateful applications, their containers need to have persistence mechanism
- This keeps the data in sync between host and container and avoids losing the data on container restart

---
### Types of Docker Volumes

**Host Volumes**
- In this case, we provide the full path of the host directory where we want to mount the container volume data

<img src="{{site.url}}/images/dkrv/host_volume.png">

**Anonymous Volume**
- In this case, we don't provide any path for the host directory
- Docker chooses the directory based on the OS and creates volumes with anonymous hashes

<img src="{{site.url}}/images/dkrv/anonymous_volume.png">

**Named Volume**
- In this case, we just provide a folder name on the host
- The full path will be created by Docker based on the OS

<img src="{{site.url}}/images/dkrv/named_volume.png">

Note:
- Named Volumes are used in production workflows

---
### Docker Volume Location on Host
- Docker saves volumes in different location based on type of OS
- Specifically for Mac OS, Docker creates a Linux VM on top of Mac OS in the background and stores all the data there
- If we want to check the host volume mount, we need to enter the VM using `screen` and `tty` 
- Then, we can go inside `/var/lib/docker/volumes` as shown below

```
# ls /var/lib/docker/volumes/urban_flooding_mongo-data/_data/
WiredTiger         collection-0-16809421161523113700.wt  index-1-16809421161523113700.wt  index-9-16909179650290339279.wt
WiredTiger.lock    collection-0-16909179650290339279.wt  index-1-16909179650290339279.wt  journal
WiredTiger.turtle  collection-2-16909179650290339279.wt  index-3-16909179650290339279.wt  mongod.lock
WiredTiger.wt      collection-4-16909179650290339279.wt  index-5-16909179650290339279.wt  sizeStorer.wt
WiredTigerHS.wt    collection-7-16909179650290339279.wt  index-6-16909179650290339279.wt  storage.bson
_mdb_catalog.wt    diagnostic.data                       index-8-16909179650290339279.wt
```

---