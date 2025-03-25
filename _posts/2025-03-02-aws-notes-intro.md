---
layout: post
title: "AWS Notes - Introduction"
date: 2025-03-02
tags: ["Cloud"]
---

AWS stands for Amazon Web Services

---

### History

- 2002 - AWS Internally Launched at Amazon
- 2003 - Idea to market came after quickly becoming one of their core strength
- 2004 - Launched publicly with SQS
- 2006 - Relaunched publicly with SQS, EC2 & S3
- 2007 - Expands to Europe

---

### Major Customers

- Netflix
- Dropbox
- AirBnb
- NASA
- 21st Century Fox

--- 

### Stats

- 13th consecutive year being market leader
- 31% market share (25% Microsoft)
- $90 bn Annual revenue (2023)
- +1mn active users

---

### AWS Regions

- A region is a cluster of data centers
- Regions are present all over the globe e.g. us-east-1, ap-south-1, eu-west-3
- Most AWS services are region-scoped

_How to chose a Region_

**Compliance** - data in France may have to stay in France; data governance and legal requirements

**Proximity** - if app users are mostly in US, then launch closer to the users to reduce latency

**Available services** - some services/features are not available in every region

**Pricing** - varies from region to region

---

### AWS Availability Zones

- In each region, there are either 3 or 6 availability zones
- Example: For region `ap-southeast-2` region, there are 3 AZs as below
    - `ap-southeast-2a`
    - `ap-southeast-2b`
    - `ap-southeast-2c`
- In each AZ, there are one or more discrete data centers with 
    - redundant power
    - networking and
    - connectivity
- AZ'z are geographically separate from each other so that they are isolated from disasters
- AZ's are mutually connected with high-bandwidth and ultra low latency networking altogether forming a region

<img src="{{site.url}}/images/aws/aws-az.png" alt="Availability Zones">

---

### AWS Points of Presence

- 400+ Edge Locations
- 10+ Regional Caches
- 90+ cities
- 40+ countries

