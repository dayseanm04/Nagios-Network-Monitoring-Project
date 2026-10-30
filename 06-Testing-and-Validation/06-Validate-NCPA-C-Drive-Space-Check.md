# 06 – Validate NCPA C: Drive Space Check

In this doc, I will verify that the **`C: Drive Space utilization`** check for **`comp-a-test`** works. The check went **WARNING** as soon as it was added, because C: was above **70%**. I deleted files and folders to free up space, and the check went back to **OK**. That makes this a full test: the alert fired, I fixed the problem, and Nagios confirmed it.

> **How this check was created:** [**Click here to see Monitor Windows C: Drive Space with NCPA**](../04-Monitoring-Use-Cases/01-Windows-Host/07-Monitor-Windows-C-Drive-Space-with-NCPA.md)

---

## Test Cases

| ID | Test | Expected | Actual | Status |
|---|---|---|---|---|
| TC-01 | Service appears with a real result | Disk usage percentage | WARNING – Used_percent was 72.20 % | ✅ PASS |
| TC-02 | Thresholds are applied | Perf data shows `70;90` | `'used_percent'=72.20%;70;90;` | ✅ PASS |

---

## TC-01 – Service Appears with a Real Result

**Steps:** I log in to **`http://192.168.1.99/nagios`** and click **Services**.

<img width="1037" height="598" alt="1" src="https://github.com/user-attachments/assets/593c19f7-d286-44d6-9ba8-17c778674d7e" />

Service totals: **9 OK, 2 WARNING, 1 CRITICAL**, 12 total.

**Result:** ✅ PASS. The check is running and returning a real value from NCPA.

> The other WARNING and the CRITICAL on this screenshot are the **swap** and **RAM** checks, which are covered in [**05 – Validate NCPA Memory and Swap Checks**](05-Validate-NCPA-Memory-and-Swap-Checks.md).

---

## TC-02 – Thresholds Are Applied

**Steps:** I click **C: Drive Space utilization** and check **Performance Data**.

<img width="1033" height="698" alt="2" src="https://github.com/user-attachments/assets/79ac2850-e5a0-418b-8d20-a157ffb556ed" />

