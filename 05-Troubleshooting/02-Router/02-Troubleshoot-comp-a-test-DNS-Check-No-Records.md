# 02 – Troubleshoot comp-a-test DNS Check (No Records)

Troubleshooting record for the **`DNS Resolution - comp-a-test`** service on **`verizon-r1`** going **CRITICAL** with **`DNS 192.168.1.1 has no records`**. Covers how the problem was found, the steps taken to isolate it, the root cause, and the fix.

> **Where this came up:** [**04-Monitoring-Use-Cases / 02-Router / 03 – Add DNS Resolution Checks for Verizon-R1**](../../04-Monitoring-Use-Cases/02-Router/03-Add-DNS-Resolution-Checks-for-Verizon-R1.md)

**Note on screenshots:** The Nagios screenshots here were taken before I fixed the typos in the service name and notes, so they show **`DNS Resolutioon - comp-a-test`** and **`comp-a-tes`**. See [**Typo Fixes**](../../04-Monitoring-Use-Cases/02-Router/03-Add-DNS-Resolution-Checks-for-Verizon-R1.md#typo-fixes).

---

## Problem Description

After adding the **`DNS Resolution - comp-a-test`** service to **`verizon-r1`**, it went **CRITICAL**. The **`DNS Resolution google.com`** service on the same router was **OK** at the same time.
