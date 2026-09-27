# 03 – Assign a Static IP to NetworkVM

## Why a Static IP for a Monitoring Server?

A DHCP address can change after a reboot or lease expiry. For a Nagios server, that causes real problems:
- Devices and agents being monitored are often configured to only accept checks from the Nagios server's IP.
- Firewall rules and SNMP access lists reference the monitoring server by IP.
- You need a static IP address to reach the Nagios web interface.

A fixed IP means everything that points at the monitoring server keeps working.

---

## Prerequisites

- **`NetworkVM`** connected to **`Net-MGNT-SW`** (see [**02 – Configure the Hyper-V Virtual Switch**](02-Configure-Hyper-V-Virtual-Switch.md))
- The LAN's subnet, gateway, and DNS server (check from the host with **`ipconfig`**)
- An unused IP address on that subnet, ideally **outside** the router's DHCP range

---

## Step 1 – Check the Current IP Address

1. Start `NetworkVM` and log in as `netvm`.
2. Open **Terminal** and run: **ip a**
3. Look at the `inet` line under `eth0`.

<img width="927" height="581" alt="40" src="https://github.com/user-attachments/assets/8193eca9-7bb7-45c1-8179-1f3daeaf1047" />
