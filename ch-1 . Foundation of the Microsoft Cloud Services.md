# Foundation of Microsoft Cloud Services

> **Goal:** Understand the basic concepts of Microsoft Cloud Services before learning Windows Server, Azure, Microsoft 365, Active Directory, and Cloud Administration.

---

# Table of Contents

1. What is Cloud Computing?
2. Traditional IT vs Cloud
3. Types of Cloud Computing
4. Cloud Service Models
5. Microsoft Cloud Platform
6. Microsoft Azure Overview
7. Microsoft 365 Overview
8. Microsoft Entra ID
9. Windows Server in Cloud
10. Azure Regions and Availability Zones
11. Resource Groups
12. Azure Virtual Machines
13. Azure Storage
14. Azure Networking
15. Identity and Access Management
16. Monitoring and Management
17. Security in Microsoft Cloud
18. Backup and Disaster Recovery
19. Cost Management
20. Real-World Architecture
21. Interview Questions

---

# 1. What is Cloud Computing?

Cloud Computing means delivering IT resources over the Internet instead of maintaining your own physical servers.

Instead of buying servers, networking devices, storage, and data centers, you rent them from a cloud provider.

Popular Cloud Providers:

- Microsoft Azure
- Amazon AWS
- Google Cloud Platform (GCP)

---

## Traditional Infrastructure

```
Company

↓

Buy Servers

↓

Install OS

↓

Networking

↓

Storage

↓

Cooling

↓

Power

↓

Maintenance

↓

Security
```

Company manages everything.

---

## Cloud Infrastructure

```
Company

↓

Internet

↓

Microsoft Azure

↓

Virtual Machines

↓

Storage

↓

Networking

↓

Database

↓

Applications
```

Microsoft manages the infrastructure.

---

# Real World Example

Suppose your company needs 50 servers.

Traditional way:

- Buy hardware
- Install Windows Server
- Configure network
- Maintain hardware
- Replace failed disks

Time:
Several weeks

Cost:
Very High

Cloud way:

Open Azure Portal

↓

Click Create VM

↓

Select Size

↓

Deploy

Time:
5 Minutes

Cost:
Pay only for usage.

---

# Advantages of Cloud

- No hardware purchase
- High availability
- Scalability
- Automatic updates
- Disaster recovery
- Global access
- Lower maintenance
- Pay-as-you-go pricing

---

# 2. Traditional IT vs Cloud

| Traditional | Cloud |
|-------------|--------|
| Buy servers | Rent servers |
| High cost | Low starting cost |
| Manual updates | Automatic updates |
| Fixed capacity | Easy scaling |
| Hardware failures | Microsoft handles hardware |
| Limited locations | Global regions |
| Long deployment | Minutes |

---

# 3. Types of Cloud

There are three deployment models.

---

## Public Cloud

Infrastructure is owned by Microsoft.

Example:

```
Company

↓

Azure Data Center

↓

Virtual Machines
```

Examples:

- Azure
- AWS
- Google Cloud

Best For:

- Startups
- Small companies
- Web applications

Advantages

- Cheap
- Highly available
- Easy to manage

---

## Private Cloud

Infrastructure belongs to one organization.

Example

```
Company Data Center

↓

Own Servers

↓

Own Storage

↓

Own Network
```

Best For

- Banks
- Government
- Military

Advantages

- Full control
- High security

Disadvantages

- Expensive

---

## Hybrid Cloud

Combination of Public + Private.

Example

```
Company Data Center

↓

VPN

↓

Microsoft Azure
```

Some workloads stay on-premises, while others move to Azure.

Best For

Large Enterprises

---

# Real Example

Hospital

Patient Database

↓

Private Cloud

Website

↓

Azure

Email

↓

Microsoft 365

This is Hybrid Cloud.

---

# 4. Cloud Service Models

Three major service models.

---

## IaaS (Infrastructure as a Service)

Microsoft provides:

- Physical servers
- Storage
- Networking
- Virtualization

Customer manages:

- Windows/Linux
- Applications
- Security
- Updates

Example

Azure Virtual Machine

Real Example

Renting an empty apartment.

You decorate and manage everything.

---

## PaaS (Platform as a Service)

Microsoft manages:

- Server
- OS
- Runtime
- Database Engine

Customer manages:

- Application
- Code

Example

Azure App Service

Real Example

Staying in a hotel.

Everything is ready.

You only use it.

---

## SaaS (Software as a Service)

Everything managed by Microsoft.

Customer only uses the software.

Examples

- Microsoft 365
- Outlook
- Teams
- OneDrive

Real Example

Netflix

You just watch movies.

---

## Responsibility Comparison

| Component | IaaS | PaaS | SaaS |
|------------|------|------|------|
| Networking | Microsoft | Microsoft | Microsoft |
| Storage | Microsoft | Microsoft | Microsoft |
| Server | Microsoft | Microsoft | Microsoft |
| Operating System | Customer | Microsoft | Microsoft |
| Runtime | Customer | Microsoft | Microsoft |
| Application | Customer | Customer | Microsoft |
| Data | Customer | Customer | Customer |

---

# 5. Microsoft Cloud Platform

Microsoft Cloud includes multiple services.

```
Microsoft Cloud

├── Azure
├── Microsoft 365
├── Dynamics 365
├── Power Platform
├── GitHub
├── Defender
├── Intune
└── Entra ID
```

---

# 6. Microsoft Azure

Azure is Microsoft's cloud computing platform.

Services include:

- Virtual Machines
- Networking
- Databases
- Storage
- Kubernetes
- AI
- Security
- Backup
- Monitoring

Example:

```
Azure

├── VM
├── Storage
├── SQL Database
├── Virtual Network
├── Firewall
└── Backup
```

---

# 7. Microsoft 365

Microsoft 365 is a SaaS platform.

Includes:

- Outlook
- Teams
- Word
- Excel
- PowerPoint
- OneDrive
- SharePoint
- Exchange Online

Business Example

Employee Login

↓

Microsoft 365

↓

Teams

↓

Outlook

↓

SharePoint

↓

OneDrive

---

# 8. Microsoft Entra ID

Formerly called Azure Active Directory.

Purpose:

Identity Management.

Features

- User Authentication
- Single Sign-On (SSO)
- Multi-Factor Authentication (MFA)
- Conditional Access
- Identity Protection

Example

```
User

↓

Entra ID

↓

Authentication

↓

Azure

↓

Microsoft 365

↓

Applications
```

---

# 9. Windows Server in Cloud

Windows Server can run inside Azure Virtual Machines.

Example Uses

- Active Directory Domain Controller
- File Server
- IIS Web Server
- SQL Server
- Application Server

---

# 10. Azure Regions

Azure has many data centers worldwide.

Example Regions

- East US
- West Europe
- Central India
- South India
- Japan East
- Australia East

Choose a region close to users for better performance and compliance.

---

# Availability Zones

Availability Zones are physically separate locations within a region.

Example

```
Central India

├── Zone 1
├── Zone 2
└── Zone 3
```

If one zone fails, another continues running.

---

# 11. Resource Groups

A Resource Group is a logical container for Azure resources.

Example

```
Resource Group

├── VM
├── Storage
├── Database
├── Virtual Network
└── Backup
```

Benefits

- Easy management
- Access control
- Billing
- Resource organization

---

# 12. Azure Virtual Machines

Azure VM is a virtual server.

Supports

- Windows Server
- Ubuntu
- Red Hat
- Debian
- SUSE

Example

```
Azure VM

↓

Windows Server 2025

↓

IIS

↓

Website
```

---

# 13. Azure Storage

Storage Types

## Blob Storage

Stores:

- Images
- Videos
- Backups
- Documents

---

## File Storage

Shared network folders.

Supports SMB protocol.

---

## Disk Storage

Used by Azure Virtual Machines.

---

## Queue Storage

Stores messages between applications.

---

## Table Storage

NoSQL database storage.

---

# 14. Azure Networking

Main Components

Virtual Network (VNet)

↓

Subnet

↓

Network Security Group

↓

Public IP

↓

Load Balancer

↓

VPN Gateway

Example

```
Internet

↓

Firewall

↓

Azure VNet

↓

Subnet

↓

Virtual Machine
```

---

# 15. Identity and Access Management (IAM)

Azure controls access using Role-Based Access Control (RBAC).

Common Roles

- Owner
- Contributor
- Reader
- User Access Administrator

Example

Admin

↓

Owner Role

↓

Can create/delete resources

Developer

↓

Contributor

↓

Can manage resources but not permissions

Viewer

↓

Reader

↓

Can only view resources

---

# 16. Monitoring and Management

Tools

- Azure Monitor
- Log Analytics
- Azure Advisor
- Azure Alerts

Purpose

- Monitor CPU
- Memory
- Disk
- Network
- Logs
- Performance

---

# 17. Security

Microsoft provides

- Microsoft Defender
- Microsoft Sentinel
- Azure Firewall
- Key Vault
- DDoS Protection

Security Features

- Encryption
- MFA
- RBAC
- Conditional Access
- Security Center

---

# 18. Backup and Disaster Recovery

Azure Backup

Automatically backs up:

- Virtual Machines
- Databases
- Files

Azure Site Recovery

Replicates servers to another region.

Benefits

- Business Continuity
- Disaster Recovery
- Fast Recovery

---

# 19. Cost Management

Azure Pricing Model

Pay only for:

- Compute
- Storage
- Networking

Tools

- Azure Cost Management
- Budgets
- Cost Alerts
- Azure Calculator

---

# 20. Real-World Architecture

```
                Users
                  │
                  ▼
             Internet
                  │
                  ▼
          Azure Load Balancer
                  │
      ┌───────────┴───────────┐
      ▼                       ▼
 Web Server VM 1         Web Server VM 2
      │                       │
      └───────────┬───────────┘
                  ▼
          Azure SQL Database
                  │
                  ▼
           Azure Backup Vault
                  │
                  ▼
          Azure Monitor & Logs
```

---

# 21. Production Example

An e-commerce company hosts its application on Azure:

- Users access the website through the Internet.
- Azure Load Balancer distributes traffic across multiple web servers.
- Product images are stored in Azure Blob Storage.
- Customer data is stored in Azure SQL Database.
- User authentication is handled by Microsoft Entra ID.
- Azure Backup protects critical data.
- Azure Monitor tracks CPU, memory, and application health.
- If one Availability Zone fails, traffic is automatically routed to another zone for high availability.

---

# Key Terms Summary

| Term | Description |
|------|-------------|
| Cloud Computing | Delivering IT resources over the Internet |
| Azure | Microsoft's cloud platform |
| Microsoft 365 | SaaS productivity suite |
| Entra ID | Identity and access management service |
| IaaS | Infrastructure as a Service |
| PaaS | Platform as a Service |
| SaaS | Software as a Service |
| Resource Group | Logical container for Azure resources |
| Virtual Machine | Cloud-based virtual server |
| VNet | Private virtual network in Azure |
| RBAC | Role-Based Access Control |
| Availability Zone | Physically separate datacenter within a region |
| Blob Storage | Object storage for unstructured data |
| Azure Monitor | Monitoring and alerting service |

---

# Interview Questions

### Basic

1. What is cloud computing?
2. What are the benefits of Microsoft Azure?
3. What is the difference between Public, Private, and Hybrid Cloud?
4. What is Microsoft Entra ID?
5. What is a Resource Group?
6. What is an Azure Virtual Machine?
7. What is the difference between Azure Storage types?
8. What is RBAC?
9. What is the difference between IaaS, PaaS, and SaaS?
10. What is an Availability Zone?

### Scenario-Based

**Q1:** Your web application receives high traffic during a sale. How can Azure help?

**Answer:** Use Azure Virtual Machine Scale Sets or Azure App Service with autoscaling to automatically add more instances based on CPU or traffic load.

**Q2:** Your company has an on-premises Active Directory but wants employees to access Microsoft 365 with the same credentials.

**Answer:** Synchronize on-premises Active Directory with Microsoft Entra ID using Azure AD Connect (Microsoft Entra Connect), enabling Single Sign-On (SSO).

**Q3:** A disk in one Azure data center fails. How do you maintain service availability?

**Answer:** Deploy resources across multiple Availability Zones or Regions and use Azure Load Balancer or Azure Front Door to redirect traffic.