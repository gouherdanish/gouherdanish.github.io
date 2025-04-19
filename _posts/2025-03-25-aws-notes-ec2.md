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

<img src="{{site.url}}/images/aws/aws-ebs.png">

- EBS volumes are not physically attached to host machine like the instance store (NVMe)
- Instead it is a network drive which stores data in a remote storage cluster in the same AZ and reads/writes data via high-speed private AWS network
- Multiple EBS volumes can be attached to one EC2
    - But one EBS volume can not be attached to multiple EC2 instances (except io1/io2)
- Feels like a local drive e.g. /dev/xvda, /dev/nvme01
    - since the EC2 OS formats the EBS with a filesystem (ext4, xfs) and mounts it
    - AWS Nitro makes it look local despite routing requests over the network
- Root EBS Volume
    - created when EC2 is launched
    - contains OS and is required to boot when launched
    - Delete on Termination - True (default) - can be changed
- Additional EBS Volume
    - extra EBS volumes attached for stora n nnnmnne.g. /dev/sdf
    - does not contain OS
    - Delete on Termination - False (default) - can be changed
- Even if the EC2 host dies, the data is safe => data durability
- EBS has lower performance (high latency) than local NVMes since it is network-bound

---
### EC2 - EBS Volume Types

- EBS volumes are characterized by 
    - Size - how much data can it store - GB/TB
    - IOPS - how many read/write operations can the volume handle per second
    - Throughput - how much volume of data is transferred per second - MB/s
- Only gp2/gp3 or io1/io2 can be used as root volumes
- General Purpose SSD
    - cost-effective and low latency (balances price and performance)
    - Used in root volumes for OS bootup, Dev/test environments
    - 1 GiB - 16 TiB
    - gp3 (newer version of SSD)
        - IOPS = 3000 to 16000
        - Throughput = 125 MiB/s, can be increased upto 1000 MiB/s
        - no link between IOPS and throughput
    - gp2 (older version, smaller)
        - IOPS = 3000 to 16000 (linked to size, 3 IOPS per GB)
- Provisioned IOPS SSD
    - used for critical business applications (e.g. database)
    - provide sustained IOPS performance (low latency/high throughput workload)
    - io1
        - Size = 4 GiB - 16 TiB
        - Max IOPS = 64000 (Nitro EC2), 32000 (others)
        - IOPS not linked to size
    - io2
        - Size = 4 GiB - 64 TiB
        - Max IOPS = 256000 (linked to size, 1000 IOPS per GiB)
    - io1/io2 support EBS Multi-attach feature
        - upto 16 EC2 instances can be attached to same EBS volume
        - all EC2 instances and EBS should be in the same AZ
        - must use cluster-aware file system
            - e.g. GFS2 (Redhat), OCFS2 (Oracle), CephFS, Lustre
            - not XFS, EXT4, NTFS etc.
- HDD
    - cannot be used as root volume
    - Size = 125 GiB - 16 TiB
    - st1
        - throughput-optimized, low cost HDD 
        - used for Big Data, log processing (frequently accessed/throughput intensive workload)
        - Max IOPS = 500
        - Max Throughput = 500 MiB/s
    - sc1
        - lowest cost HDD (less frequently accessed workloads)
        - Max IOPS = 250
        - Max Throughput = 250 MiB/s

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
### EC2 - EBS Encryption

- Creating an encrypted EBS volume encrypts the following
    - data at rest inside the volume
    - data in motion between EC2 and EBS
    - snapshots 
    - EBS volumes created from snapshots
- Minimal impact on latency
- leverages AES-256 (KMS)

_How to encrypt an un-encrypted EBS volume_

- Create a snapshot of the EBS volume
- Encrypt the snapshot (using copy)
- Create new EBS volume from the snapshot (will be encrypted)
- This new EBS volume can be attached to the EC2

---
### EC2 - EFS (Elastic File System)

<img src="{{site.url}}/images/aws/aws-efs.png">

- Managed Network File System (NFS) which can be mounted on multiple EC2 across AZs but within same region
    - Uses NFSv4.1 protocol
    - mounted like a shared folder (/mnt/efs), and it behaves like a normal POSIX file system (Linux)
    - Need to setup security group to control access to EFS
- Pros
    - Highly available 
    - Scalable
        - filesystem auto-scales, no capacity planning needed
        - 1000s of concurrent NFS clients
        - 10 GB/s throughput
- Cons
    - Very costly (3 times the gp2 cost), Pay per use
- Limitation
    - Only compatible with Linux-based AMI
- Performance Modes
    - General Purpose (default) - low-latency usecases (webserver, CMS etc)
    - Max I/O - high latency, high throughput, highly parallel use-cases (Big Data)
- Throughput Modes
    - Bursting
        - throughput increases with size (Good for general purpose/spiky workloads)
        - e.g. size = 1 TB gets baseline throughput of 50 MiB/s
        - During lean period, credits build up if not using EFS that much
        - During spikes, accumulated credits are used up to support upto 100 MiB/s
    - Provisioned
        - fixed throughput (Good for steady I/O)
        - e.g. size = 1 TB can be setup with 1 GiB/s throughput
    - Elastic
        - auto-scales throughput based on workload
            - reads can get upto 3 GiB/s and writes upto 1 GiB/s
        - Good for unpredictable workload
- Storage Classes
    - Standard Tier - for frequently accessed files (high cost to store files, no retrieval cost)
    - Infrequent Access Tier - low cost to store files, high retrieval cost
    - Archive Tier - for rarely accessed files (e.g. annual) (lowest cost, 50% cheaper)
    - Can implement lifecycle policy - move files to Archive/IA if not accessed for N days

---
### EC2 - Instance Store

- Some specific EC2 instances are created on top of such physical hosts which have a Hard Disk directly attached to it
- Pros - provides better I/O performance
- Cons - loses data once stopped (ephemeral)
- Good for buffer/cache/temporary content

