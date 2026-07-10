# Read-Only Domain Controller (RODC)

## Table of Contents

1. What is an RODC?
2. Why do we need an RODC?
3. Understanding the Diagram
4. How an RODC Works
5. Writable DC vs RODC
6. Password Replication Policy (PRP)
7. Authentication Process
8. RODC Architecture
9. Replication
10. DNS on RODC
11. Security Benefits
12. Real-World Example
13. Advantages
14. Limitations
15. Installation Requirements
16. Best Practices
17. Interview Questions

---

# What is a Read-Only Domain Controller (RODC)?

A **Read-Only Domain Controller (RODC)** is a special type of Domain Controller that contains a **read-only copy** of the Active Directory database.

Unlike a normal Domain Controller, an RODC:

- Cannot make changes to Active Directory.
- Cannot update user or computer objects.
- Does not store all user passwords by default.
- Receives updates from writable Domain Controllers.
- Is designed for branch offices where physical security is limited.

Think of an RODC as a **local copy of Active Directory** that users can read from but cannot modify.

---

# Why Do We Need an RODC?

Large organizations often have many branch offices.

Example:

```
Company

Main Office
(New York)

↓

Branch Office
(Birmingham)

↓

Sales Office
(10 Employees)
```

The branch office:

- Has very few employees.
- Does not have an IT administrator.
- Is not physically secure.
- Has slow WAN connectivity.

Installing a writable Domain Controller here would be risky.

Instead, we deploy an **RODC**.

---

# Understanding the Diagram

```
                    Company Network

                         New York
                   (Head Office - 500 Users)

                  Writable Domain Controller
                          DC01
                         DNS
                          │
            ┌─────────────┴─────────────┐
            │                           │
            │                           │
       Dallas                     Birmingham
     (300 Users)                (Sales Office)

 Writable DC02                  RODC01
                                DNS Server
                                 10 Users
```

### Explanation

## New York

```
500 Employees
```

Contains

- Writable Domain Controller
- Full Active Directory Database
- DNS
- Global Catalog
- FSMO Roles

This is the main office.

---

## Dallas

```
300 Employees
```

Contains

- Writable Domain Controller
- Full Active Directory
- Replication

Dallas can update Active Directory.

Example

```
Create User

↓

Delete User

↓

Reset Password

↓

Create OU

↓

Group Policy
```

All allowed.

---

## Birmingham

```
Sales Office

10 Employees
```

Contains

```
RODC

+

DNS
```

The Birmingham office cannot modify Active Directory.

Instead,

it receives information from New York.

---

# Why Not Install a Writable DC?

Imagine someone steals the server.

If the server is a Writable DC,

the attacker gets:

- Password Hashes
- Entire Active Directory Database
- Administrative Information
- Ability to make changes

Huge security risk.

Instead,

install an RODC.

---

# How an RODC Works

Normal Domain Controller

```
User Login

↓

Writable DC

↓

Read Database

↓

Write Changes

↓

Replication
```

RODC

```
User Login

↓

RODC

↓

Read Database

↓

Cannot Modify Database

↓

Forwards Updates
```

The RODC can authenticate users but cannot update Active Directory objects.

---

# Read-Only Database

Writable Domain Controller

```
NTDS.dit

Read

Write
```

RODC

```
NTDS.dit

Read Only
```

Changes are not allowed.

---

# Replication

Writable DC

```
Two-Way Replication

DC01

⇄

DC02
```

RODC

```
Writable DC

↓

RODC

One-Way Replication
```

The RODC receives updates only.

It never sends database changes back.

---

# Password Replication Policy (PRP)

One of the most important RODC features.

By default,

an RODC **does not store user passwords**.

Example

```
User

Rahul

↓

Logs into Branch Office

↓

RODC asks

Writable DC

↓

Authentication

↓

Login Success
```

Administrator can decide whose passwords are cached.

Example

Allowed

```
Sales Users

Branch Users
```

Denied

```
Domain Admins

Enterprise Admins

Server Admins
```

This prevents privileged credentials from being stored at the branch office.

---

# Password Authentication Flow

### First Login

```
User

↓

RODC

↓

Writable DC

↓

Password Verified

↓

(Optional Password Cache)

↓

User Login
```

### Second Login (Password Cached)

```
User

↓

RODC

↓

Password Already Cached

↓

Immediate Login
```

If the password is not cached and the WAN link is unavailable, authentication may fail for that user.

---

# DNS on an RODC

RODC usually hosts DNS.

Example

```
Client

↓

RODC DNS

↓

Name Resolution
```

The DNS database is also protected against unauthorized updates.

---

# Security Benefits

Suppose the RODC server is stolen.

The attacker gets

```
Read Only Database
```

Not

```
Writable Database
```

Also,

important administrator passwords are usually **not cached**.

Risk is greatly reduced.

---

# Writable DC vs RODC

| Feature | Writable DC | RODC |
|----------|-------------|------|
| Read AD Database | Yes | Yes |
| Write AD Database | Yes | No |
| Create Users | Yes | No |
| Reset Passwords | Yes | No |
| Delete Users | Yes | No |
| Password Cache | All (as needed) | Controlled by PRP |
| Two-Way Replication | Yes | No |
| One-Way Replication | No | Yes |
| Suitable for Branch Office | Possible | Yes |

---

# Real-World Example

Company

```
Simform Pvt Ltd
```

Head Office

```
Ahmedabad

600 Employees
```

Contains

```
DC01

DC02
```

Branch Office

```
Mumbai

15 Employees
```

Contains

```
RODC01
```

Branch users authenticate locally through the RODC, while administrative changes such as creating users or resetting passwords are performed on the writable Domain Controllers at headquarters.

---

# Installation Requirements

Before installing an RODC:

- Active Directory Forest already exists.
- At least one Writable Domain Controller is available.
- DNS is configured correctly.
- The Forest Functional Level supports RODCs (Windows Server 2008 or later).

---

# Best Practices

- Deploy RODCs only in physically insecure or remote branch offices.
- Do not cache administrator passwords.
- Use Password Replication Policy (PRP) carefully.
- Monitor replication regularly.
- Install DNS on the RODC if branch clients require local name resolution.
- Maintain at least one writable Domain Controller at the main office.

---

# Interview Questions

## What is an RODC?

A Read-Only Domain Controller is a Domain Controller that contains a read-only copy of the Active Directory database and is intended for branch offices with limited physical security.

---

## Why is an RODC more secure?

Because it cannot modify Active Directory and does not cache all passwords by default, reducing the impact if the server is compromised.

---

## Can an RODC create users?

No.

Only writable Domain Controllers can create, modify, or delete Active Directory objects.

---

## What is Password Replication Policy (PRP)?

PRP determines which user and computer passwords are allowed or denied from being cached on an RODC.

---

## Does an RODC replicate changes back to the main office?

No.

Replication is **one-way**:

```
Writable DC

↓

RODC
```

The RODC receives updates but does not replicate directory changes back.

---

# Complete Network Example

```
                        Forest
                           │
                   simform.local
                           │
        ┌──────────────────┼──────────────────┐
        │                  │                  │
        │                  │                  │
   Ahmedabad          Pune Office      Mumbai Office
   (Head Office)      (Regional)       (Branch Office)
      │                  │                  │
      │                  │                  │
   DC01 (Writable)    DC02 (Writable)   RODC01
      │                  │             Read-Only AD
      │                  │             DNS
      │                  │             Password Cache (PRP)
      └─────────── Two-Way Replication ───────────┘
                           │
                     One-Way Replication
                           ▼
                        RODC01
```

---

# Key Takeaways

- An **RODC** stores a **read-only** copy of the Active Directory database.
- It is designed for **branch offices** with limited physical security.
- **Administrative changes** must be made on **writable Domain Controllers**.
- **Password Replication Policy (PRP)** controls which passwords are cached locally.
- Replication is **one-way**, from writable DCs to the RODC.
- RODCs improve security while allowing local authentication and DNS services in remote locations.