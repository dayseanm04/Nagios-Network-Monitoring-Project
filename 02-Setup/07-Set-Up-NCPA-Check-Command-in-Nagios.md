# 07 – Set Up NCPA Check Command in Nagios

In this doc I will prepares the Nagios server (**`NetworkVM`**) to talk to the NCPA agent installed on **`comp-a-test`**. I will install the **`check_ncpa.py`** plugin, stores the NCPA token securely, and creates the **`check_ncpa`** command definition.

No service checks are defined yet. That's covered in the next guide. By the end of this one, Nagios has everything it needs to run NCPA checks; it just hasn't been told which ones.

## Prerequisites
- NCPA installed and running on **`comp-a-test`** (see [**02 – Install NCPA on Windows Host**](../04-Monitoring-Use-Cases/01-Windows-Host/02-Install-NCPA-on-Windows-Host.md))
- The NCPA token set during that install

---

## Step 1 – Download check_ncpa.py

Download the plugin straight into the Nagios plugin folder:

```bash
sudo wget -O /usr/local/nagios/libexec/check_ncpa.py https://raw.githubusercontent.com/NagiosEnterprises/ncpa/master/client/check_ncpa.py
```

| Part | What it does |
|---|---|
| **`-O /usr/local/nagios/libexec/check_ncpa.py`** | Saves the file with this exact name and location |
| **`/usr/local/nagios/libexec`** | The folder where every Nagios plugin lives. **`$USER1$`** points here |
| The URL | The official plugin from Nagios Enterprises' NCPA GitHub repository |

<img width="873" height="393" alt="1" src="https://github.com/user-attachments/assets/5a1f3c05-b7ec-40d8-b656-3519004d51b5" />

---

## Step 2 – Make the Plugin Executable

```bash
sudo chmod +x /usr/local/nagios/libexec/check_ncpa.py
```

<img width="873" height="393" alt="2" src="https://github.com/user-attachments/assets/80c9d012-99cc-4b84-9bde-4b1a1a4813e3" />

Downloaded files aren't allowed to run by default. **`chmod +x`** adds execute permission so Nagios can run the plugin.

---

## Step 3 – Point the Plugin at Python 3

```bash
sudo sed -i '1s/^#!\/usr\/bin\/env python$/#!\/usr\/bin\/env python3/' /usr/local/nagios/libexec/check_ncpa.py
```

**Why this is needed:** The first line of **`check_ncpa.py`** (the *shebang*) tells Linux which program runs the script. It says **`python`**, but Ubuntu doesn't include a **`python`** command, only **`python3`**. Without this fix, the plugin fails with an error like `/usr/bin/env: 'python': No such file or directory`.

| Part | What it does |
|---|---|
| **`sed -i`** | Edits the file in place |
| **`1s/.../.../`** | Only changes line 1 |
| **`^#!\/usr\/bin\/env python$`** | Finds the exact line **`#!/usr/bin/env python`** |
| **`#!\/usr\/bin\/env python3`** | Replaces it with **`#!/usr/bin/env python3`** |

---

## Step 4 – Verify the Plugin Runs

```bash
/usr/local/nagios/libexec/check_ncpa.py --help
```

<img width="884" height="440" alt="3" src="https://github.com/user-attachments/assets/824cebfe-e8d0-479b-a5eb-97618b1390bd" />

---

## Step 5 – Store the Token in resource.cfg

1. Go to the Nagios config folder and confirm the file permissions:

```bash
cd /usr/local/nagios/etc
ls -l
```

<img width="686" height="216" alt="4" src="https://github.com/user-attachments/assets/f9b98676-3488-454a-a4cf-4fbe192bab8f" />

2. Open the file:

```bash
sudo nano resource.cfg
```

<img width="768" height="440" alt="5" src="https://github.com/user-attachments/assets/7da92e2c-6de9-4149-8b01-a6be60f9dd26" />

3. Scroll to the bottom and add the token with a comment above it:

```
# NCPA API token for Windows hosts
$USER5$=your-ncpa-token
```

<img width="884" height="415" alt="6" src="https://github.com/user-attachments/assets/00ba8c23-cc32-4db2-b3ad-4540d96529f8" />

4. Save and exit: **Ctrl+O**, **Enter**, **Ctrl+X**.

**Why `$USER5$`?** The sample file already uses or reserves the first four:

---

## Step 6 – Create the check_ncpa Command

1. Open `commands.cfg`:

```bash
cd /usr/local/nagios/etc/objects
sudo nano commands.cfg
```

<img width="665" height="279" alt="7" src="https://github.com/user-attachments/assets/1300029d-347a-4f0f-b70d-97d606fe1cd0" />

<img width="858" height="498" alt="8" src="https://github.com/user-attachments/assets/b1dc8d64-8b68-4fd4-b6b2-d17eea50c925" />

2. Scroll down and add this block. Placing it above the **SAMPLE NOTIFICATION COMMANDS** section with a comment makes it easy to find later:

<img width="693" height="252" alt="9" src="https://github.com/user-attachments/assets/ebc3ac24-eb8c-4199-94d1-0c8e171d41de" />

3. Save and exit: **Ctrl+O**, **Enter**, **Ctrl+X**.

So when a service later uses **`check_ncpa!-M cpu/percent -w 80 -c 90`**, Nagios actually runs:

```
/usr/local/nagios/libexec/check_ncpa.py -H 192.168.1.9 -t '<token>' -P 5693 -M cpu/percent -w 80 -c 90
```

---

## Step 7 – Verify the Config

Run the pre-flight check:

```bash
sudo /usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg
```

<img width="829" height="479" alt="10" src="https://github.com/user-attachments/assets/33a9faae-b7c9-4d6e-856e-6561b6f09305" />

Restart Nagios to load the new command:




