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


