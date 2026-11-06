# 02 – Troubleshoot Windows Host Down and NCPA Services Unknown

Troubleshooting record for the Windows host **`comp-a-test`** going **DOWN** in Nagios, with all 11 of its NCPA services showing **UNKNOWN**. Covers how the problem was found, the steps taken to isolate it, the root cause, and the fix.

> This is the second time **`comp-a-test`** went DOWN because its IP address changed. The first time is covered in [**01 – Troubleshoot Windows Host Down**](01-Troubleshoot-Windows-Host-Down.md).

---

## Problem Description

When I logged in to Nagios, **`comp-a-test`** was **DOWN** and had been for almost 8 hours. Every service on it was **UNKNOWN**. The other two hosts, **`localhost`** and **`verizon-r1`**, were **UP**.


































