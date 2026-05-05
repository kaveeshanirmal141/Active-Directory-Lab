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
