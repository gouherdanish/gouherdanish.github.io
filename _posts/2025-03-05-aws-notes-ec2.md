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

**General Purpose**

_Intro_
- Good balance between compute, memory and networking
- t-series, m-series

_Use Cases_
- Great for generic workloads e.g. webservers, code repositories


Examples:
- t2.micro - 1 vCPU, 1 GB RAM, EBS-only, Low-to-moderate network performance
- t2.xlarge - 4 vCPU, 16 GB RAM, EBS-only, moderate network performance
- m5.xlarge - 4 vCPU, 16 GB RAM, EBS-only, 10 Gbps network bandwidth

Note: 
- t2.micro => AWS Free Tier (~750 hrs per month)

**Compute Optimized**

_Intro_
- Good for compute-intensive tasks requiring high-performance CPUs
- c-series

_Use Cases_
- batch processing
- media transcoding
- high performance web servers
- high performance computing
- machine learning
- gaming servers

Examples
- c5.xlarge - 4 vCPU, 8 GB RAM, EBS-only, 10 Gbps network bandwidth
- c5d.4xlarge - 16 vCPU, 64 GB RAM, NVMe SSD, ~10 Gbps network card

**Memory Optimized**

_Intro_
- Provide Fast performance on tasks that process large data sets in memory
- r-series, x-series

_Use cases_
- High performance, relational/non-relational DB
- In-memory databases optimized for BI
- Distributed cache stores

Examples
- r5.xlarge - 4 vCPU, 32 GB RAM, EBS-only, 10 Gbps network bandwidth

**Storage Optimized**

_Intro_
- Great for storage-intensive tasks requiring fast sequential read/write access to large datasets on local storage
- i-series, x-series

_Use cases_
- High frequency OLTP systems
- Cache for in-memory databases (Redis)
- Distributed file system
- Data warehousing applications

Examples
- r5.xlarge - 4 vCPU, 32 GB RAM, EBS-only, 10 Gbps network bandwidth

---

### EC2 - Security Groups

- These are a fundamental network security concept in AWS
- They act as firewall around the EC2 and control how traffic is allowed into or out of the EC2 instance
- SG only contain `allow` rules and SG rules can reference by IP or other SGs
- SGs regulate access to ports, authorised IP ranges (IPv4 and IPv6), inbound and outbound traffic
- SGs can be attached to multiple EC2 instances and an EC2 instance can have multiple SGs (Many to Many)
- SGs have to be re-created if Region/VPC changes
- It's good practice to maintain separate SG for ssh access
- In case some application faces timeout, it implies a SG issue
- In case connection is refused, then SG would've worked correctly and implies an application error
- By Default, all inbound traffic is blocked and all outbound traffic is authorized
- EC2 containing multiple SGs attached can be accessed by other EC2 instances having those individual SGs
- Important
    - Port 22 - SSH (Secure Shell) - log in to linux
    - Port 21 - FTP (File Transfer Proticol) - upload files into fileshare
    - Port 22 - SFTP (Secure FTP) - upload files using ssh
    - Port 80 - HTTP - access unsecured websites
    - Port 443 - HTTPS - access secured websites
    - Port 3389 - RDP - Remote Desktop Protocol - log into Windows Machine

---

### EC2 - Instance Purchasing Options

1. On-Demand

- pay for what you use 
    - Linux/Windows by second, other OS by hour
- Has the highest cost but no upfront payment and no long term commitment
- Recommended for short-term and un-interrupted workload

2. Reserved Instances

- ~72% discount compared to on-demand
- can reserve instances of specific features (Instance Type, OS, Region, Tenancy)
- can reserve for 1 yr or 3 yr
- can opt for full upfront payment, partial or no upfront payment
- Recommended for steady-state applications (e.g. database)

3. EC2 Savings Plan

- you commit to a certain type of usage ($10/hr for 1 or 3 yrs)
- ~72% discount, usage spills charged at on demand rate
- locked on EC2 Instance family & Region (e.g. M5 in us-east-1)
- Flexible across Instance size, OS, Tenancy

4. Spot Instance

- Most cost-efficient option (~90% discount)
- can lose such instance if current spot price is higher than your max price
- useful for workloads that are resilient to failure
    - batch jobs
    - image processing
    - data analysis
    - Distributed workloads

5. EC2 Dedicated Host

- a physical server with EC2 capacity fully dedicated for your use (it's always the same physical machine for as long as you are paying)
- allows to address compliance requirements and use server-bound software licenses (per socket, per core, per VM)
- On demand or reserved
- Most expensive
- Useful for software with complicated software licensing model (BYOL)

6. EC2 Dedicated Instances

- the hardware is "yours" (you are not sharing it with others) for the time your instance is running
- If you stop/start instance, you can get some other hardware somewhere else
- So your instance is moved around on different physical servers - whichever is not occupied by others at the time

7. EC2 Capacity Reservations

- can reserve instances in a specific AZ for any duration
- On demand charges applicable whether you launch instances or not
- can combine with Regional Reserved Instances or Savings Plan for billing discounts

---

### EC2 - IP Cost

- IPv4 
    - 3.7 bn unique IP addresses (finish soon)
    - Most websites supported
    - [0-255].[0-255].[0-255].[0-255]:
    - EC2 Free Tier applicable
    - other services IP charged @ $0.005/hr
- IPv6
    - 