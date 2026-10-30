# 06 – Validate NCPA C: Drive Space Check

In this doc, I will verify that the **`C: Drive Space utilization`** check for **`comp-a-test`** works. The check went **WARNING** as soon as it was added, because C: was above **70%**. I deleted files and folders to free up space, and the check went back to **OK**. That makes this a full test: the alert fired, I fixed the problem, and Nagios confirmed it.

> **How this check was created:** [**Click here to see Monitor Windows C: Drive Space with NCPA**](../04-Monitoring-Use-Cases/01-Windows-Host/07-Monitor-Windows-C-Drive-Space-with-NCPA.md)

---

## Test Cases

| ID | Test | Expected | Actual | Status |
|---|---|---|---|---|
| TC-01 | Service appears with a real result | Disk usage percentage | WARNING – Used_percent was 72.20 % | ✅ PASS |
| TC-02 | Thresholds are applied | Perf data shows `70;90` | `'used_percent'=72.20%;70;90;` | ✅ PASS |
| TC-03 | Usage above 70% goes WARNING | **WARNING** | WARNING at 72.20%, attempt 1/5 (SOFT) | ✅ PASS |

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

<img width="1033" height="698" alt="2" src="https://github.com/user-attachments/assets/2151b3c0-eccd-4626-9e7f-9270a1c65d5b" />


**Result:** ✅ PASS. The performance data shows the value, then the warning (**`70`**) and critical (**`90`**) thresholds from the `check_command`.

---

## TC-03 – Usage Above 70% Goes WARNING

<img width="1033" height="698" alt="3" src="https://github.com/user-attachments/assets/79ac2850-e5a0-418b-8d20-a157ffb556ed" />

72.20% is above the 70% warning and below the 90% critical, so **WARNING** is correct.

**Result:** ✅ PASS

The state is **SOFT** at attempt 1/5, and no notification has been sent yet. That's the **`time-sensitive`** template working: Nagios rechecks before treating the problem as real. Disk usage doesn't drop on its own, so this would have reached 5/5 HARD a few minutes later.

## Fix – Free Up Space on C:

C: was at **72.20%**, above my **70%** warning. To fix it, I deleted files and folders I no longer needed from **`comp-a-test`**.

This is how monitoring is supposed to work: the alert told me about a problem before it got serious, and I fixed it.


