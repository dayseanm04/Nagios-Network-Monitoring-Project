# 09 – Validate NCPA Network Interface Checks

In this doc, I will verify that the four network interface checks on **`comp-a-test`**'s **`vEthernet (Net-MGNT-SW)`** adapter are working. I'll confirm them in Nagios and check that NCPA is receiving the checks from the Nagios server.

> **How these checks were created:** [**Click here to see Monitor Network Interface Errors and Drops with NCPA**](../04-Monitoring-Use-Cases/01-Windows-Host/10-Monitor-Network-Interface-Errors-and-Drops-with-NCPA.md)

---

## Test Cases

| ID | Test | Expected | Actual | Status |
|---|---|---|---|---|
| TC-01 | All four services appear with real results | Per-second values, OK | All OK, 0.00 per second | ✅ PASS |
