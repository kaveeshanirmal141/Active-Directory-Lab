# Environment Setup

## 🔹 Overview

This section documents how the Active Directory lab environment was built from scratch, including virtualization setup, operating system installation, and system optimization.

The goal was to create a stable and realistic multi-machine environment capable of simulating enterprise-level behavior.

---

## 🔹 Virtualization Platform

- **Software Used:** VMware Workstation
- **Host System:** Windows 11 (Primary Machine)
- **Virtualization Type:** Local hypervisor-based lab

---

## 🔹 Lab Machines

The environment consists of three primary virtual machines:

| Machine | OS | Role |
------------------------
| DC01 - Windows Server 2022 - Domain Controller + DNS 

| CA01 - Windows Server 2022 - Certificate Authority (ADCS)

| CLIENT01 - Windows 11 - Domain User Workstation

| SRV01 - Windows Server 2022 - External

Link to download server ISO -- https://www.microsoft.com/en-us/evalcenter/download-windows-server-2022

---

## 🔹 VM Configuration (Optimized)

To ensure smooth performance while running multiple machines:

### DC01
- RAM: 4 GB
- CPU: 2 cores
- Disk: ~50 GB

### CA01
- RAM: 4 GB
- CPU: 2 cores
- Disk: ~50 GB

### CLIENT01
- RAM: 4 GB
- CPU: 2 cores
- Disk: ~50 GB

---

## 🔹 Network Configuration

- **Mode Used:** Bridged / NAT (depending on testing phase)
- All machines are placed within the same subnet

Example IP Range: - 192.168.8.0/24

---


### DNS Configuration

- Primary DNS for all machines: - 192.168.8.100/24

---


---

## 🔹 Operating System Installation

### Windows Server 2022 (DC01 & CA01)

1. Mounted ISO in VMware
2. Installed with default settings
3. Assigned static IP addresses
4. Renamed machines accordingly:
   - DC01
   - CA01

---

### Windows 11 (CLIENT01)

1. Installed Windows 11 ISO
2. Configured as a domain workstation
3. Joined to domain: - homecorp.local

---


---

## 🔹 Domain Setup (DC01)

- Installed:
  - Active Directory Domain Services (AD DS)
  - DNS Server

- Promoted DC01 to Domain Controller: - homecorp.local

---


---

## 🔹 ADCS Setup (CA01)

- Installed:
  - Active Directory Certificate Services (ADCS)

- Configuration:
  - Enterprise Root CA
  - Integrated with domain

---

## 🔹 Optimization & Stability

To ensure stability across multiple VMs:

- Disabled unnecessary startup services
- Used moderate RAM allocation (balanced performance)
- Avoided over-allocation to prevent host slowdown
- Ensured all machines use DC01 as DNS

---

## 🔹 Key Challenges During Setup

- DNS inconsistencies due to IPv6 preference
- Network instability when modifying adapter settings
- Domain resolution issues across machines

(Full debugging documented in Networking section)

---

## 🔹 Outcome

A fully functional multi-machine Active Directory lab environment capable of:

- Domain authentication
- Certificate issuance
- Real-world misconfiguration simulation
- Attack and defense testing
