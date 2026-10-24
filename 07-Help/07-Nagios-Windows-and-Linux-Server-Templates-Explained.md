# Windows and Linux Server Templates (`windows-server`, `linux-server`)

This guide explains the two OS-specific host templates that come with Nagios Core, what their options do, and how they differ.

**File location:** `/usr/local/nagios/etc/objects/templates.cfg`

---

## How they fit in

<img width="932" height="466" alt="1" src="https://github.com/user-attachments/assets/96ba8e3e-b71e-4b7f-95e4-f84ffaebf3c8" />

<img width="924" height="496" alt="2" src="https://github.com/user-attachments/assets/bdc56670-379b-42e0-8805-a44e4fe3e8c1" />

Both templates are built on top of **`generic-host`** with **`use generic-host`**. They inherit everything from it and add settings for *how* a host is checked and *who* gets alerted.

```
Windows host  →  windows-server  →  generic-host
Linux host    →  linux-server    →  generic-host
```

For the options that come from **`generic-host`** (notifications on, flap detection, retention, etc.), see [**05-Nagios-Generic-Host-Template-Explained.md**](05-Nagios-Generic-Host-Template-Explained.md).

---

## The templates

**windows-server**

<img width="940" height="442" alt="windows-host-template" src="https://github.com/user-attachments/assets/2b2ee248-36b2-43c8-a1c5-7e35d3f74b1d" />

**linux-server**

<img width="932" height="450" alt="linux-host-template" src="https://github.com/user-attachments/assets/e9af27a9-ed28-4d53-86c7-6cab904566e0" />

---

## Side-by-side comparison

| Option | windows-server | linux-server |
|---|---|---|
| **`check_period`** | **`24x7`** | **`24x7`** |
| **`check_interval`** | **`5`** | **`5`** |
| **`retry_interval`** | **`1`** | **`1`** |
| **`max_check_attempts`** | **`10`** | **`10`** |
| **`notification_period`** | **`24x7`** | **`workhours`** |
| **`notification_interval`** | **`30`** | **`120`** |
| **`notification_options`** | **`d,r`** | **`d,u,r`** |
| **`contact_groups`** | **`admins`** | **`admins`** |
| **`hostgroups`** | **`windows-servers`** | **(not set)** |

They check hosts the same way. The difference is in **alerting**: **`windows-server`** alerts at any time and repeats more often, while **`linux-server`** only alerts during work hours and repeats less often.

---

## Options explained

### How the host is checked

| Option | What it does |
|---|---|
| **`use`** | Which template to inherit from. Here, **`generic-host`**. |
| **`check_command`** | How Nagios decides if the host is UP or DOWN. **`check-host-alive`** pings the host. |
| **`check_period`** | When Nagios is allowed to check the host. **`24x7`** = all the time. |
| **`check_interval`** | Minutes between normal checks. **`5`** = every 5 minutes. |
| **`retry_interval`** | Minutes between rechecks after the host stops responding. **`1`** = every minute. |
| **`max_check_attempts`** | How many times Nagios checks before treating the host as really DOWN and sending an alert. Setting it to **`1`** alerts right away. |

**How the check timing works for these templates**
1. The host is checked every 5 minutes.
2. A check fails. The host goes into a **"soft"** DOWN state.
3. Nagios rechecks every 1 minute.
4. After 10 total failed attempts, the host goes into a **"hard"** DOWN state and an alert is sent.

So a host has to be unreachable for roughly 10–15 minutes before anyone gets alerted. Lower **`max_check_attempts`** if you need to know faster.

> **Ping and Windows:** **`check-host-alive`** uses ping. Windows Firewall blocks ping by default, so a Windows host can show DOWN even though it's running fine. 

### Who gets alerted and when

| Option | What it does |
|---|---|
| **`notification_period`** | When alerts can be sent. **`24x7`** = any time. **`workhours`** = Monday to Friday, 9:00 to 17:00 by default. Both are defined in **`timeperiods.cfg`**. |
| **`notification_interval`** | Minutes between repeat alerts while the host is still down. **`30`** = every 30 minutes, **`120`** = every 2 hours. **`0`** = alert only once. |










