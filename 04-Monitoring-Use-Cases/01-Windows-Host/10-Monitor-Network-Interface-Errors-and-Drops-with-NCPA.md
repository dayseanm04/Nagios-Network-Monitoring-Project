# 10 – Monitor Network Interface Errors and Drops with NCPA

In this doc, I will add four checks that watch the health of **`comp-a-test`**'s network connection: inbound and outbound **errors**, and inbound and outbound **drops**. I'll build each check in the NCPA web interface, then add them to the host's config file using my **`notify-early`** template.

> **Testing and validation for these checks:** [**Click here to see the NCPA network interface check validation**](../../06-Testing-and-Validation/09-Validate-NCPA-Network-Interface-Checks.md)

---

## Overview

## Which Interface and Why

<img width="273" height="278" alt="1" src="https://github.com/user-attachments/assets/401afbd8-7302-4b35-b2bf-2b7bcb686cf2" />

| Interface | What it is | Monitored? |
|---|---|---|
| **vEthernet (Net-MGNT-SW)** | comp-a-test's virtual adapter on the Hyper-V external switch. All of comp-a-test's traffic and NetworkVM's traffic goes through it | ✅ Yes |
| Ethernet, Ethernet 2, Ethernet 3 | Wired adapters. Not in use, since `comp-a-test` is on Wi-Fi | No |
| vEthernet (Default Switch) | Hyper-V's built-in NAT switch. NetworkVM doesn't use it | No |
| Local Area Connection\* 9 / 10 | Virtual Wi-Fi adapters for features like Mobile Hotspot | No |
| Bluetooth, Loopback | Not part of this project network | No |

**The Wi-Fi adapter isn't in the list.** When I bound **`Net-MGNT-SW`** to the Wi-Fi adapter (see [**02-Setup / 02 – Configure the Hyper-V Virtual Switch**](../../02-Setup/02-Configure-Hyper-V-Virtual-Switch.md)), Windows moved the host's network connection to **`vEthernet (Net-MGNT-SW)`**. That's why this is the adapter carrying the traffic.

### The adapter, not the switch

| Name | What it is |
|---|---|
