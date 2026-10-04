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

### Host Groups – Grid

Same groups, but lists every service as a colored box so you can spot problems at a glance.

<img width="1036" height="613" alt="10" src="https://github.com/user-attachments/assets/5493376b-71f0-4852-807b-edf6e0d68ac6" />

The icons in the **Actions** column open the host's details, service status, and map location.

### Hosts (version 4.5.0 and 4.5.14 are the same)

Lists every host with its status, last check, how long it's been in that state, and the check output.

<img width="982" height="536" alt="11" src="https://github.com/user-attachments/assets/cc7fa797-89bb-4679-99ed-e199c05e6f07" />

### Host Details

Click any host name to open its details page.

<img width="1029" height="841" alt="12" src="https://github.com/user-attachments/assets/8e052aca-fcb8-4567-9a3d-10d3e1cd04e4" />

#### View Status Detail for This Host

<img width="1029" height="670" alt="13" src="https://github.com/user-attachments/assets/d584d993-fa2b-4e10-a272-fb7c140d34f9" />

#### View Notifications for This Host

Shows every alert sent for that host, with the time, contact, notification command, and message.

<img width="1030" height="583" alt="14" src="https://github.com/user-attachments/assets/e77131ca-a3ef-46c2-87f5-45d991b415a0" />

Filter notifications:

<img width="1034" height="702" alt="15" src="https://github.com/user-attachments/assets/35dfd83c-4758-410e-9093-a223c8f8fa6f" />

### Quick Search

Type a host name in the **Quick Search** box in the sidebar to jump straight to it.

<img width="1029" height="747" alt="16" src="https://github.com/user-attachments/assets/fd63525b-2214-424d-bf02-baec67f5028a" />

---

## Reports

### Availability

Calculates the percentage of time a host or service was UP/OK over a chosen period. Useful for uptime reporting.

<img width="1026" height="408" alt="17" src="https://github.com/user-attachments/assets/056802e3-abfd-4f33-9cd1-907ee61dd58d" />

Click **Continue to Step 2**

<img width="1026" height="392" alt="18" src="https://github.com/user-attachments/assets/99ccc9fd-9fc6-4c33-8307-7cefe2494bf5" />

Click **Continue to Step 3**

<img width="1026" height="723" alt="19" src="https://github.com/user-attachments/assets/3f6ab4b1-0b43-4cfa-a8f7-1209c14d5bab" />

Select Report options
Click **Create Availability Report!**

<img width="1029" height="612" alt="20" src="https://github.com/user-attachments/assets/d3176878-5619-43e1-a506-5f34ff648cf2" />

### Trends

Draws a timeline of a host's or service's state over a period.

1. Select type (**Host or Service**)
2. Select the actual hostname or service name
3. Choose report options
4. Create the report

<img width="1029" height="682" alt="21" src="https://github.com/user-attachments/assets/e18724b2-98ea-46bc-8178-d9854907f7eb" />

### Alerts – History

A running list of every state change and Nagios start/stop event, newest first.

<img width="1038" height="752" alt="22" src="https://github.com/user-attachments/assets/e7aa0ac0-66df-44ab-8751-5138999c5f12" />

### Alerts – Summary

Select your options

<img width="1014" height="752" alt="23" src="https://github.com/user-attachments/assets/21143753-d365-48d0-b3a8-69c74d0c9262" />

Then create summary report

<img width="1019" height="428" alt="24" src="https://github.com/user-attachments/assets/e90d9ca1-fa0c-4ba0-8e65-3de290e19e0c" />

### Alerts – Histogram

Shows how many alerts happened on each day of the month (or hour of the day), to spot patterns.

1. Select type (**Host or Service**)
2. Select the actual hostname or service name
3. Choose report options
4. Create the report

<img width="1023" height="713" alt="25" src="https://github.com/user-attachments/assets/1375ba71-7762-41e6-a2e4-17edb10b0ea1" />

### Notifications

Every notification sent to every contact.

<img width="995" height="639" alt="26" src="https://github.com/user-attachments/assets/b99f9f1b-9d9c-4278-8a66-94177993830c" />

