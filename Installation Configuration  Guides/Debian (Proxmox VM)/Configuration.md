# Description:  
This guide covers Debian VM configuration.

**Contents:**  
[Updating Packages](https://github.com/sapan322/Raman-Cybersecurity-Portfolio/blob/main/Installation%20Configuration%20%20Guides/Debian%20(Proxmox%20VM)/Configuration.md#updating-packages)  

[Fix Low FPS in Debian VM](https://github.com/sapan322/Raman-Cybersecurity-Portfolio/blob/main/Installation%20Configuration%20%20Guides/Debian%20(Proxmox%20VM)/Configuration.md#fix-low-fps-in-debian-vm)

[Install OpenSSH Server](https://github.com/sapan322/Raman-Cybersecurity-Portfolio/blob/main/Installation%20Configuration%20%20Guides/Debian%20(Proxmox%20VM)/Configuration.md#install-openssh-server)

[Turn off sleep/suspend/hibernate in Debian 12](https://github.com/sapan322/Raman-Cybersecurity-Portfolio/blob/main/Installation%20Configuration%20%20Guides/Debian%20(Proxmox%20VM)/Configuration.md#turn-off-sleepsuspendhibernate-in-debian-12)

---

## Updating Packages  

Regularly updating packages ensures security fixes, stability improvements, and access to the latest features.  

### Steps:  

1. Open the **Terminal**.  
2. Log in as the root user:  
       `su`  
3. Update and upgrade system packages:  
       `apt update && apt upgrade -y`  

### Lessons Learned:  
- How to log in as the root user.  
- How to update Debian packages efficiently.  

---


## Fix Low FPS in Debian VM  

If your Debian VM in Proxmox has low FPS or sluggish graphical performance, adjusting the display settings can help.  

### Steps:  

1. Open **Proxmox Web GUI**.  
2. Navigate to **VM → Hardware → Display**.  
3. Change the display type to **VirtIO-GPU** and set **Memory to 128 MiB**.  
4. Click **OK** and restart the VM for the changes to take effect.  

### Lessons Learned:  
- How to improve graphical performance in a Debian VM.  
- How to adjust VM display settings in Proxmox for better performance.  
- How increasing video memory can enhance responsiveness.

---

## Install OpenSSH Server  

OpenSSH is a suite of secure networking utilities based on the Secure Shell (SSH) protocol, allowing secure remote access to the system.  

### Steps:  

1. Open the **Terminal**.  
2. Log in as the root user:  
   - `su`  
3. Install the OpenSSH server:  
   - `apt-get install openssh-server`  
4. Verify that the SSH service is running:  
   - `systemctl status ssh`  

### Lessons Learned:  
- How to install and configure an SSH server in Debian.  
- How to check if the SSH service is running.  

---

## Turn off sleep/suspend/hibernate in Debian 12

### Steps:  
1. `sudo nano /etc/systemd/sleep.conf`
2. Change line `#AllowSuspend=yes` to `AllowSuspend=no`
3. Change line `#AllowHibernation=yes` to `AllowHibernation=no`
4. Reboot system

### Lessons Learned:  
- How to disable sleep/suspend/hibernate in Debian

---
