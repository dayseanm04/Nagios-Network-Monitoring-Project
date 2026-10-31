# 09 – Monitor Windows System Event Logs with NCPA

In this doc, I will add a check that watches the Windows **System** event log on **`comp-a-test`** for failure messages. It counts how many System log entries in the last 24 hours mention **"failure," "failed," or "failing,"** and alerts if the count gets too high. I'll build the check in the NCPA web interface, then add it to the host's config file using the default **`generic-service`** template.

> ✅ **Testing and validation for this check:** [**Click here to see the NCPA System event log check validation**](../../06-Testing-and-Validation/08-Validate-NCPA-System-Event-Log-Check.md)

---

## Overview

## Why Monitor the System Log?

Windows keeps several event logs. The **System** log is where Windows records problems with drivers, services, hardware, and startup. A service crashing, a driver failing to load, or a disk error all show up there, often before a user notices anything.

I'm not alerting on every entry, since a normal Windows machine logs a few failures every day. Instead, the check counts failures and alerts when there are **more than usual**.

### Why generic-service?

| Setting | `generic-service` | Why it fits |
|---|---|---|
| Check interval | 10 min | Log entries don't need checking every minute. The check looks back 24 hours each time, so nothing gets missed between checks |
| Max check attempts | 3 | A high count doesn't change from one check to the next, so a few attempts is enough |

---

# Part 1 – Build the Check in NCPA

For more on this process, see [**08-Help / 04 – Find and Test NCPA Checks in the Web UI**](../../08-Help/04-Find-and-Test-NCPA-Checks-in-the-Web-UI.md).

## Step 1 – Log In to NCPA

On **`comp-a-test`**, I go to **`https://localhost:5693`** and log in with the NCPA token.

<img width="969" height="527" alt="1" src="https://github.com/user-attachments/assets/520246ea-b61c-4bff-981d-eba2b20bb85d" />

<img width="961" height="724" alt="2" src="https://github.com/user-attachments/assets/86ef9a9b-3184-4b6a-b1d8-2f1490289655" />












