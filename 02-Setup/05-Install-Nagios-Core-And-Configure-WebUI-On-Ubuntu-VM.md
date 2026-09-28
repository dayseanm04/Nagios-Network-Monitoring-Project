# 05 – Install Nagios Core and configure Web Interface

## Prerequisites

- **`NetworkVM`** with a static IP and working internet access (see **docs 03** and **04**)
- A user account with **`sudo`** rights
- Check out official installation doc: **https://support.nagios.com/kb/article.php?id=96**

---

## Step 1 – Check for SELinux

The Nagios install steps assume **SELinux** is disabled or in permissive mode. 

<img width="718" height="251" alt="1" src="https://github.com/user-attachments/assets/17f46d93-0c49-47bc-8637-3e2276d56405" />

---

## Step 2 – Update the System and Install Dependencies

1. Update the package list and upgrade installed packages:

```bash
sudo apt update
sudo apt upgrade
```

<img width="868" height="495" alt="2" src="https://github.com/user-attachments/assets/6f1c0096-5847-4a3d-b3bf-1b8ab8332c66" />

<img width="838" height="603" alt="3" src="https://github.com/user-attachments/assets/b4d12d89-0b43-4272-b959-2d36aa851c5e" />

2. Install the build tools, Apache, PHP, and libraries Nagios needs:

```bash
sudo apt install -y autoconf gcc libc6 make wget unzip apache2 php libapache2-mod-php libgd-dev openssl libssl-dev build-essential
```

<img width="866" height="602" alt="4" src="https://github.com/user-attachments/assets/c0119611-6d80-410f-a0b3-2d1917d4fd68" />

| Package(s) | Why it's needed |
|---|---|
| **`autoconf`**, **`gcc`**, **`make`**, **`build-essential`** | Compile Nagios and the plugins from source |
| **`apache2`** | Serves the Nagios web interface |
| **`php`**, **`libapache2-mod-php`** | Runs the PHP pages in the web interface |
| **`libgd-dev`** | Draws the status map and trend graphs |
| **`openssl`**, **`libssl-dev`** | SSL support for plugins that check HTTPS and other encrypted services |
| **`wget`**, **`unzip`** | Download and extract the source files |

---

## Step 3 – Create the Nagios User and Group

```bash
# Service account that runs the Nagios daemon
sudo useradd -m -s /bin/bash nagios

# Group that's allowed to send commands to Nagios
sudo groupadd nagcmd

# Add the nagios user and the Apache user to the command group
sudo usermod -aG nagcmd nagios
sudo usermod -aG nagcmd www-data
```
<img width="716" height="324" alt="5" src="https://github.com/user-attachments/assets/6436e759-d361-4b43-a170-412d3888cbde" />

> **Why `www-data` joins `nagcmd`:** Apache runs as **`www-data`**. Adding it to **`nagcmd`** lets the web interface send commands to Nagios, such as acknowledging an alert, scheduling downtime, or forcing a re-check.

---

## Step 4 – Download and Extract Nagios Core

```bash
cd /tmp
wget https://assets.nagios.com/downloads/nagioscore/releases/nagios-4.5.0.tar.gz
tar -xzf nagios-4.5.0.tar.gz
cd nagios-4.5.0
```

<img width="867" height="502" alt="6" src="https://github.com/user-attachments/assets/23e272d9-202c-44a5-bb7d-0650e69087b3" />

---

## Step 5 – Compile and Install Nagios Core

### 5.1 Configure the build

```bash
./configure --with-command-group=nagcmd
```

<img width="896" height="603" alt="7" src="https://github.com/user-attachments/assets/729ee880-0e01-4d34-98f4-37951bf407f9" />

### 5.2 Compile

```bash
make all
```

<img width="857" height="603" alt="8" src="https://github.com/user-attachments/assets/076df3ed-aafc-4613-a7b3-9208bf1e0e88" />

### 5.3 Install

Run each of these in order:

```bash
# Install the Nagios binaries, CGIs, and HTML files
sudo make install

# Install the systemd service file (nagios.service)
sudo make install-init

# Install the sample configuration files into /usr/local/nagios/etc
sudo make install-config

# Set up the external command directory with the nagcmd group
sudo make install-commandmode

# Install the Apache config for the Nagios web interface
sudo make install-webconf
```

<img width="869" height="507" alt="9" src="https://github.com/user-attachments/assets/2297d20f-53ce-43df-b74f-e4e9c20b4747" />

<img width="870" height="602" alt="10" src="https://github.com/user-attachments/assets/2405954b-88da-4afc-94bd-f665fe9ad666" />

---

## Step 6 – Configure the Web Interface

### 6.1 Create the web admin account

```bash
sudo htpasswd -c /usr/local/nagios/etc/htpasswd.users nagiosadmin
```

Enter and confirm a password when prompted. This is the login for the Nagios dashboard.

<img width="874" height="297" alt="11" src="https://github.com/user-attachments/assets/054a7931-c5a8-41b1-8c42-07cf855dd1ca" />

