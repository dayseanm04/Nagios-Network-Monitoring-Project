# 11 – Test SNMP Support on Verizon-R1

In this doc, I will test whether my router, **`verizon-r1`** [CR1000B], supports SNMP. The sample **`switch.cfg`** I used for the router's config includes checks for uptime and port status that need SNMP, so I need to know if they can work before using them.

> **Where this came up:** [**04-Monitoring-Use-Cases / 02-Router / 02 – Add Verizon Router and Ping Check**](../04-Monitoring-Use-Cases/02-Router/02-Add-Verizon-Router-and-Ping-Check.md)

---

## What Is SNMP?

SNMP (Simple Network Management Protocol) is how monitoring tools read information from network devices, like uptime, interface status, and traffic. The device runs an **SNMP agent** that listens on **UDP port 161** and answers requests. Most enterpise routers and switches support it.
