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
