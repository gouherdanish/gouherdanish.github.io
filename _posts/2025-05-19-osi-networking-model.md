---
layout: post
title: "OSI Networking"
date: 2025-05-19
tags: ["Networking"]
---

OSI stands for Open Systems Interconnection

---
## Introduction

- OSI is a conceptual framework that divides network communication into seven abstract layers
- It aims to facilitate communication between different systems, even if they have different hardware or software
- The layered structure makes it easier to troubleshoot network issues by isolating problems to specific layers
- The model is fundamental to protocol development, as each layer defines the protocols and services used at that level

---
## Layers

There are 7 layers in OSI model

### Layer 7 - Application Layer 

- This layer provides services to the end-user, such as web browsing, email, and file transfer
- involves layer 7 protocols for web applications e.g. 
    Browsers - Chrome, Firefox (HTTP/HTTPS)
    Email Clients - Outlook (SMTP)
    File Transfer -  FTP/SFTP

---
### Layer 6 - Presentation Layer

- Also knows as translation layer
- It ensures that data is presented to the application layer in a format that can be understood and used. 
- Key Functions
    - Data Translation - It translates data between different formats (e.g., ASCII to EBCDIC)
    - Data Encryption - It encrypts data to protect confidentiality (TLS Handshake)
    - Data Compression - It compresses data to reduce bandwidth usage (gzip)
    - Data Serialization - It converts complex data structures into a flat byte stream for transmission

---
### Layer 5 - Session Layer

- It manages sessions, like keeping a secure channel open
- It manages the flow of communication between devices, including handling interruptions, restarts, and re-establishing connections
- Key Functions
    - Establish Connections
    - Maintain Connections
    - Terminate Connections

---
### Layer 4 - Transport Layer (TCP)

- It manages end-to-end communication between applications, ensuring reliable data delivery
- Protocols
    - TCP - Transmission Control Protocol (for reliable, ordered delivery)
    - UDP - User Datagram Protocol (for faster, less-reliable delivery)
- Key Functions
    - Segmentation - It breaks down large messages to smaller segments "packets" for transmission
    - Flow Control - It controls the flow of data to prevent the receiver from being overwhelmed, especially with TCP
    - Error Control - It provides mechanisms for detecting and correcting errors during data transmission
    - Multiplexing - It allows multiple applications on a host to simultaneously send and receive data by assigning each connection a unique source and destination port
- Output
    - TCP Segment = [TCP Header] + [Encrypted Data]
    - TCP Header contains Source Port, Destination Port, Sequence Number, ACK Flags, Option etc.

<img src="{{site.url}}/images/networking/tcp-seg.png">

---
### Layer 3 - Network Layer (IP)

- It is responsible for routing and forwarding data packets across interconnected networks
- Protocols
    - IP - Internet Protocol
    - ICMP - Internet Control Message Protocol
- Key Functions
    - Logical Addressing - It assigns logical addresses (IP) to devices, so they can be identified across networks
    - Routing - It determines the optimal path for data packets to travel through the network
    - Packet - It encapsulates data into packets and then forwards these packets to the next hop in the network, eventually reaching the destination
- Output
    - IP Packet = [IP Header] + [TCP Segment]
    - IP header contains (source IP, dest IP, TTL, checksum, etc.)

---
### Layer 2 - Data Link Layer

- It is responsible for transferring data between neighboring nodes on a single network segment. 
- It provides the creation, transmission, and reception of data frames
- Protocols
    - Ethernet: most widely used protocol for wired networks, defining how data is framed and transmitted over Ethernet cables
    - IEEE 802.11 (Wifi) - the standard for wireless networking, defining how devices connect to wireless access points and transmit data wirelessly
    - ARP - Address Resolution Protocol (IP address -> MAC address)
- Key Functions
    - Framing - It adds ethernet headers and trailers to the data from Network Layer
    - Physical Addressing - It adds source and destination MAC addresses (to the header part)
    - Error Handling - It detects and corrects errors that may occur during data transmission (FCS, checksum)
- Output
    - Frame = [Ethernet Header/Trailer] + [IP Packet] + [CRC checksum]
    - Ethernet Header contains MAC addresses, EtherType, CRC
- Component Involved
    - NIC hardware + DMA (Direct Memory Access - copies data from main RAM)

---
### Layer 1 - Physical Layer

- It is responsible for the raw transmission of data bits (0s and 1s) over a physical medium 
    - Ex: cables, fiber optics, or wireless channels
- Key Functions
    - It establishes the raw connection between devices, handling the physical aspects of data transfer
    - It converts data into electrical, optical, or electromagnetic signals for transmission
    - devices include hubs, repeaters, modems, and cables
- Output
    - Voltage changes (Ethernet) or Modulated RF signals (Wifi)
    - Encoded signals sent on the wire or air

---
## Analogy

Think of sending a physical letter:

- You write the message (Layer 7: Application).
- Encrypt or format it nicely (Layer 6: Presentation).
- Put it in an envelope (Layer 4–5: Session & Transport).
- Write address, add tracking info (Layer 3: Network).
- Put it in a package with labels for local delivery (Layer 2: Data Link).
- Hand it to the mailman (Layer 1: Physical — the only point when it actually leaves you)

---
## Processes involved in OSI Model

- OSI encapsulates the journey of a client request from when it is created to when it actually leaves the client's device
- Throughout the layers, the data is encrypted, appended with metadata and made ready for it to be transferred via the physical medium
- So, the data leaves the client device only when it has gone through all the OSI steps

_Details_

- User types `google.com` in browser
- Browser resolves DNS to Google's public IP address (using local cache or DNS lookup)
- Browser calls OS to initiate a TCP Handshake which enables reliable connection
- Browser calls OS to initiate a TLS Handshake to get shared session keys (If HTTPS)
- Browser prepares HTTP request and calls OS which stores this data inside user space of RAM
- Browser encrypts the data using the session key. OS still stores the Encrypted Data in user space of RAM
- OS kernel copies data to kernel space
- OS kernel networking subsystem encapsulates data with TCP header => TCP segments
- OS kernel immediately wraps the TCP segment with IP header (within the same function call) => IP Packets
- OS kernel notifies NIC 
- NIC accesses the data inside RAM using DMA (Direct memory access) via dedicated PCIe lanes
- NIC adds Ethernet header => Frames 
- (Wired) 
    - NIC passes the framed bits to the PHY layer chip
    - PHY converts digital bits to precise voltage signals (DAC) using [Line Coding Schemes](https://gouherdanish.github.io/2025/05/12/nic.html)
    - Voltage signals travel as analog waveform along ethernet cables reaching Router
- (Wireless) 
    - NIC passes the framed bits to Baseband Processor
    - Baseband Processor converts digital bits to digital form of modulated waveform using [modulation](https://gouherdanish.github.io/2025/05/12/nic.html). The output is still numbers (digital)
    - DAC in the RF Front-end converts the modulated digital IQ samples to a continuous analog waveform
    - RF Front-end does mixing, amplification, filtering and sends to antenna
    - Antenna emits EM wave which is caught by Router Antenna
- Router NIC converts ADC, checks IP header, does NAT, converts back DAC 
- Data travels along the Internet Backbone reaching Google

<img src="{{site.url}}/images/networking/osi.png">

---
## Data journey

- Appropriately formatted client request leaves the client's device, it can take two pathways depending on how the home LAN is setup
    - (Wired) The data travels through twisted copper cables in form of analog electrical waveform (voltage signals) and reaches Router
    - (Wireless) It travels through air in form of Electromagnetic Waves (RF) and reaches the router
- Router NIC does Analog to Digital conversion, 
- Router performs Network Address Translation (NAT)
    - Router finds Source Private IP of device, replaces with its Public IP
    - Router finds Source port on which app was running on device, replaces it with a random high number port
- Router NIC performs D2A conversion and sends to ISP (Internet Service Provider)
- From there, data is routed through mutiple network hops across internet backbone travel through physical medium
    - (ethernet) - copper wire
    - (fibre optic) - light waves


<img src="{{site.url}}/images/networking/client-server.png">