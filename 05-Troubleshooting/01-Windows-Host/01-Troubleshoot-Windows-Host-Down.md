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


<img width="1029" height="670" alt="3" src="https://github.com/user-attachments/assets/cfb788fa-bb10-44ff-9ba1-8857ddf739d9" />

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
