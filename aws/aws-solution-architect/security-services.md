# One-Page AWS Security Cheat Sheet

A quick reference for core AWS security services, their purpose, integrations,
and when to use them.

---

## 1. AWS WAF (Web Application Firewall)
**Purpose:** Protect web apps and APIs from common web attacks (Layer 7)

**Protects Against:**
- SQL Injection
- Cross-Site Scripting (XSS)
- Bad bots & request floods

**Integrates With:**
- Application Load Balancer (ALB)
- Amazon CloudFront
- Amazon API Gateway
- AWS AppSync

**Best Used When:**
- You expose web apps or APIs to the internet

---

## 2. AWS Shield
**Purpose:** DDoS protection (Layer 3 & 4)

**Types:**
- Shield Standard (default, free)
- Shield Advanced (paid, advanced response)

**Integrates With:**
- CloudFront
- Elastic Load Balancing (ALB, NLB, CLB)
- Route 53
- Elastic IP

**Best Used When:**
- You have public-facing applications

---

## 3. Amazon Inspector
**Purpose:** Vulnerability management

**Finds:**
- CVEs
- Missing patches
- Insecure configurations

**Integrates With:**
- Amazon EC2
- Amazon ECR (container images)
- AWS Lambda
- ECS / EKS (via ECR)

**Best Used When:**
- You run EC2, containers, or Lambda workloads

---

## 4. Amazon GuardDuty
**Purpose:** Threat detection and monitoring

**Detects:**
- Compromised credentials
- Unusual API calls
- Crypto-mining
- Data exfiltration

**Data Sources:**
- CloudTrail
- VPC Flow Logs
- DNS logs
- EKS audit logs
- S3 data events

**Best Used When:**
- You need continuous security monitoring

---

## 5. Amazon Macie
**Purpose:** Sensitive data discovery & protection

**Protects:**
- PII / PHI / Financial data

**Integrates With:**
- Amazon S3
- AWS Organizations
- Security Hub
- EventBridge

**Best Used When:**
- You store sensitive data in S3

---

## 6. AWS Security Lake
**Purpose:** Centralized security log storage & analysis

**Collects From:**
- AWS WAF
- GuardDuty
- Macie
- CloudTrail
- VPC Flow Logs
- Route 53 Resolver logs

**Best Used When:**
- You want centralized threat analysis

---

## Layered Security Model (Quick View)

| Layer        | AWS Service |
|-------------|-------------|
| Edge        | CloudFront + WAF + Shield |
| Network     | Shield, VPC Flow Logs |
| Application | WAF |
| Workload    | Inspector |
| Detection   | GuardDuty |
| Data        | Macie |
| Analytics   | Security Lake |

---

## Common Interview Questions

**Q: Does AWS WAF work with NLB?**  
❌ No — only ALB, CloudFront, API Gateway, AppSync

**Q: Which service detects compromised IAM credentials?**  
✅ GuardDuty

**Q: Which service finds vulnerabilities in EC2?**  
✅ Inspector

**Q: Which service protects S3 data?**  
✅ Macie

**Q: Which service centralizes security logs?**  
✅ Security Lake

---

## Typical Secure Architecture

Client  
→ CloudFront + WAF + Shield  
→ ALB  
→ EC2 / ECS / EKS  
→ Logs → Security Lake

---

## Key Takeaway

AWS security works best when services are **combined**, not used in isolation.
Defense-in-depth is the AWS security best practice.

