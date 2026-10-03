# 01 – Validate Windows Host Connectivity

Test results confirming the Windows host **`comp-a-test`** is reachable from the Nagios server and shows **UP** in Nagios after the Windows Firewall fix.

**Troubleshooting that led to this test:** [**Click here to see the Windows host down troubleshooting**](../05-Troubleshooting/01-Windows-Host/01-Troubleshoot-Windows-Host-Down.md)

---

## Test Summary

| Item | Details |
|---|---|
| Date | October 2, 2026 |
| Host under test | **`comp-a-test`** (**`192.168.1.9`**) |
| Tested from | **`NetworkVM`** (**`192.168.1.99`**) |
| Change being validated | Inbound firewall rule **`IN-Nagios_VM-ICMP-Allow`** on **`comp-a-test`** |
| Overall result | **Success** |

---

## Objective
Confirm that:

1. **`NetworkVM`** can ping **`comp-a-test`**.
2. Nagios reports **`comp-a-test`** as **UP**.

---

## Test Cases

| ID | Test | Expected result | Actual result | Status |
|---|---|---|---|---|
| TC-01 | Ping **`comp-a-test`** from **`NetworkVM`** | Replies received, 0% packet loss | 4/4 received, 0% loss, avg 0.514 ms |  PASS |
| TC-02 | Host status on Nagios Hosts page | **`comp-a-test`** shows **UP** | **UP** – `PING OK - Packet loss = 0%, RTA = 0.67 ms` | PASS |
| TC-03 | Host details for `comp-a-test` | Host Status **UP**, correct IP, check is active | **UP**, `192.168.1.9`, Check Type ACTIVE, attempt 1/10 HARD | PASS |

---

## TC-01 – Ping from the Nagios Server

**Steps:**

On **`NetworkVM`**, run:

```bash
ping -c 4 192.168.1.9
```

**Expected:** 4 packets transmitted, 4 received, 0% packet loss.

**Actual:**

<img width="800" height="418" alt="1" src="https://github.com/user-attachments/assets/4a582566-3f10-4b70-b94d-6c7f7a03aede" />

---

## TC-02 – Host Status in Nagios

**Steps:**

1. Go to **`http://192.168.1.99/nagios`** and sign in.
2. Click **Hosts**.

<img width="1026" height="565" alt="2" src="https://github.com/user-attachments/assets/9c4b6c00-b382-47dc-90ec-a177b6d52223" />
