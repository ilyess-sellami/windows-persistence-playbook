# Scheduled Tasks Persistence

## Overview

Scheduled Tasks are a built-in Windows feature used to automate execution of programs based on triggers such as time, system events, or user activity.

From an attacker perspective, Scheduled Tasks are a reliable persistence mechanism because they are native to Windows and can execute payloads repeatedly or under specific conditions.

---

## How It Works

Windows Task Scheduler runs tasks defined by the system. Each task consists of:

- **Trigger** → Defines when the task executes (logon, startup, time-based)
- **Action** → Defines what runs (program, script, command)
- **Context** → User or SYSTEM privileges

When triggered, Windows automatically executes the configured action.

---

## Persistence Flow

```text
Trigger Event (Time / Logon / Startup)
            │
            ▼
   Task Scheduler Service
            │
            ▼
     Task Conditions Met
            │
            ▼
   Action Executed (Payload)
            │
            ▼
     Persistence Maintained
```

---

## Example

```cmd
schtasks /create /sc minute /mo 5 /tn "Malicious" /tr "C:\Temp\malicious.exe" /f
```

---

## Task Structure (Conceptual)

```text
Task Name: <Name>
Trigger:   <Logon | Startup | Time Interval>
Action:    <Executable or Script Path>
User:      <User or SYSTEM
```

---

## Related MITRE ATT&CK

**T1053.005 – Scheduled Task/Job: Scheduled Task**

---

## Summary

Scheduled Tasks are one of the most commonly abused persistence mechanisms in Windows environments due to their flexibility, legitimacy, and ability to execute payloads under different conditions and privilege levels.
