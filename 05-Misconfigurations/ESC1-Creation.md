# ESC1 – Vulnerable Template Creation

## 🔹 Overview

This section documents the intentional creation of an ESC1-vulnerable certificate template inside the `homecorp.local` Active Directory lab.

The goal was to simulate a realistic Active Directory Certificate Services (ADCS) misconfiguration where:

```text
Low-privileged users can request authentication certificates as other users.
```

including privileged accounts such as:

```text
Administrator@homecorp.local
```

---

## 🔹 Objective

The lab intentionally configured a vulnerable certificate template to demonstrate:

- ADCS misconfiguration risks
- Certificate-based privilege escalation
- PKINIT abuse
- Kerberos impersonation

---

## 🔹 Initial Environment

### Infrastructure

| Server | Role |
|--------|------|
| DC01 | Domain Controller |
| CA01 | Enterprise Root CA |
| CLIENT01 | Domain Workstation |

---

## 🔹 Tools & Consoles Used

| Tool | Purpose |
|------|---------|
| `certtmpl.msc` | Certificate template management |
| `certsrv.msc` | Certificate Authority management |
| MMC Certificates Snap-in | Certificate enrollment |
| Rubeus | Kerberos PKINIT authentication |

---

# 🔹 Step 1 – Open Certificate Templates

On:

```text
CA01
```

Open:

```cmd
certtmpl.msc
```

This launches the Certificate Templates Console.

---

# 🔹 Step 2 – Duplicate Existing Template

The following template was duplicated:

```text
Workstation Authentication
```

Reason:

- Already supports authentication usage
- Easier foundation for PKINIT-compatible abuse

---

# 🔹 Step 3 – Configure New Template

Template name:

```text
User-Auth
```

---

## 🔹 Critical Vulnerable Setting

Inside:

```text
Subject Name Tab
```

Enabled:

```text
Supply in the request
```

---

## 🔹 Why This is Dangerous

This setting allows the requester to manually specify:

- Common Name (CN)
- UPN
- Subject Alternative Name (SAN)

instead of Active Directory automatically enforcing identity ownership.

---

# 🔹 Step 4 – Authentication EKU

The template retained:

```text
Client Authentication
```

Extended Key Usage (EKU).

---

## 🔹 Security Impact

This allows issued certificates to be used for:

- Kerberos authentication
- Smartcard logon
- PKINIT authentication

---

# 🔹 Step 5 – Enrollment Permissions

Inside:

```text
Security Tab
```

The following permissions were granted:

| Group | Permissions |
|------|-------------|
| Authenticated Users | Read, Enroll |

---

## 🔹 Why This Creates ESC1

This means:

```text
Any authenticated domain user can request certificates.
```

Combined with:

```text
Supply in the request
```

this becomes highly dangerous.

---

# 🔹 Step 6 – Publish Template to CA

Open:

```cmd
certsrv.msc
```

Navigate to:

```text
Certificate Templates
```

Then:

```text
Right Click → New → Certificate Template to Issue
```

Select:

```text
User-Auth
```

---

## 🔹 Result

The vulnerable template became available for domain enrollment.

---

# 🔹 Step 7 – Certificate Enrollment Test

On:

```text
CLIENT01
```

Logged in as:

```text
HOMECORP\arthur
```

---

## 🔹 Open Enrollment Console

```cmd
certmgr.msc
```

Navigate:

```text
Personal → Certificates → All Tasks → Request New Certificate
```

---

## 🔹 Enrollment Policy

Selected:

```text
Active Directory Enrollment Policy
```

---

## 🔹 Template Selection

Selected:

```text
User-Auth
```

---

# 🔹 Initial Failure Encountered

Enrollment initially failed with:

```text
The EMail name is unavailable and cannot be added to the Subject or Subject Alternate name.
```

---

## 🔹 Root Cause

The template configuration attempted to enforce identity fields that were not populated for the user object.

---

## 🔹 Resolution

Template subject requirements were adjusted to avoid requiring unavailable email attributes.

After modification:

```text
Certificate enrollment succeeded
```

---

# 🔹 Successful Outcome

Arthur successfully received:

```text
Authentication certificate
```

despite being only a standard domain user.

---

# 🔹 Security Impact

At this stage:

```text
A low-privileged user could request authentication certificates.
```

Combined with:

```text
Supply in request
```

this enabled impersonation of other users.

---

# 🔹 Key Observation

```text
The CA trusted user-supplied identity information.
```

instead of strictly validating ownership.

---

# 🔹 Why ESC1 Exists

ESC1 is fundamentally caused by:

| Weakness | Impact |
|----------|--------|
| Arbitrary subject input | Identity spoofing |
| Broad enrollment permissions | Low-privileged access |
| Authentication EKU | Real authentication abuse |

---

# 🔹 Real-World Relevance

In enterprise environments, vulnerable templates may exist because:

- Administrators duplicate templates without understanding security implications
- Enrollment permissions are overly broad
- ADCS is often poorly monitored

---

# 🔹 Key Learning

```text
Certificate templates are security boundaries.
```

Improper configuration can transform:

```text
Standard domain user
```

into:

```text
Domain Administrator
```

without password theft.

---

# 🔹 Conclusion

The lab successfully created an ESC1-vulnerable certificate template that allowed:

- Low-privileged enrollment
- Arbitrary identity specification
- Authentication certificate issuance

This became the foundation for later:

- PKINIT abuse
- Kerberos impersonation
- Certificate-based privilege escalation