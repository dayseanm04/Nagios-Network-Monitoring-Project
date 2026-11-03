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
