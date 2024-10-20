---
layout: post
title: "Persisting Data with Docker Volumes"
date: 2024-10-20

tags: ["Software Development"]
---

Docker Volumes enable us to persist data which can be used whenever container restarts

---

### 

- MongoDB Container saves Data in Virtual Filesystem
- There is no persistence inside a container and all the data saved inside will go away if the container is restarted
- This might be a problem if we need our application to be stateful

---
### Understanding Virtual FileSystem

**List docker containers**
``` 
>>> docker ps
CONTAINER ID   IMAGE                                  COMMAND                  CREATED        STATUS        PORTS                      NAMES
24f9c21177a8   mongo-express                          "/sbin/tini -- /dock…"   22 hours ago   Up 22 hours   0.0.0.0:8081->8081/tcp     urban_flooding-mongoui-1
901a8d0c8f42   mongo                                  "docker-entrypoint.s…"   22 hours ago   Up 22 hours   0.0.0.0:27017->27017/tcp   urban_flooding-mongodb-1
adce32e96e52   ghcr.io/gouherdanish/flood-image:1.0   "streamlit run main.…"   22 hours ago   Up 22 hours   0.0.0.0:8501->8501/tcp     urban_flooding-flood-app-1
```

**Go inside MongoDB container**
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
```
root@901a8d0c8f42:/# ls /data/db   
WiredTiger         collection-0-16809421161523113700.wt  index-1-16809421161523113700.wt  index-9-16909179650290339279.wt
WiredTiger.lock    collection-0-16909179650290339279.wt  index-1-16909179650290339279.wt  journal
WiredTiger.turtle  collection-2-16909179650290339279.wt  index-3-16909179650290339279.wt  mongod.lock
WiredTiger.wt      collection-4-16909179650290339279.wt  index-5-16909179650290339279.wt  sizeStorer.wt
WiredTigerHS.wt    collection-7-16909179650290339279.wt  index-6-16909179650290339279.wt  storage.bson
_mdb_catalog.wt    diagnostic.data                       index-8-16909179650290339279.wt
```

---

### 
