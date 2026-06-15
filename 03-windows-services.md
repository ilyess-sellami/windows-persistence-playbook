# Windows Services Persistence

## Overview

Windows Services are long-running processes managed by the Service Control Manager (SCM). They are designed to start automatically during system boot or user login and often run with high privileges (commonly SYSTEM).

From a persistence perspective, services are attractive because they:
- Start automatically on boot
- Can run without user interaction
- Often execute with elevated privileges
- Blend in with legitimate system components

---

## How It Works

A service is defined by:
- Service name
- Binary path (ImagePath)
- Startup type (Automatic / Manual / Disabled)
- Execution account (LocalSystem, NetworkService, or user-defined account)

When Windows starts, the Service Control Manager launches all services configured for automatic startup.

---

## Persistence Flow

```text
System Boot
     │
     ▼
Service Control Manager (SCM)
     │
     ▼
Automatic Services Loaded
     │
     ▼
Target Service Executes Binary
     │
     ▼
Persistent Execution Achieved
```

---

## Key Service Properties

* **ImagePath** → Path to executable
* **Start Type** → Auto-start or manual
* **Service Type** → Win32 service
* **Account** → SYSTEM or custom user

---

## Example 

```cmd
sc create "Malicious" binPath= "C:\Temp\malicious.exe" start= auto
```

---

## Related MITRE ATT&CK

**T1543.003 – Create or Modify System Process: Windows Service**

---

## Summary

Windows Services provide a powerful and stealthy persistence mechanism due to their automatic startup behavior and high privilege execution context. They are frequently abused in both malware campaigns and red team operations.