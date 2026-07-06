# Foundation of Active Directory Domains (AD DS)

> **Author:** Your Notes
> **Level:** Beginner → Production
> **Platform:** Windows Server
> **Directory Service:** Active Directory Domain Services (AD DS)

---

# Table of Contents

1. Introduction
2. What is Active Directory?
3. What is a Domain?
4. Why Active Directory is Required?
5. Active Directory Architecture
6. Active Directory Components
7. Domain Controller
8. Active Directory Database
9. Active Directory Hierarchy
10. Authentication & Authorization
11. Kerberos Authentication
12. LDAP
13. DNS and Active Directory
14. Global Catalog
15. FSMO Roles
16. Group Policy
17. Trust Relationship
18. Active Directory Replication
19. SYSVOL
20. Security Identifier (SID)
21. Active Directory Ports
22. Administrative Tools
23. Best Practices
24. Real-World Example
25. Summary

---

# 1. Introduction

Active Directory Domain Services (AD DS) is Microsoft's centralized directory service used to manage users, computers, servers, printers, applications, and other network resources within an organization.

It provides:

- Centralized Authentication
- Centralized Authorization
- User Management
- Computer Management
- Security Policy Management
- Resource Sharing
- Single Sign-On (SSO)

Active Directory is available only on **Windows Server**.

---

# 2. What is Active Directory?

Active Directory (AD) is a directory service that stores information about every object in a network.

Examples of Objects

- Users
- Computers
- Servers
- Printers
- Groups
- Shared Folders
- Applications
- Policies

Think of Active Directory as a huge database that stores information about everything connected to a company's network.

---

## Without Active Directory

Imagine a company has:

- 500 Employees
- 400 Computers
- 30 Servers

Without AD:

- Every PC has separate users.
- Every user has different passwords.
- Administrators configure each PC individually.
- No centralized management.
- Difficult security management.
- Difficult password management.

Result:

Large amount of manual work.

---

## With Active Directory

One administrator can:

- Create users
- Reset passwords
- Lock accounts
- Install software
- Apply security policies
- Join computers
- Manage permissions

Everything is managed from one central server.

---

# 3. What is a Domain?

A Domain is a logical collection of users, computers, servers, and other resources that share a common security database.

Example

```
company.local
```

Everything belongs to the same domain.

```
company.local

├── Users
├── Computers
├── Servers
├── Printers
├── Groups
└── Policies
```

Every object trusts the Domain Controller.

---

# 4. Why Active Directory is Required?

Benefits include:

- Centralized management
- Centralized authentication
- Better security
- Password policies
- Group Policy management
- Easy administration
- Resource sharing
- Single Sign-On
- Permission management
- Scalability

---

# 5. Active Directory Architecture

```
                    Forest
                       |
                   Tree
                       |
                    Domain
                       |
          ------------------------
          |          |           |
         OU         OU          OU
          |          |           |
      Users     Computers     Groups
```

The hierarchy starts with the Forest and ends with individual objects.

---

# 6. Active Directory Components

## Physical Components

- Domain Controller
- Sites
- Subnets

---

## Logical Components

- Forest
- Tree
- Domain
- Organizational Unit (OU)
- Users
- Groups
- Computers

---

# 7. Domain Controller (DC)

A Domain Controller is a Windows Server that stores the Active Directory database.

Responsibilities:

- User Authentication
- Authorization
- DNS Service
- Kerberos Authentication
- LDAP Service
- Group Policy
- Replication

Example

```
Server Name

DC01.company.local

IP Address

192.168.1.10
```

Users authenticate against the Domain Controller.

---

# 8. Active Directory Database

Database Name

```
NTDS.dit
```

Default Location

```
C:\Windows\NTDS\
```

Stores:

- Users
- Groups
- Computers
- Password Hashes
- Organizational Units
- Security Descriptors

Every AD object is stored inside this database.

---

# 9. Active Directory Hierarchy

## Forest

Highest level of Active Directory.

A forest contains one or more trees.

Example

```
company.com
company.in
company.uk
```

---

## Tree

Collection of domains sharing a namespace.

Example

```
company.com

sales.company.com

hr.company.com

it.company.com
```

---

## Domain

Administrative and security boundary.

Example

```
company.local
```

---

## Organizational Unit (OU)

Logical container.

Example

```
Company

├── HR
├── IT
├── Sales
├── Finance
└── Management
```

Benefits:

- Organizes objects
- Apply Group Policies
- Delegate administration

---

## User Object

Represents a user.

Example

```
Username

john

Department

IT

Email

john@company.local
```

---

## Computer Object

Represents computers joined to the domain.

Example

```
PC-101

Operating System

Windows 11
```

---

## Group

Collection of users.

Example

```
HR Team

John
Lisa
David
Kevin
```

Permissions are assigned to groups instead of individual users.

---

# 10. Authentication and Authorization

## Authentication

Authentication verifies identity.

Question answered:

```
Who are you?
```

Example

```
Username

john

Password

********
```

The Domain Controller verifies the credentials.

---

## Authorization

Authorization determines what a user can access.

Question answered:

```
What are you allowed to do?
```

Example

```
John

↓

Member of HR Group

↓

Can access HR Folder

↓

Cannot access Finance Folder
```

---

# 11. Kerberos Authentication

Kerberos is the default authentication protocol used by Active Directory.

Login Process

```
User Login

↓

Request Ticket

↓

Domain Controller

↓

Ticket Granting Ticket (TGT)

↓

Service Ticket

↓

Access Granted
```

Advantages

- Secure
- Password never travels across the network
- Supports Single Sign-On
- Mutual authentication

Default Port

```
88
```

---

# 12. LDAP (Lightweight Directory Access Protocol)

LDAP is used to query and manage Active Directory objects.

Common Operations

- Search users
- Create users
- Modify groups
- Delete objects
- Authenticate applications

Ports

```
LDAP

389

LDAPS

636
```

---

# 13. DNS and Active Directory

DNS is mandatory for Active Directory.

Without DNS:

- Domain join fails.
- User login fails.
- Domain Controller cannot be located.
- Replication fails.

Example

```
User

↓

DNS Query

↓

Find Domain Controller

↓

Authenticate
```

---

# 14. Global Catalog (GC)

Global Catalog stores searchable information about all objects in the forest.

Purpose

- Faster searches
- Universal Group Membership
- Forest-wide logon

Ports

```
3268

3269 (SSL)
```

---

# 15. FSMO Roles

There are five Flexible Single Master Operation (FSMO) roles.

## Forest Roles

### Schema Master

Responsible for schema changes.

---

### Domain Naming Master

Creates or removes domains.

---

## Domain Roles

### RID Master

Allocates Relative IDs for security identifiers.

---

### PDC Emulator

Responsibilities:

- Password changes
- Time synchronization
- Account lockout
- Legacy support

---

### Infrastructure Master

Updates references between domains.

---

# 16. Group Policy (GPO)

Group Policy allows administrators to centrally configure Windows settings.

Examples

- Disable USB devices
- Configure desktop wallpaper
- Password policy
- Firewall settings
- Software installation
- Windows Update
- Login scripts

Applied at:

```
Site

↓

Domain

↓

Organizational Unit
```

---

# 17. Trust Relationship

Trust allows users in one domain to access resources in another domain.

Example

```
company.com

↓

Trust

↓

partner.com
```

Trust Types

- Parent-Child Trust
- Tree Root Trust
- External Trust
- Forest Trust
- Shortcut Trust

---

# 18. Active Directory Replication

Replication copies Active Directory changes between Domain Controllers.

Example

```
DC01

↓

Replication

↓

DC02

↓

Replication

↓

DC03
```

Benefits

- High Availability
- Fault Tolerance
- Data Consistency

---

# 19. SYSVOL

SYSVOL stores:

- Group Policies
- Login Scripts
- Startup Scripts

Default Location

```
C:\Windows\SYSVOL
```

Replicated automatically among Domain Controllers.

---

# 20. Security Identifier (SID)

Every object in Active Directory has a unique SID.

Example

```
User

John

SID

S-1-5-21-xxxxxxxxxxxxxxxx
```

Windows uses the SID for permissions, not the username.

---

# 21. Active Directory Ports

| Service | Port |
|----------|------|
| DNS | 53 |
| Kerberos | 88 |
| LDAP | 389 |
| LDAPS | 636 |
| SMB | 445 |
| RPC | 135 |
| Global Catalog | 3268 |
| Global Catalog SSL | 3269 |

---

# 22. Administrative Tools

| Tool | Purpose |
|------|----------|
| Active Directory Users and Computers | Manage users, groups, and OUs |
| Active Directory Administrative Center | Modern AD management |
| Active Directory Sites and Services | Configure sites and replication |
| Active Directory Domains and Trusts | Manage domains and trusts |
| ADSI Edit | Low-level directory editing |
| Group Policy Management Console | Manage GPOs |
| DNS Manager | Manage DNS records |

---

# 23. Best Practices

- Deploy at least two Domain Controllers for redundancy.
- Configure DNS correctly before installing AD DS.
- Use Organizational Units (OUs) to organize resources.
- Apply Group Policies at the OU level.
- Follow the Principle of Least Privilege.
- Regularly back up the System State.
- Monitor replication health.
- Keep Domain Controllers updated with security patches.
- Use groups instead of assigning permissions directly to users.
- Audit inactive users and computers periodically.

---

# 24. Real-World Example

A company has:

- 1000 Employees
- 600 Computers
- 40 Servers

Without Active Directory:

- Separate local accounts on each computer
- Different passwords
- Manual software installation
- No centralized security

With Active Directory:

- One user account per employee
- Single Sign-On (SSO)
- Centralized password policy
- Group Policy for all computers
- Centralized software deployment
- Simplified administration

---

# 25. Summary

Active Directory Domain Services (AD DS) is the foundation of identity and access management in Windows environments. It centralizes authentication, authorization, policy enforcement, and resource management. Core components such as Domains, Domain Controllers, Forests, Trees, Organizational Units, DNS, Kerberos, LDAP, Group Policy, Replication, and FSMO roles work together to provide a secure, scalable, and highly available infrastructure for enterprise networks.

Mastering these fundamentals is essential before moving on to advanced topics like:
- Installing AD DS
- Promoting a Domain Controller
- Creating Users and Groups
- Managing OUs
- Configuring Group Policy
- DNS Integration
- Trust Relationships
- Active Directory Backup and Recovery
- PowerShell Automation
- Hybrid Identity with Microsoft Entra ID (Azure AD)