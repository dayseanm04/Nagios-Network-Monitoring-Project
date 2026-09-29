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

2. Scroll and find the the `cfg_file=` you need eg: **cfg_file=/usr/local/nagios/etc/objects/windows.cf**.
3. Remove the `#` at the start of the line so Nagios reads the file:

```
# Before
#cfg_file=/usr/local/nagios/etc/objects/<CONFIG_FILE>

# After
cfg_file=/usr/local/nagios/etc/objects/<CONFIG_FILE>
```

4. Save and exit: **Ctrl+O**, **Enter**, **Ctrl+X**.

Skip this step if the line is already uncommented, or if you're using `localhost.cfg`, which is enabled by default.

---

## Step 2 – Define the Host

1. Open the object file:

```bash
sudo nano /usr/local/nagios/etc/objects/<CONFIG_FILE>
```

2. Scroll to the `define host` section and edit it:

```
define host {
    use             <HOST_TEMPLATE>     ; Template to inherit settings from
    host_name       <HOST_NAME>         ; Short name Nagios uses for this host
    alias           <HOST_ALIAS>        ; Friendly name shown in the dashboard
    address         <HOST_IP>           ; IP address of the host
}
```

| Directive | What to change |
|---|---|
| `use` | The host template for this device type (see the table above) |
| `host_name` | A short, unique name with no spaces. Services link to the host using this exact name |
| `alias` | A readable description of the device |
| `address` | The device's IP address |

3. If the file has sample services, update their `host_name` to match (see Section 2, Step 1), or the pre-flight check will fail.

5. Save and exit: **Ctrl+O**, **Enter**, **Ctrl+X**.

---

## Step 3 – Verify and Restart

1. Run the pre-flight check:

```bash
sudo /usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg
```

2. Confirm the result shows:

```
Total Warnings: 0
Total Errors:   0
```

3. If there are 0 errors, restart Nagios:

```bash
sudo systemctl restart nagios
```

---

## Step 4 – Confirm the Host in the Dashboard

1. Go to `http://192.168.1.99/nagios` and sign in.
2. Click **Tactical Overview**, then **Hosts**.
3. `<HOST_NAME>` should be listed.

---

# Section 2 – Add a Service

A service is a single check on a host, such as ping, disk space, or whether a web server is responding. Every service must be attached to a host that already exists (Section 1).

## Step 1 – Edit an Existing Service

Use this when the object file already has a sample service you want to keep.

1. Open the object file:

```bash
sudo nano /usr/local/nagios/etc/objects/<CONFIG_FILE>
```

2. Scroll to the **`define service`** sections.
3. Change **`host_name`** in each service you want to use:

```
define service {
    use                     generic-service
    host_name               <HOST_NAME>
    service_description     <SERVICE_NAME>
    check_command           <CHECK_COMMAND>
}
```

4. Delete or comment out any sample services you don't need. Put a `#` at the start of every line in the block, including `define service {` and the closing `}`.
