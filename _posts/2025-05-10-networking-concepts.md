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

<img src="{{site.url}}/images/networking/nic.png">

- Two Types
    - Wired NICs 
        - Uses Ethernet cables and 
        - Convert digital data into electrical signals for transmission and vice versa
    - Wireless NICs 
        - Utilize Wi-Fi and convert data into radio waves for transmission
- Two Kinds
    - Full-duplex NICs can send and receive data simultaneously
    - Half-duplex NICs can either send or receive at one time
- Modulation/Demodulation
    - Modulation is the process of encoding information into a transmitted signal
        - converting digital signal into analog signal is called Modulation
        - Amplitude Modulation (AM), Frequency Modulation (FM)
        - e.g. In radio braodcasting, audio signals are modulated onto a radio frequency carrier wave
    - Demodulation is the process of extracting information from the transmitted signal
        - converting analog signal into digital signal is called Demodulation
        - e.g. In radio reception, a receiver demodulates the radio frequency carrier wave to extract the original audio signal
