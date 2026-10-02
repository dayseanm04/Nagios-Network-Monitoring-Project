# 06 – Update Nagios Core on Ubuntu VM


## Why Upgrade?

The Nagios web interface showed **A new version of Nagios Core is available!** Clicking **Check for updates** confirmed the install was outdated.

---

## Prerequisites
- Nagios Core installed from source (see [**05 – Install Nagios Core and Web Interface on Ubuntu VM**](05-Install-Nagios-Core-And-Configure-WebUI-On-Ubuntu-VM.md))
- Internet access on `NetworkVM`

---

## Step 1 – Check the Current Version
1. Go to **`http://192.168.1.99/nagios`** and sign in.

<img width="1032" height="850" alt="1" src="https://github.com/user-attachments/assets/ba23e157-87a2-4fd3-a403-8231c992cde0" />

2. The home page shows the installed version (**4.5.0**) and a banner saying a new version is available.

3. Click **Check for updates**. The Nagios site confirms the update and shows the latest version.
<img width="968" height="652" alt="2" src="https://github.com/user-attachments/assets/b34eeaf9-9ede-41f3-ae59-bbe57a2ed6f3" />

## Step 2 – Stop Nagios and Apache

```bash
sudo systemctl stop nagios
sudo systemctl stop apache2
```

<img width="878" height="293" alt="3" src="https://github.com/user-attachments/assets/89ad7112-2b8c-4159-9b91-f2c180dbae87" />

If you see this warning, Reload systemd, then stop Nagios again

## Step 3 – Download the 4.5.14 Source Code

```bash
cd /tmp

# Remove any old Nagios Core downloads
sudo rm -rf nagioscore*

# Download version 4.5.14 from the official GitHub repository
wget -O nagioscore.tar.gz https://github.com/NagiosEnterprises/nagioscore/archive/refs/tags/nagios-4.5.14.tar.gz
```

<img width="883" height="497" alt="4" src="https://github.com/user-attachments/assets/fa4669a4-f72a-41ea-b536-96d1650882cd" />
