# AWS Solutions Architect Exam Prep

This repository contains summarized solutions and best practices for common AWS scenarios, organized by service. It is intended for exam preparation and quick reference.

---

## Table of Contents

1. [Amazon S3](#amazon-s3)  
2. [Amazon EC2](#amazon-ec2)  
3. [Amazon RDS / Databases](#amazon-rds--databases)  
4. [Amazon SQS](#amazon-sqs)  
5. [Networking / Global Access](#networking--global-access)  
6. [Analytics](#analytics)  
7. [Data Lake & Security](#data-lake--security)  
8. [Other Services & Storage](#other-services--storage)  

---

## Amazon S3

### 1. Serverless thumbnail generation
- **Scenario:** Images uploaded to S3; thumbnails need to be generated automatically.  
- **Solution:**  
  - S3 Event Notifications → AWS Lambda → generate thumbnails → store in another S3 bucket.  

### 2. Reducing KMS request costs
- **Scenario:** High-frequency S3 uploads with SSE-KMS, high KMS costs.  
- **Solution:**  
  - Enable **S3 Bucket Keys** to reduce KMS requests.  

### 3. WORM / Compliance
- **Scenario:** Regulatory compliance requires immutable storage.  
- **Solution:**  
  - Enable **S3 Object Lock in Compliance mode**.  
  - Optional: combine with **S3 Glacier** for archival.  

### 4. Cross-region replication & encryption
- **Scenario:** Analytics platform with serverless SQL queries, encryption, and DR.  
- **Solution:**  
  - **S3 SSE-KMS encryption**  
  - **S3 Cross-Region Replication (CRR)**  
  - **Amazon Athena** for querying  

### 5. Archival solution
- **Scenario:** Healthcare startup storing patient health records.  
- **Solution:**  
  - **S3 Lifecycle policies** → transition objects to **Glacier / Deep Archive**  
  - Optional: **S3 Object Lock in Compliance mode**  

### 6. High-volume media storage
- **Scenario:** 10 TB high-I/O, 450 TB active, 900 TB archival media.  
- **Solution:**  
  | Requirement | Service |  
  |------------|---------|  
  | High-performance | FSx for Lustre / EBS io2 |  
  | Active media | S3 Standard / Intelligent-Tiering |  
  | Archival | S3 Glacier / Glacier Deep Archive |  

---

## Amazon EC2

### 1. Internet connectivity
- **Solution:**  
  1. Subnet route table: `0.0.0.0/0 → IGW`  
  2. Instance has **public IPv4 or Elastic IP**  

### 2. Auto recovery for frozen instance
- **Solution:** Enable **EC2 Auto Recovery** via CloudWatch alarms.  

### 3. High-performance PostgreSQL
- **Solution:** EC2 instance + **EBS io2 / io2 Block Express** for high IOPS.

---

## Amazon RDS / Databases

### Multi-AZ failover
- **Solution:** Automatic failover to standby AZ; endpoint unchanged; minimal downtime (~1–2 min).

---

## Amazon SQS

### Handling message failures
- **Solution:**  
  - Configure **Dead-Letter Queue (DLQ)**  
  - Set maximum receive count; failed messages move to DLQ  
  - Inspect and reprocess after fixes  

---

## Networking / Global Access

### Global TCP + UDP application
- **Solution:**  
  - **Amazon Global Accelerator** → accelerates TCP/UDP globally  
  - **Network Load Balancer (NLB)** in each region for TCP EC2 traffic  

---

## Analytics

### Query historical and real-time S3 data
- **Solution:**  
  - **Amazon Athena** for serverless SQL queries  
  - **S3 SSE-KMS encryption**  
  - **S3 Cross-Region Replication** for business continuity  

---

## Data Lake & Security

### Detect unintended access
- **Solution:**  
  - **AWS IAM Access Analyzer** → analyzes IAM and resource policies  
  - Detects public or cross-account access  

---

## Other Services & Storage

### Hybrid on-premises + S3 low-latency access
- **Solution:** **AWS Storage Gateway – File Gateway**  
  - Caches frequently accessed S3 files locally for low-latency access  

---

## Notes for Exam Prep

- Prefer **serverless / fully managed solutions** for operational efficiency.  
- **S3 Object Lock, Glacier, SSE-KMS** → compliance questions.  
- **DLQs, Auto Recovery** → reliability and resilience.  
- **Global Accelerator + NLB** → multi-region / hybrid access.  
- **Athena + S3** → serverless analytics.

---

