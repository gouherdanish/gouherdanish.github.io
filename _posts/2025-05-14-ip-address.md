---
layout: post
title: "Docker Concepts and Usage"
date: 2025-05-10
tags: ["Networking"]
---

Devices must have an IP to communicate with each other in a network

---
### What is IP Address

- IP address is a unique identifier assigned to each device connected to a network that uses the Internet Protocol
- It’s how devices find and communicate with each other over the internet or a local network (LAN)
- It is a logical addressing of a device in the network (like your street address)
    - it is decided by the router (logically) to locate a device in the network (just like your mail come to your address)

---
### Can the IP address of a device change ? If yes, in what cases ?

- IP address of a device can change if the device moves to a new network
- It can also change in the same network if it is disconnected and reconnected
    - usually it won't change if reconnected in few minutes

### When is an IP address assigned 

- If the device is not connected to the network at all, then there is no point of having an IP
ifconfig | grep "inet "

---
### How is IP address assigned - DHCP

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
- When a client connects to the network, it broadcasts a DHCP request to find a DHCP server. 
- The server responds with a DHCP offer, which includes an IP address, subnet mask, and other configuration parameters. - The client then sends a DHCP request to accept the offer, and the server grants the lease

_Benefits_
- Automated and centralized IP Address Management
- IP Address Reuse
