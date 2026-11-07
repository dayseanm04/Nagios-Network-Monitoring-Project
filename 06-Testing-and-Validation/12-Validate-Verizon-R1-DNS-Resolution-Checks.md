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

```bash
sudo /usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg
```

Pre-flight check after adding the **`check_dns_server`** command

<img width="1028" height="790" alt="2" src="https://github.com/user-attachments/assets/83249235-aa6c-48a5-92b2-0e3cece2967d" />

Pre-flight check after adding the **google.com** service

<img width="1017" height="852" alt="3" src="https://github.com/user-attachments/assets/2a89e28c-1360-496a-bc18-5ea577275b3d" />

Pre-flight check after adding the **comp-a-test** service

<img width="973" height="738" alt="4" src="https://github.com/user-attachments/assets/8b0c7416-f2e5-403c-9d8b-e29e7578eecd" />

| Change | Services | Warnings | Errors |
|---|---|---|---|
| Added **`check_dns_server`** command | 19 | 0 | 0 |
| Added **`DNS Resolution google.com`** | 20 | 0 | 0 |
| Added **`DNS Resolution - comp-a-test`** | 21 | 0 | 0 |

**Result:** ✅ PASS. Each change passes, and the service count goes up by one for each new service.

> Passing the pre-flight check means the config is **valid**, not that the check is **right**. TC-06 shows the difference: the comp-a-test service passes here but fails in the web interface.

---

## TC-03 – Router Resolves google.com

**Steps:** I log in to **`http://192.168.1.99/nagios`**, click **Services**, scroll down to **`verizon-r1`**, and click **DNS Resolution google.com**.

<img width="1013" height="618" alt="5" src="https://github.com/user-attachments/assets/c3d7b334-6fca-4dd3-a890-e8de098674f9" />

<img width="976" height="722" alt="6" src="https://github.com/user-attachments/assets/e9b929b0-f23f-42bd-9582-dc8aa4f6c4d6" />

<img width="1030" height="766" alt="7" src="https://github.com/user-attachments/assets/9365e9b4-3281-4903-b854-f8c774263777" />













