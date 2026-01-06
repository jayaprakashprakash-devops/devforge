# AWS Storage Services – Protocols, Use Cases, OS Support & Storage Gateway Mapping

This document provides a concise, exam-ready overview of **AWS storage services**, including:
- Protocols used
- Operating system support
- Common use cases
- Storage Gateway integration

---

## 📁 Amazon EFS (Elastic File System)

### Protocol
- **NFS v4.1**

### OS Support
- ✅ Linux
- ❌ Windows

### Use Cases
- Shared file storage for EC2 Auto Scaling groups
- Web applications (WordPress, Drupal)
- Container storage (ECS, EKS)
- Big data and analytics
- Lift-and-shift Linux applications

### Key Notes
- Fully managed
- Multi-AZ by default
- Automatically scales

### Exam Memory Tip
> **Shared Linux file system → EFS**

---

## ⚡ Amazon FSx for Lustre

### Protocol
- **Lustre**

### OS Support
- ✅ Linux
- ❌ Windows

### Use Cases
- High Performance Computing (HPC)
- Machine learning training
- Media rendering
- Genomics & financial modeling

### Key Notes
- Extremely high throughput & low latency
- Can be linked directly with Amazon S3

### Exam Memory Tip
> **Extreme performance + Linux → FSx for Lustre**

---

## 🪟 Amazon FSx for Windows File Server

### Protocol
- **SMB**

### OS Support
- ✅ Windows
- ❌ Linux

### Use Cases
- Windows file shares
- Home directories
- Microsoft workloads (AD, SQL Server, IIS)
- Lift-and-shift Windows applications

### Key Notes
- Native Windows file system
- Active Directory integration
- NTFS permissions supported

### Exam Memory Tip
> **Windows file sharing → FSx for Windows**

---

## 🏢 Amazon FSx for NetApp ONTAP

### Protocols
- **NFS**
- **SMB**
- **iSCSI**

### OS Support
- ✅ Linux
- ✅ Windows

### Use Cases
- Enterprise workloads (SAP, Oracle)
- Hybrid cloud & migrations from NetApp
- Workloads needing snapshots, replication, deduplication
- Multi-protocol access

### Key Notes
- Advanced data management
- Enterprise-grade features

### Exam Memory Tip
> **Enterprise + multi-protocol → FSx ONTAP**

---

## 💾 Amazon EBS (Elastic Block Store)

### Protocol
- **Block storage (iSCSI-like)**

### OS Support
- ✅ Linux
- ✅ Windows

### Use Cases
- Databases (RDS, self-managed DBs)
- Boot volumes
- Low-latency transactional workloads

### Key Notes
- Attached to **one EC2 instance at a time** (except io1/io2 Multi-Attach)
- High IOPS and low latency
- AZ-specific

### Exam Memory Tip
> **Single-instance block storage → EBS**

---

## 🪣 Amazon S3 (Simple Storage Service)

### Protocol
- **HTTP / HTTPS**

### OS Support
- ✅ All operating systems (object storage)

### Use Cases
- Backup & archival
- Data lakes
- Static website hosting
- Media storage
- Analytics & ML datasets

### Key Notes
- Object storage (not a file system)
- Virtually unlimited scale
- 11 9s durability
- Lifecycle policies supported

### Exam Memory Tip
> **Object storage → S3**

---

## 🔌 AWS Storage Gateway – Supported Storage Types

### 1️⃣ File Gateway

| Backend Storage | Protocol | OS Access |
|---------------|----------|-----------|
| Amazon S3 | NFS / SMB | Linux / Windows |

**Use Case**
- File-based access to S3
- On-prem applications writing files to S3

---

### 2️⃣ Volume Gateway

| Mode | Backend Storage | Access Type |
|----|---------------|------------|
| Cached Volumes | Amazon S3 | Block |
| Stored Volumes | Amazon S3 | Block |

**Use Case**
- Low-latency block storage
- Hybrid backups

---

### 3️⃣ Tape Gateway

| Backend Storage | Emulation |
|---------------|----------|
| Amazon S3 / Glacier | Virtual Tape Library (VTL) |

**Use Case**
- Backup using existing tape-based tools

---

## 🧠 Storage Gateway Exam Memory Rules

- **File Gateway → S3 (NFS/SMB)**
- **Volume Gateway → S3 (Block)**
- **Tape Gateway → S3 / Glacier (VTL)**

---

## 📊 Quick Comparison Table (Exam Gold)

| Service | Storage Type | Protocol | OS |
|------|------------|----------|----|
| EFS | File | NFS | Linux |
| FSx Lustre | File | Lustre | Linux |
| FSx Windows | File | SMB | Windows |
| FSx ONTAP | File / Block | NFS / SMB / iSCSI | Linux & Windows |
| EBS | Block | Block | Linux & Windows |
| S3 | Object | HTTP/HTTPS | All |

---

## ✅ Final One-Line Summary

- **EFS** → Shared Linux file system  
- **FSx Lustre** → Fastest compute storage  
- **FSx Windows** → Windows file sharing  
- **FSx ONTAP** → Enterprise & hybrid workloads  
- **EBS** → Block storage for EC2  
- **S3** → Object storage & data lake  
- **Storage Gateway** → Hybrid access to S3  

---

📌 *Ideal for AWS Solutions Architect Associate exam preparation.*
