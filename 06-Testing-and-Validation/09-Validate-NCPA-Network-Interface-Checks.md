# 09 – Validate NCPA Network Interface Checks

In this doc, I will verify that the four network interface checks on **`comp-a-test`**'s **`vEthernet (Net-MGNT-SW)`** adapter are working. I'll confirm them in Nagios and check that NCPA is receiving the checks from the Nagios server.

> **How these checks were created:** [**Click here to see Monitor Network Interface Errors and Drops with NCPA**](../04-Monitoring-Use-Cases/01-Windows-Host/10-Monitor-Network-Interface-Errors-and-Drops-with-NCPA.md)

---

## Test Cases

| ID | Test | Expected | Actual | Status |
|---|---|---|---|---|
| TC-01 | All four services appear with real results | Per-second values, OK | All OK, 0.00 per second | ✅ PASS |
| TC-02 | Thresholds are applied | Perf data shows `0;1` and `1;5` | Matches on all four | ✅ PASS |
| TC-03 | Delta is working | Values are rates (per second), not running totals | 0.00 errors/s and packets/s | ✅ PASS |
| TC-04 | `notify-early` settings | 1-min interval, attempt 1/1 | 1 min apart, 1/1 (HARD) | ✅ PASS |
| TC-05 | Notes shown in Nagios | Thresholds on each service page | Shown on all four | ✅ PASS |
| TC-06 | NCPA receives the checks from Nagios | Entries from `192.168.1.99` | errin, errout, dropout logged from **`192.168.1.99`** | ✅ PASS |

---

## TC-01 – All Four Services Appear with Real Results

**Steps:** I log in to **`http://192.168.1.99/nagios`** and click **Services**.
