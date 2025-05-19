---
layout: post
title: "Networking Concepts"
date: 2025-05-10
tags: ["Networking"]
---

Computer networking is about connecting two or more computers together

---
### Introduction

- Computer Networking involves both hardware and software components
    - Cables, switches, routers (Hardware)
    - OS, network protocols (Software)
- Networking allows us to share same resource across multiple devices
    - Same printing machine can be used by multiple people in office
    - Same NAS can be accessed by multiple devices in the same network
- Networking allows two devices to communicate with each other
    - share files, emails etc

---
### Types of Networks

_Local Area Networks (LANs)_: Connect computers within a small geographic area, like a home or office. 
_Wide Area Networks (WANs)_: Connect networks across a larger geographic area, like the internet. 
_Personal Area Networks (PANs)_: Connect devices around a single person, like a smartphone and a laptop

---
### Components of LAN

<img src="{{site.url}}/images/networking/lan.png">

**NIC**

- Network Interface Card (NIC) is a hardware component that enables a computer or device to connect to a network
- Refer [this](https://gouherdanish.github.io/2025/05/12/nic.html) for more info

<img src="{{site.url}}/images/networking/nic.png">

**Switch**

- It is a hardware device that connects devices within the LAN 
- It forwards data packets _between two devices_
- It forwards data to the correct inteneded destination 
    - It keeps track of the destination MAC address in a MAC address table
- In contrast, a Hub broadcasts data packets to all connected devices, regardless of the destination
- Operates at Layer 2 (Data Link) of the OSI model

<img src="{{site.url}}/images/networking/switch.png">

**Router**

- It is a network device that connects local area networks (LANs) to the internet and other networks
- It is also referred to as a Default Gateway sometimes
- It forwards data packets _between different networks_
- It uses routing tables, which contain information about the different networks they are connected to and the best paths to reach them
- Operate at Layer 3 of the OSI model (the network layer), making decisions based on IP addresses, which are the primary identifier for network communication
- It includes additional settings like Firewall for enahanced security
- Routers assign IPs dynamically via DHCP, which can change over time as DHCP lease expires

<img src="{{site.url}}/images/networking/router.png">

**Firewall**

- A firewall protects your network by blocking or allowing traffic (incoming or outgoing) based on rules you set
- It can be hardware or software
- It acts as a gatekeeper, deciding which packets can enter or leave your network
- Ex: Block all traffic from the internet to internal IPs except port 80 (HTTP) and 443 (HTTPS).
- Firewall is a generic concept which can be setup on any network device 
    - On a Linux system, we can use iptables to ACCEPT/DROP traffic to the machine
    - On a Windows system, we can use Windows Defender GUI and setup inbound Allow TCP:80 rule
    - On a Homw router, we can login to its Web UI and add Allow TCP:80 rule in the Firewall

**DMZ**

- It stands for Demilitarized Zone
- It is a special network zone that exposes one device directly to the internet, bypassing the router’s firewall protections
- DMZs are used for hosting servers that need public access and a DMZ host exposes all ports of the designated host
- Often used for gaming consoles, surveillance systems, or test servers
- The device in DMZ is vulnerable to internet attacks and must be secured manually
- Best Practices
    - Use an internal API layer or proxy, not direct DB connection
    - Only open specific ports like 80 or 443
- Alternatively, we can use port forwarding

**Port Forwarding**

- Port forwarding tells the router to redirect specific types of incoming traffic to a specific internal device on your network
- Ex: Forward port 22 from the internet to 192.168.1.100:22 to enable remote SSH login
    - Here internet traffic comes at Router's port 22
    - It is forwarded to port 22 of the internal server located at 192.168.1.100 (Private IP known to Router)
- Usually, port forwarding rule is setup in Router Web UI -> Advanced, NAT, or Firewall Settings
- Following things are required
    - External Port (e.g. 80) - where the internet sends traffic to
    - Internal IP (e.g. 192.168.1.100) - your device or webserver private IP
    - Internal Port (e.g. 80) - where the webserver listens to
    - Protocol (TCP or UDP) - depending on the application being served

**Internal vs External Port**

In a Home LAN,
- External port is at the router's end is exposed to the internet and the traffic reaches at this port
- Internal port is at each of the internal devices.
- Using the port forwarding rule, the router forwards the internet traffic it receives to a specific device (internal IP) and the exposed port on that device

---

### Some Important Concepts

**Classful IP Addressing**

- IP addresses are globally managed by Internet Assigned Numbers Authority (IANA) and Regional Internet Registries (RIR)
- The 32-bit IPv4 address is divided into five classes - A, B, C, D and E
- Class A - 10.0.0.0/8 (first 8 bits is the network ID)
- Class B - 172.16.0.0/16 (first 16 bits are the network part)
- Class C - 192.168.1.0/24 (first 24 bits are the network part)

<img src="{{site.url}}/images/networking/classful_addressing.png">

**CIDR**

- CIDR = Classless Inter-Domain Routing 
- This notation is a concise way to represent an IP address and its associated network mask
- Example: 
    - 192.168.1.0/24
    - This means that 192.168.1 is the network part while the last 8-bit is the host part
    - IP range = 192.168.1.1 - 192.168.1.254 
    - 2 IPs are reserved
        - 192.168.1.0 - For router/gateway
        - 192.168.1.255 - For Broadcast


**Subnet**

- short form for Sub-network
- it is a smaller network logically segmented from a larger IP network (VPC)
- In IP terms, a subnet is defined by an IP address + a subnet mask
- Example:
    - VPC CIDR = 10.0.0.0/16
    - Subnet 1 is given by 10.0.1.0/24 (Public)
    - Subnet 2 is given by 10.0.2.0/24 (Private)
- In AWS, a subnet is a CIDR block within the VPC range

**Subnet Mask**

- The subnet mask defines how many bits of the IP address represent the network part and how many represent hosts.
- Example:
    - IP = 192.168.0.1
    - Subnet Mask = 255.255.255.0 means that first 24 bits of the IP address are part of network

**DNS**

- stands for Domain Name System
- translates human-friendly domain names into IP addresses so the device knows where to send the request on the internet
- without DNS, you'd need to remember and enter IPs for every website.
- works at Layer 7
- DNS servers are distributed globally and hosted by ISPs, Google (8.8.8.8), Cloudflare(1.1.1.1)
    - Request goes to these servers and gets resolved to an IP from where the requested data is relayed back to the device

`google.com -> 142.250.64.78`

**NAT**

- stands for Network Address Translation
- Maps private IP addresses to a public IP address for internet communication
- It allows multiple devices in a home network to share one internet-facing public IP address
- This way you don't need public IPs for each individual device since IPv4 addresses are limited
- works at Layer 3
- NAT happens at the router or NAT Gateway

`Device IP: 192.168.1.5 → Router (NAT) → Public IP: 203.0.113.20`

**ARP**

- stands for Address Resolution Protocol
- Maps an IP address to a MAC address in a local network
- necessary to deliver data on Ethernet or Wi-Fi, which uses MAC addresses 
- works at Layer 2
- It is a broad-cast based protocol
    - your device broadcasts an ARP request and the concerned device responds with its MAC
- There is no server involved here, the data is cached in sender's ARP cache

`Who has IP 192.168.1.1? → I do! My MAC is 00:1A:2B:3C:4D:5E`

**Forward Proxy**

- Also called client-side proxy
- The forward proxy lives on the client side, between your device and the internet
- Used for access control for outbound traffic (Workplace restrictions, parental control)
- Masks client device IP address (Maintains anonymity)
- Ex - VPN, Tor
- On home wifi (without forward proxy), request goes straight to your router 
    - Client → Router → ISP → Google
- On office wifi (with forward proxy), all HTTP/HTTPs request are forwarded to the configured proxy first
    - this way outbound requests are filtered
    - Client → **Proxy** → Router → ISP → Google

<img src="{{site.url}}/images/networking/forward_proxy.png">

**Reverse Proxy**

- Also called server-side proxy
- Accepts requests from the internet, then forwards them to one of several backend servers
    - The client doesn't know which server handled the request (Maintains anonymity)
- Reverse proxy acts as an intermediary between clients and servers
- Reverse proxy is able to inspect headers in the HTTP request and route based on 
    - hostname or domain
    - URL path
    - Content (e.g. image requests sent to separate server)
    - Geolocation (nearest to client)
- Used for 
    - load balancing
    - SSL Termination
    - security and firewalling
    - path-based routing

<img src="{{site.url}}/images/networking/reverse_proxy.png">

_Is Network Load Balancer a Reverse Proxy ?_

**No**, 
- NLB is just a load balancer, it's not a reverse proxy
- A reverse proxy does application-aware traffic routing e.g. ALB, Nginx

**Encoding vs Encryption**

Both are used to transform the input data. However there is one difference

_Usecase_
    - Encoding converts data for compatibility or representation e.g. Binary encoding, Base-62 encoding
    - Encryption secures data by making it unreadable to unauthorized parties

_Reversible_
    - Encoding is easily reversible while Encrytion is only reversible if you have the secret key that encypted it

---


