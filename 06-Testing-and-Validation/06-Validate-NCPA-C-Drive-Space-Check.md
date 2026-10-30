# 06 – Validate NCPA C: Drive Space Check

In this doc, I will verify that the **`C: Drive Space utilization`** check for **`comp-a-test`** works. The check went **WARNING** as soon as it was added, because C: was above **70%**. I deleted files and folders to free up space, and the check went back to **OK**. That makes this a full test: the alert fired, I fixed the problem, and Nagios confirmed it.

> **How this check was created:** [**Click here to see Monitor Windows C: Drive Space with NCPA**](../04-Monitoring-Use-Cases/01-Windows-Host/07-Monitor-Windows-C-Drive-Space-with-NCPA.md)

---

## Test Cases

| ID | Test | Expected | Actual | Status |
|---|---|---|---|---|
| TC-01 | Service appears with a real result | Disk usage percentage | WARNING – Used_percent was 72.20 % | ✅ PASS |

---
