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

**Example:** 

<img width="882" height="430" alt="1" src="https://github.com/user-attachments/assets/d93a6e66-f053-4920-bb15-f57c075ce1bc" />

One host can have many services. Every service belongs to exactly one host. So in your configuration file, you use **`define host`** to tell Nagios **"this machine exists,"** and **`define service`** to tell Nagios **"I am about to create a specific check for that machine."**

---

## Where to find the Definitions

| File | What it holds |
|---|---|
| **`/usr/local/nagios/etc/nagios.cfg`** | Main config. Lists which object files Nagios reads (**`cfg_file=`** lines). Hosts and services are **not** defined here |

