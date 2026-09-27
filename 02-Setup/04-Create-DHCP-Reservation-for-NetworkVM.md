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
