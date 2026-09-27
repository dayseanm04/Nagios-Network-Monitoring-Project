# 02 – Configure the Hyper-V Virtual Switch

## Why an External Switch?

Hyper-V offers three switch types:

| Type | What it connects | Fit for this project |
|---|---|---|
| **External** | VMs ↔ host ↔ physical network | Nagios can reach real devices on the LAN |
| **Internal** | VMs ↔ host only | No access to other devices on the network |
| **Private** | VMs ↔ other VMs only | No access to the host or the network |

An External switch bridges the VM onto the physical network, so **`NetworkVM`** gets its own IP address on the same subnet as the host and can monitor anything the host can reach.

---

## Prerequisites

- **`NetworkVM`** created (see [**01 – Create the Ubuntu Monitoring VM in Hyper-V**](01-Create-Ubuntu-VM-in-Hyper-V.md))
- **`NetworkVM`** is **shut down**
- Hyper-V Manager open

> **Heads up:** Creating an External switch briefly drops the host's network connection while Hyper-V rebinds the adapter. Save any work that depends on the network before starting.

## Step 1 – Shut Down the VM

1. In Hyper-V Manager, confirm `NetworkVM` shows a state of **Off**.

---

## Step 2 – Create the External Virtual Switch

<img width="861" height="317" alt="Screenshot 2026-09-27 184408" src="https://github.com/user-attachments/assets/c9b3f047-b027-4490-831d-4abb1b34925c" />

1. In Hyper-V Manager, click **Virtual Switch Manager** in the actions plan on the top right.
2. Select **New virtual network switch**.

3. Select **External**, then click **Create Virtual Switch**.

