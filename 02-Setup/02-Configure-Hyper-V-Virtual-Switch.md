# 02 – Configure the Hyper-V Virtual Switch

## Why an External Switch?

Hyper-V offers three switch types:

| Type | What it connects | Fit for this project |
|---|---|---|
| **External** | VMs ↔ host ↔ physical network | Nagios can reach real devices on the LAN |
| **Internal** | VMs ↔ host only | No access to other devices on the network |
| **Private** | VMs ↔ other VMs only | No access to the host or the network |
