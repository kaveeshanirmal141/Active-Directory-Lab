# Rubeus

## 🔹 Overview

Rubeus is a Kerberos interaction and abuse tool commonly used in Active Directory security testing.

In this lab, Rubeus was used to:

- Request Kerberos TGTs
- Perform PKINIT authentication
- Validate certificate-based privilege escalation

---

## 🔹 Source

Official Repository:

```text
https://github.com/GhostPack/Rubeus
```

---

## 🔹 Challenge Encountered

Precompiled binaries triggered Microsoft Defender alerts:

```text
VirTool:MSIL/CezAbuz
```

because offensive security tools are commonly flagged as potentially malicious.

---

## 🔹 Resolution

Instead of downloading precompiled binaries, Rubeus was compiled manually inside the lab environment using Visual Studio.

This provided:

- Better understanding of the tool
- Cleaner operational workflow
- Reduced trust concerns with third-party binaries

---

## 🔹 Build Process

### Clone Repository

```cmd
git clone https://github.com/GhostPack/Rubeus.git
```

---

### Open in Visual Studio

```text
Rubeus.sln
```

---

### Build Mode

```text
Release
```

---

## 🔹 Output Binary

Compiled binary location:

```text
Rubeus\bin\Release\Rubeus.exe
```

---

## 🔹 Primary Use in Lab

Rubeus was used to request a Kerberos TGT using a certificate obtained through ESC1 abuse.

### Example Command

```cmd
Rubeus.exe asktgt /user:Administrator /certificate:admin.pfx /password:Pass123! /ptt
```

---

## 🔹 Initial Failure Encountered

Error:

```text
KDC_ERR_PADATA_TYPE_NOSUPP
```

---

## 🔹 Root Cause

The Domain Controller lacked proper PKINIT-compatible certificate configuration.

---

## 🔹 Final Resolution

A custom certificate template:

```text
PKINIT-Auth
```

was created and enrolled by the Domain Controller.

This enabled:

- PKINIT authentication
- Certificate-based Kerberos TGT requests

---

## 🔹 Outcome

Rubeus successfully obtained and injected a Kerberos TGT for:

```text
Administrator@homecorp.local
```

without requiring the Administrator password.

---

## 🔹 Key Learning

```text
Certificate-based authentication can become equivalent to password-based authentication if ADCS is misconfigured.
```