# 01 – Troubleshoot Windows Host Down

Troubleshooting record for the Windows host **`comp-a-test`** showing **DOWN** in Nagios. Covers how the problem was found, the root cause, and the fix.

---

## Summary
| Item | Details |
|---|---|
| Date | October 2, 2026 |
| Affected host | **`comp-a-test`** (My Windows Host) |
| Host group | **`windows-servers`** |
| Monitoring server | **`NetworkVM`** (**`192.168.1.99`**) |
| Symptom | Host **DOWN** in Nagios; all 7 Windows services **CRITICAL** |
| Root cause | Host IP changed from `.151` to `.9`, and Windows Firewall blocked inbound ICMP from the Nagios server |
| Resolution | Updated the host address in Nagios and created an inbound firewall rule allowing ICMPv4 from **`192.168.1.99`**|
| Status | Resolved – host status **UP** |

---

## Problem Description

<img width="978" height="532" alt="1" src="https://github.com/user-attachments/assets/d4c0d14d-3571-4d5f-a82d-b23b3f0ac7d5" />


The Nagios dashboard showed the Windows host **`comp-a-test`** as **DOWN**, and it had been down since it was first added (about 5 days). All seven Windows service checks configured in [**01 – Add a Windows Host and Services to Nagios**](../../04-Monitoring-Use-Cases/01-Windows-Host/) were **CRITICAL**.

At the same time, **`localhost`** (the Nagios server itself) was **UP**, which meant Nagios was running and checking hosts normally. The problem was limited to the Windows host.

---

## Symptoms

<img width="1025" height="837" alt="2" src="https://github.com/user-attachments/assets/c356c541-290d-445f-8ebe-c046b149a5ce" />

| Where | What it showed |
|---|---|
| Hosts page | `comp-a-test` – **DOWN** – `CRITICAL - Host Unreachable (192.168.1.151)` |
| Host details | `Host check timed out after 31.01 seconds`, Current Attempt `10/10 (HARD state)` |
| Service details | All 7 services **CRITICAL** – `connect to address 192.168.1.151 and port 12489: No route to host` |
| Notifications | Repeated **HOST DOWN** alerts since September 26: `PING CRITICAL - Packet loss = 100%` |

**Note:** I changed comp-a-test ip to **192.168.1.9**

---

## Impact

- Nagios couldn't confirm whether the Windows host was online.
- Host DOWN alerts were being sent repeatedly, which creates alert noise and makes real outages harder to spot.
- Service checks for the host couldn't run, since Nagios treats services on a DOWN host as problem services.

---

## Troubleshooting Steps Taken

### Step 1 – Review the alert details in Nagios

Opened **Hosts** → `comp-a-test` → **View Status Detail For This Host** and **View Notifications For This Host**.

**Finding:** The service errors said `No route to host` for **`192.168.1.151`**. That error usually means no device answered at that IP at all, not that a device answered and refused the connection.

<img width="1029" height="670" alt="3" src="https://github.com/user-attachments/assets/cfb788fa-bb10-44ff-9ba1-8857ddf739d9" />

**Note:** I changed **comp-a-test** ip to **192.168.1.9** 

---

### Step 2 – Check the Windows host's current IP

```cmd
ipconfig
```

<img width="721" height="239" alt="4" src="https://github.com/user-attachments/assets/2939fdf5-8bca-40b7-98d5-282fbf160ebb" />

**Finding:** The host's IPv4 address was **`192.168.1.9`**, not **`192.168.1.151`**. Nagios was checking an address the host no longer used.


**Action:** Updated the `address` line in `/usr/local/nagios/etc/objects/windows.cfg` to **`192.168.1.9`**, ran the pre-flight check, and restarted Nagios:

<img width="858" height="481" alt="5" src="https://github.com/user-attachments/assets/225a70cd-98cc-4957-b9f3-3ac1ce8ab61d" />

**Result:** The host details page now showed `192.168.1.9`, but the host was **still DOWN** with `Host check timed out after 31.01 seconds`. Fixing the IP was necessary but not enough.

<img width="1012" height="856" alt="6" src="https://github.com/user-attachments/assets/41dc561c-ed40-4ca8-815b-0092e61beec0" />

---

### Step 3 – Ping from Windows to the Nagios server

On **`comp-a-test`**:

```cmd
ping 192.168.1.99
```

<img width="764" height="340" alt="7" src="https://github.com/user-attachments/assets/ed5e2309-4c2e-4370-9812-3c96e09415da" />

### Step 4 – Ping from the Nagios server to Windows

On **`NetworkVM`**:

```bash
ping -c 3 192.168.1.9
```

<img width="820" height="300" alt="8" src="https://github.com/user-attachments/assets/5e9a86af-23a8-4df4-ad29-a64047ee0892" />

---

### Step 5 – Ping the Windows host from itself

On `comp-a-test`:

```cmd
ping 192.168.1.9
ping 127.0.0.1
```

<img width="988" height="598" alt="9" src="https://github.com/user-attachments/assets/b50359df-44d2-43e3-a302-ae693d652554" />

**Finding:** Both succeed. The Windows network stack and adapter are working.

---

### Analysis

| Test | Result | What it rules out |
|---|---|---|
| Windows → NetworkVM | Success | Cabling, switch, routing, and the VM's network config |
| Windows → itself / loopback | Success | Windows network adapter and TCP/IP stack |
| NetworkVM → Windows | 100% loss | — |

Windows can send pings out and get replies back, but it won't answer pings coming in. That one-way pattern points to something on the Windows host blocking **inbound** ICMP: the Windows Defender Firewall, which blocks inbound echo requests by default.

---

## Root Cause

There were two separate problems:

1. **Wrong IP address in Nagios.** The Windows host's IP changed from **`192.168.1.151`** to **`192.168.1.9`**. Nagios was still checking **`.151`**, where no device was answering, which caused the **`No route to host`** errors.
2. **Windows Firewall blocking inbound ICMP.** After the address was corrected, Windows Defender Firewall dropped the echo requests from the Nagios server. Nagios uses ping to decide whether a host is UP, so with no replies the host stayed DOWN.

---

## Resolution

Created an inbound Windows Defender Firewall rule on `comp-a-test` that allows ICMPv4 only from the Nagios server.

### Using the GUI

1. On **`comp-a-test`**, search for and open **Windows Defender Firewall with Advanced Security**.

<img width="859" height="401" alt="10" src="https://github.com/user-attachments/assets/b33bc261-db16-4008-a133-fe47758768aa" />


