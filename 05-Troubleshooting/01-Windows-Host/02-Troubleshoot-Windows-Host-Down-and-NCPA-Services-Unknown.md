# 02 – Troubleshoot Windows Host Down and NCPA Services Unknown

Troubleshooting record for the Windows host **`comp-a-test`** going **DOWN** in Nagios, with all 11 of its NCPA services showing **UNKNOWN**. Covers how the problem was found, the steps taken to isolate it, the root cause, and the fix.

> This is the second time **`comp-a-test`** went DOWN because its IP address changed. The first time is covered in [**01 – Troubleshoot Windows Host Down**](01-Troubleshoot-Windows-Host-Down.md).

---

## Problem Description

When I logged in to Nagios, **`comp-a-test`** was **DOWN** and had been for almost 8 hours. Every service on it was **UNKNOWN**. The other two hosts, **`localhost`** and **`verizon-r1`**, were **UP**.

<img width="864" height="507" alt="1" src="https://github.com/user-attachments/assets/e9be25a7-bbc0-4686-b53b-9a0f350d71b9" />

<img width="934" height="573" alt="2" src="https://github.com/user-attachments/assets/d9cb0587-448a-4c4e-b2fd-1694cceabcbc" />

<img width="1026" height="577" alt="3" src="https://github.com/user-attachments/assets/25c570ff-cc2a-47d5-9855-a46c1d374671" />

Host Status Totals: **2 Up, 1 Down**. Service Status Totals: **8 OK, 11 Unknown**.

---

## Symptoms

### Host details

<img width="1022" height="728" alt="4" src="https://github.com/user-attachments/assets/50cd4f0a-26b9-40cb-a670-afb34c1a659d" />

### Services

<img width="1029" height="788" alt="5" src="https://github.com/user-attachments/assets/9cf5ba4c-d3f7-4737-9b0f-0c19fcf275ed" />

All 11 services on **`comp-a-test`** showed the same error:

```
UNKNOWN: An error occurred connecting to API. (Connection error: '[Errno 113] No route to host')
```

<img width="1048" height="735" alt="6" src="https://github.com/user-attachments/assets/46cebdac-ad7a-409f-a1aa-8c26a61f449a" />

**Why UNKNOWN and not CRITICAL?** UNKNOWN means the check couldn't run at all, so Nagios has no idea what the real value is. **`check_ncpa.py`** couldn't reach NCPA, so it couldn't get a CPU, memory, or disk reading to compare against the thresholds.

**`Errno 113 – No route to host`** is a network-level error. It means the Nagios server couldn't find any device at **`192.168.1.9`**. That pointed at the network, not at NCPA itself.

---

## Impact

- **`comp-a-test`** had **no monitoring for about 8 hours**: no CPU, memory, disk, log, or interface checks.
- If a real problem had happened on **`comp-a-test`** during that time, Nagios wouldn't have caught it.
- Nagios sent 3 host DOWN notifications (not delivered, since email isn't set up yet).

---

## Troubleshooting Steps Taken

### Step 1 – Ping comp-a-test from the Nagios server

<img width="959" height="372" alt="7" src="https://github.com/user-attachments/assets/6e0758d1-322b-46e1-b30c-080f52b15a22" />

**Finding:** The "Destination Host Unreachable" replies come **from `192.168.1.99`**, the Nagios server itself. That means `NetworkVM` tried to find **`192.168.1.9`** on the local network (using ARP) and **no device answered**. Nothing on the network had that IP.

> **Compare with the first time `comp-a-test` went DOWN:** In [**01 – Troubleshoot Windows Host Down**](01-Troubleshoot-Windows-Host-Down.md), the ping just **timed out**. That meant a device was at the address, but its firewall was dropping the pings. This time, there was **no device at the address at all**.

---

### Step 2 – Check comp-a-test's IP address

On **`comp-a-test`**, in PowerShell:

<img width="847" height="292" alt="8" src="https://github.com/user-attachments/assets/3b996d93-cc6a-4c51-bf49-4529605da629" />

I scrolled to the adapter NetworkVM shares, **`vEthernet (Net-MGNT-SW)`**:

<img width="984" height="824" alt="9" src="https://github.com/user-attachments/assets/9f86d4ea-d4bb-4c39-ac4c-cc6af8a0ff29" />

**Findings:**

1. **`comp-a-test`** had **`192.168.1.18`**, not **`192.168.1.9`**. Nagios was checking an address nothing was using.
2. The adapter's MAC address ended in **`5A`**. The DHCP reservation I created on the router for **`192.168.1.9`** used a MAC ending in **`5D`**. I hadn't changed the MAC.

I also noticed the **Microsoft Wi-Fi Direct Virtual Adapter** (**`Local Area Connection* 9`**) has a MAC ending in **`5B`**, very close to the new **`5A`**. Windows creates these related MACs from the same Wi-Fi hardware.

---

### Step 3 – Check the router's DHCP list

I logged in to the router at `https://192.168.1.1`, clicked **Advanced → Network Settings → IPv4 Address Distribution**, then **Connection List**.

<img width="956" height="727" alt="10" src="https://github.com/user-attachments/assets/75149e59-5cde-48ca-8d3f-40ca7ba16251" />

<img width="1233" height="437" alt="11" src="https://github.com/user-attachments/assets/16065c6e-5d50-4409-b790-cbe4f24e0eb9" />

The router's DHCP pool hands out `192.168.1.10` – `192.168.1.50`.

<img width="1317" height="1039" alt="12" src="https://github.com/user-attachments/assets/8b79b7d6-2c41-4c94-aa1b-bb0ea0f89e7f" />

**`comp-a-test`** appeared **twice**:

| Host Name | IP Address | MAC | Lease Type | Status |
|---|---|---|---|---|
| comp-a-test | 192.168.1.18 | **`10:…:5A`** | Dynamic | **Active** |
| comp-a-test | 192.168.1.9 | **`10:…:5D`** | Static | **Inactive** |

**Finding:** The reservation for **`192.168.1.9`** was still there, but it was tied to the old MAC (**`5D`**), which no longer exists, so it showed **Inactive**. When **`comp-a-test`** asked for an IP with its new MAC (**`5A`**), the router didn't recognize it as the reserved device and gave it the next free address from the pool, **`192.168.1.18`**.

---

```
Before:  vEthernet MAC 10-…-5D  →  router reservation matches  →  192.168.1.9  ✅  Nagios checks .9
After:   vEthernet MAC 10-…-5A  →  no reservation matches      →  192.168.1.18     Nagios checks .9  ❌
```








