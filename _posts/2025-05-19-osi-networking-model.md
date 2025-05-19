---
layout: post
title: "OSI Networking"
date: 2025-05-19
tags: ["Networking"]
---

OSI stands for Open Systems Interconnection

---
### Introduction

- OSI is a conceptual framework that divides network communication into seven abstract layers
- It aims to facilitate communication between different systems, even if they have different hardware or software
- The layered structure makes it easier to troubleshoot network issues by isolating problems to specific layers
- The model is fundamental to protocol development, as each layer defines the protocols and services used at that level

---
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
### Layer 4 - Transport Layer

- It manages end-to-end communication between applications, ensuring reliable data delivery
- Protocols
    - TCP - Transmission Control Protocol (for reliable, ordered delivery)
    - UDP - User Datagram Protocol (for faster, less-reliable delivery)
- Key Functions
    - Segmentation - It breaks down large messages to smaller segments "packets" for transmission
    - Flow Control - It controls the flow of data to prevent the receiver from being overwhelmed, especially with TCP
    - Error Control - It provides mechanisms for detecting and correcting errors during data transmission
    - Multiplexing - 