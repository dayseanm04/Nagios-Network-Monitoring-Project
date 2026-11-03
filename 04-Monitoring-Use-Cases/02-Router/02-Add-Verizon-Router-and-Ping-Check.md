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

<img width="889" height="352" alt="1" src="https://github.com/user-attachments/assets/a8587a67-63d3-4ede-9f9b-12063695f05b" />

---

## Step 2 – Update the File Header

I replace the sample header with one that says what this file is for:

<img width="903" height="296" alt="2" src="https://github.com/user-attachments/assets/4813379c-a388-4813-8061-3fd7e1d10d96" />

---

## Step 3 – Define the Router

I replace the sample Linksys switch with my router:

<img width="952" height="660" alt="3" src="https://github.com/user-attachments/assets/ff4c8a07-1cba-485c-a068-c42557278fec" />

| Line | What it does |
|---|---|
| **`use network-device-host`** | Uses my custom template for network devices: checks every minute, HARD after 2 failed checks, and alerts on down, unreachable, and recovery |
| **`host_name verizon-r1`** | Short name Nagios uses for the router. Matches the name on my network diagram |
| **`alias Verizon-R1`** | Name shown in the web interface |
| **`address 192.168.1.1`** | The router's IP, which is also the default gateway for my project network |
| **`hostgroups routers`** | Adds the router to the **`routers`** hostgroup defined in `routers-hostgroup.cfg` |

Save: **Ctrl+O**, **Enter**.

---

## Step 4 – Update the PING Service

The sample file has a PING service for the Linksys switch:

<img width="898" height="724" alt="4" src="https://github.com/user-attachments/assets/09ee38b2-afa1-4a65-ab01-5f42dd5fa245" />

I make three changes:

| Change | Before | After | Why |
|---|---|---|---|
| Host | **`linksys-srw224p`** | **`verizon-r1`** | Points the check at my router |
| Template | **`generic-service`** | **`network-device-service`** | My template for network devices: 1-minute checks and HARD after 2 failed checks |
| `check_interval` and `retry_interval` | **`5`** and **`1`** | Removed | The template already sets them, so these lines would override it |

<img width="1032" height="674" alt="5" src="https://github.com/user-attachments/assets/e52ee753-6043-4ca3-a3c8-95c1fa52e824" />

Save: **Ctrl+O**, **Enter**.

### What the PING thresholds mean

**`check_ping!200.0,20%!600.0,60%`** passes two thresholds, each with a response time and a packet loss limit:

| Threshold | Round-trip time | Packet loss | Meaning |
|---|---|---|---|
| Warning | 200 ms | 20% | WARNING if replies take over 200 ms **or** more than 20% of pings are lost |
| Critical | 600 ms | 60% | CRITICAL if replies take over 600 ms **or** more than 60% of pings are lost |

These are the sample values from **`switch.cfg`**. The router is on my local network, so normal response times should be only a few milliseconds. These thresholds leave plenty of room and will only alert on a real problem.

---

## Step 5 – Comment Out the SNMP and MRTG Checks

<img width="927" height="679" alt="6" src="https://github.com/user-attachments/assets/2eee8ef0-7617-4230-b26a-3490d91072ab" />


The other three services from the sample need SNMP or MRTG:

| Service | Needs |
|---|---|
| Uptime | SNMP |
| Port 1 Link Status | SNMP |
| Port 1 Bandwidth Usage | MRTG |


I'll check whether my router supports SNMP before deciding on these. For now, I comment them out by adding **`#`** to the start of every line in each block, including `define service {` and the closing `}`.

Save and exit: **Ctrl+O**, **Enter**, **Ctrl+X**.

> I'm keeping them in the file instead of deleting them. If my router supports SNMP, I can update and uncomment them.

---

## Step 6 – Run the Pre-Flight Check

```bash
sudo /usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg
```

<img width="1028" height="856" alt="7" src="https://github.com/user-attachments/assets/ca2f4e52-0a35-43f5-9385-c60578e8393c" />

| Count | Before | After | What changed |
|---|---|---|---|











