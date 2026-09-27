# 04 – Create a DHCP Reservation for NetworkVM

I will reserve **`192.168.1.99`** for `NetworkVM` on the home router. The VM already has this address set statically (see **doc 03**), but the router doesn't know that. Adding a DHCP reservation records the address on the router and ties it to the VM's MAC address, so the router never hands `192.168.1.99` to another device.

## Static IP vs. DHCP Reservation

These two are often confused, and this project uses both:

| | Static IP | DHCP Reservation |
|---|---|---|
| **Configured on** | The device (NetworkVM) | The DHCP server (router) |
| **What it does** | Device always uses the address it's given | Router always gives the same address to a specific MAC |
| **Router aware of it?** | No | Yes |
| **Covered in** | **Doc 03** | This doc |

Using both means the VM keeps its address no matter what, and the router has a record that `192.168.1.99` belongs to **`NetworkVM`**.

> **Why `192.168.1.99`?** The router's DHCP pool only hands out **`192.168.1.2`** through **`192.168.1.50`**. Picking an address outside that range avoids any chance of the router assigning it to a phone, laptop, or other device.

---

## Prerequisites

- **`NetworkVM`** configured with a static IP (see [**03 – Assign a Static IP to NetworkVM**](03-Configure-Static-IPv4-Address-For-The-NetworkVM.md))
- Admin login for the router's web interface
- NetworkVM's **MAC address** (Step 1)

---

## Step 1 – Find NetworkVM's MAC Address

Use any one of these methods.

**From inside the VM:**

```bash
ip a
```
