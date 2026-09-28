# 01 – Add a Windows Host and Services to Nagios

This doc I will add my windows host machine (`comp-a-test`) to Nagios using the sample **`windows.cfg`** object file. 

---

## Step 1 – Get the Windows Machine's Name and IP

On the **Windows** machine, open Command Prompt:

```cmd
hostname
ipconfig
```

Note the hostname and the **IPv4 Address** of the active adapter. For this project:
- Hostname: **`comp-a-test`**
- IP: **`192.168.1.151`** (reserved on the router so it doesn't change)

## Step 2 – Enable the Windows Object File

Nagios only reads object files listed in `nagios.cfg`. The Windows file is included but commented out by default.

1. List the main config files:

```bash
cd /usr/local/nagios/etc
ls -l *.cfg
```

<img width="639" height="373" alt="1" src="https://github.com/user-attachments/assets/9daf4c3b-31bc-4f9c-99df-62dd540cead4" />

2. Open the main config:

```bash
sudo nano /usr/local/nagios/etc/nagios.cfg
```


<img width="830" height="537" alt="2" src="https://github.com/user-attachments/assets/70cc9fe8-c713-49f4-a345-45b4f9636705" />

