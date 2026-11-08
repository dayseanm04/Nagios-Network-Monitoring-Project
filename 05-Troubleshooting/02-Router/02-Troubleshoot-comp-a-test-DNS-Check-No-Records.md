# 02 – Troubleshoot comp-a-test DNS Check (No Records)

Troubleshooting record for the **`DNS Resolution - comp-a-test`** service on **`verizon-r1`** going **CRITICAL** with **`DNS 192.168.1.1 has no records`**. Covers how the problem was found, the steps taken to isolate it, the root cause, and the fix.

> **Where this came up:** [**04-Monitoring-Use-Cases / 02-Router / 03 – Add DNS Resolution Checks for Verizon-R1**](../../04-Monitoring-Use-Cases/02-Router/03-Add-DNS-Resolution-Checks-for-Verizon-R1.md)

**Note on screenshots:** The Nagios screenshots here were taken before I fixed the typos in the service name and notes, so they show **`DNS Resolutioon - comp-a-test`** and **`comp-a-tes`**. See [**Typo Fixes**](../../04-Monitoring-Use-Cases/02-Router/03-Add-DNS-Resolution-Checks-for-Verizon-R1.md#typo-fixes).

---

## Problem Description

After adding the **`DNS Resolution - comp-a-test`** service to **`verizon-r1`**, it went **CRITICAL**. The **`DNS Resolution google.com`** service on the same router was **OK** at the same time.

<img width="1013" height="618" alt="1" src="https://github.com/user-attachments/assets/de023cac-4cb3-41fd-a18c-1534b7d6a45d" />

<img width="1020" height="807" alt="2" src="https://github.com/user-attachments/assets/a9d3a576-462f-4acc-ad43-286bf4bc11df" />

<img width="940" height="679" alt="3" src="https://github.com/user-attachments/assets/29ed1bf3-d91e-4e35-9f03-e8f755039475" />
