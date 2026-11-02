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





