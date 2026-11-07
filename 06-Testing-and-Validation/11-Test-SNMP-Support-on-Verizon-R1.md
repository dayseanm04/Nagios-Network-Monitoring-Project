# 11 – Test SNMP Support on Verizon-R1

In this doc, I will test whether my router, **`verizon-r1`** [CR1000B], supports SNMP. The sample **`switch.cfg`** I used for the router's config includes checks for uptime and port status that need SNMP, so I need to know if they can work before using them.

> **Where this came up:** [**04-Monitoring-Use-Cases / 02-Router / 02 – Add Verizon Router and Ping Check**](../04-Monitoring-Use-Cases/02-Router/02-Add-Verizon-Router-and-Ping-Check.md)

---

## What Is SNMP?

SNMP (Simple Network Management Protocol) is how monitoring tools read information from network devices, like uptime, interface status, and traffic. The device runs an **SNMP agent** that listens on **UDP port 161** and answers requests. Most enterprise routers and switches support it.

For Nagios to monitor **`verizon-r1`** with SNMP, two things would need to be true:

1. The router has an SNMP agent that can be turned on.
2. The router answers SNMP requests from the Nagios server.

This test checks both.

---

## Test Cases

| ID | Test | Expected if supported | Actual | Status |
|---|---|---|---|---|
| TC-01 | Look for SNMP settings in the router's admin interface | An SNMP page with an on/off switch and community string | No SNMP settings in any section | Not supported |
| TC-02 | Send an SNMP request from NetworkVM | The router's uptime | `Timeout: No Response from 192.168.1.1.` | ❌ No response |
| TC-03 | Send UDP packets to port 161 from **comp-a-test** | No reply, or an SNMP reply if supported | 3/3 answered with ICMP **Port unreachable** | ❌ Port closed |

---

## TC-01 – Look for SNMP Settings in the Router

**Steps:** I log in to the router's admin page at **`https://192.168.1.1`** and click the **Advanced** tab.

<img width="1069" height="684" alt="1" src="https://github.com/user-attachments/assets/41d995b6-ff5c-46b9-81b4-462adb774487" />

I check every section where SNMP settings would normally be:

### 1. I clicked security and firewall and looked for SNMP

<img width="1063" height="730" alt="2" src="https://github.com/user-attachments/assets/28a89b36-0c0b-4801-8c2b-8216ea922921" />

### 2. I clicked Network Settings there's also no section for snmp

<img width="1082" height="959" alt="3" src="https://github.com/user-attachments/assets/93c43102-6552-4405-8754-3247ca091203" />

### 3. I clicked Diagnostics and Monitoring and there's also no section for snmp

<img width="1067" height="814" alt="4" src="https://github.com/user-attachments/assets/d1283e64-e3d0-4e38-8652-6b2957b88882" />

| Section | What's in it | SNMP? |
|---|---|---|
| **Security & Firewall** | General, Access Control, DMZ Host, IPv6 Pinholes, Port Forwarding, Port Forwarding Rules, Port Triggering, Scheduler Rules, SIP ALG | ❌ |
| **Network Settings** | ARP Table, DNS Server, Dynamic DNS, IPv4 Address Distribution, IPv6, MAC Cloning, NDP Table, Network Connections, Port Configuration, Routing, Static NAT, and more | ❌ |
| **Diagnostics & Monitoring** | Bandwidth Monitoring, Diagnostics, System Logging, System-wide Connections, Backhaul Logging | ❌ |
| **System** | System Status, Date & Time, Factory Reset, LED Brightness, Open Source Software, Reboot Router, Remote Administration, System Settings | ❌ |

**Result:** ❌ There's no option anywhere to turn on SNMP or set a community string.

**About Access Control:** The **Access Control** page under Security & Firewall can create a firewall rule that allows SNMP traffic. But a firewall rule only lets traffic **in/out**. It doesn't make the router **answer**. If no SNMP agent is running, opening the port does nothing, so I didn't add that rule.

<img width="1152" height="774" alt="6" src="https://github.com/user-attachments/assets/fe363588-36da-4549-a9b1-fe4f77795d44" />

**About Bandwidth Monitoring:** The router does track its own upload and download usage (for example, 1.27 TB downloaded in the last month). But it only shows this on its own web page. Without SNMP, Nagios has no way to read it.

---

## TC-02 – Send an SNMP Request from NetworkVM

### Step 1 – Install the SNMP tools

On `NetworkVM`:

```bash
sudo apt install snmp
```

<img width="1026" height="597" alt="7" src="https://github.com/user-attachments/assets/caa558ad-2560-421e-abc8-fc2962b1fec0" />

This installs **`snmpget`** and other Net-SNMP command-line tools.

### Step 2 – Ask the router for its uptime

```bash
snmpget -v2c -c public 192.168.1.1 1.3.6.1.2.1.1.3.0
```

<img width="762" height="124" alt="8" src="https://github.com/user-attachments/assets/404b751b-bcfc-4752-bce5-502bd3968553" />

**Result:** ❌ The router didn't answer.

| Part | What it does |
|---|---|
| `snmpget` | Asks a device for one value |
| `-v2c` | Uses SNMP version 2c, the most common version |
| `-c public` | The community string, which works like a password. `public` is the usual default |
| `192.168.1.1` | The router |
| `1.3.6.1.2.1.1.3.0` | The OID (object ID) for **system uptime**. Every SNMP device that supports the standard system info should answer this |

## Analysis

A timeout on its own could mean a few things:

| Possible cause | Ruled out? |
|---|---|
| The router is down or unreachable | ✅ Yes. The router answers ping, and Nagios shows `verizon-r1` UP (see [**10 – Validate Verizon-R1 Ping Check**](10-Validate-Verizon-R1-Ping-Check.md)) |
| The community string is wrong | ✅ Yes. There's no setting to change the community string, so there's no other value to try |
| SNMP is turned off but can be turned on | ✅ Yes. TC-01 found no setting to turn it on or configure it |
| A firewall is dropping SNMP traffic | ✅ Yes. A firewall dropping the packets would give no reply at all. The router sent a reply |
| **The router doesn't have an SNMP agent** | ❌ **Confirmed.** Port 161 is closed (TC-03) |

With the router reachable and no SNMP settings anywhere in its admin interface, the timeout means the router isn't running an SNMP agent.

---

## TC-03 – Probe Port 161 from a Second Machine

To make sure the result wasn't caused by one machine or one tool, I tested again from **`comp-a-test`** using **`nping`**, a packet tool from the Nmap project.

**Steps:** On **`comp-a-test`**, in Command Prompt as Administrator:

```cmd
nping --udp -p 161 -c 3 192.168.1.1
```

<img width="1104" height="365" alt="9" src="https://github.com/user-attachments/assets/ef4f266c-551c-4559-9f74-d89a90f62e4e" />

**Result:** ❌ All 3 packets got an **ICMP Port unreachable** reply.

**What "Port unreachable" means:** ICMP type 3, code 3 is how a device says **"your packet arrived, but no program is listening on that port."** So the router received the packets on UDP 161 and told me nothing was there to answer them. If an SNMP agent were running, the port would be open and the router wouldn't send this message.

> The source address is **`192.168.1.18`** because, at the time of this test, **`comp-a-test`** had picked up a temporary IP after its MAC address changed. See [**05-Troubleshooting / 01-Windows-Host / 02 – Troubleshoot Windows Host Down and NCPA Services Unknown**](../05-Troubleshooting/01-Windows-Host/02-Troubleshoot-Windows-Host-Down-and-NCPA-Services-Unknown.md).

## Conclusion

**`verizon-r1`** (Verizon CR1000B) **does not support SNMP**. The router answered every probe with ICMP "Port unreachable." Its admin interface has no SNMP settings, and an **`snmpget`** request for system uptime timed out even though the router is reachable. This is common for ISP-provided home routers, which usually aren't built to be managed by monitoring tools.

---

## Related Docs

- [**04-Monitoring-Use-Cases / 02-Router / 02 – Add Verizon Router and Ping Check**](../04-Monitoring-Use-Cases/02-Router/02-Add-Verizon-Router-and-Ping-Check.md)
- [**06-Testing-and-Validation / 10 – Validate Verizon-R1 Ping Check**](10-Validate-Verizon-R1-Ping-Check.md)
