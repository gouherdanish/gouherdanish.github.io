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
- It forwards data packets _between different networks_
- It uses routing tables, which contain information about the different networks they are connected to and the best paths to reach them
- Operate at Layer 3 of the OSI model (the network layer), making decisions based on IP addresses, which are the primary identifier for network communication
- In includes additional settings like Firewall for enahanced security

<img src="{{site.url}}/images/networking/router.png">

#### Firewall

- A firewall protects your network by blocking or allowing traffic (incoming or outgoing) based on rules you set
- It acts as a gatekeeper, deciding which packets can enter or leave your network
- Ex: Block all traffic from the internet to internal IPs except port 80 (HTTP) and 443 (HTTPS).
- Firewall is a generic concept which can be setup on any network device 
    - On a Linux system, we can use iptables to ACCEPT/DROP traffic to the machine
    - On a Windows system, we can use Windows Defender GUI and setup inbound Allow TCP:80 rule
    - On a Homw router, we can login to its Web UI and add Allow TCP:80 rule in the Firewall

#### DMZ

- It stands for Demilitarized Zone
- It is a special network zone that exposes one device directly to the internet, bypassing the router’s firewall protections
- Often used for gaming consoles, surveillance systems, or test servers
- The device in DMZ is vulnerable to internet attacks and must be secured manually