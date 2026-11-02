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

<img width="1030" height="550" alt="1" src="https://github.com/user-attachments/assets/bdf18649-2ee0-4872-aede-6074a27d91aa" />

<img width="1027" height="707" alt="2" src="https://github.com/user-attachments/assets/389a4eef-9ed0-4532-8a03-b7b504eb97be" />

**Result:** ✅ PASS

> Two other services are CRITICAL on this screenshot: **`Logical memory usage`** (94.90%) and **`Failed System logs`** (16 logs). Neither is related to this test.

---

## TC-02 – Thresholds Are Applied

**Steps:** I click each service and check **Performance Data**.

<img width="1009" height="683" alt="3" src="https://github.com/user-attachments/assets/0756ee4b-f8e5-477f-803a-afe69e15eb7a" />

<img width="1033" height="767" alt="4" src="https://github.com/user-attachments/assets/0a4367dd-daf7-4a1f-911e-9c9df7d1ee0c" />


| Service | Performance Data | Thresholds |
|---|---|---|


