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

## Step 5 – Set Ownership and Permissions

Since I used **`sudo`** to create the folder and copy the file, both are owned by **`root`**. I change them to match the rest of the Nagios config:

```bash
sudo chown -R nagios:nagios /usr/local/nagios/etc/windows-hosts
sudo chmod 775 /usr/local/nagios/etc/windows-hosts
sudo chmod 664 /usr/local/nagios/etc/windows-hosts/*.cfg
```

<img width="845" height="496" alt="8" src="https://github.com/user-attachments/assets/ceb6daa5-7381-480d-9a88-9be05dafcfa7" />

<img width="874" height="235" alt="9" src="https://github.com/user-attachments/assets/10eb5ebe-298b-4a79-9be4-32ffdb7453b8" />

<img width="860" height="212" alt="10" src="https://github.com/user-attachments/assets/898faf99-3ad8-4049-b449-f9862bfa7cb9" />

<img width="765" height="176" alt="11" src="https://github.com/user-attachments/assets/e94700b5-e8f6-473e-a107-5ec6f33f66f8" />

| Command | What it does |
|---|---|
| **`chown -R nagios:nagios`** | Makes the **`nagios`** user and group the owner of the folder and everything in it (**`-R`** = recursive) |
| **`chmod 775`** (folder) | Owner and group can read, write, and open the folder. Everyone else can read and open it |
| **`chmod 664`** (files) | Owner and group can read and write. Everyone else can only read |

These match the `objects` folder (**`drwxrwxr-x`**) and its files (**`-rw-rw-r--`**).

> The **`x`** permission on a folder lets a user open it and see what's inside. Without it, Nagios couldn't read the files in the folder, even if the files themselves were readable.

> I don't use these permissions on files with secrets. **`resource.cfg`** stays **`660`** so only the **`nagios`** user and group can read the NCPA token.

---

## Step 6 – Move the Hostgroup Into Its Own File

### Why

**`comp-a-test.cfg`** should only hold **`comp-a-test`** and its services. But the copied file also includes the **`windows-servers`** hostgroup, which is shared by **every** Windows host. Every host that uses the **`windows-server`** template joins it automatically.

| Belongs to `comp-a-test` only | Shared by all Windows hosts |
|---|---|
| **`define host`** for **`comp-a-test`** | **`define hostgroup`** for **`windows-servers`** |

If the hostgroup stays in **`comp-a-test.cfg`**:

- **Adding a host breaks the config.** Copying **`comp-a-test.cfg`** to make **`comp-b-test.cfg`** would define **`windows-servers`** twice, causing **`Duplicate definition found for hostgroup 'windows-servers'`**.
- **Removing a host breaks the config.** Deleting **`comp-a-test.cfg`** would delete the hostgroup, and every other Windows host would fail with **`Could not find any hostgroup matching 'windows-servers'`**.

### Remove it from comp-a-test.cfg

```bash
cd windows-hosts
sudo nano comp-a-test.cfg
```

<img width="911" height="508" alt="12" src="https://github.com/user-attachments/assets/0cecbbf5-15aa-415c-b171-0e7c0c20fa8b" />

I delete the **HOST GROUP DEFINITIONS** section:

<img width="893" height="413" alt="13" src="https://github.com/user-attachments/assets/bc662be1-263e-4ce0-a636-9ece087abb77" />

Save: **Ctrl+O**, **Enter**, **Ctrl+X**.

### Create windows-hostgroup.cfg

```bash
sudo nano windows-hostgroup.cfg
```

<img width="780" height="310" alt="14" src="https://github.com/user-attachments/assets/22547772-db02-4e32-8472-8c3929d5c5cd" />

Save: **Ctrl+O**, **Enter**, **Ctrl+X**.

I set its ownership and permissions to match:

```bash
sudo chown nagios:nagios /usr/local/nagios/etc/windows-hosts/windows-hostgroup.cfg
```




























