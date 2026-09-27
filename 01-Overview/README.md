# 01 - Overview

## Goal

The goal of this project is for me to learn how to use Nagios Core to monitor a network. When I say "monitor a network," I'm not limiting it to only network devices. I will monitor:

- A Linux host (Ubuntu) — the VM I will set up Nagios on
- A Windows machine — the host machine
- A router

This means I'm covering three different monitoring scenarios at once: monitoring the local host Nagios runs on, monitoring a remote OS (Windows) that needs an agent or plugin to report back to Nagios, and monitoring a network device (the router) using protocols like SNMP or basic connectivity checks (ping, port checks). Learning all three gives me a realistic picture of what monitoring actually looks like in a mixed environment, not just a single-OS setup.

As I go, I'll also be learning Nagios' core concepts such as: hosts, services, checks, contacts and notifications. 

## Problem

A small company with a limited budget wants to monitor their devices. They have about 12–20 devices — a mix of workstations, and network hardware, but don't have the budget for an enterprise monitoring platform or a dedicated monitoring team.

I'm not going to actually monitor 10–20 devices — this is just a theory/scenario to frame the project. In practice, I will monitor about 3 devices: the Ubuntu VM, the Windows host machine, and a router. The idea is that the setup, configuration process, and monitoring logic I build for 3 devices is the same logic that would scale up to 12–20.

## Solution

Set up Nagios Core and the Web UI on an Ubuntu Linux VM, and use it to monitor the devices listed above.

Nagios Core fits this problem well because:
- It's free and open source — no licensing cost, which matters for a small company on a limited budget
- It's flexible enough to monitor almost anything: local host resources, remote hosts, and network devices, using the same core engine
