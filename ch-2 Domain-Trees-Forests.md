# Active Directory Domain Services (AD DS)
# Domain, Trees, and Forests - Complete Guide

---

# Table of Contents

1. Introduction
2. What is a Domain?
3. Domain Components
4. Domain Structure
5. Trust Relationship
6. What is a Tree?
7. Tree Architecture
8. What is a Forest?
9. Forest Architecture
10. Global Catalog
11. Schema
12. Naming Contexts
13. FSMO Roles
14. Authentication Flow
15. Domain vs Tree vs Forest
16. Real-World Company Example
17. Production Use Cases
18. Best Practices
19. Interview Questions

---

# Introduction

Active Directory organizes computers, users, printers, servers, and policies into a logical hierarchy.

The hierarchy has three main levels:

```
Forest
   │
   ├── Tree
   │      │
   │      ├── Domain
   │      ├── Child Domain
   │      └── Child Domain
   │
   └── Another Tree
          │
          ├── Domain
          └── Child Domain
```

Everything in Active Directory starts from the **Domain**.

Multiple domains form a **Tree**.

Multiple trees form a **Forest**.

---

# What is a Domain?

A Domain is a logical administrative boundary in Active Directory.

It stores:

- Users
- Groups
- Computers
- Printers
- Servers
- Policies
- Security Settings

Every object inside a domain shares the same Active Directory database.

Example:

```
simform.com
```

Inside:

```
simform.com

Users
Computers
Servers
Groups
Policies
Printers
```

---

# Domain Characteristics

Each domain has

- Unique DNS Name
- Security Boundary
- Domain Controllers
- Active Directory Database
- Group Policies
- Authentication Service

Example

```
Domain Name

simform.com
```

---

# Domain Controller (DC)

A Domain Controller is a Windows Server running Active Directory.

Responsibilities

- User Authentication
- Authorization
- DNS
- Group Policy
- Replication

Example

```
User Login

↓

Domain Controller

↓

Username Verification

↓

Password Verification

↓

Access Granted
```

---

# Domain Database

Every Domain stores data inside

```
NTDS.DIT
```

Contains

- Users
- Groups
- Password Hashes
- OU
- Policies
- Trust Information

---

# Domain Namespace

Example

```
simform.com
```

Subdomains

```
dev.simform.com

hr.simform.com

sales.simform.com
```

These are child domains.

---

# Domain Authentication

Example

Employee Login

```
Username

prins
```

Password

```
********
```

Flow

```
Computer

↓

Domain Controller

↓

Kerberos Authentication

↓

Ticket Granted

↓

Access Resources
```

---

# Trust Relationship

Domains trust each other automatically inside a forest.

Example

```
HR Domain

↓

Trust

↓

Sales Domain

↓

Trust

↓

Development Domain
```

User from HR can access Sales resources if permissions allow.

---

# What is a Tree?

A Tree is a collection of domains sharing a contiguous namespace.

Example

```
simform.com

↓

dev.simform.com

↓

qa.dev.simform.com
```

Notice

All domains share

```
simform.com
```

This makes them part of one tree.

---

# Tree Architecture

```
                simform.com
                     │
        ┌────────────┴────────────┐
        │                         │
dev.simform.com          hr.simform.com
        │
qa.dev.simform.com
```

---

# Tree Characteristics

- Parent Domain
- Child Domain
- Automatic Trust
- Shared Schema
- Shared Global Catalog

---

# Parent Domain

```
simform.com
```

Child

```
dev.simform.com
```

Grandchild

```
qa.dev.simform.com
```

---

# Child Domain

Child domains inherit

- Trust
- Schema
- Configuration

But maintain

- Own Users
- Own Computers
- Own Policies

---

# Benefits of Trees

- Easy Administration
- Department Separation
- Geographic Separation
- Security
- Delegation

---

# What is a Forest?

A Forest is the highest level in Active Directory.

It contains one or more trees.

Trees can have different DNS names.

Example

```
simform.com

company.net

abc.org
```

All belong to one forest.

---

# Forest Architecture

```
Forest
│
├───────────────┐
│               │
Tree 1          Tree 2
│               │
simform.com     company.net
│               │
├── dev         ├── hr
├── sales       └── finance
└── qa
```

---

# Forest Characteristics

- Highest Security Boundary
- Shared Schema
- Shared Configuration
- Global Catalog
- Trust Between Trees

---

# Forest Root Domain

The first domain created becomes

Forest Root Domain

Example

```
simform.com
```

This domain cannot be changed later.

---

# Global Catalog (GC)

Global Catalog contains partial information from every object inside the forest.

Purpose

- Faster Searches
- Universal Group Membership
- User Login

Example

Employee searches

```
John
```

GC quickly locates

```
John
Sales
company.net
```

Without contacting every domain.

---

# Schema

Schema defines

- Object Types
- Attributes
- Rules

Example

User Object

```
Name

Email

Phone

Department

Manager
```

Every domain shares one schema.

---

# Configuration Partition

Stores

- Sites
- Services
- Replication
- Forest Settings

Shared across the forest.

---

# Naming Contexts

There are three important partitions.

## Domain Partition

Stores

- Users
- Groups
- Computers

---

## Configuration Partition

Stores

- Forest Configuration

---

## Schema Partition

Stores

- Schema Objects

---

# FSMO Roles

There are five FSMO Roles.

Forest Level

- Schema Master
- Domain Naming Master

Domain Level

- RID Master
- PDC Emulator
- Infrastructure Master

---

# Authentication Across Forest

Example

```
User

↓

Login

↓

Domain Controller

↓

Global Catalog

↓

Kerberos

↓

Access Resource
```

---

# Domain vs Tree vs Forest

| Feature | Domain | Tree | Forest |
|----------|---------|------|---------|
| Smallest Unit | Yes | No | No |
| Security Boundary | Yes | No | Yes |
| DNS Namespace | One | Shared | Multiple |
| Contains Users | Yes | Yes | Yes |
| Trust | Domain | Automatic | Automatic |
| Schema | One | Shared | Shared |
| Global Catalog | Uses GC | Shared | Shared |
| Administration | Local | Multiple Domains | Entire Organization |

---

# Real-World Example

Suppose Microsoft owns multiple business units.

Forest

```
Microsoft
```

Tree 1

```
microsoft.com
```

Domains

```
sales.microsoft.com

hr.microsoft.com

india.microsoft.com
```

Tree 2

```
github.com
```

Domains

```
engineering.github.com

support.github.com
```

Both trees exist in one forest but have different DNS names.

---

# Production Example

A multinational company

```
ABC Corporation
```

Countries

```
USA

India

Germany
```

Forest

```
abc.com
```

Domains

```
usa.abc.com

india.abc.com

germany.abc.com
```

Departments

```
HR

Finance

IT

Support
```

Each country manages its own users while sharing a common Active Directory infrastructure.

---

# Advantages

## Domain

- Central Authentication
- Group Policy
- Security Management

---

## Tree

- Department Separation
- Geographic Organization
- Easy Delegation

---

## Forest

- Enterprise Administration
- Shared Schema
- Trust Relationships
- Centralized Identity

---

# Best Practices

- Keep one forest whenever possible.
- Create child domains only when necessary.
- Use Organizational Units (OUs) before creating new domains.
- Place multiple Domain Controllers in each domain.
- Configure Global Catalog servers for faster authentication.
- Regularly back up Active Directory.
- Monitor replication health.
- Secure administrative accounts.

---

# Interview Questions

## What is a Domain?

A logical administrative and security boundary that stores users, computers, groups, and policies.

---

## What is a Tree?

A collection of domains sharing a contiguous DNS namespace with automatic trust relationships.

---

## What is a Forest?

The highest-level Active Directory container that can contain one or more trees with shared schema and configuration.

---

## Difference Between Domain, Tree, and Forest

- Domain → Administrative and security boundary.
- Tree → Collection of related domains with the same DNS namespace.
- Forest → Collection of one or more trees with shared schema and configuration.

---

# Complete Hierarchy

```
Forest
│
├────────────── Tree 1 ──────────────┐
│                                    │
simform.com                          │
│                                    │
├── hr.simform.com                   │
├── dev.simform.com                  │
└── qa.dev.simform.com               │
│
├────────────── Tree 2 ──────────────┐
│                                    │
company.net                          │
│                                    │
├── sales.company.net                │
└── support.company.net              │
│
└── Shared Components
      │
      ├── Schema
      ├── Configuration
      ├── Global Catalog
      ├── Trust Relationships
      └── Forest-wide Administration
```

---

# Key Takeaways

- **Domain** is the basic administrative and security boundary in Active Directory.
- **Tree** is a group of domains with a contiguous DNS namespace and automatic trust.
- **Forest** is the top-level Active Directory structure containing one or more trees.
- A **Forest** shares a common schema, configuration, and Global Catalog.
- **Automatic trusts** enable secure authentication and resource access across domains within the same forest.
- In production, most organizations use **a single forest** with multiple domains or Organizational Units (OUs) based on business and administrative requirements.