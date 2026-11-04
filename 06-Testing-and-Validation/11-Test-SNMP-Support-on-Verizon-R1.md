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

| Part | What it does |
|---|---|
| `snmpget` | Asks a device for one value |
| `-v2c` | Uses SNMP version 2c, the most common version |
| `-c public` | The community string, which works like a password. `public` is the usual default |








