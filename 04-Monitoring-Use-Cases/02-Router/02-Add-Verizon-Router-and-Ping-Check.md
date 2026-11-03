# 02 – Add Verizon Router and Ping Check

In this doc, I will replace the sample switch config in **`verizon-r1.cfg`** with my Verizon router's host definition and a PING service check. The SNMP and MRTG checks from the sample are commented out until I confirm whether my router supports SNMP.

The **`routers`** folder, hostgroup, and **`verizon-r1.cfg`** file were set up in [**01 – Set Up Routers Folder and Hostgroup**](01-Set-Up-Routers-Folder-and-Hostgroup.md).

---

## Overview

The router is the device between my lab network and the internet, as shown on my [**network diagram**](../../README.md). If it goes down, everything on the network loses internet access.

## Step 1 – Open verizon-r1.cfg

```bash
cd /usr/local/nagios/etc/routers
ls -l
sudo nano verizon-r1.cfg
```
