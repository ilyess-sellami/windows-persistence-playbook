# Registry Run Keys Persistence

## Overview

Registry Run Keys are one of the most common Windows persistence mechanisms. They allow programs to execute automatically when a user logs on or when the system starts.

Windows checks specific registry locations during the logon process and launches any configured executables. This makes Run Keys a simple and reliable method for maintaining access across system reboots and user sessions.

---

## How It Works

When a user logs into Windows, the operating system processes several autorun registry locations. Any executable, script, or command configured within these keys is automatically executed.

Persistence is achieved by creating or modifying registry values that point to a payload.

---

## Common Registry Locations

### Current User (HKCU)

Executed when the specific user logs in:

```text
HKCU\Software\Microsoft\Windows\CurrentVersion\Run

HKCU\Software\Microsoft\Windows\CurrentVersion\RunOnce
```

### Local Machine (HKLM)

Executed for all users who log into the system:

```text
HKLM\Software\Microsoft\Windows\CurrentVersion\Run

HKLM\Software\Microsoft\Windows\CurrentVersion\RunOnce
```

### 32-bit Applications on 64-bit Systems

```text
HKLM\Software\WOW6432Node\Microsoft\Windows\CurrentVersion\Run
```

---

## Run vs RunOnce

### Run

Programs configured under Run execute every time the user logs in.

```text
HKCU\Software\Microsoft\Windows\CurrentVersion\Run
```

### RunOnce

Programs configured under RunOnce execute only once and are automatically removed after successful execution.

```text
HKCU\Software\Microsoft\Windows\CurrentVersion\RunOnce
```

---

## Persistence Flow

```text
System Boot
     │
     ▼
User Logon
     │
     ▼
Windows Processes Run Keys
     │
     ▼
Configured Command Executes
     │
     ▼
Payload Runs
```

---

## Example

```text
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Run" /v "Malicious" /t REG_SZ /d "\"C:\Temp\malicious.exe\"" /f
```

---

## Why It Is Popular

* Native Windows functionality
* Easy to configure
* Survives reboots
* No additional services required
* Works with executables, scripts, and command-line arguments
* Frequently encountered during incident response investigations

---

## Key Considerations

* HKCU keys provide user-level persistence.
* HKLM keys provide system-wide persistence and typically require administrative privileges to modify.
* RunOnce keys are intended for one-time execution.
* Multiple entries can exist within the same Run key.

---

## Related MITRE ATT&CK Technique

**T1547.001 – Boot or Logon Autostart Execution: Registry Run Keys / Startup Folder**

---

## Summary

Registry Run Keys are among the simplest and most widely used persistence mechanisms in Windows. By leveraging built-in autorun registry locations, programs can be automatically executed during user logon, providing reliable persistence across system reboots and user sessions.
