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
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>

What's next?
  Try Docker Debug for seamless, persistent debugging tools in any container or image → docker debug nginx1
  Learn more at https://docs.docker.com/go/debug-cli/
```

- Typing http://0.0.0.0:8081 in browser opens the following page in the browser
<img src="{{site.url}}/images/docker/nginx.png"/>

**Inspect Container**
- This gives a nested json in the following format
- Note the containers utilize the _default bridge network_
- Also, note the `IPAddress` on the container1 - 172.17.0.2
- Similarly note the `IPAddress` on the container2 - 172.17.0.3

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
                "IPAddress": "172.17.0.2",
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

**Communication between Containers - Using IP Address**
- Since the containers are in the same bridge network, they can communicate among themselves using IP addresses.
- Below connect to container 1 from inside container 2

```
% docker exec -it nginx2 curl http://172.17.0.3:80

...
<!DOCTYPE html>
<h1>Welcome to nginx!</h1>
...
```

- Similarly we can connect to container 2 from inside container 1

```
% docker exec -it nginx1 curl http://172.17.0.3:80
...
<!DOCTYPE html>
<h1>Welcome to nginx!</h1>
...
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

### Demo2 - User-Defined Bridge Networks
- User-defined bridge network support automatic service discovery

**Create a user-defined Bridge Network**

```
% docker network create mynet --driver bridge
mynet
```

- We can see that the `mynet` has been created
```
% docker ps
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
