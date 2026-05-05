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
Link to download VMware -- https://www.vmware.com/products/desktop-hypervisor/workstation-and-fusion

---

## 🔹 VM Configuration (Optimized)

To ensure smooth performance while running multiple machines:

### DC01
- RAM: 4 GB
- CPU: 2 cores
- Disk: ~60 GB

### CA01
- RAM: 4 GB
- CPU: 2 cores
- Disk: ~60 GB

### CLIENT01
- RAM: 4 GB
- CPU: 2 cores
- Disk: ~60 GB

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

---

# DC01 Setup – Promoting to Domain Controller

## 🔹 Overview

This section documents the process of configuring DC01 as a Domain Controller and creating the Active Directory domain `homecorp.local`.

This is a critical step, as the Domain Controller provides:

- Authentication (Kerberos)
- Directory Services (AD DS)
- DNS (core dependency for AD)

---

## 🔹 Step 1 – Set Static IP Address

Before promoting to a Domain Controller, configure a static IP.

### Example Configuration:

```text
IP Address:     192.168.8.100
Subnet Mask:    255.255.255.0
Default Gateway: (optional depending on lab)
DNS Server:     192.168.8.100
```

---

## 🔹 Step 2 – Rename Server

Rename the machine to:

```text
DC01
```

### Command:

```powershell
Rename-Computer -NewName "DC01" -Restart
```

---

## 🔹 Step 3 – Install AD DS Role

Open PowerShell (Run as Administrator):

```powershell
Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
```

---

## 🔹 Step 4 – Promote to Domain Controller

After installing AD DS:

```powershell
Install-ADDSForest `
-DomainName "homecorp.local" `
-DomainNetbiosName "HOMECORP" `
-InstallDNS `
-SafeModeAdministratorPassword (ConvertTo-SecureString "Pass123!" -AsPlainText -Force) `
-Force
```

---

## 🔹 Step 5 – Automatic Restart

After execution:

- Server will install required components
- System will reboot automatically

---

## 🔹 Step 6 – Verify Domain Controller

After restart, log in using:

```text
HOMECORP\Administrator
```

---

## 🔹 Verification Commands

### Check domain:

```cmd
echo %USERDOMAIN%
```

Expected:
```text
HOMECORP
```

---

### Check FQDN:

```cmd
whoami /fqdn
```

Expected:
```text
administrator@homecorp.local
```

---

### Verify DNS:

```cmd
nslookup dc01.homecorp.local
```

Expected:
```text
Server: dc01.homecorp.local
Address: 192.168.8.100
```

---

## 🔹 Services Installed

After promotion, DC01 runs:

- Active Directory Domain Services (AD DS)
- DNS Server
- Kerberos Key Distribution Center (KDC)

---

## 🔹 Key Observations

```text
Active Directory is highly dependent on DNS.
Improper DNS configuration can break the entire domain.
```

---

## 🔹 Common Issues Faced

- DNS misconfiguration (resolved later in DNS debugging section)
- IPv6 taking priority over IPv4
- Domain resolution inconsistencies

---

## 🔹 Conclusion

DC01 was successfully promoted to a Domain Controller for the domain:

```text
homecorp.local
```

This forms the core of the lab environment and enables:

- Domain authentication
- Certificate services integration (ADCS)
- Exploitation scenarios (ESC1, PKINIT)
