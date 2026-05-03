# Network Overview

## Introduction

This section describes the network architecture of the Active Directory lab, including IP addressing, DNS configuration, and how machines communicate within the environment.

Proper networking is critical in Active Directory, as nearly all core services (authentication, domain join, certificate services) depend on correct DNS resolution.

---

## Network Architecture

All machines are connected within the same subnet to simulate an internal enterprise network.

```text
192.168.8.0/24

---

## Machines & IP Allocation

Machine | IP | DNS Config

DC01 - 192.168.8.100 - Self(192.168.8.100)

CA01 - 192.168.8.101 - DC01(192.168.8.101)

CLIENT01 - 192.168.102 - DC01(192.168.8.102)

---

## Name Resolution Flow

CLIENT01 → queries dc01.homecorp.local
        ↓
DNS request sent to DC01
        ↓
DC01 resolves internally (AD-integrated DNS)
        ↓
Returns IP (192.168.8.100)

---

## DNS Design (Critical)

DC01 acts as the primary DNS server
All domain-joined machines rely on DC01 for:
 Domain resolution
 Kerberos authentication
 Service discovery (LDAP, KDC)

---

## Network modes used

During the lab, different network modes were tested:

Bridged Networking (direct LAN access)
NAT Networking (isolated internal testing)

The final working configuration ensured:

All machines were reachable
DNS resolution was stable
Domain services functioned correctly
