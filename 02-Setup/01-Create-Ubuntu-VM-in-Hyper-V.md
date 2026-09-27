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
