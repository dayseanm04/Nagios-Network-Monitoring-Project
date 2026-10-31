# 08 – Validate NCPA System Event Log Check

In this doc, I will verify that the **`Failed System logs`** check for **`comp-a-test`** is working. I'll confirm it in Nagios, look at the log entries it found, and check that NCPA is receiving the checks from the Nagios server.

> **How this check was created:** [**Click here to see Monitor Windows System Event Logs with NCPA**](../04-Monitoring-Use-Cases/01-Windows-Host/09-Monitor-Windows-System-Event-Logs-with-NCPA.md)

---

## Test Cases

| ID | Test | Expected | Actual | Status |
|---|---|---|---|---|
| TC-01 | Service appears with a real result | A count of matching log entries | OK – System has 3 logs | ✅ PASS |
