# 03 – Nagios Host and Service Definitions Explained

A line-by-line breakdown of the two most common Nagios configuration blocks: the **host** definition and the **service** definition. Use this guide to understand what each line does before adding or editing devices and checks in Nagios.

It covers:

- **Part 1** – Host definitions
- **Part 2** – Service definitions using NSClient++ (**`check_nt`**)
- **Part 3** – Service definitions using NCPA (**`check_ncpa`**)

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

**`address 192.168.1.9`**
This is the network location of the machine. Nagios uses this exact IP address, or a fully qualified domain name like **`server01.example.com`**, to send pings and queries to the host.

**`}`**
This closes the configuration block.

---

# Part 2 – Service Definition Using NSClient++

## Open the file

```bash
cd /usr/local/nagios/etc/objects
sudo nano windows.cfg
```

Scroll down to the **SERVICE DEFINITIONS** section.

<img width="882" height="430" alt="4" src="https://github.com/user-attachments/assets/2a9daa35-b5f6-4a2c-8cec-b261083279d1" />

For this project, I commented out the NSClient++ service blocks (every line starts with `#`) because I will use NCPA for the Windows. A **`#`** at the start of a line tells Nagios to ignore it. It's not part of the syntax.

## Line-by-line breakdown

**`define service {`**
This opens the configuration block. It tells Nagios that you are creating a new service check (a metric to monitor), rather than defining a machine (host) or a contact.

**`use generic-service`**
This tells Nagios to inherit default settings from a template called **`generic-service`**. This template is defined in **`templates.cfg`** and automatically fills in background settings like how often to run the check, who to notify if it breaks, and when to retry. 

**`host_name comp-a-test`**
This links this specific service check to a machine you've already defined in Nagios. Nagios needs to know which machine it's supposed to run this check against. It must match the host's **`host_name`** exactly.

**`service_description NSClient++ Version`**
This is simply the label or display name. It's exactly what you'll see in the Nagios web interface under the **Service** column for that machine.

**`check_command check_nt!CLIENTVERSION`**
This is the command that does the actual work. It's broken into two parts by the exclamation mark (**`!`**):

- **`check_nt`**: The command defined in **`commands.cfg`**. It tells Nagios to use the **`check_nt`** plugin to reach out to the Windows machine over network port **`12489`**, where the NSClient++ agent listens.
- **`CLIENTVERSION`**: The argument passed to the plugin. It tells **`check_nt`** exactly what data to ask the remote machine for. It tells the NSClient++ agent: **"Reply back and tell me exactly what version of NSClient++ you're running."**

**`}`**
This closes the configuration block.

> **Note:** This service requires the **NSClient++** agent installed on the Windows machine. Without it, the check fails with `Socket timeout` or `No route to host`.

---

# Part 3 – Service Definition Using NCPA

NCPA (Nagios Cross-Platform Agent) is the agent I will use in this project uses on the Windows host. It works differently from NSClient++: it uses HTTPS on port **`5693`**, requires a token, and asks for data using APIs.

Before Nagios can run an NCPA check, three things need to be in place:

1. The **`check_ncpa.py`** plugin installed in **`/usr/local/nagios/libexec`** on the Nagios server
2. The **NCPA token** stored in `resource.cfg`
3. A **`check_ncpa` command** defined in **`commands.cfg`**

## Step 1 – Store the token in resource.cfg

```bash
sudo nano /usr/local/nagios/etc/resource.cfg
```

Add this line at the bottom, using your own token:

```
$USER2$=your-ncpa-token
```

**Why store it here?**
**`resource.cfg`** holds private values that Nagios can use in commands without showing them anywhere else. It's only readable by the nagios user and groupe. If you ever change the token in NCPA, you only update it in this one place.

**Note**: `$USERn`$ is the only kind of variable Nagios lets you define in resource.cfg. 

**"USER"** doesn't mean a username. It means user-defined macro. Nagios gives you up to 256 of them, **$USER1$** through $USER256$, and they can only be set in a resource file. You can't make up your own name like **`$NCPA_TOKEN`$**; Nagios won't recognize it.






