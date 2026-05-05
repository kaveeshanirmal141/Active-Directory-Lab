# DNS Debugging (IPv6 vs IPv4 Issue)

## 🔹 Introduction

During the initial setup of the Active Directory lab, DNS resolution issues were encountered across multiple machines.  

Despite having basic network connectivity, domain-related services such as:

- `nslookup`
- Domain resolution
- Certificate services

were behaving inconsistently.

This section documents the full debugging process, including failed attempts, root cause analysis, and final resolution.

---

## 🔹 Initial Symptoms

The following issues were observed:

- `nslookup` showed:
  ```
  Default Server: Unknown
  ```
- DNS queries intermittently failed
- Domain-related services behaved unpredictably
- Network icon sometimes showed:
  ```
  No Internet Access
  ```
  even when connectivity existed

---

## 🔹 Key Observation

```text
Ping worked, but DNS did not behave correctly
```

Example:

```cmd
ping dc01.homecorp.local   ✔ Works
nslookup dc01.homecorp.local   ❌ Fails / inconsistent
```

---

## 🔹 Initial Hypothesis

The issue appeared to be related to:

- DNS configuration mismatch  
- IPv6 taking priority over IPv4  
- Improper DNS registration behavior  

---

## 🔹 Attempted Fix (PowerShell)

An attempt was made to disable IPv6 DNS behavior using PowerShell:

```powershell
Set-NetIPInterface -InterfaceAlias "Ethernet0" -AddressFamily IPv6 -RouterDiscovery Disabled
Set-NetIPInterface -InterfaceAlias "Ethernet0" -ManagedAddressConfiguration Disabled
Set-NetIPInterface -InterfaceAlias "Ethernet0" -OtherStatefulConfiguration Disabled

Set-DnsClientServerAddress -InterfaceAlias "Ethernet0" -ResetServerAddresses
```

---

## 🔹 Result of Attempt

```text
❌ Issue NOT fully resolved just by running these commands
```

Problems observed:

- Network instability
- “No Internet” icon persisted
- DNS behavior still inconsistent
- Some services worked, others failed unpredictably

---

## 🔹 Additional Attempt

Tried disabling IPv6 completely:

```powershell
Disable-NetAdapterBinding -Name "Ethernet" -ComponentID ms_tcpip6
```

---

## 🔹 Result

```text
❌ Caused more instability
```

- Broke expected network behavior in VMware
- Did not reliably fix DNS resolution
- Introduced new connectivity inconsistencies

---

## 🔹 Root Cause Identified

The actual issue was:

```text
Improper DNS suffix and DNS registration configuration
```

Not IPv6 itself.

---

## 🔹 Final Fix (Correct Solution)

Instead of disabling IPv6, DNS behavior was corrected through proper configuration. (In every server)

### Steps:

1. Powershell commands:
```powershell
Set-NetIPInterface -InterfaceAlias "Ethernet0" -AddressFamily IPv6 -RouterDiscovery Disabled
Set-NetIPInterface -InterfaceAlias "Ethernet0" -ManagedAddressConfiguration Disabled
Set-NetIPInterface -InterfaceAlias "Ethernet0" -OtherStatefulConfiguration Disabled

Set-DnsClientServerAddress -InterfaceAlias "Ethernet0" -ResetServerAddresses
```

2. Open:
   ```
   Network Adapter Settings → IPv4 Properties
   ```

3. Navigate to:
   ```
   Advanced → DNS Tab
   ```

4. Configure:

   - Add DNS suffix:
     ```
     homecorp.local
     ```

   - Enable:
     ```
     ✔ Register this connection's addresses in DNS
     ✔ Use this connection's DNS suffix in DNS registration
     ```

---

## 🔹 Result After Fix

Running:

```cmd
ipconfig /all
```

Now shows:

```text
DNS Servers: 192.168.8.100
Primary DNS Suffix: homecorp.local
```

---

### Verification:

```cmd
nslookup dc01.homecorp.local
```

Output:

```text
Server: dc01.homecorp.local
Address: 192.168.8.100
```

---

```cmd
ping dc01.homecorp.local
```

```text
✔ Successful resolution
```

---

## 🔹 Key Learning

```text
The problem was NOT connectivity — it was DNS identity and registration
```

---

## 🔹 Critical Insight

```text
IPv6 was not the root problem.
DNS misconfiguration was.
```

Disabling IPv6:

- Masked the issue  
- Did not solve the root cause  

---

## 🔹 Real-World Relevance

In enterprise environments:

- IPv6 is often enabled by default  
- DNS misconfiguration leads to subtle failures  
- Systems may appear “working” while AD services silently break  

---

## 🔹 Takeaways

- Ping success does NOT mean AD is working  
- DNS is the backbone of Active Directory  
- Improper DNS suffix = broken domain behavior  
- Disabling IPv6 is NOT a proper fix  
- Always fix configuration, not symptoms  

---

## 🔹 Final Conclusion

This issue demonstrated how:

```text
Small DNS misconfigurations can break entire Active Directory functionality
```

## 🔹 Additional Note

This issue highlighted that DNS misconfiguration can silently break Active Directory even when basic connectivity appears functional.

Understanding and debugging this behavior provided deeper insight into:

- AD dependencies  
- DNS mechanics  
- Real-world troubleshooting workflows  
