# 12 – Validate Verizon-R1 DNS Resolution Checks

In this doc, I will verify the two DNS checks on **`verizon-r1`**. I'll confirm the plugin and command are in place, that the router resolves **`google.com`** within my thresholds, and that it resolves **`comp-a-test`** to the right IP.

> **How these checks were created:** [**Click here to see Add DNS Resolution Checks for Verizon-R1**](../04-Monitoring-Use-Cases/02-Router/03-Add-DNS-Resolution-Checks-for-Verizon-R1.md)

---

## Test Cases

| ID | Test | Expected | Actual | Status |
|---|---|---|---|---|
| TC-01 | Plugin is installed | **`check_dns`** is in **`libexec`** and executable | **`-rwxr-xr-x ... check_dns`** | ✅ PASS |
| TC-02 | Config passes the pre-flight check | 0 warnings, 0 errors after each change | 19 → 20 → 21 services, 0 warnings, 0 errors | ✅ PASS |
