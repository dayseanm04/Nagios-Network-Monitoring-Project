# 01 – Troubleshoot Verizon-R1 Stuck in Pending

Troubleshooting record for the router host **`verizon-r1`** staying in **PENDING** after it was added to Nagios. Covers how the problem was found, the steps taken to isolate it, the root cause, and the fix.

> **Where this came up:** [**04-Monitoring-Use-Cases / 02-Router / 02 – Add Verizon Router and Ping Check**](../../04-Monitoring-Use-Cases/02-Router/02-Add-Verizon-Router-and-Ping-Check.md)


**Note on screenshots:** When I first set up the router, **`verizon-r1.cfg`** had **`hostgroups routers`** and the **`network-device-host`** template had **`check_period 24x7`**. Both lines were removed by mistake while I was editing, and I didn't notice until I reviewed my screenshots. I added them back on November 4 and retook the config screenshots, so the screenshots here show both lines. Neither line was related to this issue: the host stayed PENDING until **`check_command`** was added.

---

## Problem Description

After adding **`verizon-r1`** to Nagios, its **PING service** showed **OK** with 0% packet loss, but the **host** itself stayed in **PENDING**. The other two hosts, **`comp-a-test`** and **`localhost`**, were **UP**.

<img width="1036" height="566" alt="1" src="https://github.com/user-attachments/assets/8263d0bf-b460-483c-a90d-0f4dca5c263f" />

<img width="1030" height="622" alt="2" src="https://github.com/user-attachments/assets/e1968b61-2ccc-470b-91e3-345a498789e5" />

<img width="1035" height="798" alt="3" src="https://github.com/user-attachments/assets/a771df10-7e96-4a86-bc7e-71169d2d7a2b" />


---

## Observation

| Where | What it showed |
|---|---|
| Hosts page | `verizon-r1` – **PENDING** for 30 minutes |
| Last Check | **N/A**. The host had never been checked |
| PING service on `verizon-r1` | **OK** – Packet loss = 0%, RTA = 3.08 ms |

The PING service being OK was the key clue. The router was clearly reachable, so the problem wasn't the router or the network. It was something about how Nagios was set up to check the **host**.

---

## Impact

- Nagios couldn't report whether the router was UP or DOWN.
- If the router actually went down, there would be **no host alert**, only a service alert from PING.
- Any host later set with **`parents verizon-r1`** (like an internet connectivity check) couldn't be marked UNREACHABLE correctly, because Nagios didn't know the router's state.

---

## Troubleshooting Steps Taken

### Step 1 – Check the host definition

```bash
cd /usr/local/nagios/etc/routers
ls -l
sudo nano verizon-r1.cfg
```

<img width="999" height="552" alt="4" src="https://github.com/user-attachments/assets/643684c6-db5b-433b-9f14-017c63b13ff4" />

<img width="941" height="663" alt="2" src="https://github.com/user-attachments/assets/6a1c65f6-69bb-4c0e-a77d-f8c4835b9cf3" />

**Finding:** The host definition looked correct: right name, right IP, using **`network-device-host`**. The host block doesn't set a check command itself, so it depends on the template for one.

---

### Step 2 – Check the network-device-host template


```bash
cd ../templates
ls -l
sudo nano custom-templates.cfg
```

<img width="883" height="105" alt="3" src="https://github.com/user-attachments/assets/0939c162-17b7-4982-8080-bdbb136e2f7c" />

<img width="1026" height="396" alt="4" src="https://github.com/user-attachments/assets/2c82d87b-19f2-4945-90c9-f68581f2266e" />

**Finding:** The template sets **when** and how **often** to check, but there's no **`check_command`**, so nothing says **how** to check. It inherits everything else from **`generic-host`**.

---

### Step 3 – Check the generic-host template

```bash
cd ../objects
ls -l
sudo nano templates.cfg
```

<img width="808" height="430" alt="5" src="https://github.com/user-attachments/assets/2ee69292-f519-409c-a052-765e9c2d836c" />

<img width="953" height="485" alt="6" src="https://github.com/user-attachments/assets/1541661a-8f19-45d7-aab3-b4cace85c95d" />


**Finding:** **`generic-host`** doesn't have a **`check_command`** either. It only sets general options like notifications and flap detection. So neither template in the gives **`verizon-r1`** a way to be checked.

---

### Step 4 – Compare with a host that works

**`comp-a-test`** is UP, so I traced its templates the same way.

```bash
cd ../windows-hosts
sudo nano comp-a-test.cfg
```

<img width="886" height="385" alt="7" src="https://github.com/user-attachments/assets/564e331f-3bfd-4da5-aa8a-156e9388def8" />

**`comp-a-test`** uses **`windows-ncpa-host`**:

<img width="836" height="439" alt="8" src="https://github.com/user-attachments/assets/b5b278a2-d252-49dd-8564-6e0e99c93f35" />

**`windows-ncpa-host`** doesn't have a **`check_command`** either, but it uses **`windows-server`**. I checked that template in **`objects/templates.cfg`**:

<img width="970" height="405" alt="8" src="https://github.com/user-attachments/assets/4b550020-a1cd-4a13-80bf-cb67cad6e8ec" />

The **`linux-server`** template, used by **`localhost`**, has the same **`check_command`**:

<img width="1007" height="585" alt="9" src="https://github.com/user-attachments/assets/ef7a2034-b50c-4f2e-ba44-35159c7a913a" />

**Finding:** Both working hosts get **`check_command check-host-alive`** from their templates. **`verizon-r1`** doesn't.

---

## Analysis

| Host | Template chain | `check_command` | Status |
|---|---|---|---|
| comp-a-test | **`windows-ncpa-host`** → **`windows-server`** → **`generic-host`** | ✅ **`check-host-alive`** from **`windows-server`** | UP |
| localhost | **`linux-server`** → **`generic-host`** | ✅ **`check-host-alive`** from **`linux-server`** | UP |
| verizon-r1 | **`network-device-host`** → **`generic-host`** | ❌ None | PENDING |

When I created **`network-device-host`**, I based it on **`generic-host`**, the most basic template. **`generic-host`** is meant to be a starting point that other templates build on, so it doesn't include a check command. The ready-made templates like **`windows-server`** and **`linux-server`** add one, but mine didn't.

---

## Root Cause

The **`network-device-host`** template had **no `check_command`**, and neither did **`generic-host`**, the template it was built on. Without a check command, Nagios had no way to check whether **`verizon-r1`** was up, so the host never got a result and stayed in **PENDING**.

The pre-flight check passed with **0 warnings and 0 errors**, so it didn't catch this. A missing host check command is valid config. The host just never gets checked.

**Why the PING service still worked:** Services have their own **`check_command`**. The PING service used `check_ping` directly, so it ran fine even though the host check didn't exist.

---

## Resolution

I added **`check_command check-host-alive`** to the **`network-device-host`** template, the same command **`windows-server`** and **`linux-server`** use.

```bash
cd ../templates
ls -l
sudo nano custom-templates.cfg
```

<img width="897" height="309" alt="10" src="https://github.com/user-attachments/assets/b1b92aa9-2a22-45fe-aa81-5456a1d5e485" />

<img width="860" height="352" alt="11" src="https://github.com/user-attachments/assets/86358852-aaf1-4eab-9968-561e0df940d2" />

Save: **Ctrl+O**, **Enter**, **Ctrl+X**.

> **Why fix the template instead of the host?** Adding **`check_command`** to **`verizon-r1.cfg`** would only fix this one router. Fixing the template means every network device I add later, eg: another router, gets the host check automatically.

Run the pre-flight check:

```bash
sudo /usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg
```

<img width="998" height="789" alt="12" src="https://github.com/user-attachments/assets/25ca073e-94a6-41ad-adac-e82184fbafd6" />

Restart Nagios:

```bash
sudo systemctl restart nagios
```

<img width="1020" height="256" alt="13" src="https://github.com/user-attachments/assets/913e85e6-a873-47e4-beea-cc6ae93ac4b2" />

## Verification

### Right after the fix (Nov 1, 17:52)

I log in to `http://192.168.1.99/nagios` and click **Hosts**.

<img width="939" height="545" alt="14" src="https://github.com/user-attachments/assets/018e5d4f-1fe7-430d-a6dd-b8e6c4871124" />

<img width="1037" height="599" alt="15" src="https://github.com/user-attachments/assets/97aac846-bced-4088-9f67-fcb2bd9c1f6a" />

Host Status Totals: **3 Up, 0 Down, 0 Pending**. **`verizon-r1`** had a real Last Check time for the first time.

### Three days later (Nov 4, 12:00)

I click **`verizon-r1`** for the full details.

<img width="1028" height="848" alt="16 src="https://github.com/user-attachments/assets/9f25eaaa-761a-4dac-a9b3-c2561f79a97c" />

**Result:** **`verizon-r1`** is **UP**, checked every minute with 2 attempts from **`network-device-host`**, and has stayed stable for almost 3 days.

> **Host check vs. PING service:** Both ping the router, but they answer different questions. The **host check** (**`check-host-alive`**) asks "is the router up at all?" and uses loose thresholds, only failing if the router is nearly unreachable. The **PING service** (**`check_ping!200.0,20%!600.0,60%`**) asks "is the connection healthy?" and alerts sooner on slow replies or packet loss.

---

## Lessons Learned and Prevention

| Lesson | Action |
|---|---|
| **`generic-host`** is a base template, not a complete one. It has no **`check_command`** | Any custom host template built on **`generic-host`** must add its own **`check_command`** |
| The pre-flight check doesn't catch a missing host check command | When a new host stays PENDING with Last Check **N/A**, check its template chain for a **`check_command`** first |
| A working service doesn't prove the host check works | Services and hosts have separate check commands. Check the **Hosts** page, not just **Services** |
| Comparing with a working host finds the difference fast | Tracing **`comp-a-test`**'s template chain showed exactly what **`verizon-r1`** was missing |
| Fix the template, not just the host | Every future network device now gets a host check automatically |

---

## Related Docs

- [**04-Monitoring-Use-Cases / 02-Router / 02 – Add Verizon Router and Ping Check**](../../04-Monitoring-Use-Cases/02-Router/02-Add-Verizon-Router-and-Ping-Check.md)
- [**04-Monitoring-Use-Cases / 02-Router / 01 – Set Up Routers Folder and Hostgroup**](../../04-Monitoring-Use-Cases/02-Router/01-Set-Up-Routers-Folder-and-Hostgroup.md)
- [**02-Setup / 10 – Create Notify-Early, Network Device, and Windows NCPA Templates**](../../02-Setup/10-Create-Notify-Early-Network-Device-and-Windows-NCPA-Templates.md)
- [**06-Testing-and-Validation / 10 – Validate Verizon-R1 Ping Check**](../../06-Testing-and-Validation/10-Validate-Verizon-R1-Ping-Check.md)
