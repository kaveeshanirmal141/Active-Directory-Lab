# Domain Join Process

## 🔹 Overview

This section documents the process of joining lab machines to the `homecorp.local` domain.

Domain joining allows machines to:

- Authenticate against Active Directory
- Receive domain policies
- Access centralized identity services
- Participate in Kerberos authentication

---

## 🔹 Machines Joined

The following machines were joined to the domain:

| Machine | Role |
|----------|------|
| CA01 | Certificate Authority |
| CLIENT01 | Domain Workstation |

---

## 🔹 DNS Requirement

Before domain joining, all machines were configured to use:

```text
192.168.8.100 (DC01)
```

as their primary DNS server.

This is critical because Active Directory relies heavily on DNS for:

- Domain discovery
- Kerberos authentication
- LDAP service location

---

## 🔹 Domain Join Procedure

### Step 1 – Open System Properties

```text
System → About → Rename this PC (Advanced)
```

---

### Step 2 – Join Domain

Domain entered:

```text
homecorp.local
```

---

### Step 3 – Authenticate

Credentials used:

```text
HOMECORP\Administrator
```

---

### Step 4 – Restart Machine

After successful join:

```text
Machine restarted automatically
```

---

## 🔹 Verification

### Command

```cmd
whoami
```

Expected:

```text
homecorp\administrator
```

---

### Domain Verification

```cmd
echo %USERDOMAIN%
```

Expected:

```text
HOMECORP
```

---

## 🔹 Key Observation

```text
Machines may appear network-connected but still fail domain joining if DNS is misconfigured.
```

---

## 🔹 Outcome

All lab machines successfully joined:

```text
homecorp.local
```
This enabled:

- Centralized authentication
- Certificate enrollment
- Kerberos-based communication
- ADCS integration

This is a MUST !!!