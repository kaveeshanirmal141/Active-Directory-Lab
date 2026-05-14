# User & Privilege Configuration

## 🔹 Overview

This section documents the creation of standard and administrative users inside the `homecorp.local` Active Directory environment.

The goal was to simulate a realistic enterprise identity structure with:

- Low-privileged users
- Administrative accounts
- Domain-based authentication

---

## 🔹 Users Created

| Username | Role |
|----------|------|
| Administrator | Domain Administrator |
| Arthur | Standard Domain User |

---

## 🔹 User Creation Method

Users were created using:

```text
Active Directory Users and Computers (dsa.msc)
```

---

## 🔹 Standard User Creation

### Example User

```text
Arthur
```

### Properties

| Setting | Value |
|---------|------|
| Username | arthur |
| Domain | homecorp.local |
| Privilege Level | Standard User |

---

## 🔹 Administrative Accounts

The built-in:

```text
Administrator
```

account was used for:

- Domain administration
- Server configuration
- ADCS management
- Certificate template configuration

---

## 🔹 Authentication Testing

Users were tested by logging into:

```text
CLIENT01
```

and verifying:

```cmd
whoami
```

Expected:

```text
homecorp\arthur
```

---

## 🔹 Permission Separation

The environment intentionally maintained separation between:

| Type | Purpose |
|------|---------|
| Standard User | Simulated enterprise employee |
| Administrator | Infrastructure management |

This separation later became important during ESC1 exploitation testing.

---

## 🔹 Key Learning

```text
Identity and privilege separation are core security boundaries in Active Directory.
```

Misconfigurations in certificate services can allow low-privileged users to cross those boundaries.

---

## 🔹 Outcome

The environment successfully simulated:

- Enterprise user authentication
- Standard vs privileged access
- Domain identity management