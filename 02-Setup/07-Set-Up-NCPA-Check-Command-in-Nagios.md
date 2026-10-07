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

