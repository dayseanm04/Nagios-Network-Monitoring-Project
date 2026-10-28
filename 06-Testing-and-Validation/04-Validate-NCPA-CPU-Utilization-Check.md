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
