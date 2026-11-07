# 12 – Validate Verizon-R1 DNS Resolution Checks

In this doc, I will verify the two DNS checks on **`verizon-r1`**. I'll confirm the plugin and command are in place, that the router resolves **`google.com`** within my thresholds, and that it resolves **`comp-a-test`** to the right IP.

> **How these checks were created:** [**Click here to see Add DNS Resolution Checks for Verizon-R1**](../04-Monitoring-Use-Cases/02-Router/03-Add-DNS-Resolution-Checks-for-Verizon-R1.md)

---

## Test Cases

| ID | Test | Expected | Actual | Status |
|---|---|---|---|---|
| TC-01 | Plugin is installed | **`check_dns`** is in **`libexec`** and executable | **`-rwxr-xr-x ... check_dns`** | ✅ PASS |
| TC-02 | Config passes the pre-flight check | 0 warnings, 0 errors after each change | 19 → 20 → 21 services, 0 warnings, 0 errors | ✅ PASS |
| TC-03 | Router resolves google.com | **OK** with an IP address | **`DNS OK: 0.066 seconds response time. google.com returns 142.251.40.238`** | ✅ PASS |
| TC-04 | Thresholds are applied | Perf data shows **`0.5`** / **`1`** seconds | **`time=0.065836s;0.500000;1.000000;0.000000`** | ✅ PASS |
| TC-05 | `network-device-service` settings | 1-min interval, attempt out of 2 | 21:16:00 → 21:17:00, 1/2 (HARD) | ✅ PASS |
| TC-06 | Router resolves comp-a-test to 192.168.1.9 | **OK** | **CRITICAL**: **`DNS 192.168.1.1 has no records`** | ❌ FAIL |

---

## TC-01 – Plugin Is Installed

**Steps:**

```bash
cd /usr/local/nagios/libexec
ls -l | grep dns
```

<img width="1031" height="376" alt="1" src="https://github.com/user-attachments/assets/aaac48c9-d00a-47b6-b44b-08117aba930d" />

**Result:** ✅ PASS. **`check_dns`** is there, owned by **`nagios`**, and executable.

---

## TC-02 – Config Passes the Pre-Flight Check

**Steps:** I run the pre-flight check after each change:







