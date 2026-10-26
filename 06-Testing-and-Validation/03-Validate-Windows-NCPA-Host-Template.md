# 03 – Validate Windows NCPA Host Template

In this doc, I will verify that **`comp-a-test`** is using my custom **`windows-ncpa-host`** template after switching from the default **`windows-server`** template. I'll confirm the host is still UP and that its check interval, max attempts, and hostgroup match the new template.

> **How this change was made:** [**Click here to see Apply Windows NCPA Host Template to comp-a-test**](../04-Monitoring-Use-Cases/01-Windows-Host/04-Apply-Windows-NCPA-Host-Template-to-comp-a-test.md)

## Test Summary

| Item | Details |
|---|---|
| Date | October 25, 2026 |
| Host under test | **`comp-a-test`** (**`192.168.1.9`)** |
| Change being validated | **`use windows-server`** → **`use windows-ncpa-host`** |
| Nagios server | **`NetworkVM`** (**`192.168.1.99`**), Nagios Core 4.5.14 |
| Overall result | ✅ **PASS** |

---

## Expected Settings

| Setting | Before (`windows-server`) | Expected after (`windows-ncpa-host`) |
|---|---|---|
| Check interval | 5 min | **3 min** |
| Max check attempts | 10 | **3** |**
| Hostgroup | **`windows-servers`** | **`windows-servers` (inherited) |

---

## Test Cases

| ID | Test | Expected | Actual | Status |
|---|---|---|---|---|
| TC-01 | Host status after the change | **UP** | UP – PING OK, 0% packet loss, RTA 0.64 ms | ✅ PASS |
| TC-02 | Check interval | 3 minutes between checks | 15:02:38 → 15:05:38 (3 min) | ✅ PASS |
| TC-03 | Max check attempts | Attempt shown out of **3** | 1/3 (HARD state) | ✅ PASS |
| TC-04 | Hostgroup membership | Still in `windows-servers` | Member of `windows-servers` | ✅ PASS |
| TC-05 | Host goes HARD DOWN after 3 failed checks | 1/3 → 2/3 → 3/3 HARD DOWN | 2/3 SOFT, then 3/3 HARD about 3 minutes after the first failure | ✅ PASS |

---

## TC-01 – Host Is Still UP

**Steps:**

1. I log in to `http://192.168.1.99/nagios`.
2. I click **Hosts**.

<img width="1028" height="537" alt="1" src="https://github.com/user-attachments/assets/72f96e34-41b8-4524-b408-731c7376e4b9" />

**Result:** ✅ PASS. Changing the template didn't break the host check.

---

## TC-02 – Check Interval Is 3 Minutes

**Steps:**

On the Hosts page, I click **`comp-a-test`** to open its details and compare **Last Check Time** with **Next Scheduled Active Check**.

<img width="969" height="641" alt="2" src="https://github.com/user-attachments/assets/506db206-958f-417e-a8dd-5d027ea07f39" />

**Actual:**

| Field | Value |
|---|---|
| Last Check Time | **10-25-2026 15:02:38** |
| Next Scheduled Active Check | **10-25-2026 15:05:38** |

**Result:** ✅ PASS. The checks are exactly **3 minutes** apart, matching `check_interval 3` in `windows-ncpa-host`. Under `windows-server`, they would be 5 minutes apart.

---

## TC-03 – Max Check Attempts Is 3

**Steps:**

On the same details page, I look at **Current Attempt**.

<img width="969" height="641" alt="3" src="https://github.com/user-attachments/assets/506db206-958f-417e-a8dd-5d027ea07f39" />

**Result:** ✅ PASS. The number after the slash is the max check attempts, now **3**. Under `windows-server` it was 10. My earlier troubleshooting screenshots of `comp-a-test` show `10/10 (HARD state)`, so this confirms the change took effect.

---

## TC-04 – Still a Member of windows-servers

**Steps:**

On the same details page, I look at **Member of**.

**Actual:** `windows-servers`

**Result:** ✅ PASS. **`comp-a-test`** gets **`windows-servers`** through the chain **`windows-ncpa-host`** → **`windows-server`**, which proves the inheritance works.

---

## TC-05 – Faster DOWN Detection

TC-03 showed that max check attempts is set to 3. This test proves it actually works: I block ping from the Nagios server and watch how fast `comp-a-test` goes from SOFT to HARD DOWN.

### Step 1 – Disable the ICMP allow rule

On `comp-a-test`, I open **Windows Defender Firewall with Advanced Security**, click **Inbound Rules**, and use **Filter by State → Filter by Enabled** to find active rules.

<img width="1222" height="353" alt="4" src="https://github.com/user-attachments/assets/bdffbcec-4517-45b7-92fd-8f5754a1502c" />

I right-click **IN-Nagios_VM-ICMP-Allow** and select **Disable Rule**.

<img width="1060" height="451" alt="5" src="https://github.com/user-attachments/assets/8637947d-6d15-458d-a23e-7c770d62545b" />

### Step 2 – Create an ICMP block rule

Windows may have other built-in rules that allow ping. To make sure the Nagios server is fully blocked, I create a rule that blocks it directly. In Windows Firewall, **block rules override allow rules**, so this guarantees the pings are dropped.

I click **New Rule…** and complete the wizard:

| Wizard page | Setting |
|---|---|
| Rule Type | **Custom** |
| Program | **All programs** |
| Protocol and Ports | Protocol type: **ICMPv4** |
| Scope – local IP addresses | **Any IP address** |
| Scope – remote IP addresses | **These IP addresses:** `192.168.1.99` |
| Action | **Block the connection** |
| Profile | **Domain**, **Private**, **Public** |
| Name | **`IN-Nagios_VM-ICMP-Block`** |
| Description | Blocks incoming ICMP (ping) traffic from the Ubuntu Nagios monitoring server to test faster DOWN detection |

<img width="725" height="579" alt="6" src="https://github.com/user-attachments/assets/1a16ffeb-d0d2-4ac9-b3aa-5b2bb77c00bd" />

I click **Finish**.

### Step 3 – Watch the host in Nagios

I log in to `http://192.168.1.99/nagios`, click **Hosts**, then `comp-a-test`, and refresh as the checks run.

**First and second failures – SOFT state**


## Before and After

| | Before | After |
|---|---|---|
| Template | **`windows-server`** | **`windows-ncpa-host`** |
| Time between host checks | 5 min | **3 min** |
| Current Attempt shown as | x/10 | **x/3** |
| Hostgroup | **`windows-servers`** | **`windows-servers`** |
| Host status | UP | UP |

---

## Conclusion

**`comp-a-test`** is using the **`windows-ncpa-host`** template. The host is still UP, it's checked every 3 minutes instead of 5, it's marked HARD DOWN after 3 failed checks instead of 10, and it still belongs to **`windows-servers`** through template inheritance. All four test cases passed.

---

## Notes

- **Service totals** show 8 OK and 1 CRITICAL. The CRITICAL is the **`SSH`** check on `localhost`, which isn't related to this test.

---

## Related Docs

- [**04-Monitoring-Use-Cases / 01-Windows-Host / 04 – Apply Windows NCPA Host Template to comp-a-tes**t](../04-Monitoring-Use-Cases/01-Windows-Host/04-Apply-Windows-NCPA-Host-Template-to-comp-a-test.md)
- [**02-Setup / 10 – Create Notify-Early, Network Device, and Windows NCPA Templates**](../02-Setup/10-Create-Notify-Early-Network-Device-and-Windows-NCPA-Templates.md)
- [**06-Testing-and-Validation / 02 – Validate NCPA Uptime Check**](02-Validate-NCPA-Uptime-Check.md)




















