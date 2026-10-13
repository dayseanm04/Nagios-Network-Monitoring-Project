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

# Create a service for monitoring system uptime

define service {
    use                     generic-service
    host_name               comp-a-test
    service_description     system uptime
    check_command           check_ncpa!-M system/uptime
}
```

<img width="995" height="542" alt="2" src="https://github.com/user-attachments/assets/e4f20bf2-dd40-44fd-8ff2-fca30942140d" />

I save and exit: **Ctrl+O**, **Enter**, **Ctrl+X**.

### What each line does

| Line | What it does |
|---|---|
| **`use generic-service`** | Inherits default check interval, retries, and notification settings |
| **`host_name comp-a-test`** | Attaches this check to the Windows host |
| **`service_description system uptime`** | The name shown in the Nagios **Services** page |
| **`check_command check_ncpa!-M system/uptime`** | Runs the **`check_ncpa`** command and passes **`-M system/uptime`** as **`$ARG1$`** |

When this check runs, Nagios turns it into:

```
/usr/local/nagios/libexec/check_ncpa.py -H 192.168.1.9 -t '<token>' -P 5693 -M system/uptime
```

The host IP comes from **`$HOSTADDRESS$`** and the token from **`$USER5$`** in **`resource.cfg`**, so neither is typed into **`windows.cfg`**.

> **No thresholds yet.** This check has no **`-w`** or **`-c`**, so it always returns **OK** and just reports the uptime. 

---

## Step 3 – Run the Pre-Flight Check

```bash
sudo /usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg
```

<img width="1079" height="613" alt="3" src="https://github.com/user-attachments/assets/885bb60e-3de6-4ab7-a422-80eb2351c123" />

Expected result:

```
Checked 9 services.
Checked 2 hosts.

Total Warnings: 0
Total Errors:   0
```

The service count went from **8 to 9**, which confirms Nagios picked up the new **`system uptime`** service.

---

## Step 4 – Reload Nagios

```bash
sudo systemctl reload nagios
```

<img width="902" height="575" alt="14" src="https://github.com/user-attachments/assets/585bbb88-8e24-4c72-837c-8777c10d6809" />

---

## Next Steps

**Add thresholds so a reboot triggers an alert.** For uptime, a **low** value is the problem. 



