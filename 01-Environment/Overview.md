# Environment Overview

## Introduction

This lab environment is designed to simulate a real-world Active Directory (AD) infrastructure with integrated Certificate Services (ADCS).

The purpose of this environment is not only to build AD from scratch, but to:
- Understand how core services interact
- Identify real-world misconfigurations
- Exploit those misconfigurations (e.g., ESC1)
- Validate attack paths with proof of access

---

## Domain Details

- **Domain Name:** homecorp.local  
- **Environment Type:** Isolated Lab (Virtualized)  
- **Authentication:** Kerberos (Primary), NTLM (Fallback)

---

## Machines Overview

| Hostname  |  Role  |  Description  |
--------------------------------------
| DC01 - Domain Controller - Handles Active Directory, DNS, and Kerberos (KDC)

| CA01 - Certificate Authority - Enterprise Root CA providing certificate services 

| CLIENT01 - Workstation - Domain-joined machine used by low-privileged user

| SRV01 - External

---

## Core Services in the Lab

- **Active Directory Domain Services (AD DS)**
- **DNS (Domain-integrated)**
- **Kerberos Authentication (KDC)**
- **Active Directory Certificate Services (ADCS)**

---

## Lab Objectives

This environment was built to simulate and understand:

- Enterprise Active Directory deployment
- Dependency of AD on proper DNS configuration
- Certificate-based authentication mechanisms
- ADCS misconfigurations and their impact
- Privilege escalation without credentials

---

