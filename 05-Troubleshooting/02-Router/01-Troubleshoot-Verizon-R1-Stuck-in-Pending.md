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




























