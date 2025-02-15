# Description  
This guide outlines the process of installing the Debian Linux distribution as a virtual machine (VM) on **Proxmox VE**.

### Why Choose Debian?
- **Stable**: Known for its reliability and long-term support.
- **User-friendly**: Easy to set up and manage.
- **Versatile**: A wide range of applications can run on Debian.
- **Lightweight**: Ideal for resource-conscious environments.

---

## Installation Preparation  

Go to the [Debian official website](https://www.debian.org/distrib/) and copy the link to the 64-bit ISO image.

---

## Installation Process  

### Step 1: Download the ISO Image

1. In the **Proxmox WEB GUI**, navigate to **Storage → ISO Images → Download from URL**.
2. Paste the copied URL into the **ISO Image** field and click **Query URL**.
3. Click **Download** and wait for the "TASK OK" message to confirm the download is complete. Once done, close the window.

### Step 2: Create a New Virtual Machine

1. In the **Proxmox WEB GUI**, click **Create VM** and fill in the configuration panel:
    - **VM ID**: Choose a unique ID for the VM.
    - **ISO Image**: Select the ISO image downloaded in Step 1.
    - **Qemu checkbox**: Check this option. ([Why this is necessary?](https://pve.proxmox.com/wiki/Qemu-guest-agent))
    - **SCSI Controller**: Select **Discard** (SSD only, based on my hardware).
    - **Disk Size**: Set to **12 GiB**.
    - **Other settings**: Leave them as default.

   ![Proxmox VM Configuration](https://github.com/user-attachments/assets/54c9b2cb-5c0a-4423-a3e8-87c316e0083a)

### Step 3: Install Debian

1. Start the Debian VM.
2. Go to the **Console** tab to view the installation screen.
3. Follow the installation prompts, leaving most settings at their default values.

---

## Lessons Learned  
- How to download an ISO image directly to storage via URL in Proxmox.
- How to create and configure virtual machines in Proxmox.
- The process of installing Debian on a virtual machine.

---
