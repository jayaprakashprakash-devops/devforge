# AWS Certified Solutions Architect – Exam Prep Notes

This README consolidates key AWS services, best practices, and exam-focused scenarios for quick revision.

---

<details>
<summary>1. Amazon S3 & Storage</summary>

### **EBS Backup Safety**
- Use **EBS Recycle Bin** with retention rules (e.g., 7 days) to prevent accidental permanent deletion of snapshots.

### **S3 Object Lock**
- Use **S3 Object Lock in Compliance Mode** to enforce **WORM policies** for regulatory compliance.  

### **Data Lake Analytics**
- Store raw data in **S3**.  
- Run **SQL queries** via **Amazon Athena**.  
- Ensure **encryption at rest** with **SSE-KMS** or S3-managed encryption.

### **Archival / Compliance**
- **S3 Glacier** or **S3 Glacier Deep Archive** for long-term storage.  
- Integrate **lifecycle policies** for automatic archival.  

</details>

<details>
<summary>2. EC2, Auto Scaling & High Availability</summary>

### **Reliable Patching / Maintenance**
- **Systems Manager Maintenance Windows** to safely remove instances from **ALB** during patching.  
- **AWS Systems Manager Automation** (`AWSEC2-PatchLoadBalancerInstance`) for automated patching workflows.

### **Temporary SSH Access**
- **EC2 Instance Connect** → injects temporary public key; ephemeral SSH access.  
- **AWS Systems Manager Session Manager (SSM)** → ephemeral browser/CLI access, fully auditable.

### **Critical Workloads – Auto Scaling**
- Example: Healthcare web app for ambulances.  
- **Auto Scaling Group Configuration:**  
  - **Minimum:** 4 instances (2 per AZ)  
  - **Maximum:** 6 instances  
  - **Multi-AZ deployment** for high availability.  
- **Elastic Load Balancer (ALB)** distributes traffic across instances.

</details>

<details>
<summary>3. Networking & Private Connectivity</summary>

### **Private SaaS Connectivity**
- **AWS PrivateLink** → create a private endpoint in your VPC.  
- Traffic remains **within AWS network**, avoiding public internet.  
- Prevents **unsolicited inbound traffic**.

### **Internet Access for EC2**
- EC2 requires:  
  1. **Public IP / Elastic IP**  
  2. **Route table** configured with **Internet Gateway (IGW)**

</details>

<details>
<summary>4. Real-Time Data Processing / Streaming</summary>

### **Near-Real-Time Streaming**
- **Amazon Kinesis Data Streams** → high-throughput ingestion, multiple consumers.  
- **AWS Lambda / Kinesis Data Analytics** → process, cleanse, transform data.  
- Store results in **DynamoDB** for **low-latency queries**.

### **Use Case Example**
- E-commerce clickstream analytics:  
  - Click events → **S3 raw zone**  
  - **Athena** or **Kinesis Analytics** for SQL-based sanity checks.

</details>

<details>
<summary>5. DevOps / Security / Access</summary>

### **Ephemeral Access**
- **EC2 Instance Connect** → temporary SSH keys, automatically removed.  
- **Session Manager (SSM)** → browser/CLI sessions, auditable, ephemeral.

### **Patch & Scale Management**
- **ALB + ASG + Systems Manager** → automated patching without downtime.

### **Prevent Snapshot Loss**
- Use **EBS Recycle Bin** → retention rules prevent accidental deletion.

</details>

<details>
<summary>6. Analytics / Machine Learning</summary>

### **Customer Call Sentiment Analysis**
- Audio stored in **S3** → **Transcribe** → convert speech to text.  
- **Comprehend** → sentiment analysis on text.  
- Store output in **S3 / Parquet / Athena** for **ad-hoc queries**.  

### **Other Examples**
- IoT sensor analytics → Kinesis → Lambda → S3 → Athena.  
- Serverless dashboards → API Gateway + Lambda + DynamoDB.

</details>

<details>
<summary>7. CloudFront, ELB, API Gateway – Exam Guide</summary>

| Service | Role | Layer | Use Cases | How They Connect |
|--------|------|-------|-----------|----------------|
| **CloudFront** | CDN / global caching | Edge | Website acceleration, video streaming, static/dynamic content | Can front **ALB** or **API Gateway** for global low-latency access |
| **ELB** | Load balancing | L4/L7 | Distribute traffic to EC2 / ECS / Lambda | Receives requests from **CloudFront** or clients; distributes to compute resources |
| **API Gateway** | Managed API endpoints | L7 | REST APIs, WebSocket APIs, mobile apps | Fronted by **CloudFront**; invokes **Lambda, ALB, HTTP backends** |

### **Key Points / Relationships**
- **CloudFront** → reduces latency globally, caches content.  
- **ELB** → ensures high availability and distributes traffic.  
- **API Gateway** → exposes serverless APIs, manages auth, throttling, caching.  
- **Typical Architecture:**
