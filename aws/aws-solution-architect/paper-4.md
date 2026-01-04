# AWS Solutions Architect – Associate Exam Preparation Notes

This repository contains summarized notes and key patterns for AWS services, designed to help with **exam preparation** and real-world architecture understanding. The notes are grouped by **AWS resource/service** for easier reference.

---

## 1. Amazon DynamoDB

### Hot Partition & High-Frequency Reads
- **Scenario:** Ride-sharing/social media app storing GPS coordinates; hot partition due to popular items.
- **Problem:** Requests concentrated on a single partition key; throttling occurs even with high RCUs.
- **Solution:** 
  - Use **DynamoDB DAX (in-memory cache)** to offload frequent reads.
  - Alternative: **DynamoDB On-Demand + Adaptive Capacity** for automatic scaling.
- **Exam Tip:** “Hot partition,” “popular item,” “minimal app changes” → think **DAX / On-Demand**.

### Dynamic Workloads / Bursty Traffic
- Use **DynamoDB On-Demand capacity mode**:
  - Scales automatically to sudden spikes
  - Cost-efficient for idle periods
  - Optional DAX for sub-millisecond reads
- **Exam Tip:** “Unpredictable traffic” + “sudden spikes” → **DynamoDB On-Demand**

### Geo Queries
- Use **DynamoDB Geo Library** for nearby driver queries.
- Real-time geospatial lookups → prefer **DynamoDB over RDS**.

---

## 2. Amazon S3 / Glacier

### Data Retention / Compliance
- **Policy A:** Critical transactions → immediate access, immutable 7 years → **S3 Standard + Object Lock** (Compliance mode)
- **Policy B:** Archived compliance → low-cost, immutable 10 years → **S3 Glacier / Glacier Deep Archive + Object Lock**
- **Optional:** Lifecycle Policies to transition objects from Standard → Glacier
- **Exam Tip:** “Immutable / WORM / retention period” → **S3 Object Lock**

---

## 3. Amazon CloudFront

- **Routing:** Global edge locations serve content from nearest location → low latency
- **Security:** Integrates with **AWS WAF** and **AWS Shield**
- **High Availability:** Automatic failover across multiple edge locations
- **Edge Locations:** Physical AWS sites close to users to cache content
- **Exam Tip:** CDN questions often include “low latency,” “security at the edge,” “edge caching.”

---

## 4. Amazon EFS

- **Scenario:** Sporadic, data-intensive workloads
- **Solution:** **EFS Bursting Throughput mode**
  - Scales automatically for throughput bursts
  - Pay only for storage, not throughput
- **Exam Tip:** Unpredictable workloads + shared storage → **EFS Bursting Throughput**

---

## 5. Amazon Route 53

- **Geographic Traffic Control:** Use **Route 53 Geolocation Routing**
- **Exam Tip:** 
  - Geolocation → route by user location
  - Geoproximity → adjust weights based on distance bias
  - Latency-based → send to lowest-latency endpoint

---

## 6. Amazon EC2 Placement & HPC

- **HPC / Tightly Coupled Nodes:** Use **Cluster Placement Group**
  - Low-latency, high throughput network
  - Ideal for MPI or HPC workloads

### High Availability with Static Public IP
- **Scenario:** Bank whitelisting a public IP
- **Solution:** **Network Load Balancer + Auto Scaling Group + Elastic IPs**
- **Exam Tip:** “Whitelist IP + scaling + HA” → **NLB**, not ALB

---

## 7. Serverless Architectures

- **Ride-hailing app:** API Gateway + Lambda + DynamoDB
- **Challenges:** High-frequency updates / hot keys
- **Solutions:** 
  - **DynamoDB On-Demand + optional DAX**
  - Keep RDS for persistent relational data only
- **Exam Tip:** Real-time, write-heavy workloads → **NoSQL + caching**, not RDS

---

## 8. VPC Endpoints

- **Gateway Endpoints:** Supported only by **S3** and **DynamoDB**
- **Interface Endpoints (PrivateLink):** Most other AWS services (SNS, SQS, API Gateway, Lambda)
- **Exam Tip:** Private access from VPC → gateway for S3/DynamoDB, interface for others

---

## 📌 Exam Tips / Patterns

1. Hot partitions / bursts → **DynamoDB On-Demand + DAX**  
2. Immutable storage / compliance → **S3 Object Lock (+ Glacier for archival)**  
3. Global content delivery → **CloudFront + edge locations**  
4. HPC / tightly coupled EC2 → **Cluster Placement Group**  
5. Static public IP whitelisting → **NLB + Elastic IPs**  
6. Shared file system bursts → **EFS Bursting Throughput**  
7. Geographic routing → **Route 53 Geolocation Routing**  
8. Private access to AWS services → Gateway Endpoint (S3/DynamoDB), Interface Endpoint (others)

---

> **Note:** These notes are designed for quick revision for AWS Solutions Architect – Associate exams and understanding real-world architectures.
