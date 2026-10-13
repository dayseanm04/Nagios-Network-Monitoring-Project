# 02 – Validate NCPA Uptime Check

In this doc, I will verify that the **`system uptime`** service check for **`comp-a-test`** works end to end. I'll test it in two phases:

1. **Without thresholds** – confirm Nagios can reach NCPA and get a result.
2. **With thresholds** – confirm the check goes WARNING and CRITICAL after a reboot.

I'll confirm results from both sides: in Nagios, where the check is scheduled, and in NCPA, where the check is answered.

> **How this check was created:** [**Click here to see Monitor Windows Uptime with NCPA**](../04-Monitoring-Use-Cases/01-Windows-Host/03-Monitor-Windows-Uptime-with-NCPA.md)

---

## Test Cases

| ID | Phase | Test | Expected | Actual | Status |
|---|---|---|---|---|---|
| TC-01 | No thresholds | Service in Nagios | **OK** with uptime value | OK – Uptime was 3 hours 56 minutes 33 seconds | PASS |
| TC-02 | No thresholds | Check source in NCPA | Executed By **`192.168.1.99`** | **`192.168.1.99`**, Active | PASS |
| TC-03 | No thresholds | Checks run on a schedule | Repeated entries, uptime increasing | Entries from 17:43 to 17:50 | PASS |
