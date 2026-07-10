# Active Directory Deep Dive
# Domain, Tree, Forest, Active Directory Database & Partitions

---

# Table of Contents

1. Active Directory Hierarchy
2. Domain
3. Child Domain
4. Tree
5. Forest
6. Trust Relationship
7. Understanding the Diagram
8. Active Directory Database (NTDS.dit)
9. Active Directory Partitions
10. Configuration Partition
11. Schema Partition
12. Domain Partition
13. Application Partition
14. Global Catalog
15. ForestDNSZone
16. DomainDNSZone
17. Replication
18. Complete Login Flow
19. Real-World Company Example
20. Interview Questions

---

# Active Directory Hierarchy

Everything in Active Directory is organized in a hierarchy.

```
Forest
│
├── Tree
│     │
│     ├── Domain
│     ├── Child Domain
│     └── Child Domain
│
└── Another Tree
      │
      ├── Domain
      └── Child Domain
```

Think of it like this:

```
Country
   ↓
State
   ↓
City
```

Similarly,

```
Forest
   ↓
Tree
   ↓
Domain
```

---

# Understanding the First Diagram

The image shows two different Trees inside one Forest.

```
                    FOREST
                       │
      ┌────────────────┴───────────────┐
      │                                │
Tree 1                             Tree 2
│                                   │
examlabpractice.com          prepareforexamsnow.com
│                                   │
├── uk.examlabpractice.com          └── au.prepareforexamsnow.com
│
├── jp.examlabpractice.com
│
└── scotland.uk.examlabpractice.com
```

Notice carefully.

There are two root domains.

```
examlabpractice.com

prepareforexamsnow.com
```

Since their names are different,

they are **different Trees**.

But they belong to the **same Forest**.

---

# Domain

A Domain is the smallest administrative unit in Active Directory.

A domain stores

- Users
- Groups
- Computers
- Printers
- Policies
- Servers

Example

```
simform.com
```

Inside

```
simform.com

Users

Computers

Servers

Groups
```

---

# Child Domain

A child domain inherits from its parent.

Example

```
Parent

simform.com

↓

Child

india.simform.com

↓

Grand Child

ahmedabad.india.simform.com
```

Notice every child shares the parent DNS name.

---

# Tree

A Tree is a collection of domains with the same DNS namespace.

Example

```
simform.com

↓

india.simform.com

↓

ahmedabad.india.simform.com
```

Everything ends with

```
simform.com
```

Therefore,

they belong to the same Tree.

---

# Forest

A Forest is a collection of one or more Trees.

Example

```
Forest

├── simform.com

└── microsoft.net
```

Different names

```
.com

.net
```

Different Trees

Same Forest

---

# Why Multiple Trees?

Large companies often purchase other companies.

Example

Microsoft buys GitHub.

Microsoft already owns

```
microsoft.com
```

GitHub owns

```
github.com
```

Instead of changing every user,

Microsoft simply creates another Tree.

```
Forest

│

├── microsoft.com

└── github.com
```

Both share

- Authentication
- Trust
- Schema
- Global Catalog

---

# Automatic Trust

Inside one Forest,

every domain trusts each other automatically.

Example

```
User

john

↓

uk.examlabpractice.com

↓

Access

jp.examlabpractice.com
```

If permission exists,

John can access resources.

---

# Active Directory Database

The second image shows the Active Directory Database.

```
NTDS.dit
```

This database exists on every Domain Controller.

It stores

- Users
- Password Hashes
- Computers
- Groups
- Policies
- Trusts
- Organizational Units

Location

```
C:\Windows\NTDS\NTDS.dit
```

---

# Understanding the Database Structure

The database contains different partitions.

```
NTDS.dit

│

├── Configuration

├── Schema

├── Domain

└── Application
```

Each partition stores different information.

---

# Configuration Partition

Stores information about the Forest itself.

Contains

- Sites
- Site Links
- Replication Topology
- Services
- Domain Controller Configuration

Example

```
Site

India

↓

Domain Controller 1

↓

Replication Schedule
```

This information is shared across the Forest.

Replication

```
Forest Wide
```

Every Domain Controller receives it.

---

# Schema Partition

Schema is the blueprint of Active Directory.

It defines

- Object Types

- Attributes

Example

User Object

```
Name

Email

Phone

Department

Manager
```

Computer Object

```
Hostname

Operating System

IP Address
```

Without Schema,

Windows would not know

what a User object looks like.

Replication

```
Forest Wide
```

Every Domain Controller gets it.

---

# Domain Partition

This is the largest partition.

Stores

- Users

- Groups

- Computers

- Organizational Units

- Group Policies

Example

```
simform.com

↓

Users

↓

Prins

Rahul

Amit

Sneha
```

Only Domain Controllers inside

that domain receive this partition.

Replication

```
Domain Wide Only
```

---

# Application Partition

Application partition is optional.

It stores application-specific data.

Example

DNS Server

DHCP

Certificate Services

Custom Applications

Replication

Administrator decides

which Domain Controllers receive it.

---

# Global Catalog (GC)

The Global Catalog is one of the most important components.

It stores

A partial copy

of every object

from every domain.

Example

Forest

```
simform.com

india.simform.com

usa.simform.com

uk.simform.com
```

Suppose user

```
Rahul
```

belongs to

```
india.simform.com
```

Another user searches Rahul from

```
uk.simform.com
```

Without Global Catalog

```
UK DC

↓

India DC

↓

Search

↓

Return
```

Slow.

With Global Catalog

```
UK GC

↓

Find Rahul Immediately
```

Very fast.

---

# Why Partial Copy?

Imagine

100 Domains

Each has

500,000 users.

Complete replication would consume

huge storage.

Instead,

Global Catalog stores only

important attributes.

Example

```
Username

Email

Department

Phone
```

Not every attribute.

This saves storage.

---

# ForestDNSZone

Stores DNS information

shared across the Forest.

Example

```
Forest DNS Records

SRV Records

Forest-wide DNS

Replication
```

Every Domain Controller

can receive it.

---

# DomainDNSZone

Stores DNS records

only for one domain.

Example

```
india.simform.com

DNS Records
```

Replication

Only inside

India Domain.

---

# Replication Summary

| Partition | Replication Scope |
|------------|-------------------|
| Schema | Entire Forest |
| Configuration | Entire Forest |
| Domain | Only Domain |
| Application | Selected DCs |
| ForestDNSZone | Forest |
| DomainDNSZone | Domain |

---

# Authentication Flow

Suppose

Prins logs into Windows.

```
User

↓

Computer

↓

Domain Controller

↓

Kerberos

↓

Global Catalog

↓

Password Verification

↓

Access Granted
```

If Prins accesses another domain,

Global Catalog helps locate

his account quickly.

---

# Real-World Company Example

Company

```
Simform
```

Forest

```
simform.com
```

Domains

```
india.simform.com

usa.simform.com

uk.simform.com
```

Users

```
Prins

Rahul

Amit

John
```

Suppose Rahul moves

from India

to USA.

Administrator simply

moves Rahul's account.

Authentication continues

because

Global Catalog

updates automatically.

---

# Complete Architecture

```
                           FOREST
                               │
         ┌─────────────────────┴──────────────────────┐
         │                                            │
      Tree 1                                      Tree 2
         │                                            │
 simform.com                               microsoft.net
         │                                            │
 ├──────────────┐                             ├────────────┐
 │              │                             │            │
india      usa.simform.com             azure.microsoft.net
.simform.com                           office.microsoft.net
 │
Ahmedabad.india.simform.com

Every Domain Controller Contains

NTDS.dit

├── Configuration
├── Schema
├── Domain
└── Application

Global Catalog

Stores

Partial copy of all domains.
```

---

# Interview Questions

### What is a Domain?

A logical security and administrative boundary that stores users, computers, groups, policies, and other objects.

---

### What is a Tree?

A collection of domains sharing the same contiguous DNS namespace.

---

### What is a Forest?

A collection of one or more Trees sharing a common Schema, Configuration, and Global Catalog.

---

### What is NTDS.dit?

The Active Directory database file that stores all directory information.

---

### What is the Schema?

The blueprint that defines object classes and their attributes.

---

### What is the Configuration Partition?

Stores forest-wide configuration such as sites, replication topology, and services.

---

### What is the Domain Partition?

Stores domain-specific objects like users, groups, computers, OUs, and GPOs. It replicates only within that domain.

---

### What is the Global Catalog?

A Global Catalog (GC) server stores a searchable partial copy of objects from every domain in the forest, allowing fast searches and supporting user logon and universal group membership lookups.

---

### Difference Between ForestDNSZone and DomainDNSZone

- **ForestDNSZone**: Contains DNS data shared across the entire forest and replicates to all designated DNS servers in the forest.
- **DomainDNSZone**: Contains DNS records specific to a single domain and replicates only within that domain.

---

# Key Takeaways

- **Domain** = Security and administrative boundary.
- **Tree** = Multiple domains with the same DNS namespace.
- **Forest** = One or more trees sharing schema, configuration, and trust.
- **NTDS.dit** = Active Directory database stored on Domain Controllers.
- **Schema** = Defines object types and attributes.
- **Configuration Partition** = Forest-wide configuration data.
- **Domain Partition** = Domain-specific objects and policies.
- **Application Partition** = Custom application data with configurable replication.
- **Global Catalog** = Partial copy of all objects in the forest for fast searches and authentication.
- **ForestDNSZone** and **DomainDNSZone** store DNS information at forest and domain scopes, respectively.