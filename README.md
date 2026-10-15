# Nagios Network Monitoring Project

Monitoring a small company network with **Nagios Core** running on an Ubuntu VM. I built the monitoring server from source, connected it to a Windows host through the NCPA agent, and documented every step: setup, configuration, troubleshooting, and testing.

---

## Project Scenario

The goal of this project is to plan and build network monitoring for a **small company with 12–20 devices**. To do that, I built a project that simulates the core of that network: a Nagios monitoring server, a monitored Windows host, and the edge router.

---

## Project Network

<img width="866" height="625" alt="Lab Network – Current" src="https://github.com/user-attachments/assets/2d3611b4-8d88-4524-82c2-59cec922a8d4" />

| Device | Role | IP Address | Details |
|---|---|---|---|
| **Verizon-R1** | Edge router / gateway | **`192.168.1.1`** | Connects the project to the internet. DHCP reservations for projecct devices |
| **comp-a-test** | Monitored Windows host + Hyper-V host | **`192.168.1.9`** | Windows 11, NCPA 3.5.0 agent. Connected to the router over Wi-Fi |
| **Net-MGNT-SW** | Hyper-V external virtual switch | — | Bridged to comp-a-test's Wi-Fi adapter so the VM is on the same network |
| **NetworkVM** | Nagios monitoring server | **`192.168.1.99`** | Ubuntu 26.04.1 LTS, Nagios Core 4.5.14, Apache web UI |

---

## What's Monitored

| Host | Check | How | Status |
|---|---|---|---|
| comp-a-test | Host availability | Ping (ICMP) | ✅ Active |
| comp-a-test | System uptime (alerts after a reboot) | NCPA – **`check_ncpa`** | ✅ Active |
| NetworkVM | Load, users, disk, swap, processes, HTTP, ping | Local Nagios plugins | ✅ Active |

---

## Repository Structure

| Folder | What's inside |
|---|---|
| [**01-Overview**](01-Overview) | Project scenario, goals, and network diagrams |
| [**02-Setup**](02-Setup) | Building the Nagios server: Hyper-V VM, virtual switch, static IP, DHCP reservation, compiling Nagios Core, upgrading, NCPA command setup, and restructuring the config to scale |
| [**03-Templates**](03-Templates) | Reusable guides for recurring tasks, like adding a host and service |
| [**04-Monitoring-Use-Cases**](04-Monitoring-Use-Cases) | Monitoring specific devices, starting with the Windows host: adding it to Nagios, installing NCPA, and creating service checks |
| [**05-Troubleshooting**](05-Troubleshooting) | Real problems I ran into: what happened, how I found the cause, and how I fixed it |
| [**06-Testing-and-Validation**](06-Testing-and-Validation) | Test cases and results proving each change works |
| [**08-Help**](08-Help) | Reference guides: navigating the Nagios web UI, understanding host and service definitions, and testing checks in NCPA |



## Problems I Solved

| Problem | Cause | Fix |
|---|---|---|
| Nagios VM got a **`169.254.x.x`** (APIPA) address | DHCP wasn't working over the Wi-Fi-bridged virtual switch | Set a static IP and created a DHCP reservation on the router |
| Windows host showed DOWN in Nagios | Host IP had changed, and Windows Firewall blocked inbound ping | Updated the host address and added a firewall rule allowing ICMP only from the Nagios server |
| Uptime check went CRITICAL on a healthy machine | Thresholds alerted on values **above** the limit instead of **below** | Used Nagios range syntax (**`-w 3600: -c 600:`**) |
| Config wouldn't scale past one Windows host | Everything was in one **`windows.cfg`** file | Moved to one file per host with **`cfg_dir`**, and moved the shared hostgroup to its own file |

Full documentation are in [**05-Troubleshooting**](05-Troubleshooting) and [**06-Testing-and-Validation**](06-Testing-and-Validation).

---

## Skills Demonstrated
- **Linux administration** – compiling software from source, users and groups, file permissions, systemd services
- **Networking** – static IP and DHCP reservations, Hyper-V virtual switching, subnetting, firewall rules
- **Monitoring** – Nagios host and service definitions, templates, hostgroups, thresholds, agent-based checks with NCPA
- **Security** – secrets kept out of config files and out of this repo, firewall rules limited to the monitoring server, TLS 1.2 minimum on the agent
- **Troubleshooting** – isolating problems with two-way ping tests, reading plugin output, logs, and performance data
- **Documentation** – step-by-step guides, test cases with expected vs. actual results, and incident write-ups

---

## Roadmap
- [x] Build Nagios Core server on Ubuntu VM
- [x] Upgrade Nagios Core to 4.5.14
- [x] Add Windows host and resolve host DOWN issue
- [x] Install NCPA and create first agent-based check (uptime)
- [x] Restructure config into per-host files

---

## Security Note

No passwords, tokens, or other secrets are stored in this repository. Credentials are blurred in screenshots, and the NCPA token is stored only in **`resource.cfg`** on the Nagios server.

---

## Author

**Daysean Mensah**

B.Tech in Computer Systems (Networking and Security) – New York City College of Technology

**CompTIA Network+ · Cisco CCNA**

[**LinkedIn**]([https://www.linkedin.com/in/YOUR-PROFILE](https://www.linkedin.com/in/daysean-mensah-1ba363355)) · [**GitHub**](https://github.com/dayseanm04)

