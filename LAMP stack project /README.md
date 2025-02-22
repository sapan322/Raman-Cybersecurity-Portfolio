# Description
A LAMP stack is a group of open-source software installed together to enable a server to host dynamic websites and web apps. 

**LAMP** : **L** - Linux operating system / **A** - Apache web server / **M** - MariaDB database / **P** - PHP for dynamic content. 

*(MariaDB drop-in replacement for MySQL in Debian)*

## Steps: 

### **L** - Linux: 
1. Create Proxmox VM.
2. Install Debian on VM ( [installation](https://github.com/sapan322/Raman-Cybersecurity-Portfolio/blob/main/Installation%20Configuration%20%20Guides/Debian%20(Proxmox%20VM)/Installation.md) )
3. Configure Debian ( [configuration.](https://github.com/sapan322/Raman-Cybersecurity-Portfolio/blob/main/Installation%20Configuration%20%20Guides/Debian%20(Proxmox%20VM)/Configuration.md) )

### **A** - Apache:
1. Update apt packages: Terminal -> `sudo apt update` 
2. Install Apache: Terminal -> `sudo apt install apache2` -> Confirm by pressing `Y`
