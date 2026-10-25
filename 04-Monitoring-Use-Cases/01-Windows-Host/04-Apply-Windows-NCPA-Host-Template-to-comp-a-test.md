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






















