# 04 – Validate NCPA CPU Utilization Check

In this doc, I will verify that the new **`CPU Utilization`** check for **`comp-a-test`** is running, returning real CPU data, and using the settings from my `time-sensitive` template.

 **How this check was created:** [**Click here to see Monitor Windows CPU Utilization with NCPA**](../04-Monitoring-Use-Cases/01-Windows-Host/05-Monitor-Windows-CPU-Utilization-with-NCPA.md)

---

## Test Cases

| ID | Test | Expected | Actual | Status |
|---|---|---|---|---|
| TC-01 | Service appears with a real result | **OK** with a CPU percentage | OK – Percent was 22.43 % | ✅ PASS |
| TC-02 | Thresholds are applied | Performance data shows `70;85` | `'percent'=22.43%;70;85;` | ✅ PASS |

---

## TC-01 – Service Appears with a Real Result

**Steps:**

1. I log in to **`http://192.168.1.99/nagios`**.
2. I click **Services**.

<img width="1032" height="547" alt="1" src="https://github.com/user-attachments/assets/b112dc93-cc66-4abc-8bef-a6a5248e1500" />

**Result:** ✅ PASS. The check is running and returning a real CPU value from NCPA.

---

## TC-02 – Thresholds Are Applied

**Steps:**

I click **CPU Utilization** to open the service details and look at **Performance Data**.


<img width="1028" height="768" alt="2" src="https://github.com/user-attachments/assets/f34eed69-5a58-495d-8831-8c4e1c10e50b" />
