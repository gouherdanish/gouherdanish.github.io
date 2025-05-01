---
layout: post
title: "AWS Notes - ELB"
date: 2025-04-18
tags: ["Cloud"]
---

ELB stands for Elastic Load Balancer

---
### Scalability

- It means the system/application can handle greater load by adapting
- Vertical Scalability
    - increase the size of the EC2 instance (scale up/down)
    - e.g. changing from t2.micro to t2.large
    - used for non-distributed applications e.g. Database (RDS, ElastiCache)
    - limited by hardware
- Horizontal Scalability
    - add more EC2 instances (scale out/in)
    - used in distributed systems
    - applicable to
        - Load Balancer
        - Auto-scaling Group

---
### High Availability (HA)

- It means running the application on at least 2 data centers so application keeps running in case one data center goes down
- applicable to
    - Load Balancer with Multi-AZ
    - Auto-scaling Group with Multi-AZ
- Active-Passive HA
    - One system is active (handling all traffic)
    - Other system is passive (on standby/idle, waiting to take over in case active system fails)
    - Pros
        - simple setup
        - Good for stateful systems e.g Database
    - Cons 
        - Passive node is idle => underutilized resources
        - may involve failover time => downtime
    - e.g. MySQL setup as a Master-Slave mode
- Active-Active HA
    - All nodes are active, sharing the load simultaneously
    - Even if one node fails, others keep serving requests without interruption
    - Pros
        - No idle resources => Better utilization
        - Instant failover => No disruption
    - Cons
        - complext to setup (state syncing)
    - e.g. Webserver behind a Load Balancer

---
### Load Balancer (LB)

- Load Balancer forwards traffic to downstream EC2 instances
- Why use it ?
    - the load is spread evenly
    - it provides single point of access (DNS) to our application
    - it performs SSL termination
        - LB decrypts HTTPS requests from client and sends plain HTTP to EC2 (simplified backend)
        - LB handles the central management of SSL Certs and offloads CPU of backend servers (performance)
    - it enforces stickiness with cookies
        - LB maintains a session state by inserting a cookie while sending back response to browser (client)
        - cookie (e.g. AWSALB, AWSALBAPP) has the info on the target instance ID
        - when browser sends new request, it attaches the same cookie which is decoded by LB and routed to same backend instance
    - it handles failure of instances
        - LB does regular health checks (/health => 200 OK else Unhealthy)

---
### Elastic Load Balancer (ELB)

<img src="{{site.url}}/images/aws/aws-elb.png">

- Elastic Load balancer is a managed service provided by AWS
    - AWS takes care of upgrades, maintenance, HA
    - integrated with many AWS services - EC2, ASGs, Certificate Manager, CloudWatch, Route53

- 4 types of ELBs
    - Classic Load Balancer (CLB) - 2009 - v1
        - HTTP, HTTPS, TCP, SSL(Secure TCP)
        - deprecated
    - Application Load Balancer (ALB) - 2016 - v2
        - HTTP, HTTPS, Websocket
    - Network Load Balancer (NLB) - 2017 - v2
        - TCP, TLS (Secure TCP), UDP
    - Gateway Load Balancer (GWLB) - 2020
        - IP

---
### Application Load Balancer

<img src="{{site.url}}/images/aws/aws-alb.png">

- Operates on Application Layer 7 (HTTP)
- Can load balance on multiple applications
    - either on multiple machines (target group)
    - or on same machine (ex: containers)
- Target groups can be 
    - EC2 instances
    - ECS tasks
    - Lambda
    - IP addresses (on-premise private hosts)
- Can route to different target groups based on
    - route path (/users, /search)
    - hostname (one.example.com, other.example.com)
    - query strings (/users?id=123)
- Supports redirects from HTTP to HTTPS
- Good for microservices & container-based apps (ex: Docker and ECS)
- Client IP is not visible to servers directly
    - inserted in header X-Forwared-For

_Can a target group have instances across Region ?_
**NO, target group is tied to one Region**

_Can a target group have instances across AZ ?_
**YES, it is recommended for High Availability**

_Can a target group have multiple instances in same AZ ? Explain with example scenario._
**YES**
- Imagine you are running an E-commerce APP and you are expecting high spikes during a sale.
- Having 3 EC2 instances can handle 3X more traffic than a single EC2 => Scalability
- In case one EC2 instance fails, other EC2 instances can serve the traffic => High Availability
- Updating the app can be done one instance at a time without app downtime => Rolling Deployments



