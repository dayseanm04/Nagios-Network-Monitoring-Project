# 08 – Restructure Nagios Config for Multiple Hosts

I configured the system uptime check for **`comp-a-test`** in the **`windows.cfg`** file. Instead of defining every Windows host and its service checks in that one file, I will create a directory to hold Windows host config files, with one file per host.

This makes it easy to add a new Windows host by adding a new file, instead of adding every Windows host to **`windows.cfg`**. My project scenario is a small company with 12–20 devices [servers, etc], so the config needs to be organized in a way that scales.

---

## Before and After

**Before** – everything in one file:

```
/usr/local/nagios/etc/
└── objects/
    └── windows.cfg        ← hostgroup + comp-a-test + all its services
```

**After** – one file per host, shared definitions in their own file:

```
/usr/local/nagios/etc/
├── objects/
│   └── windows.cfg            ← disabled, kept for reference
└── windows-hosts/
    ├── windows-hostgroup.cfg  ← windows-servers hostgroup (shared)
    └── comp-a-test.cfg        ← comp-a-test host + its services
```

---

## cfg_file vs. cfg_dir

| Directive | What it loads | Example |
|---|---|---|
| **`cfg_file=`** | One specific file | **`cfg_file=/usr/local/nagios/etc/objects/windows.cfg`** |
| **`cfg_dir=`** | **Every file ending in `.cfg`** in a folder, including subfolders | **`cfg_dir=/usr/local/nagios/etc/windows-hosts`** |

With **`cfg_dir`**, I don't have to edit **`nagios.cfg`** every time I add a host. I just drop a new **`.cfg`** file into the folder.

---

## Step 1 – Create the Directory

I make sure I'm in my home directory, then go to the Nagios **`etc`** folder:

```bash
cd
cd /usr/local/nagios/etc
ls -l
```

<img width="904" height="436" alt="1" src="https://github.com/user-attachments/assets/1729bf33-4236-459e-a983-fa43d9a5741d" />

I create the folder:

```bash
sudo mkdir windows-hosts
ls -l
```

<img width="811" height="235" alt="2" src="https://github.com/user-attachments/assets/d17161cc-96e2-421d-bd65-84dfbc303987" />

The new folder is owned by **`root:root`** because I created it with **`sudo`**. I'll fix that in Step 5.

---

## Step 2 – Add the Directory to nagios.cfg

























