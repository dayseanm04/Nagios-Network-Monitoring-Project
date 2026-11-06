# 02 – Troubleshoot Windows Host Down and NCPA Services Unknown

Troubleshooting record for the Windows host **`comp-a-test`** going **DOWN** in Nagios, with all 11 of its NCPA services showing **UNKNOWN**. Covers how the problem was found, the steps taken to isolate it, the root cause, and the fix.

> This is the second time **`comp-a-test`** went DOWN because its IP address changed. The first time is covered in [**01 – Troubleshoot Windows Host Down**](01-Troubleshoot-Windows-Host-Down.md).

---

## Problem Description

When I logged in to Nagios, **`comp-a-test`** was **DOWN** and had been for almost 8 hours. Every service on it was **UNKNOWN**. The other two hosts, **`localhost`** and **`verizon-r1`**, were **UP**.

<img width="864" height="507" alt="1" src="https://github.com/user-attachments/assets/e9be25a7-bbc0-4686-b53b-9a0f350d71b9" />

<img width="934" height="573" alt="2" src="https://github.com/user-attachments/assets/d9cb0587-448a-4c4e-b2fd-1694cceabcbc" />

<img width="1026" height="577" alt="3" src="https://github.com/user-attachments/assets/25c570ff-cc2a-47d5-9855-a46c1d374671" />

Host Status Totals: **2 Up, 1 Down**. Service Status Totals: **8 OK, 11 Unknown**.

---

## Symptoms

### Host details

<img width="1022" height="728" alt="4" src="https://github.com/user-attachments/assets/50cd4f0a-26b9-40cb-a670-afb34c1a659d" />

### Services

<img width="1029" height="788" alt="5" src="https://github.com/user-attachments/assets/9cf5ba4c-d3f7-4737-9b0f-0c19fcf275ed" />

All 11 services on **`comp-a-test`** showed the same error:

```
UNKNOWN: An error occurred connecting to API. (Connection error: '[Errno 113] No route to host')
```

<img width="1048" height="735" alt="6" src="https://github.com/user-attachments/assets/46cebdac-ad7a-409f-a1aa-8c26a61f449a" />

**Why UNKNOWN and not CRITICAL?** UNKNOWN means the check couldn't run at all, so Nagios has no idea what the real value is. **`check_ncpa.py`** couldn't reach NCPA, so it couldn't get a CPU, memory, or disk reading to compare against the thresholds.

**`Errno 113 – No route to host`** is a network-level error. It means the Nagios server couldn't find any device at **`192.168.1.9`**. That pointed at the network, not at NCPA itself.

---








































