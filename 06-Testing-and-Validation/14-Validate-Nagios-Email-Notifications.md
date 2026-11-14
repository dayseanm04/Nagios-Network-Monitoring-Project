# 14 – Validate Nagios Email Notifications

In this doc, I will verify that Nagios sends alert emails to my inbox, first with a custom notification I trigger by hand, and then with real alerts from a real problem.

> **How notifications were set up:** [**Click here to see Configure Nagios Email Notifications**](../02-Setup/13-Configure-Nagios-Email-Notifications.md)

---

## Test Cases

| ID | Test | Expected | Actual | Status |
|---|---|---|---|---|
| TC-01 | Custom notification | **CUSTOM** email arrives | **`CUSTOM Service Alert: Computer A/CPU Utilization is OK`** | ✅ PASS |
| TC-02 | Real CRITICAL alert | **PROBLEM** email when memory goes over 90% | **`Logical memory usage is CRITICAL`**, 94.50% | ✅ PASS |
| TC-03 | State change to WARNING | New email when memory drops to 70–90% | **`Logical memory usage is WARNING`**, 80.90% | ✅ PASS |
| TC-04 | Repeat notifications | Email repeats about every 15 min while the problem lasts | CRITICAL emails at 18:12, 18:28, 18:44 | ✅ PASS |
| TC-05 | RECOVERY alert | **RECOVERY** email when a service returns to OK | **`RECOVERY Service Alert: Computer A/system uptime is OK`** | ✅ PASS |

---

## TC-01 – Custom Notification

**Steps:** In Nagios, I click **Services** → **CPU Utilization** on **`comp-a-test`** → **Send custom service notification**. I leave **Forced** and **Broadcast** unchecked and click **Commit**.

<img width="1008" height="598" alt="1" src="https://github.com/user-attachments/assets/30b9f2a1-72a3-4d9b-8ec4-6008aa5bf82b" />

<img width="1028" height="782" alt="2" src="https://github.com/user-attachments/assets/8282da0a-12a1-4e2f-ba6b-855c2a641106" />

<img width="1033" height="804" alt="3" src="https://github.com/user-attachments/assets/52af7847-f68a-4216-9dd4-28addca3c7fa" />

<img width="1039" height="767" alt="4" src="https://github.com/user-attachments/assets/35db28f4-17a5-4d80-bdc7-7e9db2269192" />

<img width="1034" height="548" alt="5" src="https://github.com/user-attachments/assets/228f9dd0-af60-481b-8f3b-09c363f5278d" />

<img width="904" height="696" alt="6" src="https://github.com/user-attachments/assets/aa9fb9ac-0b4b-4b11-940b-ebde15ed2994" />

> **Why leave Forced unchecked?** **Forced** sends the notification even if notifications are turned off.

---

## TC-02 – Real CRITICAL Alert

**Steps:** Memory on **`comp-a-test`** was high with many Chrome tabs open. My memory check is **`-w 70 -c 90`**.

<img width="961" height="709" alt="7" src="https://github.com/user-attachments/assets/419c2f3d-df57-49a5-9447-ca0725838f08" />
















