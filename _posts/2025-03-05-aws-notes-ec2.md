---
layout: post
title: "AWS Notes - IAM"
date: 2025-03-05
tags: ["Cloud"]
---

EC2 stands for Elastic Compute Cloud;

---

### EC2 : Intro

- Infrastructure as a Service (IaaS)
- Consists of multiple services
    - Rent virtual machines (EC2)
    - Store data on virtual drives (EBS)
    - Distribute Load across machines (ELB)
    - Scaling the services (ASG)

---

### EC2 : Configuration Options

- OS - Windows/Linux/Mac OS
- CPU - How many cores needed
- RAM - How much memory needed
- Storage - EBS, EFS, EC2 instance store
- Network Card - speed of the card
- Firewall Rules - Security Groups
- Bootstrap Script - EC2 user data (Runs at machine boot time with root user; has sudo access)

---

### EC2 - Instance Types

- t2.micro - 1 vCPU, 1 GB RAM, EBS-only, Low-to-moderate network performance
- t2.xlarge - 4 vCPU, 16 GB RAM, EBS-only, moderate network performance
- c5d.4xlarge - 16 vCPU, 64 GB RAM, NVMe SSD, ~10 Gbps network card

Note: 
- t2.micro => AWS Free Tier (~750 hrs per month)