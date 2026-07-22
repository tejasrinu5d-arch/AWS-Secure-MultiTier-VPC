# AWS Secure Multi-Tier VPC

![AWS](https://img.shields.io/badge/AWS-VPC-orange)
![Amazon EC2](https://img.shields.io/badge/Amazon-EC2-blue)
![Amazon VPC](https://img.shields.io/badge/Amazon-VPC-green)
![Security](https://img.shields.io/badge/Cloud-Security-red)
![Networking](https://img.shields.io/badge/AWS-Networking-yellow)
![License](https://img.shields.io/badge/License-MIT-blue)

---

# Project Overview

This project demonstrates the design and implementation of a **secure multi-tier Virtual Private Cloud (VPC)** architecture on Amazon Web Services (AWS).

The environment follows AWS networking best practices by separating resources into **public** and **private** subnets, enabling secure internet access through an **Internet Gateway** and **NAT Gateway**, while protecting private resources from direct internet exposure.

This project simulates a production-ready cloud networking environment commonly used by organizations to securely host web applications, backend services, and databases.

---

# Project Objectives

- Design a custom Amazon VPC
- Configure public and private subnets
- Implement secure routing using Route Tables
- Deploy an Internet Gateway
- Configure outbound internet access using a NAT Gateway
- Launch EC2 instances in both public and private subnets
- Secure communication using Security Groups
- Validate secure connectivity between network components
- Demonstrate AWS networking best practices

---

# AWS Services Used

- Amazon VPC
- Amazon EC2
- Internet Gateway
- NAT Gateway
- Elastic IP
- Route Tables
- Security Groups
- IPv4 CIDR
- Public Subnets
- Private Subnets

---

# Architecture

> **Architecture Diagram**

```
                    Internet
                        │
                Internet Gateway
                        │
          ───────────────────────────
                Public Route Table
                        │
        ┌────────────────────────────────┐
        │                                │
 Public Subnet                    Private Subnet
 (10.0.1.0/24)                    (10.0.2.0/24)
        │                                │
        │                         Private Route Table
        │                         0.0.0.0/0 → NAT Gateway
        │                                │
        │                                │
+-------------------+            +----------------------+
| Bastion Host EC2  |            | Private App Server   |
| Public IP (EIP)   |            | No Public IP         |
+-------------------+            +----------------------+
        │
        │
+-------------------+
| NAT Gateway (EIP) |
+-------------------+
```

---

# Network Design

## VPC

| Component | Configuration |
|------------|---------------|
| CIDR | 10.0.0.0/16 |
| Region | ap-south-1 |
| DNS Resolution | Enabled |
| DNS Hostnames | Enabled |

---

## Public Subnet

| Property | Value |
|-----------|-------|
| CIDR | 10.0.1.0/24 |
| Availability Zone | ap-south-1a |
| Internet Access | Yes |

---

## Private Subnet

| Property | Value |
|-----------|-------|
| CIDR | 10.0.2.0/24 |
| Availability Zone | ap-south-1b |
| Internet Access | Outbound Only (NAT Gateway) |

---

# Route Tables

## Public Route Table

| Destination | Target |
|-------------|--------|
| 10.0.0.0/16 | local |
| 0.0.0.0/0 | Internet Gateway |

---

## Private Route Table

| Destination | Target |
|-------------|--------|
| 10.0.0.0/16 | local |
| 0.0.0.0/0 | NAT Gateway |

---

# EC2 Deployment

## Public EC2

- Bastion Host
- Public IP
- SSH Accessible
- Used for administration

---

## Private EC2

- Private Application Server
- No Public IP
- Accessible only through Bastion Host
- Internet access via NAT Gateway

---

# Security Implementation

## Security Groups

### Bastion Host

- SSH (22) from My IP

### Private EC2

- SSH (22) from Bastion Host Security Group

---

# Connectivity Validation

| Test | Status |
|-------|--------|
| Laptop → Bastion Host | ✅ Success |
| Laptop → Private EC2 | ❌ Blocked |
| Bastion → Private EC2 | ✅ Success |
| Private EC2 → Internet | ✅ Success |
| Public EC2 → Internet | ✅ Success |

---

# Testing Performed

Successfully verified:

- SSH connectivity
- Internet Gateway functionality
- NAT Gateway outbound routing
- Route Table associations
- Security Group configuration
- Private subnet isolation
- Internet connectivity from private resources

Commands executed:

```bash
ping -c 4 google.com

curl ifconfig.me
```

---

# Security Best Practices Demonstrated

- Network segmentation
- Public and private subnet isolation
- Least privilege Security Groups
- Bastion Host architecture
- No public IP for backend servers
- Controlled outbound internet access
- Static public IP using Elastic IP

---

# Learning Outcomes

Through this project I gained hands-on experience with:

- Amazon VPC
- CIDR Planning
- Subnet Design
- Route Tables
- Internet Gateway
- NAT Gateway
- Elastic IP
- EC2 Networking
- Security Groups
- Secure AWS Infrastructure Design

---

# Repository Structure

```
AWS-Secure-MultiTier-VPC/
│
├── README.md
├── LICENSE
├── Architecture/
├── Screenshots/
├── Report/
└── Images/
```

---

# Future Improvements

- Multi-AZ deployment
- Auto Scaling Group
- Application Load Balancer
- Network ACL implementation
- VPC Flow Logs
- AWS Systems Manager Session Manager
- VPC Endpoints
- Transit Gateway integration

---

# Author

**E. Teja Srinivas**

B.Tech Computer Science Engineering (Cyber Security)

Cloud Security | AWS | Networking | Linux

GitHub:
https://github.com/tejasrinu5d-arch

LinkedIn:
www.linkedin.com/in/tejasrinucloud

---

# License

This project is licensed under the MIT License.
