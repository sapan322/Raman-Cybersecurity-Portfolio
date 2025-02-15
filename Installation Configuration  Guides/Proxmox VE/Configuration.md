# Description  
This guide covers **Proxmox VE** configuration.  
You can manage Proxmox VE using:  
- **Web GUI**: `https://<IP-assigned-by-DHCP>:8006`  
- **Shell**: Directly on the machine where Proxmox VE is installed


**Contents:**

[Removing Unnecessary Storage Volumes](https://github.com/sapan322/Raman-Cybersecurity-Portfolio/blob/main/Installation%20Configuration%20%20Guides/Proxmox%20VE/Configuration.md#removing-unnecessary-storage-volumes)
    
[Prevent Proxmox VE from Turning Off When Closing the Laptop Lid](https://github.com/sapan322/Raman-Cybersecurity-Portfolio/blob/main/Installation%20Configuration%20%20Guides/Proxmox%20VE/Configuration.md#prevent-proxmox-ve-from-turning-off-when-closing-the-laptop-lid)

[Disable the enterprise repository and enable the repository for non-subscribers](https://github.com/sapan322/Raman-Cybersecurity-Portfolio/blob/main/Installation%20Configuration%20%20Guides/Proxmox%20VE/Configuration.md#disable-the-enterprise-repository-and-enable-the-repository-for-non-subscribers)

---
<!--- 
## NAME  

DESCRIPTION

### Steps:  

- STEP 1
- STEP 2
- STEP 3

### Lessons Learned:  
- LESSON 1  
- LESSON 2
- LESSON 3

---
--- --->

## Removing Unnecessary Storage Volumes  

After installation, Proxmox automatically creates two storage locations: **local** and **local-lvm**. By default, Proxmox uses these for file storage.  
We can remove **local-lvm** and use **local** for everything.  

### Steps:  

- **Via Web GUI**:  
  - Navigate to **Data Center → Storage → local-lvm → Remove**  

- **Via Proxmox Shell**:  
  Run the following commands to remove **local-lvm** and allocate its space to **local**:  

      lvremove /dev/pve/data  
      lvresize -l +100%FREE /dev/pve/root  
      resize2fs /dev/mapper/pve-root  

- **Modify storage rules**:  
  Adjust the **local** storage settings to allow installing disk images and containers.  

### Lessons Learned:  
- How to remove **local-lvm** storage  
- How to resize **local** storage  
- How to modify storage rules  

---

## Prevent Proxmox VE from Turning Off When Closing the Laptop Lid  

By default, closing the laptop lid may put the system into sleep mode.

### Steps:

- Open `/etc/systemd/logind.conf` file with a text editor:  

      nano /etc/systemd/logind.conf  

- Find and modify the following lines (remove `#` if present):  

      HandleLidSwitch=ignore  
      HandleLidSwitchDocked=ignore  

- Restart logind service:  

      systemctl restart systemd-logind
  
### Lessons Learned:  
- How to configure system settings to prevent sleep mode.
- Modify system configuration files safely.

---
 
## Disable the enterprise repository and enable the repository for non-subscribers 

Change default **enterprise subscriptions** to update Proxmox VE

### Steps:  

- **WEB GUI:** Updates -> Repositories -> Select the enterprise repository -> Click disable 
- **WEB GUI:** Click add -> choose "No-Subscription"

### Lessons Learned:  
- Change enterprise repository to No-Subscription repository

---
