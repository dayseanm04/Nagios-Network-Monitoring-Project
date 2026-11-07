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
