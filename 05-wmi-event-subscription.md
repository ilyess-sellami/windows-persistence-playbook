# WMI Event Subscription Persistence

## Overview

Windows Management Instrumentation (WMI) Event Subscriptions allow automation based on system events.

WMI can monitor events such as:
- System startup
- User logon
- Process creation
- Time-based triggers

From a persistence perspective, WMI subscriptions are powerful because they are:
- Stored in the WMI repository (not obvious filesystem locations)
- Event-driven (no visible scheduled task required)
- Long-lasting across reboots
- Often overlooked during basic system reviews

---

## How It Works

A WMI permanent event subscription is composed of three main components:

1. **Event Filter**
   - Defines what event to monitor

2. **Event Consumer**
   - Defines what action to execute

3. **Filter-to-Consumer Binding**
   - Links filter and consumer together

---

## Persistence Flow

```text
System Event Occurs
        │
        ▼
   WMI Event Filter
        │
        ▼
Filter-to-Consumer Binding
        │
        ▼
   Event Consumer Triggered
        │
        ▼
   Payload Execution
```

---

## WMI Core Components

### Event Filter

Defines conditions such as:

* System startup
* User logon
* Time interval
* Process creation events
* Event Consumer

### Defines actions such as:

* Running an executable
* Launching a script
* Executing a command

### Binding

Connects the filter to the consumer to activate execution.

---

## Storage Location

WMI subscriptions are stored in the WMI repository:

```text
root\subscription
```

Key classes involved:

```text
__EventFilter
__EventConsumer
__FilterToConsumerBinding
```

---

## Example

```powershell

# 1. Create the Event Filter (Triggers on System Boot)
$FilterArgs = @{
    Name = 'StartupAppFilter'
    QueryLanguage = 'WQL'
    Query = "SELECT * FROM __InstanceModificationEvent WITHIN 60 WHERE TargetInstance ISA 'Win32_PerfFormattedData_PerfOS_System' AND TargetInstance.SystemUpTime = 5"
    EventNamespace = 'root\cimv2'
}
$Filter = Set-CimInstance -Namespace root\subscription -ClassName __EventFilter -Property $FilterArgs

# 2. Create the Event Consumer (The EXE to run)
$ConsumerArgs = @{
    Name = 'StartupAppConsumer'
    CommandLineTemplate = 'C:\Path\To\Your\Application.exe'
}
$Consumer = Set-CimInstance -Namespace root\subscription -ClassName CommandLineEventConsumer -Property $ConsumerArgs

# 3. Bind the Filter to the Consumer
$BindingArgs = @{
    Filter = $Filter
    Consumer = $Consumer
}
Set-CimInstance -Namespace root\subscription -ClassName __FilterToConsumerBinding -Property $BindingArgs
```

---

## Related MITRE ATT&CK

**T1546.003 – Event Triggered Execution: WMI Event Subscription**

---

## Summary

WMI Event Subscription persistence is a stealthy, event-driven mechanism that allows execution of payloads based on system triggers. Because it resides in the WMI repository rather than common startup locations, it is frequently used in advanced persistence scenarios.