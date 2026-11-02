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


