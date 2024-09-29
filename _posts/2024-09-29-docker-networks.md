---
layout: post
title: "Docker: Networking in Containers"
date: 2024-09-29
tags: ["Software Development"]
---

Docker includes a networking system for managing communications between containers, your Docker host, and the outside world

---
### Introduction

- Docker offers versatile networking options for different use cases
- Docker networks configure communications between neighboring containers and external services.
- Containers must be connected to a Docker network to receive any network connectivity

---
### Network Types

#### 1. Bridge Network (Default)


---

### Demo

**List networks**
```
docker network ls

NETWORK ID     NAME      DRIVER    SCOPE
059a6cf12dba   bridge    bridge    local
ef3b5b8c6741   host      host      local
67e0dc7301a8   none      null      local
```

**Create a Bridge Network**

```
docker network create mynet --driver bridge

mynet
```
