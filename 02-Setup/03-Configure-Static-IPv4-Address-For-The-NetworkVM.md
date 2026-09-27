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

<img width="927" height="581" alt="1" src="https://github.com/user-attachments/assets/8193eca9-7bb7-45c1-8179-1f3daeaf1047" />

The VM received `169.254.81.254/16`. Any address in the `169.254.0.0/16` range is an **APIPA** (link-local) address. The system assigns one to itself when it asks for a DHCP address and gets no answer. With an APIPA address the VM can't reach the gateway or the internet.

---

## Step 2 – Confirm the Address Is Free

Before assigning `192.168.1.99`, make sure nothing else on the network is using it. From the **host**, open Command Prompt:

```cmd
ping 192.168.1.99
```

If every reply says **Request timed out** or **Destination host unreachable**, the address is likely free.

<img width="878" height="344" alt="2" src="https://github.com/user-attachments/assets/836caecb-d8de-4320-906f-b76c66675516" />

> For a permanent setup, log in to the router and either pick an address outside the DHCP pool or create a DHCP reservation for the VM's **MAC address**. This prevents the router from ever handing **`.99`** to another device.

---

## Step 3 – Set the Static IP in Network Settings

1. Open **Settings** → **Network**.
