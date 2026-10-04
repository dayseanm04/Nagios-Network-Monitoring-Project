# 02 – Navigate the Nagios Core Web Interface

A guide to the Nagios Core web interface: what each menu item does, what to look for on each page, and when to use it. Use this as a reference when checking the health of monitored devices or investigating a problem.

**Version note:** Most screenshots in this guide were taken on **Nagios Core 4.5.0**. The Event Log and Scheduling Queue screenshots were taken after upgrading to **4.5.14** (see **`02-Setup/`**). Version 4.5.14 has an updated look with a dark sidebar, and the **(Legacy)** versions of Map, Trends, and Histogram are no longer in the menu. The pages themselves work the same way.

## Accessing the Web Interface

go to **http: //IPAddress/nagios**, Enter the username: **nagiosadmin** and password. 

## Status Colors and States

**Host states**
| State | Meaning |
|---|---|
| 🟩 **UP** | Host responded to its check (usually ping) |
| 🟥 **DOWN** | Host didn't respond |

**Service states**

| State | Meaning |
|---|---|
| 🟩 **OK** | Check passed |
| 🟥 **CRITICAL** | Crossed the critical threshold or the check failed |

## General

### Home

The landing page after you sign in.

**nagios core version 4.5.0:**

<img width="1032" height="850" alt="1" src="https://github.com/user-attachments/assets/f78bc116-a41e-4438-ada6-907052619d7d" />

**nagios core latest version 4.5.14:**

<img width="988" height="685" alt="2" src="https://github.com/user-attachments/assets/9f6c9293-420f-4cc8-96b8-23e2de6f34d8" />

