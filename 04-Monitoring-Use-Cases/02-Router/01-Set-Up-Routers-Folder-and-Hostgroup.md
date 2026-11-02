# 01 – Set Up Routers Folder and Hostgroup

In this doc, I will prepare Nagios to monitor routers. I'll create a **`routers`** folder with its own hostgroup, the same way I organized Windows hosts, and use the sample **`switch.cfg`** that comes with Nagios as the starting point for my router's config file.  I'll also update my **`network-device-host`** template so it's ready for network devices.

---

## Overview

### Before and After

**Before:**
```
/usr/local/nagios/etc/
├── objects/
├── templates/
└── windows-hosts/
```

**After:**
```
/usr/local/nagios/etc/
├── objects/
├── routers/                    ← new
│   ├── routers-hostgroup.cfg   ← routers hostgroup (shared)
│   └── verizon-r1.cfg          ← the router (configured in the next doc)
├── templates/
└── windows-hosts/
```
