---
layout: post
title: "Docker Concepts and Usage"
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

#### NIC

- Network Interface Card (NIC) is a hardware component that enables a computer or device to connect to a network
- Refer [this](https://gouherdanish.github.io/2025/05/12/nic.html) for more info

<img src="{{site.url}}/images/networking/nic.png">

#### Switch

- It is a hardware device that connects devices within the LAN 
- It forwards data packets _between two devices_
- It forwards data to the correct inteneded destination 
    - It keeps track of the destination MAC address in a MAC address table
- In contrast, a Hub broadcasts data packets to all connected devices, regardless of the destination
- Operates at Layer 2 (Data Link) of the OSI model

<img src="{{site.url}}/images/networking/switch.png">

#### Router

- It is a network device that connects local area networks (LANs) to the internet and other networks
- It is also referred to as a Default Gateway sometimes
- It forwards data packets _between different networks_
- It uses routing tables, which contain information about the different networks they are connected to and the best paths to reach them
- Operate at Layer 3 of the OSI model (the network layer), making decisions based on IP addresses, which are the primary identifier for network communication
- It includes additional settings like Firewall for enahanced security
- Routers assign IPs dynamically via DHCP, which can change over time as DHCP lease expires

<img src="{{site.url}}/images/networking/router.png">

#### DHCP

- It stands for Dynamic Host Configuration Protocol
- It is a network protocol that automatically assigns IP addresses and other network configuration parameters to devices on a network

_DHCP Server_

- A server on the network that manages the IP address pool and assigns addresses to clients.
- It is situated inside the router

_DHCP Client_

- A device (like a computer, phone, or router) that requests an IP address from the DHCP server.

_DHCP Lease_

- The time period for which a device is assigned a particular IP address.

_Process_

- When a client is not connected to a network, it will not have any IP address
    - Since IP address is a logical addressing to identify a device in a network
    - If the device is not connected to the network at all, then there is no point of having an IP
- When a client connects to the network, it broadcasts a DHCP request to find a DHCP server. 
- The server responds with a DHCP offer, which includes an IP address, subnet mask, and other configuration parameters. - The client then sends a DHCP request to accept the offer, and the server grants the lease

_Benefits_
- Automated and centralized IP Address Management
- IP Address Reuse


#### Firewall

- A firewall protects your network by blocking or allowing traffic (incoming or outgoing) based on rules you set
- It can be hardware or software
- It acts as a gatekeeper, deciding which packets can enter or leave your network
- Ex: Block all traffic from the internet to internal IPs except port 80 (HTTP) and 443 (HTTPS).
- Firewall is a generic concept which can be setup on any network device 
    - On a Linux system, we can use iptables to ACCEPT/DROP traffic to the machine
    - On a Windows system, we can use Windows Defender GUI and setup inbound Allow TCP:80 rule
    - On a Homw router, we can login to its Web UI and add Allow TCP:80 rule in the Firewall

#### DMZ

- It stands for Demilitarized Zone
- It is a special network zone that exposes one device directly to the internet, bypassing the router’s firewall protections
- DMZs are used for hosting servers that need public access and a DMZ host exposes all ports of the designated host
- Often used for gaming consoles, surveillance systems, or test servers
- The device in DMZ is vulnerable to internet attacks and must be secured manually
- Best Practices
    - Use an internal API layer or proxy, not direct DB connection
    - Only open specific ports like 80 or 443
- Alternatively, we can use port forwarding

#### Port Forwarding

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


