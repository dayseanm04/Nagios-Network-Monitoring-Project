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

<img width="861" height="317" alt="1" src="https://github.com/user-attachments/assets/c9b3f047-b027-4490-831d-4abb1b34925c" />

1. In Hyper-V Manager, click **Virtual Switch Manager** in the actions plan on the top right.
2. Select **New virtual network switch**.
3. Select **External**, then click **Create Virtual Switch**.

<img width="722" height="313" alt="2" src="https://github.com/user-attachments/assets/cd0a02ae-407a-4e9c-9b67-ac8ed68e7815" />

5. Fill in the switch properties:

<img width="724" height="684" alt="3" src="https://github.com/user-attachments/assets/bf0245d3-a038-4c6b-894b-4cb692e37800" />

6. Click **OK**.

7. When the warning about pending changes disrupting network connectivity appears, click **Yes** to continue.

> **Why keep "Allow management operating system to share this network adapter" checked?** It lets the host keep using the same adapter for its own network access. If it's unchecked, the adapter is handed over entirely to the VMs and the host loses its connection.

## Step 3 – Connect NetworkVM to the Switch

1. In Hyper-V Manager, right-click `NetworkVM` → **Settings**.
2. In the left pane, select **Network Adapter**.
3. Set **Virtual switch** to `Net-MGNT-SW`.
4. Leave **VLAN ID** and **Bandwidth Management** unchecked.

<img width="719" height="686" alt="4" src="https://github.com/user-attachments/assets/58b6a1c7-d652-49b0-b48f-f03b10374951" />

6. Click **Apply** → **OK**.




