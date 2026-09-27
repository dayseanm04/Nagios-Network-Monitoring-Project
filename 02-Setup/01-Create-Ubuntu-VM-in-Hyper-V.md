# 01 – Create the Ubuntu Monitoring VM in Hyper-V

## Prerequisites

- Windows host with the Hyper-V role enabled
- Hyper-V Manager open with administrator rights
- Ubuntu 26.04.1 LTS Desktop ISO downloaded from [ubuntu.com/download/desktop](https://ubuntu.com/download/desktop)
- At least 150 GB of free disk space and 8 GB of free RAM on the host

## Step 1 – Create the Virtual Machine

1. Open up **Hyper-V Manager**
2. In the **Actions** pane, click **New → Virtual Machine**. The **New Virtual Machine Wizard** opens.
3. **Specify Name and Location**
   - Name: **`NetworkVM`**
   - Leave the default storage location unless you want the VM stored on another drive.
4. **Specify Generation**
   - Select **Generation 2**. Gen 2 uses UEFI firmware and is the recommended option for modern 64-bit Linux distributions.
5. **Assign Memory**
   - Startup memory: **`8000`** MB
6. **Configure Networking**
   - Connection: **Not Connected**
   - The VM is built offline on purpose. Networking is configured in a later step once the OS is installed.
7. **Connect Virtual Hard Disk**
   - Select **Create a virtual hard disk**
   - Name: **`NetworkVM.vhdx`**
   - Size: **`150`** GB
8. **Installation Options**
   - Select **Install an operating system from a bootable image file**
   - Click **Browse** and select the **Ubuntu 26.04.1 Desktop ISO**.
9. **Summary** – Review the settings and confirm they match the table above.

<img width="692" height="522" alt="1" src="https://github.com/user-attachments/assets/1cb1a4ca-0387-457a-a2b2-c8518e44acc5" />

10. Click **Finish**.

The new VM now appears in the **Virtual Machines** list with a state of **Off**.

<img width="966" height="348" alt="2" src="https://github.com/user-attachments/assets/60025bfd-d6d4-4cc9-b007-cf65f35f10a2" />

---

## Step 2 – Check Secure Boot Settings (Gen 2 only)

Generation 2 VMs have Secure Boot enabled with the **Microsoft Windows** template by default, which can stop Ubuntu from booting.

1. Right-click `NetworkVM` → **Settings**.
2. Go to **Security**.
3. Make sure **Enable Secure Boot** is checked and the template is set to **Microsoft UEFI Certificate Authority**.
4. Click **Apply** → **OK**.

---

## Step 3 – Start the VM and Launch the Installer

1. Right-click **`NetworkVM`** → **Connect** to open the Virtual Machine Connection window.
2. Click **Start**.
3. At the GRUB menu, choose **Try or Install Ubuntu** and wait for the installer to load.

---

## Step 4 – Run the Ubuntu Installer

Work through the installer screens. The key choices are below; anything not listed was left at its default.

### Internet Connection

<img width="1027" height="844" alt="3" src="https://github.com/user-attachments/assets/fc7ab7cd-f566-4c1e-8436-6256428d5de2" />

Because the VM has no network adapter connected, the installer shows **No wired connection detected**.

- **Do not connect to the internet** Is already selected
- Click **Next**

### Type of Installation

<img width="1000" height="598" alt="22" src="https://github.com/user-attachments/assets/4070ab4e-92cf-49b7-b7e6-8500a20055ec" />

- Select **Interactive installation** (guided, step-by-step setup)
- Click **Next**

### Create Account

<img width="920" height="480" alt="23" src="https://github.com/user-attachments/assets/a5344b70-35d7-43d3-b658-b4f2bea5bfe8" />
