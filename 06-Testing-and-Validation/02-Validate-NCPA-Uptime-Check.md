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

<img width="963" height="442" alt="2" src="https://github.com/user-attachments/assets/6f3573cf-c48a-4bbc-b07b-22dc3f872483" />

The **Check Results** count is now **36**, up from 2 when I was testing in the NCPA web interface.

2. I click **Checks**.

<img width="966" height="763" alt="3" src="https://github.com/user-attachments/assets/37c6104c-3c2e-4ea6-8092-53fc0e79ce7d" />


**Result:** PASS

The uptime in NCPA matches what Nagios shows, and **Executed By `192.168.1.99`** proves the check came from the Nagios server. My earlier tests from the NCPA web interface showed **`127.0.0.1`** instead.

---

## TC-03 – Confirm the Check Runs on a Schedule

In the NCPA **Checks** log, I look at the **`system/uptime`** entries over time.

| Check Time | Uptime reported |
|---|---|
| 14:34:17 | 3 hours 28 minutes 31 seconds |
| 14:36:08 | 3 hours 30 minutes 22 seconds |
| 14:37:59 | 3 hours 32 minutes 13 seconds |
| ... | ... |
| 15:02:19 | 3 hours 56 minutes 33 seconds |

**Result:** PASS. Checks keep arriving from **`192.168.1.99`**, and the uptime goes up each time, which shows the value is live.

---

# Phase 2 – With Thresholds

Without thresholds, the check always returns OK, so it can't alert me to a reboot. In this phase, I add thresholds and restart **`comp-a-test`** to make sure a recent reboot triggers WARNING and CRITICAL.

**Goal:**

| Uptime | Expected result |
|---|---|
| More than 1 hour (3600 seconds) | 🟩 OK |
| Under 1 hour | 🟨 WARNING |
| Under 10 minutes (600 seconds) | 🟥 CRITICAL |

---

## TC-04 – First Attempt: Thresholds Without Colons

I edit the service in **`windows.cfg`**:

<img width="820" height="431" alt="4" src="https://github.com/user-attachments/assets/8b435bd2-dba8-415b-87a7-72e1d7116513" />

I run the pre-flight check, reload Nagios, and restart **`comp-a-test`**.

<img width="1028" height="761" alt="5" src="https://github.com/user-attachments/assets/8b70cc01-0883-472d-a501-01f82ed0d288" />

**Actual:**







