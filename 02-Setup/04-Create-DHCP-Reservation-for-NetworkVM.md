# 04 – Create a DHCP Reservation for NetworkVM

I will reserve **`192.168.1.99`** for `NetworkVM` on the home router. The VM already has this address set statically (see **doc 03**), but the router doesn't know that. Adding a DHCP reservation records the address on the router and ties it to the VM's MAC address, so the router never hands `192.168.1.99` to another device.
