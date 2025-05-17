---
layout: post
title: "IP Address"
date: 2025-05-14
tags: ["Networking"]
---

Devices must have an IP to communicate with each other in a network

---
### What is IP Address

- IP address is a unique identifier assigned to each device connected to a network that uses the Internet Protocol
- It’s how devices find and communicate with each other over the internet or a local network (LAN)
- It is a logical addressing of a device in the network (like your street address)
- IP address is decided by the router (logically) to locate a device in the network (just like your mail come to your address)

---
### How is IP address assigned 

- When a client is not connected to a network, it will not have any IP address
    - as seen below, there is just the localhost IP available, not the router-assigned one

```
$ ifconfig | grep "inet "
inet 127.0.0.1 netmask 0xff000000 
```

- When a client connects to the network, the router assigns an IP from its pool of available IPs

```
$ ifconfig | grep "inet "
inet 127.0.0.1 netmask 0xff000000 
inet 192.168.0.104 netmask 0xffffff00 broadcast 192.168.0.255
```

- Internally, while connecting to wifi, the client broadcasts a DHCP request to find a DHCP server
- The server responds with a DHCP offer, which includes an IP address, subnet mask, and other configuration parameters
- The client then sends a DHCP request to accept the offer, and the server grants the lease
- This lease is for 24hr after which the IP address expires and returned back to the DHCP IP pool 
- The devices remember their last assigned IP and while reconnecting they ask the DHCP if they can reuse the last one if available

---
### Can the IP address of a device change ? If yes, in what cases ?

Yes, IP can change in following cases
- IP address of a device can change if the device moves to a new network 
- It can also change in the same network in the following cases
    - if the DHCP lease of the ip expired (24 hr)
    - if the device is restarted or disconnected and reconnected
        - usually it won't change if reconnected in few minutes
    - if another device takes your old IP before you reconnect.
    - if the router reboots or restarts or resets its lease tables

---

### What is DHCP

- DHCP stands for Dynamic Host Configuration Protocol
- It is a network protocol that automatically assigns IP addresses and other network configuration parameters to devices on a network

_DHCP Server_

- A server on the network that manages the IP address pool and assigns addresses to clients.
- It is situated inside the router

_DHCP Client_

- A device (like a computer, phone, or router) that requests an IP address from the DHCP server.

_DHCP Lease_

- The time period for which a device is assigned a particular IP address.

_DHCP Reservation_

- It is also know as static IP address assignment
- It allows you to reserve a specific IP address from a DHCP server for a device based on its MAC address
- This ensures the device consistently receives the same IP address whenever it connects to the network
- Ex: Printers, servers, VoIP Devices, Routers, Switches etc have fixed IPs

_Benefits_
- Automated and centralized IP Address Management
- IP Address Reuse

---
### IP Address vs MAC Address

_logical / physical_
- IP Address is a logical addressing of a device in the network (like your street address)
- MAC Address is physical addressing of device 

_assigned by_
- IP address is assinged by Router DHCP (software)
- MAC address is assigned by Hardware and is burned into NIC by manufacturer

_changes?_
- IP address can change dynamically
- MAC address can't change usually

_layer_
- IP address is used at Layer 3 (Network Layer)
- MAC address is used at Layer 2 (Data Link Layer)

_Format_
- IP address is 32-bit (IPv4) or 128-bit (IPv6)
- MAC address is 48-bit hexadecimal 

_Example_
- IP = 192.168.0.1
- MAC = 00:1A:3B:4C:5D:6E

---
### IPv4 vs IPv6

- IPv4 is 32-bit while IPv6 is 128-bit
- Ex:
    - IPv4 = 192.168.0.1
    - IPv6 = 2001:0db8:0000:0000:0000:0000:0042:8329
- In IPv4 each group is 8 bit integer, while in IPv6 each group is 16 bit hexadecimal (each digit is a 4-bit hexadecimal)

---
### Public vs Private IP

- Public IP is used to identify a machine on the public internet
- Private IP is used to identify a machine over the private network