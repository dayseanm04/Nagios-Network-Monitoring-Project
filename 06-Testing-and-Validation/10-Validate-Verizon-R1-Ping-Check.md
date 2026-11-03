# 11 – Validate Verizon-R1 Ping Check

In this doc, I will verify that the PING service check for `verizon-r1` is working. I'll confirm the router answers pings, that the thresholds are applied, and that the check uses my `network-device-service` template.

> **How this check was created:** [**Click here to see Add Verizon Router and Ping Check**](../04-Monitoring-Use-Cases/02-Router/02-Add-Verizon-Router-and-Ping-Check.md)

---

## Test Summary

| Item | Details |
|---|---|
| Date | November 1, 2026 |
| Host under test | **`verizon-r1`** (**`192.168.1.1`**) |
| Router model | Verizon CR1000B |
| Service under test | `PING` |

---

## Test Cases

| ID | Test | Expected | Actual | Status |
|---|---|---|---|---|
| TC-01 | Service appears with a real result | **OK** with packet loss and response time | PING OK - Packet loss = 0%, RTA = 3.08 ms | PASS |
| TC-02 | Thresholds are applied | Perf data shows `200` / `600` ms and `20` / `60`% | `rta=5.374000ms;200.000000;600.000000;0.000000 pl=0%;20;60;0` | ✅ PASS |
| TC-03 | `network-device-service` settings | 1-min interval, attempt out of 2 | 16:52:02 → 16:53:02, 1/2 (HARD) | ✅ PASS |

---

## TC-01 – Service Appears with a Real Result

**Steps:** I log in to **`http://192.168.1.99/nagios`**, click **Services**, and scroll down to **`verizon-r1`**.

<img width="1036" height="566" alt="1" src="https://github.com/user-attachments/assets/9037918b-702b-4d34-8c1d-f719c7f9178e" />

<img width="1035" height="798" alt="2" src="https://github.com/user-attachments/assets/1ae73af0-68cf-42a7-a288-df99e0e4a8a9" />

**Result:** ✅ PASS. The router answers pings with no loss and a response time of about 3 ms.

> For comparison, **`comp-a-test`** answers in about 0.7 ms and **`localhost`** in under 0.1 ms. The router is a few milliseconds slower because the traffic goes out over Wi-Fi, while the other two are on the same machine as Nagios.

---

## TC-02 – Thresholds Are Applied

**Steps:** I click **PING** to open the service details.

<img width="968" height="648" alt="3" src="https://github.com/user-attachments/assets/f2c6935c-66ea-4768-9c7e-16c5a2fc0325" />

| Part | Meaning |
|---|---|
| **`rta=5.374000ms;200.000000;600.000000`** | Round-trip time of 5.37 ms, with warning at 200 ms and critical at 600 ms |
| **`pl=0%;20;60`** | Packet loss of 0%, with warning at 20% and critical at 60% |

**Result:** ✅ PASS. Both thresholds from **`check_command`** are in use. At 5.37 ms and 0% loss, the router is far below both.

---

## TC-03 – network-device-service Settings Are Applied

From the same details page:

<img width="968" height="648" alt="4" src="https://github.com/user-attachments/assets/b01e3fc9-dc8c-45c1-9056-c0710aa1dc5d" />

**Result:** ✅ PASS. Checks are **1 minute** apart and the attempt count is out of **2**, both from **`network-device-service`**. The sample check used a 5-minute interval, so this also confirms removing **`check_interval`** and **`retry_interval`** from the service worked.

---

## Conclusion

The PING service for **`verizon-r1`** works. The router answers with 0% packet loss and a 3–5 ms response time. Nagios applies the 200 ms / 20% warning and 600 ms / 60% critical thresholds, checks every minute with 2 attempts from `network-device-service`, and the check is stable with no flapping.

---







