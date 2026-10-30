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
| TC-04 | After freeing space, check returns to OK | **OK** below 70% | OK – Used_percent was 67.00 % | ✅ PASS |
| TC-05 | NCPA receives the checks from Nagios | Entries executed by `192.168.1.99` | `disk/logical/C:|/used_percent` from `192.168.1.99`, Active | ✅ PASS |

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

---

## TC-04 – Check Returns to OK After Freeing Space

**Steps:** Two days later, I go back to **Services** in Nagios.

<img width="957" height="481" alt="4" src="https://github.com/user-attachments/assets/ff0b5e28-d425-4ee5-a597-bab16d10b0be" />

**Result:** ✅ PASS. After freeing space, **C: dropped from 72.20% to 67.00%**, below the **70%** warning, and the check went back to **OK**.

### Before and After

| | Oct 26 (before cleanup) | Oct 28 (after cleanup) |
|---|---|---|
| Used | 72.20% | 67.00% |
| Status | 🟨 WARNING | 🟩 OK |

On a 482 GiB drive, that 5.2% drop is roughly **25 GiB** freed.

> 67% is still close to the 70% warning. If C: keeps filling up, this check will go WARNING again, which is what I want it to do.

---

## TC-05 – NCPA Receives the Checks from Nagios

**Steps:** On **`comp-a-test`**, I log in to **`https://localhost:5693`**, click **Checks**, filter by **Type: Active** and **Executed By: 192.168.1.99**, and click **Filter**.

<img width="947" height="512" alt="5" src="https://github.com/user-attachments/assets/170aeb6f-bb83-497f-8a46-3107d9d25df4" />

**Result:** ✅ PASS. The disk check is logged in NCPA as an active check from **`192.168.1.99`**. The check time (19:33:22) is exactly 2 minutes after the Nagios check in TC-05 (19:31:22), so this is the next scheduled check arriving from Nagios.

---

## Conclusion

The **`C: Drive Space utilization`** check works. Nagios gets the real C: usage from NCPA and applies the 70% and 90% thresholds. It retries every minute while there's a problem and checks every 2 minutes when things are OK. When C: was at 72.20%, the check went **WARNING**. After I deleted files and brought it down to 67.00%, it went back to **OK**. NCPA's Checks log confirms the requests come from the Nagios server. All six test cases passed.

---

## Related Docs

