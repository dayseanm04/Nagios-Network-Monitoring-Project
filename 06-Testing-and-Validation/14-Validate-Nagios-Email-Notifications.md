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

<img width="964" height="625" alt="5-1" src="https://github.com/user-attachments/assets/10f7b0be-0daf-49a5-9b1c-8e0b02f347cb" />


<img width="904" height="696" alt="6" src="https://github.com/user-attachments/assets/aa9fb9ac-0b4b-4b11-940b-ebde15ed2994" />

> **Why leave Forced unchecked?** **Forced** sends the notification even if notifications are turned off.

---

## TC-02 – Real CRITICAL Alert

**Steps:** Memory on **`comp-a-test`** was high with many Chrome tabs open. My memory check is **`-w 70 -c 90`**.

<img width="961" height="709" alt="7" src="https://github.com/user-attachments/assets/419c2f3d-df57-49a5-9447-ca0725838f08" />

<img width="1088" height="582" alt="8" src="https://github.com/user-attachments/assets/acd81d89-a9fe-4688-ae82-e7572defea65" />

**Result:** ✅ PASS. I didn't trigger this one. Nagios found a real problem and emailed me on its own.

---

## TC-03 – State Change to WARNING

**Steps:** I closed the Chrome tabs.

<img width="1079" height="758" alt="9" src="https://github.com/user-attachments/assets/a278199d-bd74-40b0-9ecc-c1a6bda06c5d" />

<img width="628" height="474" alt="10" src="https://github.com/user-attachments/assets/262a2307-d181-4c64-8722-a66f2da2e9fb" />

**Result:** ✅ PASS. Memory went from over 90% to between 70% and 90%, so the state changed from **CRITICAL** to **WARNING**, and Nagios sent a new email. It's a WARNING and not a RECOVERY because 80.90% is still over the 70% warning line.

---

## TC-04 – Repeat Notifications

**Steps:** On Nov 8, memory went CRITICAL again and stayed high. I searched my inbox for the alerts account.

<img width="1343" height="708" alt="11" src="https://github.com/user-attachments/assets/cccd4e73-becf-4d1f-81f5-13aee3dd0fcb" />

<img width="1102" height="866" alt="12" src="https://github.com/user-attachments/assets/193a9d13-541a-4d1d-8d21-d75266761043" />

| Email | Date/Time | Additional Info |
|---|---|---|
| 1st | Nov 8, 6:12 PM | CRITICAL |
| 2nd | Nov 8, 18:28:56 | **`CRITICAL: Percent was 95.60 %`** |
| 3rd | Nov 8, 18:44:56 | **`CRITICAL: Percent was 95.70 %`** |


**Result:** ✅ PASS. While the problem lasted, Nagios re-sent the alert about every 16 minutes. My **`time-sensitive`** template sets a **15-minute** notification interval. Nagios only checks whether it's time to re-send when it runs the check, and this check runs every 2 minutes, so the re-send happens at the first check after 15 minutes.

---

## TC-05 – RECOVERY Alert

**Steps:** I checked my inbox for a RECOVERY email.

<img width="917" height="451" alt="13" src="https://github.com/user-attachments/assets/d30dcc74-bd7b-4ab4-8d4b-1bd864ddd498" />

**Result:** ✅ PASS. When the **system uptime** service went back to OK, Nagios sent a **RECOVERY** email.

> This recovery is for **system uptime**, not memory. Memory was still CRITICAL at this point (TC-04). A memory RECOVERY email will come once memory drops below 70%.

---

## Conclusion

Email notifications work from start to finish. Nagios sends **CUSTOM**, **PROBLEM** (CRITICAL and WARNING), and **RECOVERY** emails to my inbox, sends a new email when the state changes, and repeats the alert about every 15 minutes while a problem lasts.

---

## Related Docs

- [**02-Setup / 13 – Configure Nagios Email Notifications**](../02-Setup/13-Configure-Nagios-Email-Notifications.md)












