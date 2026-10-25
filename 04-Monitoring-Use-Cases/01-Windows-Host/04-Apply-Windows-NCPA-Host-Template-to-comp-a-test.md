# 04 – Apply Windows NCPA Host Template to comp-a-test

In this doc, I will switch **`comp-a-test`** from the default **`windows-server`** host template to my custom **`windows-ncpa-host`** template, and clean up the leftover sample comments in its config file.

> **Testing and validation for this change:** [Click here to see the Windows NCPA host template validation](../../06-Testing-and-Validation/03-Validate-Windows-NCPA-Host-Template.md)

I created **`windows-ncpa-host`** template in [**02-Setup / 10 – Create Notify-Early, Network Device, and Windows NCPA Templates**](../../02-Setup/10-Create-Notify-Early-Network-Device-and-Windows-NCPA-Templates.md).

---

## Step 1 – Open the Host File

I go to the Windows hosts folder:

```bash
cd /usr/local/nagios/etc/windows-hosts
ls -l
```

<img width="889" height="488" alt="1" src="https://github.com/user-attachments/assets/53c93fc8-30f7-498a-b4b3-dddbfd2a368d" />

The folder has **`comp-a-test.cfg`** and **`windows-hostgroup.cfg`**. I open the host file:

```bash
sudo nano comp-a-test.cfg
```

<img width="861" height="695" alt="2" src="https://github.com/user-attachments/assets/9966f3ef-bd76-4baf-ab70-898214b91799" />

---

## Step 2 – Clean Up the Sample Comments

I created **`comp-a-test.cfg`** by copying the sample **`windows.cfg`** that comes with Nagios, so its header still describes the sample file. That's no longer accurate, so I replace it with a header that describes what this file actually is:

<img width="984" height="679" alt="3" src="https://github.com/user-attachments/assets/f31c7d09-5f6d-49dc-b560-6e553dc0fc5c" />

> Comments don't change how Nagios works, but a header that says **`WINDOWS.CFG - SAMPLE`** in a file named **`comp-a-test.cfg`** is misleading to anyone reading it later.

---

## Step 3 – Change the Template

In the host definition, I change **`use windows-server`** to **`use windows-ncpa-host`**:

<img width="990" height="495" alt="4" src="https://github.com/user-attachments/assets/2facb7e8-e065-44e4-ad22-a39f8baebd8f" />

Only the **`use`** line changes. The host name, alias, and address stay the same.

Save: **Ctrl+O**, **Enter**, **Ctrl+X**.

---

## Step 4 – Run the Pre-Flight Check

```bash
sudo /usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg
```

<img width="997" height="735" alt="5" src="https://github.com/user-attachments/assets/82850fbd-dd5d-4eda-a543-34936190f505" />

---

## Step 5 – Restart Nagios

```bash
sudo systemctl restart nagios
```

<img width="878" height="237" alt="6" src="https://github.com/user-attachments/assets/9a090a66-9ca1-4ad5-aa6a-d29c29f79639" />

---

## Result

**`comp-a-test`** now uses my custom **`windows-ncpa-host`** template, and its config file has a header for **`comp-a-test`** instead of the leftover sample comments. The pre-flight check passes with 0 warnings and 0 errors, and Nagios has been restarted.





















