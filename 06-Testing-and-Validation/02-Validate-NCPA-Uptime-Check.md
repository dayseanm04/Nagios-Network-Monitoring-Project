# 02 – Validate NCPA Uptime Check

In this doc, I will verify that the **`system uptime`** service check for **`comp-a-test`** works end to end. I'll test it in two phases:

1. **Without thresholds** – confirm Nagios can reach NCPA and get a result.
2. **With thresholds** – confirm the check goes WARNING and CRITICAL after a reboot.

I'll confirm results from both sides: in Nagios, where the check is scheduled, and in NCPA, where the check is answered.

> **How this check was created:** [**Click here to see Monitor Windows Uptime with NCPA**](../04-Monitoring-Use-Cases/01-Windows-Host/03-Monitor-Windows-Uptime-with-NCPA.md)

---

## Test Cases

| ID | Phase | Test | Expected | Actual | Status |
|---|---|---|---|---|---|
| TC-01 | No thresholds | Service in Nagios | **OK** with uptime value | OK – Uptime was 3 hours 56 minutes 33 seconds | PASS |
| TC-02 | No thresholds | Check source in NCPA | Executed By **`192.168.1.99`** | **`192.168.1.99`**, Active | PASS |
| TC-03 | No thresholds | Checks run on a schedule | Repeated entries, uptime increasing | Entries from 17:43 to 17:50 | PASS |
| TC-04 | Thresholds (first attempt) | **`-w 3600 -c 600`**, uptime 23 min | **WARNING** (under 1 hour) | **CRITICAL** | FAIL – config issue |
| TC-05 | Thresholds (fixed) | **`-w 3600: -c 600:`**, uptime under 10 min | **CRITICAL** | CRITICAL – Uptime was 8 minutes 20 seconds | PASS |
| TC-06 | Thresholds (fixed) | Uptime between 10 min and 1 hour | **WARNING** | WARNING – Uptime was 12 minutes 17 seconds |  PASS |

---

# Phase 1 – Without Thresholds

Check command:

```
check_command           check_ncpa!-M system/uptime
```

## TC-01 – Check the Service in Nagios Core

**Steps:**

1. I log in to the Nagios web interface at **`http://192.168.1.99/nagios`**.
2. I click **Services**.
3. I find **system uptime** for **`comp-a-test`**.

<img width="1035" height="521" alt="1" src="https://github.com/user-attachments/assets/243a7682-57dc-4f0c-a381-2254e0be8536" />

**Actual:**

| Host | Service | Status | Attempt | Status Information |
|---|---|---|---|---|
| **`comp-a-test`** | system uptime | **OK** | 1/3 | OK: Uptime was 3 hours 56 minutes 33 seconds |

**Result:** PASS

> The service totals show **8 OK, 1 CRITICAL**. The CRITICAL is the **`SSH`** check on **`localhost`**, which is unrelated to this test.

---

## TC-02 – Confirm NCPA Received the Check

**Steps:**

1. On **`comp-a-test`**, I open **`https://localhost:5693`** and log in with the token.




























