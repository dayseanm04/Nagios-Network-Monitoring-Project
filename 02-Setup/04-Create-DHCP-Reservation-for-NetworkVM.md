# 04 – Create a DHCP Reservation for NetworkVM

I will reserve **`192.168.1.99`** for `NetworkVM` on the home router. The VM already has this address set statically (see **doc 03**), but the router doesn't know that. Adding a DHCP reservation records the address on the router and ties it to the VM's MAC address, so the router never hands `192.168.1.99` to another device.

## Static IP vs. DHCP Reservation

These two are often confused, and this project uses both:

| | Static IP | DHCP Reservation |
|---|---|---|
| **Configured on** | The device (NetworkVM) | The DHCP server (router) |
| **What it does** | Device always uses the address it's given | Router always gives the same address to a specific MAC |
| **Router aware of it?** | No | Yes |
| **Covered in** | [**03 – Assign a Static IP to NetworkVM**](03-Configure-Static-IPv4-Address-For-The-NetworkVM.md) | This doc |

Using both means the VM keeps its address no matter what, and the router has a record that `192.168.1.99` belongs to **`NetworkVM`**.

> **Why `192.168.1.99`?** The router's DHCP pool only hands out **`192.168.1.2`** through **`192.168.1.50`**. Picking an address outside that range avoids any chance of the router assigning it to a phone, laptop, or other device.

---

## Prerequisites

- **`NetworkVM`** configured with a static IP (see [**03 – Assign a Static IP to NetworkVM**](03-Configure-Static-IPv4-Address-For-The-NetworkVM.md))
- Admin login for the router's web interface
- NetworkVM's **MAC address** (Step 1)

---

## Step 1 – Find NetworkVM's MAC Address

**From inside the VM terminal :**

```bash
ip a
```

The MAC address is on the `link/ether` line (format **`xx:xx:xx:xx:xx:xx`**).

## Step 2 – Log In to the Router

1. From the host, open a browser and go to `https://192.168.1.1`.
2. The browser will show a **Not secure** warning. This is expected — the router uses a self-signed certificate. Continue to the site.
3. Log in with the router's admin password.
4. Click the **Advanced** tab at the top.

<img width="751" height="669" alt="1" src="https://github.com/user-attachments/assets/cbdadee2-b46e-45d1-8e1c-df0d5880a291" />

---

## Step 3 – Open the DHCP Connection List
1. In the left menu, expand **Network Settings**.
2. Click **IPv4 Address Distribution**.

<img width="1070" height="543" alt="2" src="https://github.com/user-attachments/assets/45c3cc39-d54a-430f-b414-684dcc8eff5c" />

This page shows the router's DHCP server settings. Note the **Dynamic IP Range** of `192.168.1.2 – 192.168.1.50` the reserved IP should be outside this range.

3. Click **Connection List**. The **DHCP Connections** page opens.

<img width="1033" height="520" alt="3" src="https://github.com/user-attachments/assets/a76cf56c-c372-4cab-87e7-abd4ede81411" />

---

## Step 4 – Add the Reservation

1. Click **Add static connection**.
2. Fill in the **DHCP Connection Settings**:

