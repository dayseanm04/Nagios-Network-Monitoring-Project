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

2. Click **Inbound Rules** → **New Rule…**

<img width="1052" height="393" alt="11" src="https://github.com/user-attachments/assets/7c9e0ad8-8749-4727-aceb-6f0c97509fe9" />

3. Select **Custom** rule and Click **Next**

<img width="714" height="574" alt="12" src="https://github.com/user-attachments/assets/0402f44e-63b3-425d-a3ab-1dd7701eae67" />

4. Select **All programs** and Click **Next**

<img width="698" height="573" alt="13" src="https://github.com/user-attachments/assets/b2185910-19e4-4d23-9c36-022254291457" />

5. Select **ICMPv4** for Protocol type and Click **Next**

<img width="710" height="570" alt="14" src="https://github.com/user-attachments/assets/a72fb734-6ea9-4395-8ee8-dbfe41c21498" />

Under scope **Which local ip addresses does this rule apply** to chose any

Under scope **Which remote ip addresses does this rule appl** to chose these ip and click add [**added 192.168.1.99**] and Click **Next**:

<img width="702" height="567" alt="15" src="https://github.com/user-attachments/assets/3451faf6-723c-412d-a1a5-338a1d4ddbd0" />

Select **Allow the connection** and click **Next**

<img width="706" height="567" alt="16" src="https://github.com/user-attachments/assets/d16aa7fc-f651-4584-ab50-d1c6e9e17e10" />

Select **Domain, Private and public** and click **Next**

<img width="714" height="571" alt="17" src="https://github.com/user-attachments/assets/c51d431b-c2a2-48bd-8b4c-bfddac772bd7" />

named the rule **IN-Nagios_VM-ICMP-Allow** and gave it a description: 

#### Allows incoming ICMP (ping) traffic from the Ubuntu Nagios monitoring server to check device availability and uptime across the local network. 

<img width="709" height="574" alt="18" src="https://github.com/user-attachments/assets/1ab45b5d-7200-49c9-a774-58030f30cee0" />

Click **Finish**. The rule appears in the Inbound Rules list as **Enabled**.

<img width="864" height="277" alt="19" src="https://github.com/user-attachments/assets/fdc995f1-c662-4ea0-9cf3-a53ad47f7b3c" />

---

## Verification

After the rule was added, **`NetworkVM`** could ping `comp-a-test` and Nagios changed the host to **UP** on its next check.

**Full test steps and results:** [**Click here to see the Windows host connectivity test**](../../06-Testing-and-Validation/01-Validate-Windows-Host-Connectivity.md)

The 7 Windows **services** are still **CRITICAL**. That's expected: they use **`check_nt`**, which needs a Windows agent on port **`12489`**. This will be addressed when NCPA is installed.

---

## Lessons Learned and Prevention

| Lesson | Action |
|---|---|
| A monitored host's IP changing breaks every check for that host | Confirm the router reservation for **`comp-a-test`** is in place and matches its MAC address, so its IP stays fixed |
| **`No route to host`** and **`timed out`** mean different things | **`No route to host`** usually means nothing is at that IP; `timed out` usually means something is there but not answering, often a firewall |
| Testing in both directions isolates the problem fast | Always ping both ways. One-way failure points to a firewall on the side that isn't answering |
| The firewall rule's local IP is tied to **`192.168.1.9`** | If the host's IP changes again, the rule stops matching. Setting the local IP scope to **Any IP address** would avoid that, while the remote IP limit still restricts it to the Nagios server |
| Windows blocks inbound ping by default | When adding any new Windows host to Nagios, add this firewall rule as part of setup |

