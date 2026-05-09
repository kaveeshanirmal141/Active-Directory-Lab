# CA01 Setup – Active Directory Certificate Services (ADCS)

## 🔹 Overview

This section documents the setup of the Certificate Authority server (CA01) and the deployment of Active Directory Certificate Services (ADCS) within the `homecorp.local` domain.

ADCS enables:

- Certificate-based authentication
- Smartcard logon
- PKINIT authentication
- Enterprise certificate enrollment

This service later became the foundation for ADCS exploitation scenarios such as ESC1.

---

## 🔹 Initial Server Configuration

### Server Name

```text
CA01
```

### Static IP Configuration

```text
IP Address:     192.168.8.101
Subnet Mask:    255.255.255.0
DNS Server:     192.168.8.100 (DC01)
```

---

## 🔹 Domain Join

CA01 was joined to the domain:

```text
homecorp.local
```

### Verification

```cmd
whoami
```

Expected:

```text
homecorp\administrator
```

---

## 🔹 Installing ADCS Role

### PowerShell Installation

```powershell
Install-WindowsFeature ADCS-Cert-Authority -IncludeManagementTools
```

---

## 🔹 Configuring Certificate Services

After installation:

```powershell
Install-AdcsCertificationAuthority `
-CAType EnterpriseRootCA `
-CryptoProviderName "RSA#Microsoft Software Key Storage Provider" `
-KeyLength 2048 `
-HashAlgorithmName SHA256 `
-CACommonName "homecorp-CA01-CA" `
-ValidityPeriod Years `
-ValidityPeriodUnits 5 `
-Force
```

---

## 🔹 CA Type Used

The lab uses:

```text
Enterprise Root CA
```

This allows integration with:

- Active Directory
- Certificate templates
- Auto-enrollment
- Kerberos authentication

---

## 🔹 Verification

### Check CA Configuration

```cmd
certutil -cainfo
```

Expected output contains:

```text
Enterprise Root CA
```

---

## 🔹 Certificate Templates

Certificate templates were managed using:

```cmd
certtmpl.msc
```

This allowed:

- Viewing default templates
- Duplicating templates
- Modifying enrollment permissions
- Configuring vulnerable templates

---

## 🔹 Certificate Authority Management

CA management console:

```cmd
certsrv.msc
```

Used for:

- Issuing templates
- Managing requests
- Viewing issued certificates
- Configuring CA behavior

---

## 🔹 Initial Issue Encountered

Certain templates such as:

- Kerberos Authentication
- Domain Controller Authentication

appeared in:

```text
certtmpl.msc
```

but did NOT appear inside:

```text
Certificate Templates → New → Certificate Template to Issue
```

---

## 🔹 Troubleshooting Performed

The following actions were attempted:

### Restart Certificate Services

```cmd
net stop certsvc
net start certsvc
```

---

### Refresh CA Templates

```cmd
certutil -pulse
```

---

### Attempt Manual Template Publication

```cmd
certutil -setcatemplates +KerberosAuthentication
```

---

## 🔹 Final Resolution

Instead of relying on unavailable default templates, a custom template was created by duplicating:

```text
Workstation Authentication
```

The duplicate template:

```text
PKINIT-Auth
```

was modified to support:

- Client Authentication
- Server Authentication
- Supply in the request

This successfully enabled PKINIT functionality later in the lab.

---

## 🔹 Key Learning

```text
Not all templates visible in certtmpl.msc are automatically issuable by the CA.
```

Enterprise CAs may require:

- Template publication
- Replication
- Proper permissions
- CA refresh/restart

---

## 🔹 Services Installed

CA01 now provides:

- Active Directory Certificate Services
- Enterprise PKI functionality
- Certificate enrollment
- PKINIT support

---

## 🔹 Real-World Relevance

ADCS is heavily used in enterprise environments for:

- VPN authentication
- Smartcard authentication
- Wi-Fi authentication
- Device certificates
- Internal TLS certificates

Misconfiguration of these services can lead to:

- Domain compromise
- Privilege escalation
- Certificate abuse (ESC1–ESC8)

---

## 🔹 Conclusion

CA01 was successfully configured as an Enterprise Root CA integrated with Active Directory.

This server became the foundation for:

- Certificate enrollment
- PKINIT authentication
- ESC1 exploitation
- Certificate-based privilege escalation scenarios
