# 01 – Troubleshoot Windows Host Down

Troubleshooting record for the Windows host `comp-a-test` showing **DOWN** in Nagios. Covers how the problem was found, the root cause, and the fix.

---

## Summary
| Item | Details |
|---|---|
| Date | October 2, 2026 |
| Affected host | **`comp-a-test`** (My Windows Host) |
| Host group | **`windows-servers`** |
| Monitoring server | **`NetworkVM`** (**`192.168.1.99`**) |
| Symptom | Host **DOWN** in Nagios; all 7 Windows services **CRITICAL** |
| Root cause | Host IP changed from `.151` to `.9`, and Windows Firewall blocked inbound ICMP from the Nagios server |
| Resolution | Updated the host address in Nagios and created an inbound firewall rule allowing ICMPv4 from **`192.168.1.99`**|
| Status | Resolved – host status **UP** |

