# 02 – Troubleshoot comp-a-test DNS Check (No Records)

Troubleshooting record for the **`DNS Resolution - comp-a-test`** service on **`verizon-r1`** going **CRITICAL** with **`DNS 192.168.1.1 has no records`**. Covers how the problem was found, the steps taken to isolate it, the root cause, and the fix.

> **Where this came up:** [**04-Monitoring-Use-Cases / 02-Router / 03 – Add DNS Resolution Checks for Verizon-R1**](../../04-Monitoring-Use-Cases/02-Router/03-Add-DNS-Resolution-Checks-for-Verizon-R1.md)

**Note on screenshots:** The Nagios screenshots here were taken before I fixed the typos in the service name and notes, so they show **`DNS Resolutioon - comp-a-test`** and **`comp-a-tes`**. See [**Typo Fixes**](../../04-Monitoring-Use-Cases/02-Router/03-Add-DNS-Resolution-Checks-for-Verizon-R1.md#typo-fixes).

---

## Problem Description

After adding the **`DNS Resolution - comp-a-test`** service to **`verizon-r1`**, it went **CRITICAL**. The **`DNS Resolution google.com`** service on the same router was **OK** at the same time.

<img width="1013" height="618" alt="1" src="https://github.com/user-attachments/assets/de023cac-4cb3-41fd-a18c-1534b7d6a45d" />

<img width="1020" height="807" alt="2" src="https://github.com/user-attachments/assets/a9d3a576-462f-4acc-ad43-286bf4bc11df" />

<img width="940" height="679" alt="3" src="https://github.com/user-attachments/assets/29ed1bf3-d91e-4e35-9f03-e8f755039475" />

---

## Observation

| Where | What it showed |
|---|---|
| Current Status | **CRITICAL** (HARD state, 2/2) |
| Status Information | **`DNS 192.168.1.1 has no records`** |
| Performance Data | Empty. No lookup succeeded, so there was nothing to time |
| `DNS Resolution google.com` | **OK** on the same router at the same time |

**What this tells me:**
- The router **answered**, because the output names **`192.168.1.1`** as the server. If it hadn't, the error would be a timeout.
- The router's DNS service works, because google.com resolves.
- The problem is specific to the name **`comp-a-test`**: the router doesn't have a record for it.

---

## Impact

| Area | Impact |
|---|---|
| Nagios monitoring of `comp-a-test` | None. Nagios checks **`comp-a-test`** by IP (**`192.168.1.9`**), not by name, so its host and NCPA checks kept working |
| DNS check | **CRITICAL** and sending alerts |

---

## Troubleshooting Steps Taken

### Step 1 – Check the router's DNS records

The CRITICAL says the router has no record, so I checked what the router actually has. I log in to the router at **`https://192.168.1.1`** and click **Advanced**.

<img width="1069" height="684" alt="4" src="https://github.com/user-attachments/assets/fd718a20-62b2-40b5-97c4-f663bf041c7a" />

I click **Network Settings** → **DNS Server**.

<img width="1061" height="1004" alt="5" src="https://github.com/user-attachments/assets/60e2869e-e560-40a5-8f00-23b902c1db68" />

| Device | IPv4 record? | Source |
|---|---|---|
| iPad, iPhone, other devices | Yes (e.g. `192.168.1.16`, `192.168.1.21`) | **DHCP** |
| **`comp-a-test`** | ❌ **No**. Only IPv6 addresses | **NDP** |

**Finding:** The router knows the name **`comp-a-test`**, but only with IPv6 addresses. There's no IPv4 record pointing to **`192.168.1.9`**. **`check_dns`** asks for an IPv4 address, so the router had nothing to give back.

---

## Resolution

Since **`comp-a-test`** will keep its static IP, I configured a DNS record for it.

On the **DNS Server** page, I click **Add DNS Entry**:

<img width="963" height="560" alt="6" src="https://github.com/user-attachments/assets/a6365a49-0948-4742-a0c0-90b412ec68db" />

I click **Apply**.

<img width="949" height="675" alt="7" src="https://github.com/user-attachments/assets/aa21f064-2958-4794-a12b-d7d30515a6de" />

**`comp-a-test`** now has an IPv4 record for **`192.168.1.9`** with the source **Manually**.

> **Why not switch back to DHCP?** The static IP is what keeps **`comp-a-test`** at **`192.168.1.9`** when its MAC address changes. Going back to DHCP would bring back the earlier problem.

---

## Verification

### From comp-a-test (Windows)

**Reverse lookup.** I ask the router which name belongs to **`192.168.1.9`**:

```cmd
nslookup 192.168.1.9
```

<img width="680" height="223" alt="8" src="https://github.com/user-attachments/assets/0cd7c2d2-354f-448c-8bc2-a721ea27ae91" />

The router (**`CR1000B.mynetworksettings.com`** at **`192.168.1.1`**) answers with **`comp-a-test`**.

**Forward lookup in PowerShell:**

```powershell
Resolve-DnsName comp-a-test -Server 192.168.1.1 -Type A
```

<img width="903" height="337" alt="9" src="https://github.com/user-attachments/assets/f16e23bb-962c-40c7-b294-f52d284269b1" />

**`192.168.1.9`** is in the results.

> This returned four addresses: **`192.168.1.9`**, **`169.254.20.194`**, **`172.20.144.1`**, and **`192.168.56.1`**. Only **`192.168.1.9`** comes from the router. The router's DNS Server page has only that one IPv4 record for **`comp-a-test`**, and the lookup from NetworkVM below returns only **`192.168.1.9`**. The other three are **`comp-a-test`**'s own virtual adapters. Because I ran the command on **`comp-a-test`** itself, Windows added its own addresses.

### From Nagios





















