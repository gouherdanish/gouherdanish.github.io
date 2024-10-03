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
- It can give performance improvements since there is no Network Address Translation (NAT) involved. 
- It can help also when container has lot of ports
- It can also introduce potential security risks
- It is only supported on Linux

#### 3. Overlay Network
- This mode enables containers running on different hosts to communicate with each other as if they were on the same network
- Overlay networks use a distributed control plane to manage network routing
- Containers across different hosts but connected to overlay network can communicate


---

### Important commands

**List networks**

```
docker network ls
```

**Create new network**

```
docker network create mynet -d bridge
```

**Inspect a network**

```
docker network inspect mynet
[
    {
        "Name": "mynet",
        "Id": "ed35c394a57c97248efddb7bdedbb3b6b6a9876b03bc4e4df6e43f3c49f389e3",
        "Created": "2024-09-29T17:27:16.222426512Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.19.0.0/16",
                    "Gateway": "172.19.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "0cb1aa688fa3e3709a803ccce6432f7451310e78876060b5705acb0ff844440a": {
                "Name": "nginx1",
                "EndpointID": "493db165368f169eb12401bc982a13f46b5ac596466eefa5a65fdd028e1c91f5",
                "MacAddress": "02:42:ac:13:00:02",
                "IPv4Address": "172.19.0.2/16",
                "IPv6Address": ""
            },
            "54f6603fdd17de678089c9d25bc4428f7b9ce674d7aa2c6157f00afd1fe6b450": {
                "Name": "nginx2",
                "EndpointID": "423608656ddf11ec0a2038b6b5a0b1fb45e84eea2f8b838ce36ab1539d3987e3",
                "MacAddress": "02:42:ac:13:00:03",
                "IPv4Address": "172.19.0.3/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {}
    }
]
```

---

### Demo1 - Using Default Bridge Networks

**Create two containers from `nginx` image**

```
docker run -itd --name nginx1 -p 8081:80 nginx
39568968b1612f70a2370bd06f6f11590ad505ad3e392ea7b90e0b738aeb3247

docker run -itd --name nginx2 -p 8082:80 nginx
e77b9f9dfd501ee74a834ccded2dbe515a4d7371d7ab95d27cda18af9b2b30a6
```

**List Containers**
```
% docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                  NAMES
e77b9f9dfd50   nginx     "/docker-entrypoint.…"   5 seconds ago    Up 4 seconds    0.0.0.0:8082->80/tcp   nginx2
d00b9e56ccbd   nginx     "/docker-entrypoint.…"   9 minutes ago    Up 9 minutes    0.0.0.0:8080->80/tcp   nginx1
```

**Communication between Host and Container**
- We can communicate to the containers from our local machine

```
% curl http://0.0.0.0:8081
HTTP/1.1 200 OK
Server: nginx/1.27.1
Date: Tue, 01 Oct 2024 05:14:30 GMT
Content-Type: text/html
Content-Length: 615
Last-Modified: Mon, 12 Aug 2024 14:21:01 GMT
Connection: keep-alive
ETag: "66ba1a4d-267"
Accept-Ranges: bytes
```

- Typing http://0.0.0.0:8081 in browser opens the following page in the browser
<img src="{{site.url}}/images/docker/nginx.png"/>

**Inspect Container**
- This gives a nested json in the following format
- Note the containers utilize the default network _bridge_
- Note the key `DNSNames` is null
    - this is the reason why default bridge network does not support automatic service discovery through DNS
```
% docker inspect nginx1
    ...
        ...
        "Networks": {
            "bridge": {
                "IPAMConfig": null,
                "Links": null,
                "Aliases": null,
                "MacAddress": "02:42:ac:11:00:02",
                "NetworkID": "059a6cf12dbae364b16b2d314a3cd8ec030ad406455c7a73ca7d833b405ef8a8",
                "EndpointID": "8f9483947445e38c52eeb0b86f911386dacb33b596955186ed34cd73bb09f690",
                "Gateway": "172.17.0.1",
                "IPAddress": "172.17.0.2", <<--
                "IPPrefixLen": 16,
                "IPv6Gateway": "",
                "GlobalIPv6Address": "",
                "GlobalIPv6PrefixLen": 0,
                "DriverOpts": null,
                "DNSNames": null
            }
        }
        ...
    ...
```

- Note the IP addresses of container 1 and 2 after inspecting them
    - container1 - 172.19.0.2
    - container2 - 172.19.0.3

**Communication between Containers - Using IP Address**
- Since the containers are in the same bridge network, they can communicate among themselves using IP addresses.
- Below connect to container 1 from inside container 2

```
% docker exec -it nginx2 curl -I http://172.17.0.3:80
HTTP/1.1 200 OK
Server: nginx/1.27.1
Date: Tue, 01 Oct 2024 05:14:30 GMT
Content-Type: text/html
Content-Length: 615
Last-Modified: Mon, 12 Aug 2024 14:21:01 GMT
Connection: keep-alive
ETag: "66ba1a4d-267"
Accept-Ranges: bytes
```

- Similarly we can connect to container 2 from inside container 1

```
% docker exec -it nginx1 curl http://172.17.0.3:80
HTTP/1.1 200 OK
Server: nginx/1.27.1
Date: Tue, 01 Oct 2024 05:14:30 GMT
Content-Type: text/html
Content-Length: 615
Last-Modified: Mon, 12 Aug 2024 14:21:01 GMT
Connection: keep-alive
ETag: "66ba1a4d-267"
Accept-Ranges: bytes
```

**Communication between Containers - Using DNS**
- Working with IP addresses can be tricky or incovenient so it is preferred that the containers should be able to communicate using DNS itself.
- However, the default bridge network doesn't support automatic service discovery

```
% docker exec -it nginx1 curl http://172.17.0.3:80
# FAILS
```

```
% docker exec -it nginx2 curl http://172.17.0.2:80
# FAILS
```

**Stop Containers**
```
docker rm nginx1 ngix2
```

---
### Demo2 - User-Defined Bridge Networks
- User-defined bridge network support automatic service discovery

**Create a user-defined Bridge Network**

```
% docker network create mynet --driver bridge
mynet
```

- We can see that the `mynet` has been created

```
% docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
059a6cf12dba   bridge    bridge    local
ef3b5b8c6741   host      host      local
ed35c394a57c   mynet     bridge    local <<--
67e0dc7301a8   none      null      local
```

**Create Containers with User-defined network**
- We can provide `--network` tag to assign the user-defined network while creating containers

```
docker run -itd --name nginx1 -p 8081:80 --network mynet nginx
0cb1aa688fa3e3709a803ccce6432f7451310e78876060b5705acb0ff844440a

docker run -itd --name nginx2 -p 8082:80 --network mynet nginx
%54f6603fdd17de678089c9d25bc4428f7b9ce674d7aa2c6157f00afd1fe6b450
```

**List Containers**

```
% docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                  NAMES
54f6603fdd17   nginx     "/docker-entrypoint.…"   4 seconds ago    Up 3 seconds    0.0.0.0:8082->80/tcp   nginx2
0cb1aa688fa3   nginx     "/docker-entrypoint.…"   12 seconds ago   Up 12 seconds   0.0.0.0:8081->80/tcp   nginx1
```

**Inspect Containers**
- Note the containers utilize the user-defined bridge _mynet_
- Note the key `DNSNames` is not null now and has `nginx1` tagged
    - this is the reason why user-defined bridge network can support automatic service discovery through DNS
    
```
% docker inspect nginx1
...
    ...
        "Networks": {
            "mynet": {
                "IPAMConfig": null,
                "Links": null,
                "Aliases": null,
                "MacAddress": "02:42:ac:13:00:02",
                "NetworkID": "ed35c394a57c97248efddb7bdedbb3b6b6a9876b03bc4e4df6e43f3c49f389e3",
                "EndpointID": "493db165368f169eb12401bc982a13f46b5ac596466eefa5a65fdd028e1c91f5",
                "Gateway": "172.19.0.1",
                "IPAddress": "172.19.0.2",
                "IPPrefixLen": 16,
                "IPv6Gateway": "",
                "GlobalIPv6Address": "",
                "GlobalIPv6PrefixLen": 0,
                "DriverOpts": null,
                "DNSNames": [
                    "nginx1",
                    "0cb1aa688fa3"
                ]
            }
            }
    ...
...

```

- Note the IP addresses of container 1 and 2 after inspecting them
    - container1 - 172.19.0.2
    - container2 - 172.19.0.3


**Communication between Containers - Using DNS**
- Since the user-defined network support DNS discovery, we can communicate between containers using DNS 
- We can connect to nginx1 container from inside nginx2 container

```
docker exec -it nginx2 curl nginx1:80 
HTTP/1.1 200 OK
Server: nginx/1.27.1
Date: Tue, 01 Oct 2024 05:14:30 GMT
Content-Type: text/html
Content-Length: 615
Last-Modified: Mon, 12 Aug 2024 14:21:01 GMT
Connection: keep-alive
ETag: "66ba1a4d-267"
Accept-Ranges: bytes
```

- Similarly, we can connect to nginx2 container from inside nginx2 container

```
docker exec -it nginx2 curl nginx1:80 
HTTP/1.1 200 OK
Server: nginx/1.27.1
Date: Tue, 01 Oct 2024 05:14:30 GMT
Content-Type: text/html
Content-Length: 615
Last-Modified: Mon, 12 Aug 2024 14:21:01 GMT
Connection: keep-alive
ETag: "66ba1a4d-267"
Accept-Ranges: bytes

```

### Demo3 - Host network works on Linux only

**Create Container in `host` network mode**
- We can use `--network host` flag to create container in host mode

```
% docker run -dit --name nginx_host --network host nginx
13746c4ceb52d4bccbbe615a78dcb46e4c98f9eec5c6ed39b705b13fe20c1c7d
```

**Communicate to container using host port**
- This fails because I am working on Mac
- Host mode is only supported on Linux

```
% curl -I 0.0.0.0:80
curl: (7) Failed to connect to 0.0.0.0 port 80 after 0 ms: Couldn't connect to server
```

---
### Conclusion
- Docker provides networking capabilities to containers
- Containers are created in default `bridge` network
- Containers in the same network can communicate using IP addresses
- User-defined bridge networks allow communication between containers using DNS as well
