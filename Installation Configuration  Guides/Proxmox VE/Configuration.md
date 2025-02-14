# Description
This is guide for Proxmox VE configuration. Web GUI on *"IP address assigned by DHCP server:8006"* or shell on machine with installed Proxmox VE can be used for configuration

## Removing unnecessary storage volumes

After installation, Proxmox automatically creates two storage locations: local and local-lvm. By default, Proxmox uses these for file storage.
We can remove second storage and use main storage for everything.  
  - Web GUI: Data Center -> Storage -> local_lvm -> Remove
  - Proxmox shell: Enter the following commands to resize the **local** storage
    
    `lvremove /dev/pve/data`
    
    `lvresize -l +100%FREE /dev/pve/root`
    
    `resize2fs /dev/mapper/pve-root`
    
  - Modify the directory content rules on remain storage to allow installing disk images/containers  on **local** storage.


**Lessons Learned**:
    How to remove local-lvm storage, resize local storage, and change storage rules.
