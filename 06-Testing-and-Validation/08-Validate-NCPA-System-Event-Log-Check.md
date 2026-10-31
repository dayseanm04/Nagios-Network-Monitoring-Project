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
