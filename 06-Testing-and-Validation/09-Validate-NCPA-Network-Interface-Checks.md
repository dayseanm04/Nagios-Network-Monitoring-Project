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

<img width="1023" height="770" alt="5" src="https://github.com/user-attachments/assets/3cf509ee-5b13-43bf-a57c-315673064e5c" />

<img width="1029" height="768" alt="6" src="https://github.com/user-attachments/assets/d1a68f2d-fa41-486a-9c7e-07da6f156719" />



| Service | Performance Data | Thresholds |
|---|---|---|
| Errors In | `'errin'=0.00;0;1;` | Warning 0, Critical 1 |
| Errors Out | `'errout'=0.00;0;1;` | Warning 0, Critical 1 |
| Drops In | `'dropin'=0.00;1;5;` | Warning 1, Critical 5 |
| Drops Out | `'dropout'=0.00;1;5;` | Warning 1, Critical 5 |

**Result:** ✅ PASS

---

## TC-03 – Delta Is Working

Without Delta, NCPA would return running totals since the adapter came up. Windows showed the adapter had sent and received hundreds of thousands of packets, so a total would be a large and growing number.

**Actual:** Every check reports a **rate**: **`errors/s`** and **`packets/s`**, with a value of `0.00`.

**Result:** ✅ PASS. The checks measure what's happening now, not since the last reboot.

---

## TC-04 – notify-early Settings Are Applied

| Service | Last Check | Next Check | Current Attempt |
|---|---|---|---|
| Drops In | 14:25:51 | 14:26:51 | 1/1 (HARD) |
| Errors In | 14:26:16 | 14:27:16 | 1/1 (HARD) |
| Errors Out | 14:27:02 | 14:28:02 | 1/1 (HARD) |
| Drops Out | 14:28:03 | 14:29:03 | 1/1 (HARD) |

**Result:** ✅ PASS. All four check every **1 minute** with **1** attempt, from **`notify-early`**.

---

## TC-05 – Notes Are Shown in Nagios

| Service | Notes |
|---|---|
















