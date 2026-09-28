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
