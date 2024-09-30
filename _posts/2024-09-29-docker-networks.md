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

#### 1. Bridge Network
- After a fresh Docker installation, a default bridge network comes up and running
- A bridge network acts as a software bridge that connects multiple containers together and provides isolation from other networks
- Containers running in the same bridge network can connect to each other using IP addresses or container names

Note:
- Containers are created in bridge network by default

#### 2. Host Network
- This mode gives the container full access to the host’s network stack
- It can give performance improvements when container has lot of ports
- it can also introduce potential security risks

#### 3. Overlay Network
- This mode enables containers running on different hosts to communicate with each other as if they were on the same network
- Overlay networks use a distributed control plane to manage network routing
- Containers across different hosts but connected to overlay network can communicate


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
