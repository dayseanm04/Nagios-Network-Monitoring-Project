# 10 – Create Notify-Early, Network Device, and Windows NCPA Templates

In this doc, I will add four more templates to my custom templates file: two **service** templates and two **host** templates. These prepare my config for faster alerts on critical services, monitoring network devices like my router, and giving every Windows host monitored by NCPA the same settings. 

The templates folder and file were set up in [**09 – Create Time-Sensitive Service Template**](09-Create-Time-Sensitive-Service-Template.md).

---

### How they inherit

```
SERVICE TEMPLATES                          HOST TEMPLATES

generic-service  (Nagios default)          generic-host  (Nagios default)
   ├── time-sensitive   (doc 09)              ├── windows-server  (Nagios default)
   └── notify-early                           │      └── windows-ncpa-host
          └── network-device-service          └── network-device-host
```

Each template only lists the settings it **changes**. Everything else comes from the template above it.

---

## Step 1 – Open the Templates File

```bash
cd /usr/local/nagios/etc/templates
ls -l
sudo nano custom-templates.cfg
```

<img width="845" height="486" alt="1" src="https://github.com/user-attachments/assets/89b6fbd6-8a31-4cc2-a0cb-b04279d1ae88" />

The file already contains my **`time-sensitive`** template from doc 09.

<img width="1027" height="623" alt="2" src="https://github.com/user-attachments/assets/e826f6ee-b943-4e5a-a322-98c1c5b9d00c" />

---

# Service Templates

## Template 1 – notify-early

**Purpose:** Alert on the **first** failure for critical services where any failure matters, like DNS or the gateway.

<img width="944" height="661" alt="3" src="https://github.com/user-attachments/assets/e18a6645-82a9-4239-a21b-6a04312caf3c" />

| Setting | What it does |
|---|---|
| **`check_interval 1`** | Runs the check every minute |
| **`retry_interval 1`** | Rechecks every minute when not OK |
| **`max_check_attempts 1`** | The **first** failed check goes straight to HARD and sends an alert. There's no SOFT state |
| **`notification_interval 10`** | Resends the alert every 10 minutes until it's fixed |
































