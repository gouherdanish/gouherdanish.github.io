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
        - client is always redirected to the same instance behind the LB
        - Ex. when the user makes the first request, if the request goes to backend instance 1, then on second request also, his request will go to instance 1 itself
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
### Target Groups

- It is a group of EC2 instances to which the traffic is routed by the ELB
- There can be various types of target groups supported by ALB or NLB

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


---
### Application Load Balancer

- Operates on Application Layer (L7)
- Balances load from HTTP/HTTPS traffic
- Can load balance on multiple applications
    - either on multiple machines (target group)
    - or on same machine (ex: containers)

<img src="{{site.url}}/images/aws/aws-alb.png">

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

_Does ALB support fixed static IP or Elastic IP ?_
- **NO**, we can't associate Elastic IPs (EIPs) with an ALB
- ALB is provisioned with dynamic public or private IPs which change if ALB is stopped and restarted
- So, instead of using IPs directly, AWS provides DNS name (e.g., my-alb-1234567890.us-east-1.elb.amazonaws.com)
- If we want to share fixed IP with clients or whitelist IPs in a firewall, we can
    - use NLB instead which support static IP
    - use NLB in front of ALB which gets us both static IPs and Layer 7 features

_If ALB is slower than NLB, why is it still used ?_
- ALB provides smart routing based on the request (Path-based, Host-based, Header)
- It provides SSL termination which offloads decryption from backend servers end
- It also provides sticky sessions using cookies
- It also provides Full support for HTTP/2 and websocket based traffic

_Unlike NLB, ALB has to inspect the HTTP traffic and do SSL termination, is it possible to become a bottleneck ?_
- **NO**, ALB is fully managed by AWS
- In case of increased traffic, it auto-scales to multiple Load Balancer Nodes
- It is also distributed across multi-AZ

---
### Network Load Balancer

- Operates on Transport Layer (L4)
- Balances load from TCP, UDP, TLS traffic
- Very high performance and ultra-low latency (millions of requests per second)
- NLB supports one static IP (Elastic IP) per AZ but ALB does not

<img src="{{site.url}}/images/aws/aws-nlb.png">

Use cases
- Services requiring ultra-low latency and high throughput e.g. Gaming apps, streaming, databases
- Need SSL/TLS passthrough (end to end encryption) where SSL decryption is handled by backend servers
- If clients need fixed static IP for each AZ

_Why is NLB faster than ALB ?_
- NLB works at Layer 4, it just forwards packets
- it does not inspect or modify traffic 

_If UDP traffic comes to NLB, does it convert to tcp and then forward it ?_
- **NO**, NLB operates at Layer 4 which supports UDP packets without any modifications

_Can NLB route http/https traffic from internet ?_
- **YES, it can**, but when it forwards HTTP or HTTPS traffic, it treats it as TCP traffic and doesn't inspect the contents of the HTTP/HTTPS protocol itself or perform URL-based routing
- Also, For HTTPS traffic, NLB does TLS passthrough, meaning it doesn't decrypt the traffic. The backend EC2 instances would need to handle the SSL decryption

_Does NLB support health check of targets ?_
- **YES**, NLB performs Layer 4 (TCP or HTTP/HTTPS) health checks which are used to determine whether to send traffic to a target
- If you're using TCP health checks, NLB checks whether it can establish a TCP connection to the target.
- If you're using HTTP/HTTPS health checks, NLB sends an HTTP GET to the specified path and expects a 200 OK response.

---
### Gateway Load Balancer

- Deploy and manage 3rd party network security appliances on AWS
    - e.g. Firewal, IDPS, DPIS
- Before routing the request to the backend app server, the Load balancer routes to this target group consisting of these firewall which analyze the request for intrusion
- If all looks good, then it is routed back to the LB and then to the backend server

<img src="{{site.url}}/images/aws/aws-gwlb.png">

- Works on Network Layer (L3)
- Uses GENEVE Protocol (Port 6081)
- Two functions
    - Single Entry/Exit point for all traffic
    - distributes traffic to the virtual appliances not to the backend instances

---

### Cookies

- HTTP requests are stateless by design
    - Each request from the client is treated as completely new
    - Ex. if you log in and the session ID isn't saved in a cookie (or URL, or storage), the server forgets you on the next request — you'll be logged out or prompted again
- Cookies maintain "state" to the otherwise stateless HTTP protocol
- Cookies enable Load Balancer stickiness by redirecting same client to the same backend instance 
    - works for CLB, ALB, NLB
    - we can control expiration date of the cookie
    - may bring imbalance to the load over EC2 instances

_Types of Cookies_
- Application-based Cookies 
    - duration is specified by the application
    - Custom cookie
        - generated by target, so can have any custom attribute reqd by app
        - cookie names can be anything except AWSALB, AWSALBAPP, AWSALBTG
    - Application cookie
        - generated by LB; name is AWSALBAPP
- Duration-based Cookies
    - generated by LB; 
    - name is AWSALB (for ALB), AWSELB (for CLB)

_Breakdown of Cookie attributes_
- `session_id=abc123` : actual cookie data (key-value pair)
- `Path=/` : Restricts cookie to URLs under this path. / means all paths
- `Domain=example.com` : Specifies which domain(s) should receive the cookie; subdomains not included
- `Expires=Wed, 15 May 2025 12:00:00 GMT` : When the cookie will be deleted by the browser. Without it, it’s a session cookie (deleted on tab/browser close)
- `Secure` : Only send this cookie over HTTPS connections
- `HttpOnly` : JavaScript cannot access this cookie (prevents XSS attacks)
- `SameSite=Strict` : Controls cross-site requests. Strict blocks cookie on all cross-origin requests (prevents CSRF)

_How does cookie work ?_
- User logs in, server verifies credentials, creates a session ID
- Server sets a cookie by sending a Set-Cookie header in the HTTP response
`Set-Cookie: session_id=abc123; Path=/; Domain=example.com; Expires=Wed, 15 May 2025 12:00:00 GMT; Secure; HttpOnly; SameSite=Strict`
- Browser attaches this cookie on future requests

```
GET /dashboard HTTP/1.1
Host: example.com
Cookie: session_id=abc123
```

- Server recognizes the session and knows which user is making request
- The user stays logged in as long as the cookie is valid.


_What's the alternate way ?_
- URL-based session ID
    - User logs in and gets assigned a session ID (abc123)
    - This session ID is passed as a query parameter or part of the URL path (`https://example.com/dashboard?sessionid=abc123`)
    - On each request, the server extracts the session ID from the URL and looks it up
    - This allows session continuity without cookies

- Using Local Storage (ex - JWT Token)
    - User logs in -> server returns a JWT token in the response body
    - client stores the token in `localStorage`
    - On every request, the browser client manually attaches the token
    - Server verifies the token and responds
    - Cons 
        - Overhead on client-side to store and attach the token
        - Need to refresh the token before it expires

_Just like JWT token, cookie is also stored at client side. then why cookie is better ?_
- Cookies are stored per domain (google.com) in the browser cookie jar. All cookies matching a domain are automatically attached while sending a HTTP request
- JWT token are stored in `localStorage` or `sessionStorage` which are completely accessible to Javascript, making them vulnerable to XSS if your site is compromised

_How does Stickiness work in LB ?_
- LB maintains a session state by inserting a cookie while sending back response to client (browser)
- cookie has the info on the target instance ID (e.g. AWSALB, AWSALBAPP)
- when browser sends new request, it attaches the same cookie which is decoded by LB and routed to same backend instance

---
### Cross-zone Load Balancing

**With cross-zone LB**

- The load balancer node in any AZ can distribute requests across all registered targets in all AZs, helping balance traffic more evenly.
<img src="{{site.url}}/images/aws/aws-elb-cross-on.png">

**Without cross-zone LB** 

- Each load balancer node in a given AZ only routes to targets (EC2s) in its own AZ.
<img src="{{site.url}}/images/aws/aws-elb-cross-off.png">