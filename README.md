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
| **NetworkVM** | Nagios monitoring server | `192.168.1.99` | Ubuntu 26.04.1 LTS, Nagios Core 4.5.14, Apache web UI |

---

## Roadmap
