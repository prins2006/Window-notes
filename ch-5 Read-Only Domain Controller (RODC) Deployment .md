# Read-Only Domain Controller (RODC) Deployment Workflow

---

# Table of Contents

1. What is an RODC?
2. Why Companies Deploy RODCs
3. Real-World Business Scenario
4. Network Topology
5. Complete Deployment Workflow
6. Authentication Workflow
7. Password Replication Policy (PRP)
8. WAN Failure Scenario
9. Replication Workflow
10. Administrator Workflow
11. Security Benefits
12. Branch Office Design
13. Production Best Practices
14. Interview Questions

---

# Introduction

A **Read-Only Domain Controller (RODC)** is used when a company has a **remote office** or **branch office** that:

- Has very few employees
- Does not have a local IT administrator
- Is connected through a slow WAN link
- Is not physically secure

Instead of installing a Writable Domain Controller, the company installs an **RODC**.

The RODC provides:

- Local user authentication
- DNS services
- Faster logins
- Better WAN performance
- Improved security

---

# Real-World Company Example

Suppose a company named **ABC Technologies** has offices worldwide.

```
                     ABC Technologies

                        Head Office
                         New York
                      500 Employees
                    Writable DC01
                    Writable DC02

              /                       \
             /                         \

      Dallas Office               Birmingham Office
      300 Employees                 10 Employees
      Writable DC03                 RODC01
                                    DNS
```

---

# Why is Birmingham Using an RODC?

The Birmingham office:

- Only 10 employees
- Sales office only
- No IT staff
- Shared office building
- Physical server room is not secure
- Internet connection through VPN

Installing a writable Domain Controller would increase security risk.

Therefore:

```
Install

RODC

instead of

Writable DC
```

---

# Production Network

```
                     Internet
                         │
                    Corporate VPN
                         │
        ┌─────────────────────────────────────┐
        │                                     │
        ▼                                     ▼

   Head Office                         Branch Office

 Writable DC                      Read-Only DC

 DNS                              DNS

 Global Catalog                   Cached Passwords

 FSMO Roles                       No FSMO

 Full Database                    Read-Only Database
```

---

# Deployment Workflow

## Step 1

Create Active Directory at Head Office

```
DC01

↓

Install AD DS

↓

Create Forest

↓

Create Domain

↓

Install DNS
```

Example

```
company.local
```

---

## Step 2

Install Second Writable Domain Controller

Purpose

- High Availability
- Replication
- Backup Authentication

```
DC01

⇄

DC02
```

Two-way replication.

---

## Step 3

Prepare Branch Office

Install Windows Server

Configure

- Static IP
- DNS
- Hostname

Example

```
RODC01
```

---

## Step 4

Join the Server to the Domain

```
RODC01

↓

Join

company.local
```

The server is still a member server.

Not yet a Domain Controller.

---

## Step 5

Install AD DS Role

```
Server Manager

↓

Add Roles

↓

Active Directory Domain Services
```

---

## Step 6

Promote Server

Select

```
Add Domain Controller

↓

Choose Existing Domain

↓

company.local
```

Then select

```
✔ Read Only Domain Controller
```

Windows installs

```
DNS

+

RODC
```

---

# What Happens During Promotion?

The Writable Domain Controller sends:

```
Schema

↓

Configuration

↓

Domain Objects

↓

DNS Records
```

The RODC creates

```
NTDS.dit

(Read Only)
```

---

# Active Directory Replication

Writable DC

```
DC01

↓

RODC01
```

Only one direction.

```
Updates

↓

RODC
```

RODC never sends directory changes back.

---

# Password Replication Policy (PRP)

One of the most important RODC features.

Default:

```
No passwords stored.
```

Administrator decides:

Allowed:

```
Branch Users

Sales Team

Support Team
```

Denied:

```
Domain Admins

Enterprise Admins

Server Admins

Backup Operators
```

---

# First User Login

Suppose

```
Rahul
```

works in Birmingham.

Login process:

```
Rahul

↓

RODC

↓

Password Not Cached

↓

DC01

↓

Password Verified

↓

(Optional Password Cache)

↓

Login Success
```

---

# Second Login

Now Rahul logs in again.

```
Rahul

↓

RODC

↓

Password Cached

↓

Immediate Login
```

The WAN link is no longer required for this authentication.

---

# Another User Login

Suppose

```
Administrator
```

tries to log in.

Administrator password

```
Not Cached
```

Authentication:

```
Administrator

↓

RODC

↓

Head Office

↓

Authentication

↓

Access
```

If the WAN is unavailable, the administrator cannot log in because privileged passwords are intentionally not cached.

---

# User Password Reset

Suppose Rahul forgets his password.

He contacts IT.

Workflow:

```
Help Desk

↓

Writable DC

↓

Password Reset

↓

Replication

↓

RODC Receives Update
```

The RODC cannot reset passwords locally.

---

# Creating a User

HR creates a new employee.

```
HR

↓

DC01

↓

New User

↓

Replication

↓

RODC
```

RODC automatically receives the new object.

---

# Group Policy Workflow

Administrator creates a new GPO.

```
DC01

↓

SYSVOL

↓

Replication

↓

RODC

↓

Branch PCs Receive Policy
```

---

# DNS Workflow

Client

```
Laptop

↓

RODC DNS

↓

Resolve Name

↓

Server Found
```

Local DNS reduces WAN traffic and improves response time.

---

# WAN Failure Scenario

Imagine the VPN between Birmingham and New York fails.

### User Already Cached

```
Rahul

↓

RODC

↓

Cached Password

↓

Login Success
```

### New User

```
Amit

↓

RODC

↓

Password Not Cached

↓

Cannot Reach DC01

↓

Authentication Fails
```

This illustrates why PRP planning is important.

---

# If Someone Steals the Server

Imagine an attacker steals the Birmingham server.

They obtain:

```
Read-Only NTDS.dit

Limited Cached Passwords

DNS Data
```

They do **not** gain:

- Domain Admin password (if not cached)
- Enterprise Admin credentials
- Ability to modify Active Directory
- FSMO roles

This greatly limits the impact compared to stealing a writable Domain Controller.

---

# Complete Authentication Workflow

```
                User Login

                    │

                    ▼

               Branch Office

                    │

                 RODC01

         Password Cached ?

            /            \

         YES              NO

          │                │

          ▼                ▼

     Login Success     Writable DC

                            │

                    Password Verified

                            │

              PRP Allows Password Cache?

                   /                 \

                YES                  NO

                 │                    │

                 ▼                    ▼

      Cache Password         Do Not Cache

                 │

                 ▼

           Login Success
```

---

# Production Best Practices

- Deploy RODCs only in remote or physically insecure locations.
- Install DNS on the RODC for local name resolution.
- Do not cache privileged accounts.
- Configure Password Replication Policy carefully.
- Monitor replication using `repadmin`.
- Regularly verify replication health with `dcdiag`.
- Maintain at least two writable Domain Controllers at the main site.
- Secure the branch server with BitLocker and physical access controls.

---

# RODC vs Writable Domain Controller

| Feature | Writable DC | RODC |
|----------|-------------|------|
| Read Active Directory | Yes | Yes |
| Write Active Directory | Yes | No |
| Password Reset | Yes | No |
| Create Users | Yes | No |
| Delete Users | Yes | No |
| FSMO Roles | Yes | No |
| Password Caching | Full | Controlled by PRP |
| Replication | Two-Way | One-Way (Receive Only) |
| Best Location | Headquarters, Data Center | Branch Office |

---

# Interview Questions

### Why should a company deploy an RODC?

To provide local authentication and DNS services in remote or physically insecure branch offices while minimizing the security risks associated with storing a writable Active Directory database.

---

### Can an RODC modify Active Directory?

No. An RODC has a read-only copy of the Active Directory database and cannot create, modify, or delete directory objects.

---

### What happens if the WAN connection is down?

- Users whose passwords are **cached** on the RODC can still log in.
- Users whose passwords are **not cached** cannot be authenticated until connectivity to a writable Domain Controller is restored.

---

### What is Password Replication Policy (PRP)?

PRP controls which user and computer passwords are allowed or denied from being cached on an RODC, helping protect privileged accounts.

---

# Key Takeaways

- An **RODC** is a secure, read-only Domain Controller designed for branch offices.
- It **receives replication** from writable Domain Controllers but never sends directory changes back.
- **Password Replication Policy (PRP)** determines which passwords are stored locally.
- Cached users can continue to authenticate during WAN outages, improving branch office availability.
- Administrative tasks such as creating users, resetting passwords, and managing Group Policy must always be performed on a **writable Domain Controller**.
- RODCs reduce security risks while providing fast local authentication and DNS services.