# 04 – Validate NCPA CPU Utilization Check

In this doc, I will verify that the new **`CPU Utilization`** check for **`comp-a-test`** is running, returning real CPU data, and using the settings from my `time-sensitive` template.

 **How this check was created:** [**Click here to see Monitor Windows CPU Utilization with NCPA**](../04-Monitoring-Use-Cases/01-Windows-Host/05-Monitor-Windows-CPU-Utilization-with-NCPA.md)

---

## Test Cases

| ID | Test | Expected | Actual | Status |
|---|---|---|---|---|
| TC-01 | Service appears with a real result | **OK** with a CPU percentage | OK – Percent was 22.43 % | ✅ PASS |
| TC-02 | Thresholds are applied | Performance data shows `70;85` | `'percent'=22.43%;70;85;` | ✅ PASS |
| TC-03 | Check interval | 2 minutes between checks | 12:26:26 → 12:28:26 (2 min) | ✅ PASS |
| TC-04 | Max check attempts | Attempt shown out of **5** | 1/5 (HARD state) | ✅ PASS |
| TC-05 | Notes shown in Nagios | Threshold text on the service page | Threshold >70% Warn, >85% Crit. | ✅ PASS |
| TC-06 | NCPA receives the checks from Nagios | `cpu/percent` entries executed by `192.168.1.99` | `cpu/percent` checks, OK, Active, executed by `192.168.1.99` | ✅ PASS |

---

## TC-01 – Service Appears with a Real Result

**Steps:**

1. I log in to **`http://192.168.1.99/nagios`**.
2. I click **Services**.

<img width="1032" height="547" alt="1" src="https://github.com/user-attachments/assets/b112dc93-cc66-4abc-8bef-a6a5248e1500" />

**Result:** ✅ PASS. The check is running and returning a real CPU value from NCPA.

---

## TC-02 – Thresholds Are Applied

**Steps:**

I click **CPU Utilization** to open the service details and look at **Performance Data**.

<img width="1028" height="768" alt="2" src="https://github.com/user-attachments/assets/f34eed69-5a58-495d-8831-8c4e1c10e50b" />

---

## TC-03 – Check Interval Is 2 Minutes

**Steps:**

On the same details page, I compare **Last Check Time** and **Next Scheduled Check**.

<img width="1028" height="768" alt="2" src="https://github.com/user-attachments/assets/f34eed69-5a58-495d-8831-8c4e1c10e50b" />

**Result:** ✅ PASS. The checks are **2 minutes** apart, matching `check_interval 2` in `time-sensitive`. With `generic-service`, they would be 10 minutes apart.

---

## TC-04 – Max Check Attempts Is 5

**Steps:**

On the same details page, I look at **Current Attempt**.

<img width="1028" height="768" alt="2" src="https://github.com/user-attachments/assets/f34eed69-5a58-495d-8831-8c4e1c10e50b" />

**Result:** ✅ PASS. The number after the slash is the max check attempts, **5**, from `time-sensitive`. My uptime check uses `generic-service` and shows `1/3`, so the two templates are clearly being applied separately.

---

## TC-05 – Notes Are Shown in Nagios

**Steps:**

On the same details page, I look under the host's IP address at the top of the page.

<img width="1028" height="768" alt="2" src="https://github.com/user-attachments/assets/f34eed69-5a58-495d-8831-8c4e1c10e50b" />

**Result:** ✅ PASS. The `notes` text is shown on the service page, so anyone checking this service in Nagios can see its thresholds without opening the config file.

---

## TC-06 – NCPA Receives the Checks from Nagios

TC-01 through TC-05 confirm the check from the Nagios side. This test confirms it from the NCPA side: that the requests are actually reaching the agent on `comp-a-test` and coming from the Nagios server.

**Steps:**

1. On **`comp-a-test`**, I go to **`https://localhost:5693`** and log in with the NCPA token.

## Conclusion

The **`CPU Utilization`** check for **`comp-a-test`** is working. Nagios gets a real, averaged CPU value from NCPA, applies the 70% and 85% thresholds, checks every 2 minutes, and requires 5 failed attempts before alerting, all matching the **`time-sensitive`** template. The **`notes`** text shows the thresholds right on the service page. All five test cases passed.


