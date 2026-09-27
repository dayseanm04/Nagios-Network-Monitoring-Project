# 03 – Assign a Static IP to NetworkVM

## Why a Static IP for a Monitoring Server?

A DHCP address can change after a reboot or lease expiry. For a Nagios server, that causes real problems:
- Devices and agents being monitored are often configured to only accept checks from the Nagios server's IP.
- Firewall rules and SNMP access lists reference the monitoring server by IP.
- You need a static IP address to reach the Nagios web interface.
