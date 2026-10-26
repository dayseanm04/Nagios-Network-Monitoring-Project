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

