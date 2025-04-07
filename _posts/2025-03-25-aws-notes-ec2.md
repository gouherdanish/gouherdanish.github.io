---
layout: post
title: "AWS Notes - EC2"
date: 2025-03-25
tags: ["Cloud"]
---

EC2 stands for Elastic Compute Cloud;

---

### EC2 : Intro

- Cloud providers have many physical hosts/servers stacked on Racks in Data Center
- Each host has hardware components e.g. CPU, RAM, Network Cards etc.

<img src="{{site.url}}/images/aws/host.png">
 
- AWS uses Hypervisor software (AWS Nitro) to virtualize the underlying hardware

<img src="{{site.url}}/images/aws/virtualization.png">

- EC2 Instances are virtual machines (VMs) having their own provisioned resources sliced out of the bare metal hardware.
- Multiple EC2 instances can share the same physical host
    - but they have to belong to the same family (e.g. m5)
    - E.g. the above host can support multiple EC2s as follows
        - 12 instances of m5.2xlarge
        - 24 instances of m5.xlarge
        - 48 instances of m5.large
        - or a mix of these (10 * m5.2xlarge, 1 * m5.xlarge, 2 * m5.large)
- EC2 service represents Infrastructure as a Service (IaaS)
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
- r5.xlarge - 4 vCPU, 32 GB RAM, 468 GB SSD, 10 Gbps network bandwidth

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
- i8g.large - 2 vCPU, 16 GB RAM, EBS-only, 10 Gbps network bandwidth
- i3.16xlarge - 64 vCPU, 488 GB, 8 * 1900GB NVMe SSD, 3.3 million Read IOPS, 1.4 million Write IOPS

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

**On-Demand**

- pay for what you use 
    - Linux/Windows by second, other OS by hour
- Has the highest cost but no upfront payment and no long term commitment
- Recommended for short-term and un-interrupted workload

**Reserved Instances**

- ~72% discount compared to on-demand
- can reserve instances of specific features (Instance Type, OS, Region, Tenancy)
- can reserve for 1 yr or 3 yr
- can opt for full upfront payment, partial or no upfront payment
- Recommended for steady-state applications (e.g. database)

**EC2 Savings Plan**

- you commit to a certain type of usage ($10/hr for 1 or 3 yrs)
- ~72% discount, usage spills charged at on demand rate
- locked on EC2 Instance family & Region (e.g. M5 in us-east-1)
- Flexible across Instance size, OS, Tenancy

**Spot Instance**

- Most cost-efficient option (~90% discount)
- can lose such instance if current spot price is higher than your max price
- useful for workloads that are resilient to failure
    - batch jobs
    - image processing
    - data analysis
    - Distributed workloads

**EC2 Dedicated Host**

- a physical server with EC2 capacity fully dedicated for your use (it's always the same physical machine for as long as you are paying)
- allows to address compliance requirements and use server-bound software licenses (per socket, per core, per VM)
- On demand or reserved
- Most expensive
- Useful for software with complicated software licensing model (BYOL)

**EC2 Dedicated Instances**

- the hardware is "yours" (you are not sharing it with others) for the time your instance is running
- If you stop/start instance, you can get some other hardware somewhere else
- So your instance is moved around on different physical servers - whichever is not occupied by others at the time

**EC2 Capacity Reservations**

- can reserve instances in a specific AZ for any duration
- On demand charges applicable whether you launch instances or not
- can combine with Regional Reserved Instances or Savings Plan for billing discounts

---

### EC2 - IP Address

- IPv4 
    - 3.7 bn unique IP addresses (running out)
    - Most websites support only IPv4
    - Format: [0-255].[0-255].[0-255].[0-255]
    - Ex: 192.168.0.1
    - EC2 Free Tier applicable
    - other services IP charged @ $0.005/hr
- IPv6
    - AWS supports both IPv4 and IPv6
    - Contains hexadecimal characters
    - Ex: abcd.1.efgh.23.4567.gh

- Public IP
    - the machine can be identified on the internet (can be geolocated)
    - must be unique globally; no two machines can have same public IP

- Private IP
    - the machine can only be identified over the private network
    - must be unique on the private network
    - two different private networks can have same private IPs
    - machines connect to internet using NAT + internet gateway (a proxy)

- Elastic IP
    - Public IP of EC2 instance changes when re-started
    - Elastic IP is needed when public IP needs to be kept fixed
    - Elastic IP is attached to one instance at a time (limit of 5 Elastic IPs)
    - Generally advised not to use Elastic IP, instead use random public IP and attach DNS

---
### EC2 - Placement Groups

It allows us to define how we want the EC2 instances to be placed wrt each other on AWS

**Cluster PG**

_What_ - all EC2 instances are placed in same AZ

_Pros_ - Low Latency and High Throughput (upto 10 Gbps bandwidth)

_Cons_ - If AZ fails, all instances will fail at once

_Uses_ - Big Data jobs that needs to complete fast

<img src="{{site.url}}/images/aws/aws-pg-cluster.png">

**Spread PG**

_What_ - each EC2 instance is on different hardware (rack) across different AZ

_Pros_ - minimizes failure risk, high availability

_Cons_ - Limitation : 7 EC2 instances per PG; 7 EC2 instances per AZ

_Uses_ - Critical applications where each instance must be isolated from failure

<img src="{{site.url}}/images/aws/aws-pg-spread.png">

**Partition PG**

_What_ - partition represents a rack in a data center; many EC2 in one partition; upto 7 partition per AZ; can span across multiple AZ; 100s of EC2 can be in one PG

_Pros_ - safe from rack failure as EC2 instances are distributed across multiple hardware racks

_Cons_ - still one partition failure can cause multiple EC2 instances to fail

_Uses_ - Partition aware applications viz. Big Data e.g. HDFS, HBase, Cassandra

<img src="{{site.url}}/images/aws/aws-pg-partition.png">

---
### EC2 - Elastic Network Interface (ENI)

- logical component in a VPC that represents a Virtual Network Card and can have
    - one primary private IPv4
    - one or more secondary IPv4
    - one Elastic IP per private IPv4
    - one public IPv4
    - one or more security groups
    - MAC address
- can be created independently and attached to EC2 instances on the fly for failover
- bound to specific AZ

---
### EC2 - Hibernate

- **Stop** - The data on EBS is kept intact on next start
- **Terminate** - EBS volumes are lost
- **Start**
    - _First Start_ - OS boots and EC2 user script is run
    - _Following starts_ - OS boots
    - application starts, caches get warmed up => takes time
- **Hibernate**
    - while stopping the EC2, the RAM state is written to a file in EBS volume => in-memory state is preserved
    - while re-starting the EC2, the RAM state is loaded from the EBS => OS boot is much faster
    - Available for almost all instance families and instance purchase options
    - Instance RAM size s/b <150 GB; bare metal instances are not supported
    - Root volume must be EBS, large and encrypted. (Instance store is not supported)
    - Not more than 60 days

---
### EC2 - EBS (Elastic Block Store)

- EBS volumes are not physically attached to host machine like the instance store (NVMe)
- Instead it is a network drive which stores data in a remote storage cluster in the same AZ and reads/writes data via high-speed private AWS network
- Multiple EBS volumes can be attached to one EC2
    - But one EBS volume can not be attached to multiple EC2 instances
- Root EBS Volume
    - created when EC2 is launched e.g. /dev/xvda
    - contains OS and is required to boot when launched
    - Delete on Termination - True (default) - can be changed
- Additional EBS Volume
    - extra EBS volumes attached for storage e.g. /dev/sdf
    - does not contain OS
    - Delete on Termination - False (default) - can be changed
- Even if the EC2 host dies, the data is safe => data durability
- EBS has lower performance (high latency) than local NVMes since it is network-bound

---
### EC2 - EBS Volume Types

- General Purpose SSD
    - cost-effective and low latency (balances price and performance)
    - Used in root volumes for OS bootup, Dev/test environments
    - 1 GiB - 16 TiB
    - gp2/gp3
- Provisioned IOPS
    - io1/io2 - highest performance SSD volume (low latency/high throughput workload)
    - st1 - low cost HDD (frequently accessed/throughput intensive workload)
    - sc1 - lowest cost HDD (less frequently accessed workloads)
- EBS volumes are characterized by Size/Throughput/IOPS
- gp2/gp3 or io1/io2 can only be used as root volumes

---
### EC2 - EBS Snapshots

- EBS Snapshots can be saved to S3 and new EBS volumes can be recreated from that
    - Recommended to detach volumes before taking snapshot but not necessary
    - can copy snapshots across AZ / Regions
- Features
    - EBS Enapshots can be saved to Archive tier
        - 75% cheaper
        - 24-72 hr for restoring
    - Recycle Bin can be setup for EBS Snapshots
        - accidental deletion can be recovered
        - can specify retention from 1day - 1yr
    - Fast Snapshot Restore (FSR)
        - no latency on first use
        - costs money

---
### EC2 - Instance Store

- Some specific EC2 instances are created on top of such physical hosts which have a Hard Disk directly attached to it
- Pros - provides better I/O performance
- Cons - loses data once stopped (ephemeral)
- Good for buffer/cache/temporary content


---
### EC2 - AMI (Amazon Machine Image)

- EC2 instances are launched using an AMI image
    - can use public AMI (provided by AWS)
    - can make our own AMI
    - can use 3rd party AMI (AWS Marketplace)
- Own AMI might be needed in case when we want our EC2 to always have some predefined softwares installed
    - Usually these are installed via EC2 user scripts
    - Doing this everytime might take a longer time
    - So, recommended to create own image which will launch EC2 faster
- AMI are built for specific region and can be copied across regions
- AMI Creation Process
    - Launch EC2 with custom script
    - Stop EC2 instance
    - Build AMI by saving image (saves EBS snapshots)
    - Launch EC2 using My AMI