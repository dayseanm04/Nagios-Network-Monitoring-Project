# 03 – Monitor Windows Uptime with NCPA

In this doc, I will create my first NCPA service check in Nagios. It monitors how long **`comp-a-test`** has been running since its last reboot, using the **`system/uptime`** metric from the NCPA agent.

> **Testing and validation for this check:** [**Click here to see the NCPA uptime check validation**](../../06-Testing-and-Validation/02-Validate-NCPA-Uptime-Check.md)

---

## Prerequisites
- NCPA installed and running on **`comp-a-test`** (see [**02 – Install NCPA on Windows Host**](02-Install-NCPA-on-Windows-Host.md))
- **`check_ncpa.py`** plugin and **`check_ncpa`** command set up on the Nagios server (see [**02-Setup / 07 – Set Up NCPA Check Command in Nagios**](../../02-Setup/07-Set-Up-NCPA-Check-Command-in-Nagios.md))
- The metric tested in the NCPA web interface first (see [**07-Help / 04 – Find and Test NCPA Checks in the Web UI**](../../07-Help/04-Find-and-Test-NCPA-Checks-in-the-Web-UI.md))

---

## Why Monitor Uptime?

Uptime tells me how long a machine has been running since it last restarted. I'm starting with uptime because it's a simple, low-risk metric for confirming that Nagios, the **`check_ncpa`** command, and the NCPA agent all work together.

---

## Step 1 – Open the Windows Config File

Ensure I was in my home directory, then I change directory the Nagios objects folder:

```bash
cd
cd /usr/local/nagios/etc/objects
ls -l
```

<img width="936" height="411" alt="1" src="https://github.com/user-attachments/assets/d85da69f-8fc3-461f-8aee-92bd3146ded1" />

Then I open `windows.cfg`:

```bash
sudo nano windows.cfg
```

---

## Step 2 – Create the Service

I scroll down to **SERVICE DEFINITIONS** and add a labeled section so I can group future NCPA system checks together:

```
#----------------------------------
# SECTION: system
# Purpose: monitor core OS metrics.
#----------------------------------

```

