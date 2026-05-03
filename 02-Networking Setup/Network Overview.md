# Network Overview

## 🔹 Introduction

This section describes the network architecture of the Active Directory lab, including IP addressing, DNS configuration, and how machines communicate within the environment.

Proper networking is critical in Active Directory, as nearly all core services (authentication, domain join, certificate services) depend on correct DNS resolution.

---

## 🔹 Network Architecture

All machines are connected within the same subnet to simulate an internal enterprise network.

```text
192.168.8.0/24
```

---

## 🔹 Machines & IP Allocation

| Machine   | IP Address       | DNS Configuration        |
|-----------|------------------|--------------------------|

| DC01      | 192.168.8.100    | Self (192.168.8.100)     |

| CA01      | 192.168.8.101    | DC01 (192.168.8.100)     |

| CLIENT01  | 192.168.8.102    | DC01 (192.168.8.100)     |

---

## 🔹 Name Resolution Flow

```text
CLIENT01 → queries dc01.homecorp.local
        ↓
DNS request sent to DC01
        ↓
DC01 resolves internally (AD-integrated DNS)
        ↓
Returns IP (192.168.8.100)
```

---

## 🔹 DNS Design (Critical)

DC01 acts as the primary DNS server.

All domain-joined machines rely on DC01 for:

- Domain resolution  
- Kerberos authentication  
- Service discovery (LDAP, KDC)  

---

## 🔹 Key Rule in Active Directory

```text
If DNS is broken → Active Directory is broken
```

Even when basic connectivity exists (ping works), improper DNS configuration leads to:

- Domain join failures  
- Authentication issues  
- Certificate enrollment failures  

---

## 🔹 Connectivity vs Functionality

A key observation during this lab:

```text
Ping success ≠ Active Directory working
```

Machines may appear connected, but AD services will fail if DNS is misconfigured.

---

## 🔹 Network Modes Used

During the lab, different network configurations were tested:

- Bridged Networking (direct LAN access)  
- NAT Networking (isolated internal testing)  

The final working configuration ensured:

- All machines were reachable  
- DNS resolution was stable  
- Domain services functioned correctly  

---

## 🔹 Dependencies

Active Directory heavily depends on:

- DNS (primary dependency)  
- Proper IP configuration  
- Correct domain suffix settings  

Any inconsistency in these leads to unstable or broken AD behavior.

---

## 🔹 Notes

This lab environment intentionally exposed how fragile Active Directory networking can be when DNS is not properly configured.

A detailed breakdown of the DNS issues and debugging process is documented in:

👉 DNS-Debugging.md
