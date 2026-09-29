# TEMPLATE – Add a Host and Service to Nagios

Reusable steps for adding any device to Nagios and setting up its service checks. Replace every value in **`<ANGLE_BRACKETS>`** with the details of the device you're adding.

---

## Fill In Before You Start

| Placeholder | Description | Example |
|---|---|---|
| **`<CONFIG_FILE>`** | Object file for this device type | **`windows.cfg`** |
| **`<HOST_TEMPLATE>`** | Host template to inherit settings from | **`windows-server`** |
| **`<HOST_NAME>`** | Short name Nagios uses for the device (no spaces) | **`PC-01`** |
| **`<HOST_ALIAS>`** | Friendly name shown in the dashboard | **`My Windows Host`** |
| **`<HOST_IP>`** | Device's IPv4 address | `10.10.10.1` |
| **`<SERVICE_NAME>`** | Name of the check shown in the dashboard | **`PING`** |
| **`<CHECK_COMMAND>`** | Command and arguments Nagios runs | **`check_ping!100.0,20%!500.0,60%`** |

### Choosing a Config File and Host Template

Nagios come with sample object files for common device types. Pick the one that matches the device:
| Device type | `<CONFIG_FILE>` | `<HOST_TEMPLATE>` |
|---|---|---|
| Windows machine | **`windows.cfg`** | **`windows-server`** |
| Linux machine | **`localhost.cfg`** | **`linux-server`** |
| Router or switch | **`switch.cfg`** | **`generic-switch`** |
| Printer | **`printer.cfg`** | **`generic-printer`** |

**Tip:** Assign each device (server, printer, network devices etc) a static IP or a DHCP reservation before adding it. If its IP changes, Nagios will report it as DOWN.

---

# Section 1 – Add a Host

## Step 1 – Enable the Config File

1. Open the main Nagios configuration file:

```bash
sudo nano /usr/local/nagios/etc/nagios.cfg
```

2. Scroll and find the the `cfg_file=` you need eg"**cfg_file=/usr/local/nagios/etc/objects/windows.cf**".
3. Remove the `#` at the start of the line so Nagios reads the file:


