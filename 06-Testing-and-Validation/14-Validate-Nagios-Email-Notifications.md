# 14 – Validate Nagios Email Notifications

In this doc, I will verify that Nagios sends alert emails to my inbox, first with a custom notification I trigger by hand, and then with real alerts from a real problem.

> **How notifications were set up:** [**Click here to see Configure Nagios Email Notifications**](../02-Setup/13-Configure-Nagios-Email-Notifications.md)

---

## Test Cases

| ID | Test | Expected | Actual | Status |
|---|---|---|---|---|
| TC-01 | Custom notification | **CUSTOM** email arrives | **`CUSTOM Service Alert: Computer A/CPU Utilization is OK`** | ✅ PASS |
| TC-02 | Real CRITICAL alert | **PROBLEM** email when memory goes over 90% | **`Logical memory usage is CRITICAL`**, 94.50% | ✅ PASS |
| TC-03 | State change to WARNING | New email when memory drops to 70–90% | **`Logical memory usage is WARNING`**, 80.90% | ✅ PASS |
| TC-04 | Repeat notifications | Email repeats about every 15 min while the problem lasts | CRITICAL emails at 18:12, 18:28, 18:44 | ✅ PASS |
| TC-05 | RECOVERY alert | **RECOVERY** email when a service returns to OK | **`RECOVERY Service Alert: Computer A/system uptime is OK`** | ✅ PASS |

---
