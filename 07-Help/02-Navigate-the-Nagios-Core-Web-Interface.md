# 02 – Navigate the Nagios Core Web Interface

A guide to the Nagios Core web interface: what each menu item does, what to look for on each page, and when to use it. Use this as a reference when checking the health of monitored devices or investigating a problem.

**Version note:** Most screenshots in this guide were taken on **Nagios Core 4.5.0**. The Event Log and Scheduling Queue screenshots were taken after upgrading to **4.5.14** (see **`02-Setup/`**). Version 4.5.14 has an updated look with a dark sidebar, and the **(Legacy)** versions of Map, Trends, and Histogram are no longer in the menu. The pages themselves work the same way.

## Accessing the Web Interface

go to **http: //IPAddress/nagios**, Enter the username: **nagiosadmin** and password. 

## General

### Home

The landing page after you sign in.

**nagios core version 4.5.0:**

<img width="1032" height="850" alt="1" src="https://github.com/user-attachments/assets/f78bc116-a41e-4438-ada6-907052619d7d" />

**nagios core latest version 4.5.14:**

<img width="988" height="685" alt="2" src="https://github.com/user-attachments/assets/9f6c9293-420f-4cc8-96b8-23e2de6f34d8" />

| What to look for | Why it matters |
|---|---|
| **Daemon running with PID** | Confirms the Nagios service is running. If this is missing, Nagios is stopped |
| Version number | Shows which Nagios Core version is installed |
| **Check for updates** / new version banner | Tells you when an upgrade is available |

### Documentation

Links to the official Nagios Core documentation.

### Tactical Overview

A single-page summary showing total hosts and services by state, network outages, and which monitoring features are enabled. Start here for a quick overall health check.

<img width="1029" height="876" alt="3" src="https://github.com/user-attachments/assets/d120d99d-09f2-4587-9abb-b636e8e60db7" />

### Map (version 4.5.0)

Shows each host as a node connected to the **Nagios Process** in the center. Green nodes are UP, red nodes are DOWN.

<img width="1013" height="632" alt="4" src="https://github.com/user-attachments/assets/9024a969-e263-47be-887a-35e58d2f13da" />

Hover over a node to see its details:

<img width="935" height="485" alt="5" src="https://github.com/user-attachments/assets/9ae4a88d-3ff5-402a-b88b-a45b5ad4030e" />

### Map (version 4.5.14)

<img width="1035" height="489" alt="6" src="https://github.com/user-attachments/assets/a09d4da9-737b-414c-b3c4-d42bcc0503c1" />

note: you have to change theses settings to view the map

<img width="1028" height="591" alt="7" src="https://github.com/user-attachments/assets/15de3a0d-ff17-4060-be09-a690c170c380" />

### Host Groups – Summary

Groups hosts by type and shows a count of host and service states for each group.

<img width="1030" height="567" alt="8" src="https://github.com/user-attachments/assets/ec551095-aa5f-49be-b187-7695fa407674" />

<img width="1023" height="511" alt="9" src="https://github.com/user-attachments/assets/37e1652c-9580-4954-bd8a-e126f6b88c29" />

