# 03 – Nagios Host and Service Definitions Explained

A line-by-line breakdown of the two most common Nagios configuration blocks: the **host** definition and the **service** definition. Use this guide to understand what each line does before adding or editing devices and checks in Nagios.

It covers:

- **Part 1** – Host definitions
- **Part 2** – Service definitions using NSClient++ (`check_nt`)
- **Part 3** – Service definitions using NCPA (`check_ncpa`)

---

## Hosts vs. Services

In Nagios, a **service** is simply the label for any individual metric or health check you want to track.

The word **"service"** can be confusing because we usually think of a Windows Service, like a background program. But in Nagios terminology, a service is just an item or attribute you are monitoring on a machine (host).

To make it clear, look at the hierarchy:

| Object | What it is | Example |
|---|---|---|
| **Host** | The physical or virtual machine itself | `comp-a-test` (the Windows host) |
| **Service** | A specific health metric you want to watch on that machine | CPU Usage, Memory Usage, C:\ Drive Space |

One host can have many services. Every service belongs to exactly one host. So in your configuration file, you use **`define host`** to tell Nagios **"this machine exists,"** and **`define service`** to tell Nagios **"I am about to create a specific check for that machine."**

---

## Where to find the Definitions

Nagios keeps its main settings in **`/usr/local/nagios/etc/`**, and almost all of the host, service, and command definitions in the **`objects/`** folder inside it.


```
/usr/local/nagios/etc/
├── nagios.cfg          ← main config; lists which object files to read
├── resource.cfg        ← private variables ($USER1$, tokens)
├── cgi.cfg             ← web interface settings
├── htpasswd.users      ← web interface logins
└── objects/            ← individual object config files
    ├── commands.cfg
    ├── contacts.cfg
    ├── localhost.cfg
    ├── printer.cfg
    ├── switch.cfg
    ├── templates.cfg
    ├── timeperiods.cfg
    └── windows.cfg
```

### Main folder – `/usr/local/nagios/etc/`

<img width="686" height="216" alt="1" src="https://github.com/user-attachments/assets/3b587fd1-592a-4e86-9746-eb19c0c37057" />

| File | What it holds |
|---|---|
| **`nagios.cfg`** | Main config. Lists which object files Nagios reads (`cfg_file=` lines). Hosts and services are **not** defined here |
| **`resource.cfg`** | Private variables like `$USER1$` (the plugin folder) and secrets like the NCPA token (`$USER2$`) |
| **`cgi.cfg`** | Settings for the web interface, like which users can see and run commands |
| **`htpasswd.users`** | Usernames and hashed passwords for logging in to the web interface |

### Objects folder – `/usr/local/nagios/etc/objects/`

This folder holds the individual config files where you define what Nagios monitors and how.

<img width="684" height="281" alt="2" src="https://github.com/user-attachments/assets/51e4e616-cdc0-4dfd-9910-1bc285250ea1" />

| File | What it holds |
|---|---|
| **`windows.cfg`** | Host and service definitions for Windows machines|
| **`templates.cfg`** | Templates like `windows-server` and `generic-service` that hosts and services inherit from |
| **`commands.cfg`** | Command definitions like **`check_nt`**, **`check_ping`**, and **`check_ncpa`** |
| **`contacts.cfg`** | Who gets notified (e.g. `nagiosadmin`) and how |
| `**timeperiods.cfg`** | When checks and notifications happen (e.g. **`24x7`**, **`workhours`**) |

A file in **`objects/`** only takes effect if its **`cfg_file=`** line is uncommented in **`nagios.cfg**`. For example, **`windows.cfg`** was commented out by default and had to be enabled before the Windows host showed up in Nagios.

---

# Part 1 – Host Definition

## Open the file

```bash
cd /usr/local/nagios/etc/objects
sudo nano windows.cfg
```

**Note**: **`cd`** on its own takes you back to your home directory if you're somewhere else. You don't need it before an absolute path like the one above.

## The block

This project's Windows host:

<img width="775" height="321" alt="3" src="https://github.com/user-attachments/assets/abd8540e-50ae-44eb-b785-484856b7ec60" />

Text after a **`;`** is a comment. Nagios ignores it, so it's a good place to leave notes.

## Line-by-line breakdown

**`define host {`**
This opens the configuration block. It tells Nagios: **"I am registering a new hardware/machine in the system."**

**`use windows-server`**
This tells Nagios to inherit default settings from a template called **`windows-server`**. This template is pre-defined in **`templates.cfg`** and automatically configures background settings like how often to ping the machine to see if it's online, what hours to monitor it (24x7), and who to notify if the host goes down. 

Without a template, you'd have to type all of those settings into every host.

**`host_name comp-a-test`**
This is the short, unique ID for the machine. It must contain no spaces. This is the exact name you will use later inside your **`define service`** blocks to link health checks to this specific machine.

**`alias My Windows Host`**
This is a longer, user-friendly description or nickname for the machine. It allows spaces and capital letters so people looking at the Nagios web interface can easily understand what the machine does. In a real company this might be something like **`Accounting Database Server`**.



