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

### DNS Resolution google.com` OK in the services list

<img width="976" height="722" alt="6" src="https://github.com/user-attachments/assets/e9b929b0-f23f-42bd-9582-dc8aa4f6c4d6" />

### Service details page

<img width="1030" height="766" alt="7" src="https://github.com/user-attachments/assets/9365e9b4-3281-4903-b854-f8c774263777" />

On a later check (21:30), it returned a different IP, **`142.251.41.174`**, in **0.026 seconds**. That's normal for Google, which answers from many addresses, and it's why this check doesn't use **`-a`** to expect a specific IP. see **TC-06**

---

## TC-04 – Thresholds Are Applied

From the same details page:

| Part | Meaning |
|---|---|
| **`time=0.065836s`** | The lookup took about 0.066 seconds |
| **`0.500000`** | Warning threshold: 0.5 seconds (from **`-w 0.5`**) |
| **`1.000000`** | Critical threshold: 1 second (from **`-c 1`**) |

**Result:** ✅ PASS. Both thresholds from **`check_command`** are in use. At 0.066 seconds, the lookup is far under the warning line.

---

## TC-05 – network-device-service Settings Are Applied

From the same details page:

| Field | Value |
|---|---|
| Last Check Time | 11-06-2026 21:16:00 |
| Next Scheduled Check | 11-06-2026 21:17:00 |
| Current Attempt | 1/2 (HARD state) |

**Result:** ✅ PASS. Checks are **1 minute** apart and attempts are out of **2**, both from **`network-device-service`**.

---

## TC-06 – Router Resolves comp-a-test to 192.168.1.9

**Steps:** On the **Services** page, I scroll down to **`verizon-r1`** and click **DNS Resolution - comp-a-test**.

**`DNS Resolution - comp-a-test`** CRITICAL in the services list

<img width="1020" height="807" alt="8" src="https://github.com/user-attachments/assets/121c1fcb-f0f6-4f27-a042-5ab9e4761e37" />

Service details page

<img width="940" height="679" alt="Screenshot 2026-11-06 213201" src="https://github.com/user-attachments/assets/1bc7c4f9-63de-4fd3-8d15-5341e9153879" />

| Field | Value |
|---|---|
| Current Status | **CRITICAL** |
| Status Information | **`DNS 192.168.1.1 has no records`** |
| Performance Data | (empty) |
| Current Attempt | 2/2 (HARD state) |
| Last State Change | 11-06-2026 21:27:38 |

**Result:** ❌ FAIL. The router answered, since the output names **`192.168.1.1`** as the server, but it had no record for **`comp-a-test`**. The performance data is empty because there was no successful lookup to time.

The google.com check on the same router was **OK** at the same time, so the router's DNS service is working. The problem is specific to the **`comp-a-test`** lookup.

> **Troubleshooting:** [**Click here to see how I fixed the comp-a-test DNS check**](../05-Troubleshooting/02-Router/02-Troubleshoot-comp-a-test-DNS-Check-No-Records.md)

---

## Conclusion

The **`check_dns_server`** command works, and the **`DNS Resolution google.com`** check is working as intended. The router resolves internet names in under 0.1 seconds, Nagios applies the 0.5-second warning and 1-second critical thresholds, and it checks every minute with 2 attempts from **`network-device-service`**.

The **`DNS Resolution - comp-a-test`** check fails with **`DNS 192.168.1.1 has no records`**. It will be retested after troubleshooting.

---

## Related Docs

- [**04-Monitoring-Use-Cases / 02-Router / 03 – Add DNS Resolution Checks for Verizon-R1**](../04-Monitoring-Use-Cases/02-Router/03-Add-DNS-Resolution-Checks-for-Verizon-R1.md)
- [**05-Troubleshooting / 02-Router / 02 – Troubleshoot comp-a-test DNS Check (No Records)**](../05-Troubleshooting/02-Router/02-Troubleshoot-comp-a-test-DNS-Check-No-Records.md)
- [**10 – Validate Verizon-R1 Ping Check**](10-Validate-Verizon-R1-Ping-Check.md)
