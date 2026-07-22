# AWS Secure Multi-Tier VPC Project Report

**Project:** AWS Secure Multi-Tier VPC with Public and Private Subnets

**Prepared By:**  
**E. Teja Srinivas**

Bachelor of Technology  
Computer Science Engineering (Cyber Security)

Sri Venkateswara College of Engineering  
Tirupati, Andhra Pradesh, India

---

# Table of Contents

1. Abstract
2. Introduction
3. Project Objectives
4. AWS Services Used
5. Architecture Overview
6. Network Design
7. Implementation
8. Security Configuration
9. Connectivity Testing
10. Screenshots
11. Best Practices
12. Cost Optimization
13. Learning Outcomes
14. Conclusion

---

# 1. Abstract

This project demonstrates the implementation of a secure multi-tier network architecture using Amazon Web Services (AWS). A custom Virtual Private Cloud (VPC) was designed with separate public and private subnets to provide secure isolation between internet-facing and internal resources.

The public subnet hosts a Bastion Host that allows secure administrative access, while the private subnet hosts an application server without a public IP address. Internet connectivity for public resources is provided through an Internet Gateway, whereas outbound internet access for private resources is enabled using a NAT Gateway.

Security Groups were configured according to the principle of least privilege, ensuring that only required traffic was permitted. Route Tables were configured to direct traffic appropriately, enabling secure communication between AWS resources while preventing unauthorized external access.

Connectivity tests validated secure SSH access, internet connectivity, and proper routing. The resulting architecture closely resembles a production-ready AWS networking environment and demonstrates practical knowledge of cloud networking, security, and infrastructure design.

---

# 2. Introduction

Amazon Virtual Private Cloud (Amazon VPC) is a networking service that allows users to create logically isolated virtual networks inside AWS. A VPC enables complete control over networking components including IP address ranges, subnets, route tables, internet gateways, and security controls.

Organizations use VPCs to deploy secure cloud infrastructure while maintaining complete control over communication between applications and the internet.

One of the most common production architectures is a multi-tier VPC, where internet-facing components are placed in public subnets while backend services remain isolated in private subnets.

This project demonstrates the implementation of such an architecture using AWS networking services.

---

# 3. Project Objectives

The primary objectives of this project are:

- Design a custom Amazon VPC
- Configure IPv4 CIDR addressing
- Create public and private subnets
- Configure Route Tables
- Deploy an Internet Gateway
- Allocate Elastic IP addresses
- Configure a NAT Gateway
- Launch EC2 instances
- Configure Security Groups
- Validate secure connectivity
- Demonstrate secure outbound internet access
- Follow AWS networking best practices

---

# 4. AWS Services Used

| AWS Service | Purpose |
|-------------|---------|
| Amazon VPC | Creates an isolated virtual network |
| Amazon EC2 | Virtual servers used for Bastion Host and Private Application Server |
| Internet Gateway | Provides internet connectivity for public resources |
| NAT Gateway | Enables outbound internet access for private resources |
| Elastic IP | Provides static public IP addresses |
| Route Tables | Controls traffic routing inside the VPC |
| Security Groups | Instance-level virtual firewall |
| Subnets | Logically separate the VPC into public and private networks |

---

# 5. Architecture Overview

The architecture consists of a custom VPC with two separate subnets.

The Public Subnet contains:

- Bastion Host
- NAT Gateway
- Elastic IP

The Private Subnet contains:

- Private Application Server

The Internet Gateway provides internet connectivity to the public subnet.

The NAT Gateway allows outbound internet access for the private subnet without exposing private resources directly to the internet.

The Bastion Host acts as the secure entry point for administrators who need to manage private infrastructure.

---

## Architecture Diagram

> Insert the generated `architecture.png` here.

Example:

```
Internet
    │
Internet Gateway
    │
──────────────
Public Subnet
│
├── Bastion Host
│
└── NAT Gateway
      │
──────────────
Private Subnet
│
└── Private App Server
```

---

# 6. Network Design

## Virtual Private Cloud

| Property | Value |
|----------|-------|
| Name | Secure-MultiTier-VPC |
| CIDR | 10.0.0.0/16 |
| DNS Resolution | Enabled |
| DNS Hostnames | Enabled |

The VPC provides logical isolation from other AWS customers while allowing complete control over networking components.

---

## Public Subnet

| Property | Value |
|----------|-------|
| Name | Public-Subnet-1A |
| CIDR | 10.0.1.0/24 |
| Availability Zone | ap-south-1a |

Resources placed inside the public subnet receive internet connectivity through the Internet Gateway.

The Bastion Host and NAT Gateway were deployed inside this subnet.

---

## Private Subnet

| Property | Value |
|----------|-------|
| Name | Private-Subnet-1B |
| CIDR | 10.0.2.0/24 |
| Availability Zone | ap-south-1b |

The private subnet contains backend resources that should never be directly accessible from the internet.

Only outbound internet traffic is permitted through the NAT Gateway.

# 7. Implementation

This section describes the complete implementation process used to build the AWS Secure Multi-Tier VPC architecture.

The deployment follows AWS networking best practices by separating internet-facing resources from backend resources using public and private subnets.

---

## Step 1 – Create a Custom VPC

A new Virtual Private Cloud (VPC) was created to provide an isolated networking environment.

### Configuration

| Property | Value |
|----------|-------|
| Name | Secure-MultiTier-VPC |
| IPv4 CIDR | 10.0.0.0/16 |
| DNS Resolution | Enabled |
| DNS Hostnames | Enabled |

### Purpose

The VPC provides complete control over:

- IP addressing
- Routing
- Security
- Internet connectivity
- Network isolation

---

## Step 2 – Create Public Subnet

A public subnet was created inside the VPC.

### Configuration

| Property | Value |
|----------|-------|
| Name | Public-Subnet-1A |
| CIDR | 10.0.1.0/24 |
| Availability Zone | ap-south-1a |

### Purpose

The public subnet hosts resources that require direct internet access.

Examples include:

- Bastion Host
- NAT Gateway
- Load Balancer

---

## Step 3 – Create Private Subnet

A private subnet was created to host backend resources.

### Configuration

| Property | Value |
|----------|-------|
| Name | Private-Subnet-1B |
| CIDR | 10.0.2.0/24 |
| Availability Zone | ap-south-1b |

### Purpose

The private subnet protects application servers from direct internet access while still allowing outbound internet connectivity through the NAT Gateway.

---

## Step 4 – Configure Route Tables

Two separate Route Tables were created.

### Public Route Table

| Destination | Target |
|-------------|--------|
| 10.0.0.0/16 | Local |
| 0.0.0.0/0 | Internet Gateway |

The Public Route Table was associated with the Public Subnet.

This allows resources inside the Public Subnet to communicate with the internet.

---

### Private Route Table

Initially the Private Route Table contained only the local route.

| Destination | Target |
|-------------|--------|
| 10.0.0.0/16 | Local |

After creating the NAT Gateway, an additional route was added.

| Destination | Target |
|-------------|--------|
| 0.0.0.0/0 | NAT Gateway |

The Private Route Table was associated with the Private Subnet.

---

## Step 5 – Create and Attach Internet Gateway

An Internet Gateway (IGW) was created and attached to the VPC.

### Purpose

The Internet Gateway enables communication between AWS resources in the public subnet and the public internet.

Without the Internet Gateway:

- Public EC2 instances cannot receive traffic.
- Public IP addresses become unusable.

---

## Step 6 – Allocate Elastic IP

A static Elastic IP address was allocated.

### Purpose

Elastic IP provides a permanent public IPv4 address.

It was associated with:

- Bastion Host
- NAT Gateway

This ensured stable internet connectivity and simplified SSH administration.

---

## Step 7 – Create NAT Gateway

The NAT Gateway was deployed inside the Public Subnet.

### Configuration

| Property | Value |
|----------|-------|
| Subnet | Public-Subnet-1A |
| Elastic IP | Associated |
| State | Available |

### Purpose

The NAT Gateway allows instances located in private subnets to:

- Download software packages
- Install updates
- Access AWS services
- Access the internet

while preventing inbound internet connections.

This is one of the most common production networking architectures used in AWS.

---

## Step 8 – Launch Public EC2 (Bastion Host)

A Bastion Host EC2 instance was launched inside the Public Subnet.

### Configuration

| Property | Value |
|----------|-------|
| Instance Name | Public-Bastion-Host |
| AMI | Amazon Linux 2023 |
| Instance Type | t3.micro |
| Public IP | Elastic IP |

### Purpose

The Bastion Host provides secure administrative access to resources located inside private subnets.

Instead of exposing backend servers directly to the internet, administrators first connect to the Bastion Host and then securely access private instances.

---

## Step 9 – Launch Private EC2 Instance

A second EC2 instance was deployed inside the Private Subnet.

### Configuration

| Property | Value |
|----------|-------|
| Instance Name | Private-App-Server |
| Public IP | None |
| Subnet | Private-Subnet-1B |

### Purpose

The Private Application Server hosts backend workloads that should never be directly accessible from the internet.

All administrative access occurs through the Bastion Host.

Outbound internet traffic is provided using the NAT Gateway.

---

## Step 10 – Configure Security Groups

Two Security Groups were configured.

### Bastion Host Security Group

| Rule | Port | Source |
|------|------|--------|
| SSH | 22 | My IP Address |

This allows only the administrator to connect from the internet.

---

### Private EC2 Security Group

| Rule | Port | Source |
|------|------|--------|
| SSH | 22 | Bastion Host Security Group |

This prevents direct SSH access from the internet while allowing secure administrative access from the Bastion Host.

---

## Implementation Summary

The following AWS networking components were successfully deployed:

- Custom Amazon VPC
- Public Subnet
- Private Subnet
- Public Route Table
- Private Route Table
- Internet Gateway
- Elastic IP
- NAT Gateway
- Bastion Host EC2
- Private Application Server
- Security Groups
- Secure SSH Connectivity

The completed architecture provides a secure and scalable networking foundation suitable for hosting production workloads while following AWS networking best practices.

# 8. Connectivity Testing

After completing the deployment, a series of connectivity tests were performed to verify that the network architecture behaved as expected.

The purpose of these tests was to validate:

- Public internet connectivity
- Private subnet isolation
- Bastion Host functionality
- NAT Gateway outbound internet access
- Secure SSH communication

---

## Test 1 – SSH Access to Bastion Host

The Bastion Host was accessed directly from the local computer using SSH.

### Command

```bash
ssh -i secure-ec2-key.pem ec2-user@<Elastic-IP>
```

### Expected Result

The SSH connection should be established successfully.

### Actual Result

✅ Successful

### Screenshot

Insert screenshot:

**Public EC2 Server in Public Subnet.png**

---

## Test 2 – SSH Access to Private EC2

After connecting to the Bastion Host, an SSH connection was established to the private EC2 instance.

### Command

```bash
ssh ec2-user@10.0.2.x
```

### Expected Result

Private EC2 should accept SSH only from the Bastion Host.

### Actual Result

✅ Successful

### Screenshot

Insert screenshot:

**Bastion Host to Private Server.png**

---

## Test 3 – Internet Connectivity from Private EC2

Internet connectivity was verified from the private instance.

### Command

```bash
ping -c 4 google.com
```

### Expected Result

The Private EC2 instance should successfully reach internet hosts through the NAT Gateway.

### Actual Result

✅ Successful

### Screenshot

Insert screenshot:

**Connectivity Testing.png**

---

## Test 4 – Verify Public IP Translation

To verify outbound internet connectivity, the public IP address was checked.

### Command

```bash
curl ifconfig.me
```

### Expected Result

The command should return a public IP address.

The private instance itself does not possess a public IP. Instead, outbound traffic is translated by the NAT Gateway using its Elastic IP.

### Actual Result

✅ Successful

### Screenshot

Insert screenshot:

**Connectivity Testing.png**

---

# Connectivity Validation Summary

| Test | Expected Result | Status |
|------|-----------------|--------|
| SSH to Bastion Host | Success | ✅ Passed |
| SSH to Private EC2 | Success | ✅ Passed |
| Internet Access from Private EC2 | Success | ✅ Passed |
| NAT Gateway Translation | Success | ✅ Passed |
| Public Internet Access | Success | ✅ Passed |

---

# 9. Security Analysis

Security was one of the primary design objectives of this project.

The deployed architecture follows the principle of defense in depth by implementing multiple layers of protection.

---

## Network Segmentation

The VPC was divided into two independent network segments.

### Public Subnet

Contains:

- Bastion Host
- NAT Gateway

These resources require controlled internet connectivity.

---

### Private Subnet

Contains:

- Private Application Server

The server has:

- No Public IP
- No direct internet access
- Access only through the Bastion Host

This significantly reduces the attack surface.

---

## Security Groups

Security Groups acted as stateful virtual firewalls.

### Bastion Host

Allowed:

- SSH (TCP Port 22)
- Source: My Public IP

Blocked:

- All other inbound traffic

---

### Private EC2

Allowed:

- SSH
- Source: Bastion Host Security Group

Blocked:

- Direct internet access

---

## Internet Gateway Security

The Internet Gateway provided internet connectivity only to the Public Subnet.

The Private Subnet had no direct route to the Internet Gateway.

---

## NAT Gateway Security

The NAT Gateway allowed:

- Outbound internet communication

The NAT Gateway did not permit:

- Inbound internet connections

This ensured backend servers remained protected while still receiving software updates.

---

## Secure Administrative Access

Instead of exposing every server to the internet, administrators first connect to the Bastion Host.

After authentication, they securely access private resources using SSH.

This architecture is widely adopted across production AWS environments.

---

# 10. Screenshots

The following screenshots document the successful deployment of the AWS Secure Multi-Tier VPC architecture.

## Infrastructure

- VPC Created
- Public Subnet
- Private Subnet
- Public Route Table
- Private Route Table
- Internet Gateway
- Elastic IP
- NAT Gateway

---

## Compute

- Public EC2 Instance
- Private EC2 Instance

---

## Security

- Bastion Host Security Group
- Private EC2 Security Group

---

## Connectivity

- SSH to Bastion Host
- SSH to Private EC2
- Internet Connectivity Test
- NAT Gateway Validation

These screenshots provide evidence of the successful implementation and verification of all networking components.

# 11. AWS Best Practices

Throughout this project, AWS networking and security best practices were followed to build a secure, scalable, and production-ready cloud environment.

## 11.1 Network Segmentation

The infrastructure was divided into public and private subnets.

Benefits include:

- Improved security
- Better resource isolation
- Easier management
- Reduced attack surface

Internet-facing resources were deployed only inside the public subnet, while backend resources remained isolated inside the private subnet.

---

## 11.2 Principle of Least Privilege

Security Groups were configured to allow only the minimum required traffic.

Examples:

- Bastion Host allows SSH only from the administrator's public IP.
- Private EC2 allows SSH only from the Bastion Host Security Group.
- No unnecessary inbound ports were opened.

This minimizes unauthorized access.

---

## 11.3 Bastion Host Architecture

Rather than exposing all EC2 instances to the internet, a dedicated Bastion Host was used.

Advantages include:

- Centralized administrative access
- Improved security
- Reduced exposure of backend servers
- Easier monitoring of administrative activities

This architecture is commonly used in enterprise cloud environments.

---

## 11.4 Private Subnet Isolation

The Private Application Server was launched without a public IPv4 address.

As a result:

- It cannot be accessed directly from the internet.
- Administrative access is only possible through the Bastion Host.
- Internet access is available only for outbound connections via the NAT Gateway.

This significantly improves the security posture of backend services.

---

## 11.5 Controlled Internet Access

The NAT Gateway provides outbound internet connectivity for private resources while preventing inbound internet connections.

Typical use cases include:

- Installing software packages
- Downloading operating system updates
- Accessing AWS services
- Communicating with external APIs

This design is widely adopted for production workloads.

---

# 12. Cost Optimization

AWS resources were managed carefully to avoid unnecessary charges.

The NAT Gateway was used only during testing because it is a billable service.

After successful validation and documentation, all project resources should be deleted.

Resources to remove include:

- NAT Gateway
- Elastic IP associated with the NAT Gateway
- Elastic IP associated with the Bastion Host
- Public Bastion Host EC2 instance
- Private Application Server EC2 instance
- Internet Gateway
- Public Route Table (if project-specific)
- Private Route Table (if project-specific)
- Public Subnet
- Private Subnet
- Security Groups created specifically for this project
- Secure-MultiTier-VPC

Cleaning up these resources ensures that no unnecessary AWS charges continue after completing the project.

---

# 13. Challenges Faced

During the implementation of this project, several practical challenges were encountered and resolved.

## SSH Authentication

Initially, SSH access from the Bastion Host to the Private EC2 instance failed due to authentication issues.

The problem was resolved by using the appropriate key pair and verifying Security Group rules.

---

## Route Table Configuration

Correct association of Route Tables with their respective subnets was essential.

Improper route configuration prevented internet connectivity until the correct routes were configured.

---

## NAT Gateway Validation

The private EC2 instance initially had no internet connectivity until:

- NAT Gateway deployment was completed.
- The Private Route Table was updated.
- Proper route associations were verified.

---

## Security Group Configuration

SSH connectivity required careful configuration of Security Groups.

Only the Bastion Host Security Group was permitted to access the Private EC2 instance, ensuring secure administrative access.

---

# 14. Learning Outcomes

This project provided extensive hands-on experience with AWS networking services and secure cloud infrastructure design.

Key concepts learned include:

- Amazon VPC architecture
- CIDR planning
- IPv4 addressing
- Public and Private Subnets
- Route Tables
- Internet Gateway
- NAT Gateway
- Elastic IP
- Amazon EC2 networking
- Security Groups
- Bastion Host architecture
- Private subnet isolation
- Secure remote administration
- Outbound internet access through NAT Gateway
- Cloud networking best practices
- AWS infrastructure design
- Cost optimization strategies

The project also strengthened practical skills in troubleshooting connectivity issues, validating secure communication, and documenting cloud infrastructure.

---

# 15. Conclusion

This project successfully demonstrated the design and implementation of a secure multi-tier networking architecture using Amazon Web Services.

A custom Amazon VPC was created with separate public and private subnets to provide secure network segmentation. An Internet Gateway enabled internet connectivity for public resources, while a NAT Gateway provided controlled outbound internet access for private resources.

The deployment of a Bastion Host enabled secure administrative access to backend servers without exposing them directly to the internet. Security Groups were configured according to the principle of least privilege, ensuring that only authorized communication was permitted.

Connectivity testing confirmed successful communication between network components and validated outbound internet access from the private subnet through the NAT Gateway.

Overall, this project provided practical experience with AWS networking, cloud security, and infrastructure design. It demonstrates the ability to build secure, scalable, and production-ready networking environments using AWS best practices.

---

# References

1. Amazon Web Services Documentation – Amazon VPC

2. Amazon Web Services Documentation – Amazon EC2

3. Amazon Web Services Documentation – Internet Gateway

4. Amazon Web Services Documentation – NAT Gateway

5. Amazon Web Services Documentation – Elastic IP Addresses

6. Amazon Web Services Documentation – Route Tables

7. Amazon Web Services Documentation – Security Groups

8. AWS Well-Architected Framework

---

# Appendix

## Project Repository

AWS-Secure-MultiTier-VPC

---

## GitHub Repository

(Add your GitHub repository URL here.)

---

## Project Duration

July 2026

---

## Developed By

**E. Teja Srinivas**

Bachelor of Technology

Computer Science Engineering (Cyber Security)

Sri Venkateswara College of Engineering

Tirupati, Andhra Pradesh, India

---

**End of Report**
