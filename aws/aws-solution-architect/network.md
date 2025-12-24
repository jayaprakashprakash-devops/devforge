# Cloud Network Architecture Resources

This repository provides an overview of key networking resources used in cloud and hybrid architectures. Each resource is explained with its purpose, typical use cases, and relationships with other network components.

---

## Table of Contents

1. [VPC (Virtual Private Cloud)](#vpc-virtual-private-cloud)  
2. [Internet Gateway (IGW)](#internet-gateway-igw)  
3. [NAT Gateway](#nat-gateway)  
4. [Route Tables](#route-tables)  
5. [Network ACLs](#network-acls)  
6. [Security Groups](#security-groups)  
7. [Elastic IP](#elastic-ip)  
8. [VPN](#vpn)  
9. [Site-to-Site VPN](#site-to-site-vpn)  
10. [Direct Connect](#direct-connect)  
11. [VPC Peering](#vpc-peering)  
12. [Transit Gateway](#transit-gateway)  
13. [VPC Endpoint](#vpc-endpoint)  
14. [VPC PrivateLink](#vpc-privatelink)  
15. [Virtual Private Gateway (VGW)](#virtual-private-gateway-vgw)  
16. [Customer Gateway (CGW)](#customer-gateway-cgw)  

---

## VPC (Virtual Private Cloud)

**Definition:**  
A logically isolated virtual network in the cloud where you can launch resources such as compute, storage, and databases.

**Use Cases:**  
- Securely host applications.  
- Control IP addressing, subnets, and routing.  
- Enable hybrid cloud deployments.  

---

## Internet Gateway (IGW)

**Definition:**  
A horizontally scaled, redundant gateway that allows communication between instances in your VPC and the internet.

**Use Cases:**  
- Enable public access for web applications.  
- Connect VPC resources to the public internet.  

---

## NAT Gateway

**Definition:**  
Provides outbound internet access for private subnets while preventing inbound internet traffic.

**Use Cases:**  
- Allow private instances to download updates.  
- Securely access external services without exposing instances to the internet.  

---

## Route Tables

**Definition:**  
Defines rules that determine where network traffic from subnets is directed.

**Use Cases:**  
- Control traffic flow between subnets, IGWs, VGWs, and endpoints.  
- Enable hybrid connectivity and multi-VPC communication.  

---

## Network ACLs (NACLs)

**Definition:**  
A stateless firewall controlling inbound and outbound traffic at the subnet level.

**Use Cases:**  
- Provide an additional layer of security.  
- Allow or deny traffic for subnets based on IP, protocol, and port.  

---

## Security Groups

**Definition:**  
A stateful firewall that controls traffic to and from individual instances.

**Use Cases:**  
- Secure EC2 instances, databases, and other resources.  
- Define application-level access rules.  

---

## Elastic IP (EIP)

**Definition:**  
A static, public IPv4 address associated with a cloud resource.

**Use Cases:**  
- Assign a fixed IP to an instance or load balancer.  
- Maintain consistent IP for external communication.  

---

## VPN

**Definition:**  
A Virtual Private Network enables encrypted connections over the public internet between remote users or networks and your cloud network.

**Use Cases:**  
- Secure remote employee access.  
- Temporary encrypted connectivity for migrations or testing.  

---

## Site-to-Site VPN

**Definition:**  
Connects an on-premises network to a VPC using an encrypted IPsec connection.

**Use Cases:**  
- Extend on-premises data centers to the cloud.  
- Secure inter-office connectivity.  
- Backup and disaster recovery connectivity.  

---

## Direct Connect

**Definition:**  
A dedicated, private network connection between on-premises and cloud networks.

**Use Cases:**  
- Low-latency, high-bandwidth workloads.  
- Secure hybrid cloud connections.  
- Private connection to cloud services.  

---

## VPC Peering

**Definition:**  
Enables private communication between two VPCs using internal IPs.

**Use Cases:**  
- Connect multiple VPCs across accounts or regions.  
- Share resources privately without internet exposure.  

---

## Transit Gateway

**Definition:**  
A hub-and-spoke model that connects multiple VPCs and on-premises networks through a single gateway.

**Use Cases:**  
- Centralize routing for multi-VPC networks.  
- Simplify complex peering architectures.  
- Support large-scale hybrid networks.  

---

## VPC Endpoint

**Definition:**  
Provides private access from a VPC to supported AWS services without using the internet.

**Use Cases:**  
- Access AWS services like S3 or DynamoDB securely.  
- Reduce exposure to the public internet.  
- Lower latency and egress costs.  

---

## VPC PrivateLink

**Definition:**  
Enables private access to services hosted in other VPCs or AWS accounts.

**Use Cases:**  
- Securely connect to SaaS applications.  
- Expose internal services to specific VPCs.  
- Multi-account service sharing.  

---

## Virtual Private Gateway (VGW)

**Definition:**  
A virtual router on AWS side for VPN or Direct Connect connections to VPCs.

**Use Cases:**  
- Establish Site-to-Site VPNs.  
- Connect multiple VPCs to on-premises networks.  
- Enable hybrid cloud architectures.  

---

## Customer Gateway (CGW)

**Definition:**  
Represents the customer side of a VPN connection in a hybrid network.

**Use Cases:**  
- Define on-premises VPN device settings.  
- Enable connectivity wit
