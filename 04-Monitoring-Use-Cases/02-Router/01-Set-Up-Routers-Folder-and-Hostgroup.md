# 01 – Set Up Routers Folder and Hostgroup

In this doc, I will prepare Nagios to monitor routers. I'll create a **`routers`** folder with its own hostgroup, the same way I organized Windows hosts, and use the sample **`switch.cfg`** that comes with Nagios as the starting point for my router's config file.  I'll also update my **`network-device-host`** template so it's ready for network devices.

---

## Overview

### Before and After

**Before:**
```
/usr/local/nagios/etc/
├── objects/
├── templates/
└── windows-hosts/
```

**After:**
```
/usr/local/nagios/etc/
├── objects/
├── routers/                    ← new
│   ├── routers-hostgroup.cfg   ← routers hostgroup (shared)
│   └── verizon-r1.cfg          ← the router (configured in the next doc)
├── templates/
└── windows-hosts/
```

This follows the same rule I used for Windows hosts in [**02-Setup / 08 – Restructure Nagios Config for Multiple Hosts**](../../02-Setup/08-Restructure-Nagios-Config-for-Multiple-Hosts.md): **one file per device, and shared definitions like hostgroups in their own file.** If I add another router later, I only add a new file to this folder.

---

## Step 1 – Check the cfg_dir Examples in nagios.cfg

```bash
cd /usr/local/nagios/etc
ls -l
sudo nano nagios.cfg
```

<img width="852" height="236" alt="1" src="https://github.com/user-attachments/assets/1d27a16c-9e00-496d-9f3f-291e34077ef5" />

I scroll down to the section for monitoring a router/switch.

**`nagios.cfg`** already has commented-out examples for organizing devices by type

<img width="854" height="582" alt="2" src="https://github.com/user-attachments/assets/b3d0f810-838a-492c-a6c2-daf84f10a01b" />

There's a line for a `routers` folder, so I'll name my folder **`routers`** to match it.

I exit without saving: **Ctrl+X**.

---

## Step 2 – Review the Sample switch.cfg

Nagios comes with a sample config for monitoring a network device. I looked at it before using it:

```bash
cd objects
ls -l
sudo nano switch.cfg
```

<img width="814" height="231" alt="3" src="https://github.com/user-attachments/assets/ebfb9103-d810-4a2d-89b6-5db0a1a2ae34" />

### Host and hostgroup

<img width="919" height="574" alt="4" src="https://github.com/user-attachments/assets/9366f7e2-2c7d-4f59-84e6-0e2c4e827414" />

<img width="846" height="269" alt="5" src="https://github.com/user-attachments/assets/b7c28cc3-77fd-45e1-986e-666d9e192606" />


### Services

<img width="951" height="537" alt="5" src="https://github.com/user-attachments/assets/43b07d5e-0519-4e0c-8340-b28290dad28a" />

<img width="950" height="419" alt="6" src="https://github.com/user-attachments/assets/389c1234-1819-4c10-b1bb-fd623b87fc07" />


**What's SNMP?** Simple Network Management Protocol. It's how monitoring tools read stats like uptime, port status, and traffic from network devices. Enterprise routers and switches support it. Most ISP home routers don't, so I'll use ping and other checks that don't need it.


| Service | How it checks | Useful for my router? |
|---|---|---|
| PING | **`check_ping`** |  Yes. Works on any device that answers ping |
| Uptime | SNMP (`sysUpTime.0`) | Only if my router supports SNMP |
| Port 1 Link Status | SNMP (`ifOperStatus.1`) | Only if my router supports SNMP |
| Port 1 Bandwidth Usage | MRTG log files | Needs MRTG, which isn't installed |

> I'll check whether my Verizon router supports SNMP before deciding on the Uptime and Port 1 Link Status checks.

I exit: **Ctrl+X**, then go back to the **etc** folder:

```bash
cd ..
```

<img width="847" height="192" alt="7" src="https://github.com/user-attachments/assets/f3ff162d-1cd0-410c-a095-7f994218d199" />

---

## Step 3 – Create the Router Folder

I created the folder by copying **`windows-hosts`** and then removing its files:

```bash
sudo cp -r windows-hosts router
ls -l
```

<img width="941" height="531" alt="8" src="https://github.com/user-attachments/assets/2eb25760-6f1a-4801-861f-bc08b6305d48" />

```bash
cd router
ls -l
sudo rm comp-a-test.cfg
sudo rm windows-hostgroup.cfg
ls -l
```

<img width="853" height="314" alt="9" src="https://github.com/user-attachments/assets/a9e939cb-9253-4c86-8807-7ead49d9c764" />

**Note:** Copying a folder and then deleting everything in it works (it helps me practice moving and copying files/folders), but creating an empty folder does the same thing in one step:

```bash
sudo mkdir routers
```

---

## Step 4 – Copy switch.cfg into the Folder

```bash
sudo cp /usr/local/nagios/etc/objects/switch.cfg .
ls -l
```

<img width="998" height="510" alt="10" src="https://github.com/user-attachments/assets/893f5d15-c8c1-4441-b034-f7941073684b" />

The **`.`** means "copy it here," into the current folder.

---

## Step 5 – Remove the switches Hostgroup

**`switch.cfg`** defines a **`switches`** hostgroup. I don't want that in my router's config file, for the same reason I moved **`windows-servers`** out of **`comp-a-test.cfg`**: a hostgroup is shared by every device in the group, so it belongs in its own file. If it stayed in the router file, adding a second router by copying this file would define the hostgroup twice.

```bash
sudo nano switch.cfg
```

I scroll down to **HOST GROUP DEFINITIONS**.

<img width="846" height="382" alt="11" src="https://github.com/user-attachments/assets/9ad9ccf7-d976-4c8b-aa09-7f34a702b132" />

To delete the whole section in nano:

1. Move the cursor to the start of the section.
2. Press **Alt+A** to set a mark. **Mark Set** appears at the bottom.

<img width="841" height="475" alt="12" src="https://github.com/user-attachments/assets/92add1f8-89e8-44d9-91e8-437a1efbe168" />

3. Press the **down arrow** to highlight the section.
4. Press **Ctrl+K** to cut it.

<img width="887" height="276" alt="12" src="https://github.com/user-attachments/assets/9da06ef2-a0b6-4810-85c6-5b550bef140b" />

<img width="880" height="313" alt="13" src="https://github.com/user-attachments/assets/9a9c5a40-9f20-45d0-991c-213d21362c17" />


Save: **Ctrl+O**, **Enter**, **Ctrl+X**.

---

## Step 6 – Create the routers Hostgroup

```bash
sudo nano routers-hostgroup.cfg
```

<img width="881" height="330" alt="14" src="https://github.com/user-attachments/assets/03a5d679-9b0b-4b57-80aa-328e89c4dccc" />

Save: **Ctrl+O**, **Enter**, **Ctrl+X**.

Any router I add later joins this group with `hostgroups routers` in its host definition.

## Step 7 – Rename switch.cfg to verizon-r1.cfg

I name the file after the device it's for, the same way **`comp-a-test.cfg`** is named after its host:

```bash
sudo mv switch.cfg verizon-r1.cfg
ls -l
```

<img width="774" height="113" alt="15" src="https://github.com/user-attachments/assets/1913ea01-2bf5-48a6-b25d-f95f9cbc64e7" />

<img width="845" height="109" alt="16" src="https://github.com/user-attachments/assets/82b53b37-f00a-42fd-837e-964757bea7ca" />

**`verizon-r1`** matches the router's name on my [**network diagram**](../../README.md).

---

## Step 8 – Set Ownership and Permissions

Because I created everything with **`sudo`**, the folder and files are owned by **`root`**. I change them to match the rest of the Nagios config:

<img width="957" height="482" alt="17" src="https://github.com/user-attachments/assets/d1fc4b81-be3b-4ecb-a4f2-714818cb325b" />

| Command | What it does |
|---|---|
| **`chown -R nagios:nagios`** | Makes the **`nagios`** user and group the owner of the folder and everything in it |
| **`chmod 775` (folder)** | Owner and group can read, write, and open the folder. Everyone else can read and open it |
| **`chmod 664` (files)** | Owner and group can read and write. Everyone else can only read |

---

## Step 9 – Rename the Folder to routers

I rename **`router`** to **`routers`** so it matches the example line in **`nagios.cfg`** and the **`routers`** hostgroup:

<img width="918" height="237" alt="18" src="https://github.com/user-attachments/assets/1665cfee-7a6e-46fe-9472-8ccdccc508ce" />

Ownership and permissions stay the same after renaming.

---

## Step 10 – Load the Folder in nagios.cfg

```bash
sudo nano nagios.cfg
```

I scroll to the **`cfg_dir`** section and uncomment the **`routers`** line:

<img width="961" height="615" alt="19" src="https://github.com/user-attachments/assets/ec637ee2-7c9e-4cb6-841b-18edf0d68f0f" />

Save: **Ctrl+O**, **Enter**, **Ctrl+X**.

Every **`.cfg`** file in **`routers/`** will now be loaded when Nagios starts.

---

## Step 11 – Update the network-device-host Template

My **`network-device-host`** template, created in [**02-Setup / 10 – Create Notify-Early, Network Device, and Windows NCPA Templates**](../../02-Setup/10-Create-Notify-Early-Network-Device-and-Windows-NCPA-Templates.md), will be used by the router. I add a **`check_period`** so network devices are checked around the clock:

```bash
cd templates
ls -l
sudo nano custom-templates.cfg
```

<img width="823" height="104" alt="20" src="https://github.com/user-attachments/assets/8dbf903e-b49d-43b6-b533-1b2768e4c132" />

<img width="960" height="435" alt="21" src="https://github.com/user-attachments/assets/c8517864-7911-4c90-a4b8-893811b02ea4" />

Save: **Ctrl+O**, **Enter**, **Ctrl+X**.

| Setting | What it does |
|---|---|
| **`check_period 24x7`** | Checks the device at all hours, every day. Network devices affect everything behind them, so they shouldn't have gaps in monitoring |

---

## Where Things Stand

**I haven't run the pre-flight check yet.** At this point, **`verizon-r1.cfg`** is still the unedited sample from **`switch.cfg`**. It defines the sample Linksys switch, points to the **`switches`** hostgroup I removed, and has SNMP and MRTG service. Running the pre-flight now would fail.

In the next doc, I replace the sample with my router's host definition and service checks, then run the pre-flight check.

---

## Related Docs

- [**02-Setup / 08 – Restructure Nagios Config for Multiple Hosts**](../../02-Setup/08-Restructure-Nagios-Config-for-Multiple-Hosts.md)
- [**02 – Add Verizon Router to Nagios**](02-Add-Verizon-Router-to-Nagios.md)
