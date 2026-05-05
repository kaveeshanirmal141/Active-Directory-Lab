# Network Testing & Validation

## 🔹 Introduction

After configuring the network and resolving DNS-related issues, a series of tests were performed to validate that all machines in the Active Directory lab were correctly communicating and that domain services were functioning as expected.

This section documents the commands used, their outputs, and what each test confirms.

---

## 🔹 Test Objectives

The purpose of testing was to confirm:

- Network connectivity between machines  
- DNS resolution is working correctly  
- Domain name resolution is functional  
- Active Directory services are reachable  

---

## 🔹 Test 1 — Basic Connectivity (Ping)

### Command

```cmd
ping dc01.homecorp.local
```

### Result

```text
Reply from 192.168.8.100: bytes=32 time<1ms TTL=128
```

### Validation

✔ Confirms network connectivity  
✔ Confirms hostname resolution is working  

---

## 🔹 Test 2 — Direct IP Connectivity

### Command

```cmd
ping 192.168.8.100
```

### Result

```text
Reply from 192.168.8.100: bytes=32 time<1ms TTL=128
```

### Validation

✔ Confirms Layer 3 connectivity  
✔ Ensures network path is working independently of DNS  

---

## 🔹 Test 3 — DNS Resolution (nslookup)

### Command

```cmd
nslookup dc01.homecorp.local
```

### Result

```text
Server:  dc01.homecorp.local
Address: 192.168.8.100

Name:    dc01.homecorp.local
Address: 192.168.8.100
```

### Validation

✔ DNS server is correctly configured  
✔ Domain name resolution is functional  
✔ Queries are handled by DC01  

---

## 🔹 Test 4 — Reverse DNS Lookup

### Command

```cmd
nslookup 192.168.8.100
```

### Result

```text
Name: dc01.homecorp.local
```

### Validation

✔ Reverse lookup zones working (if configured)  
✔ Confirms DNS integrity  

---

## 🔹 Test 5 — Domain Awareness

### Command

```cmd
whoami /fqdn
```

### Result

```text
arthur@homecorp.local
```

### Validation

✔ Machine is correctly joined to domain  
✔ User identity resolved via Active Directory  

---

## 🔹 Test 6 — IP Configuration Verification

### Command

```cmd
ipconfig /all
```

### Expected Key Output

```text
DNS Servers . . . . . . . . . . : 192.168.8.100
Primary DNS Suffix . . . . . . : homecorp.local
```

### Validation

✔ Correct DNS server configured  
✔ Domain suffix applied properly  

---

## 🔹 Test 7 — Inter-Machine Communication

### Command

```cmd
ping ca01.homecorp.local
```

### Result

```text
Reply from 192.168.8.101: bytes=32 time<1ms TTL=128
```

### Validation

✔ Machines can communicate using domain names  
✔ DNS resolution works across hosts  

---

## 🔹 Test 8 — Shared Resource Access (Post-Exploitation)

### Command

```cmd
dir \\dc01\c$
```

### Result

```text
Directory listing successful
```

### Validation

✔ Administrative access confirmed  
✔ Kerberos authentication working  
✔ Domain privileges successfully applied  

---

## 🔹 Key Observations

```text
Network connectivity alone does not guarantee Active Directory functionality.
```

Even when ping tests succeeded earlier, DNS misconfiguration caused:

- Domain resolution failures  
- Service inconsistencies  
- Authentication issues  

---

## 🔹 Conclusion

All tests confirm that:

✔ Network connectivity is stable  
✔ DNS is correctly configured  
✔ Domain services are operational  
✔ Machines communicate reliably  

This validates that the environment is correctly configured for further testing and exploitation scenarios.
