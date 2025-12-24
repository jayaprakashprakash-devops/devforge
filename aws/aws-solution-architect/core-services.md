# AWS Solution Architect – Core Services Notes

These notes are designed for **AWS Certified Solutions Architect – Associate** exam preparation.  
They focus on **key AWS services, concepts, and exam-oriented explanations**.

---

## 1. Amazon GuardDuty

**Purpose:** Threat detection service

**What it does**
- Continuously monitors AWS accounts
- Uses machine learning and threat intelligence
- No agents required

**Data sources**
- AWS CloudTrail logs
- VPC Flow Logs
- DNS logs

**Detects**
- Compromised EC2 instances
- Malicious IP communication
- Account takeover attempts

**Exam keyword:**  
Threat detection without installing software

---

## 2. Amazon Shield

**Purpose:** DDoS protection service

### Shield Standard (Free)
- Automatic protection
- Protects against Layer 3 & Layer 4 attacks

### Shield Advanced (Paid)
- Advanced DDoS protection
- Cost protection
- AWS DDoS Response Team (DRT)

**Exam keyword:**  
Distributed Denial of Service (DDoS)

---

## 3. Amazon Inspector

**Purpose:** Automated vulnerability assessment

**What it scans**
- EC2 instances
- Amazon ECR container images
- AWS Lambda functions

**Finds**
- Common Vulnerabilities and Exposures (CVEs)
- Network exposure
- Insecure configurations

**Exam keyword:**  
Security vulnerabilities and compliance

---

## 4. Amazon Aurora & Aurora Global Database

### Amazon Aurora
- MySQL and PostgreSQL compatible
- 6 copies of data across 3 AZs
- Automatic storage scaling
- Fast failover

### Amazon Aurora Global Database
- Primary Region with secondary Regions
- Replication lag < 1 second
- Used for global reads and disaster recovery

**Exam keyword:**  
Globally distributed relational database

---

## 5. Amazon CloudFront

**Purpose:** Content Delivery Network (CDN)

**What it does**
- Caches content at edge locations
- Reduces latency
- Improves performance

**Common origins**
- Amazon S3
- Application Load Balancer
- EC2

**Use cases**
- Static websites
- Video streaming
- API acceleration

**Exam keyword:**  
Caching and edge delivery

---

## 6. Amazon API Gateway

**Purpose:** Managed service to create APIs

**Supports**
- REST APIs
- HTTP APIs
- WebSocket APIs

**Features**
- Authentication and authorization
- Throttling and rate limiting
- Caching
- Monitoring and logging

**Exam keyword:**  
Serverless API management

---

## 7. AWS Global Accelerator

**Purpose:** Improve global application performance and availability

**How it works**
- Uses static Anycast IP addresses
- Routes traffic to nearest healthy endpoint
- Uses AWS global network

**Supports**
- ALB
- NLB
- EC2
- Elastic IPs

**Exam keyword:**  
Static IP and global routing

---

## 8. Multi-AZ vs Read Replicas

### Multi-AZ
- Synchronous replication
- Spans multiple AZs in a single region
- Automatic failover
- Used for high availability

### Read Replicas
- Asynchronous replication
- Same AZ, Cross-AZ, or Cross-Region
- Used for read scaling
- Manual failover

**Exam keyword:**  
High availability vs scalability

---

## 9. AWS Auto Scaling Group (ASG)

**Purpose:** Automatically scale EC2 instances

**Core components**
- Launch template or configuration
- Auto Scaling group
- Scaling policies
- Health checks (EC2/ELB)

### Scaling Policies

#### Dynamic Scaling
- Reacts to **metric thresholds**
- CloudWatch metrics trigger scale-out/scale-in
- Example: Add 2 instances if CPU > 70%, remove 1 if CPU < 30%

#### Scheduled Scaling
- Adjust capacity **at specific times**
- Independent of metrics
- Example: Increase to 10 instances at 9 AM, decrease to 2 at 7 PM

#### Target Tracking Scaling
- Maintains a **specific metric at target value**
- Automatically adjusts instances to keep metric close to target
- Example: Target CPU utilization = 50%

**Comparison Table**

| Policy Type          | Trigger Type      | Use Case                                  | Notes                            |
|---------------------|-----------------|------------------------------------------|----------------------------------|
| Dynamic Scaling      | CloudWatch metric | Reactive scaling to load changes        | Threshold-based                  |
| Scheduled Scaling    | Time / schedule  | Predictable workloads                     | Pre-planned, ignores metrics     |
| Target Tracking      | CloudWatch metric | Maintain a metric at target value       | Auto-adjusts smoothly             |

**Exam keyword:**  
Automatic EC2 scaling

---

## 10. EC2 Instance Types

| Type                  | Description / Use Case                                      |
|----------------------|--------------------------------------------------------------|
| **On-Demand**         | Pay per hour, flexible, no upfront cost                     |
| **Reserved**          | Long-term commitment (1 or 3 years), lower cost            |
| **Spot**              | Unused capacity at lowest cost, can be interrupted          |
| **Dedicated Instances** | Runs on single-tenant hardware                              |
| **Dedicated Hosts**    | Physical server assigned to you, compliance and licensing   |
| **Instance Store**     | Ephemeral local storage, tied to the lifecycle of instance; very high I/O performance; data lost if instance stops |

**Exam keyword:**  
Choosing the right EC2 type for cost, performance, and compliance

---

## 11. AWS Storage Gateway

**Purpose:** Hybrid cloud storage service

### File Gateway
- File → Amazon S3
- Protocols: NFS, SMB
- Used for backups and hybrid file storage

### FSx File Gateway
- File → Amazon FSx
- Used for enterprise file systems

### Volume Gateway
- Block storage using iSCSI
- Cached volumes
- Stored volumes

### Tape Gateway
- Virtual tape library
- Stores data in S3 and Glacier

**Exam keyword:**  
Hybrid storage integration

---

## 12. Amazon Kinesis

**Purpose:** Real-time streaming data processing

### 12.1 Kinesis Data Streams (KDS)
- **Type:** Managed real-time data streaming service
- **How it works:** Producers send data into streams, consumers read in real-time
- **Use cases:** Telecom device status, IoT telemetry, real-time analytics
- **Exam keyword:** Real-time data ingestion and processing

### 12.2 Kinesis Data Firehose
- **Type:** Managed service to load streaming data into destinations (S3, Redshift, Elasticsearch)
- **How it works:** Automatically buffers, transforms, and loads data
- **Use cases:** Streaming ETL, dashboards, log delivery
- **Exam keyword:** Streaming ETL and delivery

### 12.3 Kinesis Data Analytics
- **Type:** Real-time SQL/Apache Flink analytics on streaming data
- **How it works:** Connects to KDS or Firehose, processes data
- **Use cases:** Anomaly detection, aggregates, real-time metrics
- **Exam keyword:** Real-time analytics on streaming data

### 12.4 Kinesis Video Streams
- **Type:** Stream video/audio from devices to AWS
- **How it works:** Captures live video/audio, stores in S3, process with ML/analytics
- **Use cases:** Security cameras, smart devices, video analytics
- **Exam keyword:** Streaming video data ingestion and processing

---

## 13. Amazon SQS (Simple Queue Service)

**Purpose:** Decoupling microservices and applications using message queues

### Types of Queues

| Type          | Features                                                                 | Throughput |
|---------------|--------------------------------------------------------------------------|------------|
| Standard      | At-least-once delivery, best-effort ordering                               | Unlimited  |
| FIFO          | Exactly-once processing, preserves order                                   | 300 messages/sec by default (can increase with batching) |

### FIFO Queue Throughput Example
- Default limit: 300 messages/sec
- Batch up to 10 messages/operation → 3,000 messages/sec
- Example calculation:
  - Required rate: 1,200 messages/sec
  - Batch size: 4 messages/operation
  - Operations/sec = 1,200 ÷ 4 = 300 → within FIFO limit
- ✅ Batching allows handling higher throughput while preserving order

### Use Cases
- Standard queues: asynchronous task processing, decoupling services
- FIFO queues: financial transactions, order processing, inventory management
- Can be combined with **Lambda** or **EC2 consumers**

### SQS FIFO Queue: Batching, Operation, and Throughput Explanation

This README explains the concepts of **operations, batching, and calculating effective throughput** in AWS SQS FIFO queues.

---

### 1. What is an Operation?
- An **operation** is one action performed on the SQS queue.
- Common operations include:
  - **SendMessage** → send a message to the queue
  - **ReceiveMessage** → read a message from the queue
  - **DeleteMessage** → remove a message after processing

**Example:** Sending 1 message = 1 operation

---

### 2. What is Batching?
- **Batching** means handling multiple messages in a single operation.
- SQS FIFO queues allow up to **10 messages per batch**.

**Example:** Sending 5 messages in one operation = 1 operation, 5 messages sent

---

### 3. Effective Throughput Calculation
- FIFO queues have a default **limit of 300 operations/sec**.

### Scenario 1: No Batching
- Each operation = 1 message
- Max operations/sec = 300
- Throughput = 300 × 1 = **300 messages/sec**

#### Scenario 2: Maximum Batching (10 messages per operation)
- Each operation = 10 messages
- Max operations/sec = 300
- Throughput = 300 × 10 = **3,000 messages/sec**

#### Scenario 3: Batching 4 Messages per Operation
- Each operation = 4 messages
- Max operations/sec = 300
- Throughput = 300 × 4 = **1,200 messages/sec**

---

#### 4. Why Choose 4 Messages per Operation?
- Peak load = 1,200 messages/sec
- Batch size = 4 messages → 300 × 4 = 1,200 messages/sec
- Using 10 messages per batch would exceed the requirement → not efficient

---

## 5. Key Takeaways
- **Operation** = 1 action (send/receive/delete)
- **Batch** = multiple messages in 1 operation
- **Throughput** = operations/sec × messages per operation
- Adjust **batch size** to match your required message rate


**Exam keyword:**  
Queue-based decoupling, FIFO ordering, batching for throughput

---

## 14. AWS Direct Connect

**Purpose:** Dedicated private network connection from your on-premises network to AWS.

**Why use it**
- Consistent, low-latency connectivity for hybrid workloads.
- Bypasses the public internet for secure data transfer.
- Predictable bandwidth options (1/10 Gbps dedicated, 50 Mbps – 10 Gbps hosted).
- Reduces outbound data transfer charges versus internet VPN.

**Connection workflow**
1. Request dedicated or hosted connection via AWS or a Direct Connect partner.
2. Establish physical cross-connect at the Direct Connect location.
3. Create virtual interface (VIF) on top of the physical link.
4. Configure BGP routing between on-premises router and AWS router.

**Virtual Interface Types**
| VIF Type | Connects To | Primary Use Case |
|----------|-------------|------------------|
| **Private** | VPC via Virtual Private Gateway or Direct Connect Gateway | Access private subnets and hybrid applications |
| **Public** | AWS public services (S3, DynamoDB, etc.) | Low-latency access to public endpoints without using the public internet |
| **Transit** | AWS Transit Gateway | Multi-VPC, multi-region connectivity at scale |

**High availability guidance**
- Provision redundant connections in separate Direct Connect locations.
- Terminate on separate customer routers and diverse providers.
- Combine with Site-to-Site VPN for automatic failover (VPN over DX or AWS VPN backup).
- Monitor link and BGP health with CloudWatch and Direct Connect metrics.

**Exam focus**
- Keyword: *Dedicated, private connectivity, bypass public internet*.
- Recognize Direct Connect Gateway for cross-region VPC connectivity.
- Remember data transfer savings and latency benefits.
- Design for resilience: two connections, diverse paths.

---

## 15. AWS Site-to-Site VPN

**Purpose:** Secure IPsec VPN connection between on-premises networks and AWS over the public internet.

**Core components**
- **Customer Gateway (CGW):** Your physical router or software appliance.
- **Virtual Private Gateway (VGW)** *or* **Transit Gateway:** AWS endpoint.
- **Two IPsec tunnels per VPN:** Provide automatic failover.

**Characteristics**
- Uses public internet; encryption handled by IPsec (AES-256, SHA-2).
- Supports static or dynamic routing (BGP recommended for failover).
- Throughput depends on internet path and device capacity (up to ~1.25 Gbps per tunnel).
- Deployable within minutes—ideal for quick hybrid connectivity.

**High availability patterns**
- Configure both tunnels across separate customer routers where possible.
- Create redundant VPN connections to different AWS Regions for disaster recovery.
- Optionally pair with Direct Connect for backup or overlay connectivity.

**Use cases**
- Rapid hybrid connectivity without waiting for dedicated circuits.
- Backup path for Direct Connect or SD-WAN fallbacks.
- Securely connecting multiple VPCs via Transit Gateway.

**Direct Connect vs Site-to-Site VPN (Exam Tip)**
| Feature | Direct Connect | Site-to-Site VPN |
|---------|----------------|------------------|
| Connectivity | Private dedicated fiber | Public internet (encrypted) |
| Provisioning time | Weeks (coordinate with carrier) | Minutes (self-service) |
| Bandwidth | Predictable (50 Mbps – 100 Gbps) | Variable (depends on internet) |
| Cost model | Higher fixed cost, lower data transfer | Pay per data + standard transfer out |
| Encryption | Not encrypted by default (MACsec optional) | IPsec encrypted |

**Exam focus**
- Keyword: *IPsec tunnels over the internet*.
- Two tunnels per VPN, BGP for automatic failover.
- Choose VPN for rapid deployment, Direct Connect for consistent high-bandwidth workloads.
- Hybrid best practice: Direct Connect primary + VPN backup.

---

## Exam Preparation Tips

- Identify keywords in questions (HA, scaling, security, global access)
- Eliminate services that don’t match the use case
- Focus on differences between similar services
- Understand **why** one service is chosen over another
- Remember distinctions:
  - **Kinesis:** Data Streams, Firehose, Analytics, Video Streams
  - **SQS:** FIFO vs Standard, batching for throughput

---

## End of Notes

Good luck with your **AWS Solutions Architect exam** 🚀
