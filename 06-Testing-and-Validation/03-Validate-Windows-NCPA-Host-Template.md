# 03 – Validate Windows NCPA Host Template

In this doc, I will verify that **`comp-a-test`** is using my custom **`windows-ncpa-host`** template after switching from the default **`windows-server`** template. I'll confirm the host is still UP and that its check interval, max attempts, and hostgroup match the new template.

> **How this change was made:** [**Click here to see Apply Windows NCPA Host Template to comp-a-test**](../04-Monitoring-Use-Cases/01-Windows-Host/04-Apply-Windows-NCPA-Host-Template-to-comp-a-test.md)

## Test Summary

| Item | Details |
|---|---|
| Date | October 25, 2026 |
| Host under test | **`comp-a-test`** (**`192.168.1.9`)** |
| Change being validated | **`use windows-server`** → **`use windows-ncpa-host`** |
| Nagios server | **`NetworkVM`** (**`192.168.1.99`**), Nagios Core 4.5.14 |
| Overall result | ✅ **PASS** |

---

## Expected Settings

| Setting | Before (`windows-server`) | Expected after (`windows-ncpa-host`) |
|---|---|---|
| Check interval | 5 min | **3 min** |
| Max check attempts | 10 | **3** |**
| Hostgroup | **`windows-servers`** | **`windows-servers` (inherited) |

---

## Test Cases

| ID | Test | Expected | Actual | Status |
|---|---|---|---|---|
| TC-01 | Host status after the change | **UP** | UP – PING OK, 0% packet loss, RTA 0.64 ms | ✅ PASS |
| TC-02 | Check interval | 3 minutes between checks | 15:02:38 → 15:05:38 (3 min) | ✅ PASS |
| TC-03 | Max check attempts | Attempt shown out of **3** | 1/3 (HARD state) | ✅ PASS |
| TC-04 | Hostgroup membership | Still in `windows-servers` | Member of `windows-servers` | ✅ PASS |

---

## TC-01 – Host Is Still UP

**Steps:**

1. I log in to `http://192.168.1.99/nagios`.
2. I click **Hosts**.

<img width="1028" height="537" alt="1" src="https://github.com/user-attachments/assets/72f96e34-41b8-4524-b408-731c7376e4b9" />

**Result:** ✅ PASS. Changing the template didn't break the host check.

---

## TC-02 – Check Interval Is 3 Minutes

**Steps:**

On the Hosts page, I click **`comp-a-test`** to open its details and compare **Last Check Time** with **Next Scheduled Active Check**.

<img width="969" height="641" alt="2" src="https://github.com/user-attachments/assets/506db206-958f-417e-a8dd-5d027ea07f39" />

**Actual:**

| Field | Value |
|---|---|
| Last Check Time | **10-25-2026 15:02:38** |
| Next Scheduled Active Check | **10-25-2026 15:05:38** |

**Result:** ✅ PASS. The checks are exactly **3 minutes** apart, matching `check_interval 3` in `windows-ncpa-host`. Under `windows-server`, they would be 5 minutes apart.

---

## TC-03 – Max Check Attempts Is 3

**Steps:**

On the same details page, I look at **Current Attempt**.

<img width="969" height="641" alt="3" src="https://github.com/user-attachments/assets/506db206-958f-417e-a8dd-5d027ea07f39" />

**Result:** ✅ PASS. The number after the slash is the max check attempts, now **3**. Under `windows-server` it was 10. My earlier troubleshooting screenshots of `comp-a-test` show `10/10 (HARD state)`, so this confirms the change took effect.

---

## TC-04 – Still a Member of windows-servers

**Steps:**

On the same details page, I look at **Member of**.

**Actual:** `windows-servers`

**Result:** ✅ PASS. **`comp-a-test`** gets **`windows-servers`** through the chain **`windows-ncpa-host`** → **`windows-server`**, which proves the inheritance works.

---

## Before and After

| | Before | After |
|---|---|---|
| Template | **`windows-server`** | **`windows-ncpa-host`** |



























