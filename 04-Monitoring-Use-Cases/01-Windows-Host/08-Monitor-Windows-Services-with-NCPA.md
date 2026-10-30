# 08 – Monitor Windows Services with NCPA

In this doc, I will add a check that confirms a Windows service is running on **`comp-a-test`**. The first service I'm monitoring is **NCPA** itself, the agent every other check on this host depends on. I'll build the check in the NCPA web interface, then add it to the host's config file using my **`notify-early`** template.

> **Testing and validation for this check:** [**Click here to see the NCPA Windows service check validation**](../../06-Testing-and-Validation/07-Validate-NCPA-Windows-Service-Checks.md)

---

## Overview

## Why Monitor NCPA?

NCPA is the agent that answers every NCPA check on **`comp-a-test`**. If it stops, Nagios loses CPU, memory, disk, and uptime monitoring for this host all at once. A check named **`NCPA Service Status`** makes that failure easy to spot, instead of leaving me to figure out why five checks broke at the same time.

### Why notify-early?

This check uses **`notify-early`** instead of **`time-sensitive`**:

| Setting | `time-sensitive` | `notify-early` |
|---|---|---|
| Check interval | 2 min | **1 min** |
| Max check attempts | 5 | **1** |
| Notification interval | 15 min | **10 min** |

A stopped service doesn't fix itself, so there's no reason to wait for 5 attempts the way I do for a CPU spike. With 1 attempt, the first failed check goes straight to HARD and alerts.

### What happens if NCPA stops
