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
