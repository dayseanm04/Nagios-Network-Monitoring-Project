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

I open the main config:

```bash
sudo nano nagios.cfg
```

I scroll down to the **`cfg_dir`** section. It already has commented-out examples like **`servers`**, **`printers`**, **`switches`**, and **`routers`**. Under them, I add:

<img width="873" height="382" alt="3" src="https://github.com/user-attachments/assets/28dbce63-4c07-40de-95ee-47b0470c2c65" />

Save: **Ctrl+O**, **Enter**, **Ctrl+X**.

---

## Step 3 – Copy windows.cfg and Rename It

I go into the new folder and copy **`windows.cfg`** from **`objects`**:

```bash
cd windows-hosts
sudo cp /usr/local/nagios/etc/objects/windows.cfg .
ls -l
```

<img width="937" height="378" alt="4" src="https://github.com/user-attachments/assets/9da42d28-520e-43be-acb1-e7268b366939" />

The **`.`** at the end means **"copy it here,"** into the current folder.

I rename it after the host it's for:

```bash
sudo mv windows.cfg comp-a-test.cfg
ls -l
```

<img width="958" height="437" alt="5" src="https://github.com/user-attachments/assets/afe5eced-a285-4737-8ccc-d811bd1fadeb" />

Naming each file after its host means I can tell what's in it without opening it, and I can find any host's config with **`ls`**.

> The file must end in **`.cfg`**. **`cfg_dir`** skips anything else, like **`.conf`** or **`.txt`**.

---

## Step 4 – Disable the Old windows.cfg

Now **`comp-a-test`** is defined in two places: **`objects/windows.cfg`** and **`windows-hosts/comp-a-test.cfg`**. If both load, the pre-flight check fails with:

```
Error: Duplicate definition found for host 'comp-a-test'
```

I go back to the **`etc`** folder and open **`nagios.cfg`**:

```bash
cd ..
sudo nano nagios.cfg
```

<img width="781" height="215" alt="6" src="https://github.com/user-attachments/assets/180e4be3-2eb8-4916-b7ec-89ff01a5848b" />

I comment out the old **`windows.cfg`** line:

```
# Definitions for monitoring a Windows machine
#cfg_file=/usr/local/nagios/etc/objects/windows.cfg
```

<img width="925" height="415" alt="7" src="https://github.com/user-attachments/assets/4df08ff9-6f51-4d1d-acb8-0530b3962632" />

Save: **Ctrl+O**, **Enter**, **Ctrl+X**.

I keep **`objects/windows.cfg`** as a reference copy. It won't load as long as that line is commented out.



















