# 11 – Test SNMP Support on Verizon-R1

In this doc, I will test whether my router, **`verizon-r1`** [CR1000B], supports SNMP. The sample **`switch.cfg`** I used for the router's config includes checks for uptime and port status that need SNMP, so I need to know if they can work before using them.

> **Where this came up:** [**04-Monitoring-Use-Cases / 02-Router / 02 – Add Verizon Router and Ping Check**](../04-Monitoring-Use-Cases/02-Router/02-Add-Verizon-Router-and-Ping-Check.md)

---

## What Is SNMP?

SNMP (Simple Network Management Protocol) is how monitoring tools read information from network devices, like uptime, interface status, and traffic. The device runs an **SNMP agent** that listens on **UDP port 161** and answers requests. Most enterpise routers and switches support it.

For Nagios to monitor **`verizon-r1`** with SNMP, two things would need to be true:

1. The router has an SNMP agent that can be turned on.
2. The router answers SNMP requests from the Nagios server.

This test checks both.

---

## Test Cases

| ID | Test | Expected if supported | Actual | Status |
|---|---|---|---|---|
| TC-01 | Look for SNMP settings in the router's admin interface | An SNMP page with an on/off switch and community string | No SNMP settings in any section | Not supported |
| TC-01 | Look for SNMP settings in the router's admin interface | An SNMP page with an on/off switch and community string | No SNMP settings in any section | Not supported |

---

## TC-01 – Look for SNMP Settings in the Router

**Steps:** I log in to the router's admin page at **`https://192.168.1.1`** and click the **Advanced** tab.

<img width="1069" height="684" alt="1" src="https://github.com/user-attachments/assets/41d995b6-ff5c-46b9-81b4-462adb774487" />

I check every section where SNMP settings would normally be:

### I clicked security and firewall and looked for SNMP

<img width="1063" height="730" alt="2" src="https://github.com/user-attachments/assets/28a89b36-0c0b-4801-8c2b-8216ea922921" />

### I clicked Network Settings there's also no section for snmp






