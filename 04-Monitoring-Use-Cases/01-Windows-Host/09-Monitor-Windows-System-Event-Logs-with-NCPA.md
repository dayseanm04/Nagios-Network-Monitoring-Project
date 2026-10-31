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


## Step 2 – Set Up the Log Filter

I click **API**, then under **API Endpoint** select **logs**. I fill in:

| Setting | Value | What it does |
|---|---|---|
| Log Names – Name | **`System`** | Only reads the Windows System log |
| Log Timeframe – Logged After | **`24` Hours** | Only counts entries from the last 24 hours |
| Message | **`failure\|failed\|failing`** | Only counts entries whose message contains one of these words |

> **What does `|` mean here?** The Message field is a pattern, and `|` means **OR**. So `failure|failed|failing` matches any entry with "failure" **or** "failed" **or** "failing" in its message.

I leave EventType, EventID, SourceName, ComputerName, and EventCategory blank so the filter only looks at the message text.

## Step 3 – Set Thresholds

I check **Run as a Nagios check** and set:

| Threshold | Value | Meaning |
|---|---|---|
| Warning | 5 | More than 5 matching entries in the last 24 hours |
| Critical | 10 | More than 10 matching entries in the last 24 hours |

Unlike CPU or disk, these numbers are a **count of log entries**, not a percentage.

I click **Reload**.

<img width="963" height="749" alt="3" src="https://github.com/user-attachments/assets/af7b81c3-381c-4029-9ae5-dabed389dba9" />

There are 3 matching entries, which is under the warning threshold of 5.

## Step 4 – Get the Command

I click **View in alternate format** → **As active check using check_ncpa.py**.

<img width="635" height="123" alt="4" src="https://github.com/user-attachments/assets/1ce88717-79b5-4362-91c1-e1f64ede6aa6" />

<img width="966" height="710" alt="5" src="https://github.com/user-attachments/assets/e36cbd3c-c120-41d6-9c22-11a70436da79" />

The part I need for Nagios:

```
-M 'logs' -w 5 -c 10 -q 'name=System,message=failure|failed|failing'
```

| Part | What it does |
|---|---|
| **`-M 'logs'`** | Asks NCPA to search the Windows event logs |
| **`-w 5 -c 10`** | WARNING above 5 matches, CRITICAL above 10 |
| **`-q 'name=System,...'`** | Only the System log |
| **`message=failure\|failed\|failing`** | Only entries mentioning failure, failed, or failing |

> The single quotes around the query matter. In Linux, **`|`** is a pipe, so without quotes the shell would split the command.

---

# Part 2 – Add the Service in Nagios

## Step 1 – Open the Host File

```bash
cd /usr/local/nagios/etc/windows-hosts
ls -l
sudo nano comp-a-test.cfg
```

