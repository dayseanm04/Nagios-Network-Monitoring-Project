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
- IP: **`192.168.1.9`** (reserved on the router so it doesn't change)

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

3. Find the section **Definitions for monitoring a Windows machine** and remove the **`#`** at the start of this line:

<img width="759" height="435" alt="3" src="https://github.com/user-attachments/assets/ed52ee10-c93a-46d9-8735-b77398d96362" />

4. Save and exit: **Ctrl+O**, **Enter**, **Ctrl+X**.

---

## Step 3 – Define the Windows Host

1. Open the Windows object file:

```bash
cd /usr/local/nagios/etc/objects
ls -l
sudo nano windows.cfg
```

<img width="731" height="396" alt="4" src="https://github.com/user-attachments/assets/a523bbb9-9c9a-4656-8b31-002af03bc8fe" />

<img width="861" height="315" alt="5" src="https://github.com/user-attachments/assets/dfd6f008-37c2-4f8b-978a-30bf3d056f5b" />

| Directive | What it does |
|---|---|
| `use` | Inherits check interval, notification, and host group settings from the `windows-server` template in `templates.cfg` |
| `host_name` | Short name Nagios uses to link services to this host. Every service below must use this exact name |
| `alias` | Friendly name shown in the web interface |
| `address` | IP address Nagios pings and connects to |

---

## Step 4 – Point the Services at the Host

Further down in **`windows.cfg`** are seven service definitions. Each one has a **`host_name`** line that must match the host defined in **Step 3**.

Example: 

<img width="879" height="443" alt="6" src="https://github.com/user-attachments/assets/83023e37-594e-4093-aec6-d5de3f14898e" />

Im only showing the NSClient++ Version block is. And I changed the **`host_name`** in all seven service definitions.
- Updated **`host_name`** to **`comp-a-test`** in **all seven** service definitions:

Save and exit: **Ctrl+O**, **Enter**, **Ctrl+X**.

---

## Step 5 – Verify the Config and Restart Nagios


1. Run the pre-flight check. Do this every time you change any Nagios config file:

```bash
sudo /usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg
```

<img width="867" height="604" alt="7" src="https://github.com/user-attachments/assets/ee4da552-fdf4-4203-aeca-518c6c87d11a" />

2. Restart Nagios to load the new config:

```bash
sudo systemctl restart nagios
```

---

## Step 6 – Check the Dashboard

1. Go to `http://192.168.1.99/nagios` and sign in.
2. Click **Tactical Overview**.

<img width="1024" height="812" alt="8" src="https://github.com/user-attachments/assets/7f65187c-0278-41e9-9dd3-991e069755f0" />

3. Click **Hosts**.

<img width="1029" height="566" alt="9" src="https://github.com/user-attachments/assets/b6397b71-dd77-4393-92f3-51477ce809de" />

4. Click **Alerts → History** to see each check result as it came in.

<img width="1029" height="800" alt="10" src="https://github.com/user-attachments/assets/c7d856e7-15dc-42c4-a04b-24df33c78159" />

| Host | Status | Status Information |
|---|---|---|
| `comp-a-test` | DOWN | Host check timed out after 30.01 seconds |
| `localhost` | UP | PING OK - Packet loss = 0% |
| Services **CRITICAL**, `Socket timeout` | No agent is listening on TCP `12489`, so `check_nt` has nothing to connect to |

## Why the Host Shows DOWN

These results mean Nagios is configured correctly and actively checking the Windows machine. The Windows side isn't answering yet:

| Result | Cause |
|---|---|
| Host **DOWN**, `PING CRITICAL - Packet loss = 100%` | Windows Firewall blocks inbound ping by default |
