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
