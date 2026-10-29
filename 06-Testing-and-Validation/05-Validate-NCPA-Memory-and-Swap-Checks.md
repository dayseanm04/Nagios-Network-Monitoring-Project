# 05 – Validate NCPA Memory and Swap Checks

In this doc, I will verify that the **`Logical memory usage`** and **`Swap memory usage`** checks for **`comp-a-test`** are working. During testing, RAM was really above 90%, so this test also proves the CRITICAL threshold triggers on a real problem.

>  **How these checks were created:** [**Click here to see Monitor Windows Memory and Swap Usage with NCPA**](../04-Monitoring-Use-Cases/01-Windows-Host/06-Monitor-Windows-Memory-and-Swap-Usage-with-NCPA.md)

---

## Test Cases

| ID | Test | Expected | Actual | Status |
|---|---|---|---|---|
| TC-01 | Both services appear with real results | Values for RAM and swap | RAM 95.70%, swap 16.20% | ✅ PASS |
| TC-02 | Thresholds are applied | Perf data shows `70;90` and `50;80` | `'percent'=95.30%;70;90;` and `'percent'=16.00%;50;80;` | ✅ PASS |
| TC-03 | RAM above 90% goes CRITICAL | **CRITICAL** | CRITICAL – Percent was 95.30 % | ✅ PASS |
| TC-04 | Swap below 50% stays OK | **OK** | OK – Percent was 16.00 % | ✅ PASS |
| TC-05 | `time-sensitive` settings | 2-min interval, attempts out of 5 | 19:33:01 → 19:35:01, 5/5 and 1/5 | ✅ PASS |
| TC-06 | Notes shown in Nagios | Thresholds on the service page | Shown on both services | ✅ PASS |
| TC-07 | Matches Task Manager | Same range as Windows reports | Task Manager 94%, Nagios 95.30% | ✅ PASS |
| TC-08 | NCPA receives the checks from Nagios | Entries executed by `192.168.1.99` | Memory checks logged from `192.168.1.99`, Active | ✅ PASS |

---

## TC-01 – Both Services Appear with Real Results

**Steps:** I log in to `http://192.168.1.99/nagios` and click **Services**.

<img width="957" height="481" alt="1" src="https://github.com/user-attachments/assets/a0b8690a-8469-499c-9cf8-f267aa29cde3" />

Service totals: **11 OK, 1 CRITICAL**, 12 total.

**Result:** ✅ PASS. Both memory checks are running and returning real values from NCPA.

**Note:** The total is 12, not 11, because I configured the **`C:\ Drive Space utilization`** check after this doc's config steps. That check is covered in its own doc.

---

## TC-02 – Thresholds Are Applied

**Steps:** I click each memory service and check **Performance Data**.

### Logical memory check:

<img width="951" height="599" alt="2" src="https://github.com/user-attachments/assets/e1a0817a-632a-406a-82f3-c4d5d6f46294" />

### Swap memory check:

<img width="956" height="598" alt="3" src="https://github.com/user-attachments/assets/d5a952dc-bd08-4fc6-aed8-a41d8fbd9bb6" />

| Service | Performance Data | Thresholds |
|---|---|---|
| **C:\ Drive Space utilization** | OK | 1/5 | OK: Used_percent was 67.00 % |
| **CPU Utilization** | OK | 1/5 | OK: Percent was 12.03 % |
| **Logical memory usage** | **CRITICAL** | 5/5 | CRITICAL: Percent was 95.70 % |
| **Swap memory usage** | OK | 1/5 | OK: Percent was 16.20 % |
| **system uptime** | OK | 1/3 | OK: Uptime was 3 hours 38 minutes 9 seconds |

Service totals: **11 OK, 1 CRITICAL**, 12 total.

**Result:** ✅ PASS. Both memory checks are running and returning real values from NCPA.

**Note:** The total is 12, not 11, because I added the **`C:\ Drive Space utilization`** check after this doc's config steps. That check is covered in its own doc.

---

## TC-02 – Thresholds Are Applied

**Steps:** I click each service and check **Performance Data**.

### Logical memory check:

<img width="951" height="599" alt="4" src="https://github.com/user-attachments/assets/e1a0817a-632a-406a-82f3-c4d5d6f46294" />

### Swap memory check:

<img width="956" height="598" alt="5" src="https://github.com/user-attachments/assets/d5a952dc-bd08-4fc6-aed8-a41d8fbd9bb6" />

| Service | Performance Data | Thresholds |
|---|---|---|
| Logical memory usage | `'percent'=95.30%;70;90;` | Warning 70, Critical 90 |
| Swap memory usage | `'percent'=16.00%;50;80;` | Warning 50, Critical 80 |

**Result:** ✅ PASS. The thresholds in the performance data match each service's `check_command`.

---

## TC-03 – RAM Above 90% Goes CRITICAL

I click **Logical memory usage**.

<img width="951" height="599" alt="6" src="https://github.com/user-attachments/assets/e1a0817a-632a-406a-82f3-c4d5d6f46294" />

95.30% is above the 90% critical threshold, so **CRITICAL** is correct.

**Result:** ✅ PASS

The service has been CRITICAL since October 26 and has triggered 6 notifications. With **`notification_interval 15`** in **`time-sensitive`**, Nagios keeps re-sending the alert until the problem is fixed.

---

## TC-04 – Swap Below 50% Stays OK

I click **Swap memory usage**.

<img width="956" height="598" alt="5" src="https://github.com/user-attachments/assets/d5a952dc-bd08-4fc6-aed8-a41d8fbd9bb6" />

16% is below the 50% warning threshold, so **OK** is correct.

**Result:** ✅ PASS

This also shows why I raised the swap thresholds. Swap normally runs around **15–40%** on this machine. With my first thresholds **(10/30)**, this check would have been in **WARNING** most of the time, even though nothing was wrong.

---

## TC-05 – time-sensitive Settings Are Applied

### Logical memory usage:

<img width="948" height="596" alt="6" src="https://github.com/user-attachments/assets/0bee03d5-91ac-48a1-97a1-592baa09f528" />


| Service | Last Check | Next Scheduled Check | Current Attempt |
|---|---|---|---|
| **Logical memory usage** | 19:33:01 | 19:35:01 | 5/5 |
| **Swap memory usage** | 19:33:29 | 19:35:29 | 1/5 |


