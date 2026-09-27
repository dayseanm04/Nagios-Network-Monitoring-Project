# 01 - Overview

## Goal

The goal of this project is for me to learn how to use Nagios Core to monitor a network. When I say "monitor a network," I'm not limiting it to only network devices. I will monitor:

- A Linux host (Ubuntu) — the VM I will set up Nagios on
- A Windows machine — the host machine
- A router

This means I'm covering three different monitoring scenarios at once: monitoring the local host Nagios runs on, monitoring a remote OS (Windows) that needs an agent or plugin to report back to Nagios, and monitoring a network device (the router) using protocols like SNMP or basic connectivity checks (ping, port checks). Learning all three gives me a realistic picture of what monitoring actually looks like in a mixed environment, not just a single-OS setup.

As I go, I'll also be learning Nagios' core concepts such as: hosts, services, checks, contacts and notifications. 
