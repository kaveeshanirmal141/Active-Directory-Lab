# ESC1 – Overview

## 🔹 Introduction

ESC1 is a well-known Active Directory Certificate Services (ADCS) misconfiguration that allows low-privileged users to request certificates on behalf of other users.

If exploited successfully, this can lead to:

- Privilege escalation
- Domain compromise
- Certificate-based impersonation
- Kerberos authentication as another user

including Domain Administrators.

---

## 🔹 What is ESC1?

ESC1 occurs when a certificate template is configured with:

- Client Authentication enabled
- "Supply in the request" enabled
- Enrollment permissions granted to low-privileged users

This combination allows attackers to:

```text
Request a certificate for ANY user identity
```

instead of only themselves.

---

## 🔹 Why This is Dangerous

Certificates in Active Directory can be used for:

- PKINIT authentication
- Smartcard logon
- Kerberos authentication

This means:

```text
A certificate can become equivalent to a password
```

If an attacker can request a certificate as:

```text
Administrator@domain.local
```

they may authenticate as that user without knowing the password.

---

## 🔹 Root Cause

The vulnerability exists because the Certificate Authority trusts:

```text
Information supplied by the requester
```

instead of strictly validating identity ownership.

---

## 🔹 Vulnerable Configuration

The following settings commonly create ESC1 conditions:

| Setting | Risk |
|----------|------|
| Supply in the request | Allows arbitrary identities |
| Client Authentication EKU | Enables authentication usage |
| Enroll permission for Authenticated Users | Any domain user can request |

---

## 🔹 Simplified Attack Flow

```text
Low-privileged user
        ↓
Requests certificate as Administrator
        ↓
CA issues certificate
        ↓
Attacker authenticates using certificate
        ↓
Domain compromise
```

---

## 🔹 Real-World Relevance

ADCS is widely deployed in enterprise environments for:

- Smartcard authentication
- VPN authentication
- Device certificates
- Internal PKI

Misconfigured templates are often overlooked because:

- ADCS is less monitored than passwords
- Certificate abuse leaves different artifacts
- Many administrators misunderstand template security

---

## 🔹 Authentication Impact

ESC1 enables:

- Certificate-based impersonation
- Kerberos TGT requests via PKINIT
- Passwordless privilege escalation

This bypasses many traditional security assumptions.

---

## 🔹 Key Observation

```text
The attacker does NOT need the target user's password.
```

The certificate itself becomes the trusted identity object.

---

## 🔹 Detection Challenges

Certificate abuse may be harder to detect because:

- No password guessing occurs
- No brute force activity occurs
- Authentication appears legitimate

---

## 🔹 Security Lessons

ESC1 demonstrates the importance of:

- Proper certificate template permissions
- Least privilege
- Restricting enrollment rights
- Monitoring certificate issuance

---

## 🔹 Conclusion

ESC1 is one of the most impactful ADCS misconfigurations because it can convert:

```text
Low-privileged domain access
```

into:

```text
Full domain compromise
```

through trusted certificate-based authentication mechanisms.