# 10 – Monitor Network Interface Errors and Drops with NCPA

In this doc, I will add four checks that watch the health of **`comp-a-test`**'s network connection: inbound and outbound **errors**, and inbound and outbound **drops**. I'll build each check in the NCPA web interface, then add them to the host's config file using my **`notify-early`** template.

> **Testing and validation for these checks:** [**Click here to see the NCPA network interface check validation**](../../06-Testing-and-Validation/09-Validate-NCPA-Network-Interface-Checks.md)

---

## Overview

### Which Interface and Why

<img width="273" height="278" alt="1" src="https://github.com/user-attachments/assets/401afbd8-7302-4b35-b2bf-2b7bcb686cf2" />

| Interface | What it is | Monitored? |
|---|---|---|
| **vEthernet (Net-MGNT-SW)** | comp-a-test's virtual adapter on the Hyper-V external switch. All of comp-a-test's traffic and NetworkVM's traffic goes through it | ✅ Yes |
| Ethernet, Ethernet 2, Ethernet 3 | Wired adapters. Not in use, since **`comp-a-test`** is on Wi-Fi | No |
| vEthernet (Default Switch) | Hyper-V's built-in NAT switch. NetworkVM doesn't use it | No |
| Local Area Connection\* 9 / 10 | Virtual Wi-Fi adapters for features like Mobile Hotspot | No |
| Bluetooth, Loopback | Not part of this project network | No |

**The Wi-Fi adapter isn't in the list.** When I bound **`Net-MGNT-SW`** to the Wi-Fi adapter (see [**02-Setup / 02 – Configure the Hyper-V Virtual Switch**](../../02-Setup/02-Configure-Hyper-V-Virtual-Switch.md)), Windows moved the host's network connection to **`vEthernet (Net-MGNT-SW)`**. That's why this is the adapter carrying the traffic.

### The adapter, not the switch

| Name | What it is |
|---|---|
| **Net-MGNT-SW** | The Hyper-V virtual switch |
| **vEthernet (Net-MGNT-SW)** | comp-a-test's own virtual adapter that connects Windows to that switch |

These checks monitor **comp-a-test's adapter**, not the switch itself. The switch has no IP address and no agent, so Nagios can't monitor it directly. That's why these services belong in **`comp-a-test.cfg`** and not in a separate host file.

---

# Part 1 – Build the Checks in NCPA

For more on this process, see [**08-Help / 04 – Find and Test NCPA Checks in the Web UI**](../../08-Help/04-Find-and-Test-NCPA-Checks-in-the-Web-UI.md).

## Step 1 – Log In and Select the Interface

On **`comp-a-test`**, I go to **`https://localhost:5693`**, log in with the NCPA token, and click **API**.

<img width="965" height="697" alt="2" src="https://github.com/user-attachments/assets/340d81a6-d543-4268-a6c6-4b1c3150f6f8" />

Under **API Endpoint**, I select **interface**, then **vEthernet (Net-MGNT-SW)**.

<img width="963" height="551" alt="3" src="https://github.com/user-attachments/assets/397f9dfa-0cc0-4861-a9d3-089c075b86d8" />

## Step 2 – Build Each Check

For each metric, I select it in the third dropdown, check **Delta**, check **Run as a Nagios check**, enter the thresholds, and click **Reload**. Then I click **View in alternate format** → **As active check using check_ncpa.py** to get the command.

### Errors In – `errin`

<img width="963" height="510" alt="4" src="https://github.com/user-attachments/assets/62cbd6c0-ac74-4b9e-a334-7dff5a7e68eb" />

<img width="617" height="301" alt="5" src="https://github.com/user-attachments/assets/63f70372-85e1-48ad-960d-04d63df0178a" />

I will use:

```
-M 'interface/vEthernet (Net-MGNT-SW)/errin' -w 0 -c 1 --delta
```

### Errors Out – `errout`

<img width="963" height="511" alt="6" src="https://github.com/user-attachments/assets/ad3aea5f-f1f6-4ce8-a405-c60763dc64c0" />

<img width="616" height="312" alt="7" src="https://github.com/user-attachments/assets/e4d38b10-2c5c-4a42-b0df-825969167c2a" />

I will use:

```
-M 'interface/vEthernet (Net-MGNT-SW)/errout' -w 0 -c 1 --delta
```

### Drops In – `dropin`

<img width="958" height="523" alt="8" src="https://github.com/user-attachments/assets/bc28df02-8f4a-4a10-b441-a1f43fdcb075" />

<img width="603" height="319" alt="9" src="https://github.com/user-attachments/assets/be2f76eb-a7f5-478e-b485-8bdea1fcffd8" />

I will use:

```
-M 'interface/vEthernet (Net-MGNT-SW)/dropin' -w 1 -c 5 --delta
```

### Drops Out – `dropout`

<img width="965" height="515" alt="10" src="https://github.com/user-attachments/assets/5ede31c2-a22b-40eb-ad04-2ce1bf2baaf3" />

<img width="606" height="311" alt="11" src="https://github.com/user-attachments/assets/e4124901-7bd7-4ad7-9d8b-7bc0d4bfb999" />

I will use:

```
-M 'interface/vEthernet (Net-MGNT-SW)/dropout' -w 1 -c 5 --delta
```

### Why these thresholds

| Check | Warning | Critical | Reasoning |
|---|---|---|---|
| Errors | Above 0 | Above 1/s | A healthy interface has no errors, so any error is worth a warning. 1 error per second is about 60 damaged packets a minute, which points to a real problem |
| Drops | Above 1/s | Above 5/s | Some drops can be normal on Wi-Fi and a virtual switch, so these leave a little room. I'll adjust them if my normal level turns out higher |


**Notes on the commands:**
- **`--delta`** turns on Delta, the same as the checkbox in NCPA.
- No unit prefix is needed. These are counts, not sizes.

---

# Part 2 – Add the Services in Nagios

## Step 1 – Open the Host File

```bash
cd /usr/local/nagios/etc/windows-hosts
ls -l
sudo nano comp-a-test.cfg
```

<img width="903" height="533" alt="12" src="https://github.com/user-attachments/assets/52a4a975-87b3-48b0-bb05-ef3df24b090b" />

## Step 2 – Add the Network Interface Section

Below the Logs section, I add:

<img width="890" height="465" alt="13" src="https://github.com/user-attachments/assets/46c6361b-7232-4beb-9443-9951b2321d7d" />

<img width="883" height="390" alt="14" src="https://github.com/user-attachments/assets/d1183922-d032-49eb-945e-f01f0c7cad0d" />

Save: **Ctrl+O**, **Enter**, **Ctrl+X**.

## Step 3 – Run the Pre-Flight Check

<img width="1016" height="852" alt="15" src="https://github.com/user-attachments/assets/46476a61-596f-404e-bad0-148fdf9d9a29" />




