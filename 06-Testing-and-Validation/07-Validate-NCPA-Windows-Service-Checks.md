# 07 – Validate NCPA Windows Service Checks

In this doc, I will verify that the **`NCPA Service Status`** check for **`comp-a-test`** is working. I'll confirm it in Nagios, compare it with the Windows Services app, and check that NCPA is receiving the checks from the Nagios server.

> **How this check was created:** [**Click here to see Monitor Windows Services with NCPA**](../04-Monitoring-Use-Cases/01-Windows-Host/08-Monitor-Windows-Services-with-NCPA.md)

---

## Test Cases

| ID | Test | Expected | Actual | Status |
|---|---|---|---|---|
| TC-01 | Service appears with a real result | **OK** – NCPA is running | OK: NCPA is running | ✅ PASS |
| TC-02 | `notify-early` settings | 1-min interval, attempt 1/1 | 13:54:01 → 13:55:01, 1/1 (HARD) | ✅ PASS |
| TC-03 | Matches Windows Services | Nagios Cross-Platform Agent is Running | Running, Automatic (Delayed Start) | ✅ PASS |
| TC-04 | NCPA receives the checks from Nagios | `services` entry from `192.168.1.99` | OK: NCPA is running, from `192.168.1.99`, Active | ✅ PASS |

---

## TC-01 – Service Appears with a Real Result

**Steps:** I log in to **`http://192.168.1.99/nagios`** and click **Services**.

<img width="1020" height="616" alt="1" src="https://github.com/user-attachments/assets/927b2347-fbcc-4378-a361-45a9565f7a8e" />

Service totals: **12 OK, 1 CRITICAL**, 13 total.

**Result:** ✅ PASS

> The CRITICAL is `Logical memory usage` at 95.30%, covered in [**05 – Validate NCPA Memory and Swap Checks**](05-Validate-NCPA-Memory-and-Swap-Checks.md).

---

## TC-02 – notify-early Settings Are Applied

**Steps:** I click **NCPA Service Status**.

<img width="1003" height="648" alt="2" src="https://github.com/user-attachments/assets/eef0bf66-98fe-4825-9dbe-172e194c2849" />

**Result:** ✅ PASS. Checks are **1 minute** apart and the attempt count is out of **1**, both from `notify-early`. My other `comp-a-test` checks show x/5 (`time-sensitive`) or x/3 (`generic-service`), so the template is clearly applied.

> **Why is Performance Data empty?** The CPU, memory, and disk checks return a number Nagios can graph. A service check only returns running or not running, so there's no number to report.

---

## TC-03 – Matches Windows Services

**Steps:** On **`comp-a-test`**, I search for **Services** in the Start menu and open it, then scroll to **Nagios Cross-Platform Agent**.

<img width="856" height="274" alt="3" src="https://github.com/user-attachments/assets/0f3505e8-37e5-4067-94c1-7eaad7c08aeb" />

<img width="950" height="297" alt="S4" src="https://github.com/user-attachments/assets/14b69287-dc0e-41b4-ac7e-896cc11b7097" />






