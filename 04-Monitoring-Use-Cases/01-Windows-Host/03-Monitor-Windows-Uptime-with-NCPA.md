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

