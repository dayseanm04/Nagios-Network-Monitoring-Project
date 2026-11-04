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
