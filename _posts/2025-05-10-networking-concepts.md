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
- It acts as the bridge between the computer and the physical network medium (ethernet cable or wifi)
- Two Types
    - Wired NICs - Use Ethernet cables and convert digital data into electrical signals for transmission
    - Wireless NICs - Utilize Wi-Fi and convert data into radio waves for transmission
- Two Kinds
    - Full-duplex NICs can send and receive data simultaneously
    - Half-duplex NICs can either send or receive at one time
- 