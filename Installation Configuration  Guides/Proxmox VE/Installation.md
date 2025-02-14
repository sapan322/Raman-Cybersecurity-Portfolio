# Description  
Installation guide for **Proxmox VE** on an old laptop.  

**Proxmox VE** is a key component that will be used to run VMs, including servers, endpoints, firewalls, and more.  

## Hardware Setup  
Purchased a used laptop **HP EliteBook Folio 9470m** with the following specifications:  
- **CPU:** Intel i5-3427U (1.80 GHz – 2.80 GHz)  
- **GPU:** Intel HD Graphics 4000  
- **RAM:** 8GB DDR3  
- **Storage:** 120GB SSD  
- **Networking:** Ethernet controller / Wireless network adapter  

Additionally, purchased a **32GB USB flash drive** for installation.  

## Laptop Preparation  
- Changed BIOS boot mode from **Legacy** to **UEFI Hybrid**.  
- Enabled **Virtualization Technology (VT-x)** in BIOS settings.  

## Installation Preparation  
- Downloaded the **Proxmox VE ISO Installer**.  
- Used **Rufus** to create a bootable USB drive from the ISO.  

## Installation Process  
1. Booted from the USB drive with the Proxmox installer, selected **"Install Proxmox VE (Graphical)"**, and set up the **administrator password** and **email**.  
2. Configured the following network settings:  
   - **Management interface:** Ethernet controller  
   - **Hostname:** Device name within the network  
   - **IP address:** Assigned by the DHCP server on the physical router  
   - **Gateway:** Router’s IP address  
   - **DNS Server:** Google DNS (or any preferred DNS)
  ![408222561-07e30c4f-bfc2-4a7c-a03b-747e910f68b8](https://github.com/user-attachments/assets/1d84d7d6-5ef9-4c3b-9be9-529a4074cadc)

    And confirmed the settings on the **summary screen** before proceeding with installation.  
     
3. After installation, removed the **USB drive** and waited for Proxmox to boot. Once booted, the system displayed the **IP address** for remote access.  
   ![Przechwytywanie](https://github.com/user-attachments/assets/37f8c295-6b3a-49e6-87e1-8217e6538dfc)  

## Lessons Learned  
- BIOS settings must be properly configured before installing Proxmox.  
- Proxmox requires correctly assigned network interfaces for proper configuration.  
