# 08 – Validate NCPA System Event Log Check

In this doc, I will verify that the **`Failed System logs`** check for **`comp-a-test`** is working. I'll confirm it in Nagios, look at the log entries it found, and check that NCPA is receiving the checks from the Nagios server.

> **How this check was created:** [**Click here to see Monitor Windows System Event Logs with NCPA**](../04-Monitoring-Use-Cases/01-Windows-Host/09-Monitor-Windows-System-Event-Logs-with-NCPA.md)

---

## Test Cases

| ID | Test | Expected | Actual | Status |
|---|---|---|---|---|
| TC-01 | Service appears with a real result | A count of matching log entries | OK – System has 3 logs | ✅ PASS |
| TC-02 | Thresholds are applied | Perf data shows `5;10` | `'System'=3;5;10; 'Total Count'=3;5;10;` | ✅ PASS |
| TC-03 | Matching entries are real failures | Entries mention failure/failed/failing | 3 "driver failed to load" entries | ✅ PASS |
| TC-04 | `generic-service` settings | 10-min interval, attempts out of 3 | 14:30:38 → 14:40:38, 1/3 | ✅ PASS |
| TC-05 | NCPA receives the checks from Nagios | `logs` entry from `192.168.1.99` | OK, Active, from `192.168.1.99` | ✅ PASS |

---

## TC-01 – Service Appears with a Real Result

**Steps:** I log in to **`http://192.168.1.99/nagios`** and click **Services**.

<img width="1020" height="616" alt="1" src="https://github.com/user-attachments/assets/4e600768-160a-4771-95a3-f503ae36bda0" />

Service totals: **13 OK, 1 CRITICAL**, 14 total.

**Result:** ✅ PASS

> The CRITICAL is `Logical memory usage` at 94.50%, covered in [**05 – Validate NCPA Memory and Swap Checks**](05-Validate-NCPA-Memory-and-Swap-Checks.md).

---

## TC-02 – Thresholds Are Applied

**Steps:** I click **Failed System logs** and check **Performance Data**.

<img width="937" height="758" alt="2" src="https://github.com/user-attachments/assets/ffda8b7d-529e-48f5-82ec-28117ad6cf5a" />

**Actual:** **`'System'=3;5;10; 'Total Count'=3;5;10;`**

**Result:** ✅ PASS. Each value shows the count (3), then the warning (5) and critical (10) thresholds. There are two values because NCPA reports a count for each log it searched (System) plus a total. With only one log in the query, they're the same.

---

## TC-03 – Matching Entries Are Real Failures

<img width="937" height="758" alt="3" src="https://github.com/user-attachments/assets/6da1a8f2-cc22-48d8-a9c0-085cb5c7eb36" />

The details page lists the 3 entries the check found:

| Time | Level | Event ID | Source | Message |
|---|---|---|---|---|
| 2026-10-30 11:08:04 | WARNING | 219 | Microsoft-Windows-Kernel-PnP | The driver \Driver\WUDFRd failed to load |
| 2026-10-30 11:08:02 | WARNING | 219 | Microsoft-Windows-Kernel-PnP | The driver \Driver\WUDFRd failed to load |
| 2026-10-30 11:08:01 | WARNING | 219 | Microsoft-Windows-Kernel-PnP | The driver \Driver\WUDFRd failed to load |

**Result:** ✅ PASS. All three entries contain "failed," so the message filter is matching correctly.

**What these entries are:** All three were logged within 3 seconds of each other at 11:08. The `system uptime` check shows **`comp-a-test`** had been up about 3 hours 24 minutes at 14:31, so it started around 11:07. 
