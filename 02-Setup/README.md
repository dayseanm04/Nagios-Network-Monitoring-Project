# 02 – Setup

Building the Nagios monitoring server, from creating the VM to custom templates.

| Doc | What it covers |
|---|---|
| [**01 – Create the Ubuntu Monitoring VM in Hyper-V**](01-Create-Ubuntu-VM-in-Hyper-V.md) | Create the Ubuntu VM (NetworkVM) in Hyper-V |
| [**02 – Configure the Hyper-V Virtual Switch**](02-Configure-Hyper-V-Virtual-Switch.md) | Create the external virtual switch (Net-MGNT-SW) bridged to the Wi-Fi adapter |
| [**03 – Assign a Static IP to NetworkVM**](03-Configure-Static-IPv4-Address-For-The-NetworkVM.md) | Give NetworkVM a static IP after it got an APIPA address |
| [**04 – Create a DHCP Reservation for NetworkVM**](04-Create-DHCP-Reservation-for-NetworkVM.md) | Reserve NetworkVM's IP on the router |
| [**05 – Install Nagios Core and configure Web Interface**](05-Install-Nagios-Core-And-Configure-WebUI-On-Ubuntu-VM.md) | Compile Nagios Core from source and set up the web UI |
| [**06 – Update Nagios Core on Ubuntu VM**](06-Update-Nagios-Core-on-Ubuntu-VM-to-4.5.14.md) | Upgrade Nagios Core from 4.5.0 to 4.5.14 |
| [**07 – Set Up NCPA Check Command in Nagios**](07-Set-Up-NCPA-Check-Command-in-Nagios.md) | Add the check_ncpa command and store the NCPA token in resource.cfg |
| [**08 – Restructure Nagios Config for Multiple Hosts**](08-Restructure-Nagios-Config-for-Multiple-Hosts.md) | Move to one config file per host with cfg_dir |
| [**09 – Create Custom Nagios Templates [Time Sensitive Service Template]**](09-Create-Time-Sensitive-Service-Template.md) | Custom template for CPU, memory, and disk checks |
| [**10 – Create Notify-Early, Network Device, and Windows NCPA Templates**](10-Create-Notify-Early-Network-Device-and-Windows-NCPA-Templates.md) | Custom templates for notify-early services, network devices, and Windows NCPA hosts |
