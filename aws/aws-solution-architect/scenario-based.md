# AWS Solutions Architect Scenario-Based Questions (Correct Answers with Full Explanation)

This repository contains **scenario-based questions** for the AWS Solutions Architect Associate exam. Only the **correct answers** are listed, along with **full explanations** for each scenario. The scenarios are grouped by **AWS service/resource**.

---

## Table of Contents

- [EC2 & Instance Store](#ec2--instance-store)
- [Amazon S3 & Glacier](#amazon-s3--glacier)
- [AWS WAF](#aws-waf)
- [EC2 Instance Connect](#ec2-instance-connect)
- [AWS Systems Manager](#aws-systems-manager)
- [Amazon DynamoDB Global Tables](#amazon-dynamodb-global-tables)
- [AWS Global Accelerator & Network Load Balancer](#aws-global-accelerator--network-load-balancer)

---

## EC2 & Instance Store

**Scenario:**  
A media company needs **high-performance storage (≥10 TB)** for processing large video files.

**Correct Answer:**  
- **Amazon EC2 instance store**

**Explanation:**  
- EC2 instance store provides **direct-attached ephemeral storage** with **ultra-low latency and very high I/O performance**, making it ideal for **processing large video files**.  
- Data stored on instance store is **temporary** and lost if the instance stops or terminates, so it is suitable only for **processing workloads**, not for long-term storage.

---

## Amazon S3 & Glacier

**Scenario:**  
The company requires **450 TB of durable storage** for active media content and **900 TB for archival of legacy data**.

**Correct Answers:**  
- **Amazon S3 Standard** for active media storage  
- **Amazon S3 Glacier / Glacier Deep Archive** for archival storage  

**Explanation:**  
- **S3 Standard** provides **11 nines of durability** and high availability, ideal for frequently accessed content.  
- **S3 Glacier / Deep Archive** is extremely **cost-effective** for rarely accessed legacy data.  
- Use **lifecycle policies** to automatically transition objects from S3 Standard to Glacier for long-term archival.

---

## AWS WAF

**Scenario:**  
Protect a web application containing **sensitive customer data** from cyberattacks.

**Correct Answer:**  
- Deploy **AWS WAF** in front of the application (via **ALB or CloudFront**) and configure **managed and custom rules**.

**Explanation:**  
- AWS WAF blocks **common web exploits** such as **SQL injection, cross-site scripting (XSS), and bots** before requests reach the EC2 instances.  
- Logging to **CloudWatch** or **S3** allows monitoring and auditing.  
- CloudFront integration improves **global performance and availability**.

---

## EC2 Instance Connect

**Scenario:**  
Enable **temporary SSH access** to EC2 instances via the **AWS Management Console**.

**Correct Answer:**  
- Use **EC2 Instance Connect** to inject **temporary public keys** and allow SSH access using the instance’s **public IP address**.

**Explanation:**  
- EC2 Instance Connect generates **ephemeral SSH keys** valid for a short period (typically 60 seconds).  
- No permanent key distribution is needed.  
- CloudTrail logs all access events for auditing.  
- Ideal for **temporary, auditable SSH access**.

---

## AWS Systems Manager

**Scenario:**  
Safely patch EC2 instances while preserving **service availability**.

**Correct Answers:**  
- Configure **Systems Manager Maintenance Windows** to coordinate patching and remove instances from the ALB during the defined window  
- Use **AWS Systems Manager Automation** with the **AWSEC2-PatchLoadBalancerInstance** document to manage patching  

**Explanation:**  
- **Maintenance Windows** schedule patching during a specific time while ensuring traffic is drained from ALB targets.  
- **Automation documents** (SSM documents) manage patching and ALB deregistration/re-registration, preventing service disruptions.  
- This combination ensures **compliance, operational safety, and high availability**.

---

## Amazon DynamoDB Global Tables

**Scenario:**  
A global e-commerce platform requires **low-latency access** and **globally synchronized order data**.

**Correct Answer:**  
- Migrate order data to **Amazon DynamoDB Global Tables** and deploy the application in each region to connect to the **local DynamoDB replica**.

**Explanation:**  
- DynamoDB Global Tables provide **multi-master replication** across regions, enabling **local reads and writes** with minimal latency.  
- Updates propagate automatically to all other regions, keeping data **globally synchronized**.  
- Fully managed, highly scalable, and ideal for **multi-region applications**.

---

## AWS Global Accelerator & Network Load Balancer

**Scenario:**  
A hybrid global application requires **low-latency, highly available access** for:  
- TCP-based EC2 workloads across regions  
- UDP-based on-premises workloads  

**Correct Answers:**  
- Configure an **AWS Global Accelerator** standard accelerator and register **TCP-based EC2 workloads** behind the load balancers  
- Create **Network Load Balancers (NLBs)** in each region to handle:  
  - TCP EC2 traffic  
  - UDP traffic to **on-premises endpoints via IP-based target groups**

**Explanation:**  
- **Global Accelerator** uses the **AWS global network** to route traffic to the **closest healthy endpoint**, reducing latency.  
- **NLBs** efficiently handle both TCP and UDP workloads and integrate with **on-premises resources** for hybrid architectures.  
- Ensures **high availability, low latency, and reliable access** globally.

---

## License
This repository is for **educational purposes** to prepare for the **AWS Solutions Architect Associate exam**.
