# 05 – Install Nagios Core and configure Web Interface

## Prerequisites

- **`NetworkVM`** with a static IP and working internet access (see **docs 03** and **04**)
- A user account with **`sudo`** rights

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
