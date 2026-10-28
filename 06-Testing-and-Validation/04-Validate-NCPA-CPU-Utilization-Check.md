# 04 – Validate NCPA CPU Utilization Check

In this doc, I will verify that the new **`CPU Utilization`** check for **`comp-a-test`** is running, returning real CPU data, and using the settings from my `time-sensitive` template.

 **How this check was created:** [**Click here to see Monitor Windows CPU Utilization with NCPA**](../04-Monitoring-Use-Cases/01-Windows-Host/05-Monitor-Windows-CPU-Utilization-with-NCPA.md)

---

## Test Cases

| ID | Test | Expected | Actual | Status |
|---|---|---|---|---|
| TC-01 | Service appears with a real result | **OK** with a CPU percentage | OK – Percent was 22.43 % | ✅ PASS |

---

## TC-01 – Service Appears with a Real Result

**Steps:**

1. I log in to **`http://192.168.1.99/nagios`**.
2. I click **Services**.

<img width="1032" height="547" alt="1" src="https://github.com/user-attachments/assets/b112dc93-cc66-4abc-8bef-a6a5248e1500" />

**Result:** ✅ PASS. The check is running and returning a real CPU value from NCPA.
