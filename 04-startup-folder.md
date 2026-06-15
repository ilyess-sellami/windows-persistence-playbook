# Startup Folder Persistence

## Overview

The Windows Startup Folder is a built-in mechanism that automatically executes programs when a user logs in.

From a persistence perspective, it is one of the simplest techniques because any file placed inside the Startup directory will be executed during user session initialization.

It is commonly used in both legitimate software auto-launch and malicious persistence scenarios.

---

## How It Works

When a user logs into Windows, the system processes the Startup folders and executes all shortcuts and executables found inside.

There are two primary Startup locations:

- Per-user Startup folder
- All users Startup folder

---

## Startup Folder Locations

### Per-user Startup Folder

```text
C:\Users\<User>\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup
```

### All Users Startup Folder

```text
C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp
```

---

## Persistence Flow

```text
User Logon
    │
    ▼
Windows Explorer Starts
    │
    ▼
Startup Folder Parsed
    │
    ▼
Executables/Shortcuts Launched
    │
    ▼
Payload Executes
```

---

## Example 

```cmd
copy "C:\Temp\malicious.exe" "%APPDATA%\Microsoft\Windows\Start Menu\Programs\Startup\"
```

---

## Related MITRE ATT&CK

**T1547.001 – Boot or Logon Autostart Execution: Startup Folder**

---

## Summary

The Startup Folder is a simple but effective persistence mechanism in Windows. While easy to detect, it remains commonly used due to its simplicity and ability to execute payloads automatically during user login.