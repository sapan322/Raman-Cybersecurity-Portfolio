# Description

A **virtual home lab** is essential for conducting cybersecurity operations such as malware analysis, reverse engineering, indicators of compromise (IOC) identification, vulnerability testing, tool evaluation, malware development, and system hardening.

A virtual lab provides a **controlled** environment for performing potentially risky actions without exposing the main network or physical machines. By using virtualization, you can deploy virtual machines (VMs), create snapshots, control network flow via a virtual firewall, execute malware, analyze network traffic, test software updates, and experiment with security configurations.

This document outlines the steps **I followed** to build my Proxmox-based Virtual Home Lab, including software installations, network configurations, and system preparations.


## Requirements

- Hardware with virtualization support
- USB flash drive for installation media
- Internet connection for software downloads and updates

## Software used
- Rufus (for creating bootable media)
- Proxmox VE (virtualization platform)
- pfSense (firewall and router)
- Windows 10 (for testing malware in a controlled environment)
- VirtIO Drivers (for optimizing Windows VM performance)
- Kali Linux (for penetration testing)

## Hardware Preparation: Proxmox VE Installation and Configuration.
I'm already write about it in my "Installation Configuration Guides" - [Proxmox VE](https://github.com/sapan322/Raman-Cybersecurity-Portfolio/tree/main/Installation%20Configuration%20%20Guides/Proxmox%20VE).
I have documented the Proxmox VE installation and configuration process in my [Installation Configuration Guides](https://github.com/sapan322/Raman-Cybersecurity-Portfolio/tree/main/Installation%20Configuration%20%20Guides/Proxmox%20VE). 

***Outcome***: Proxmox VE is successfully installed and ready for VM deployment.

## Software preparation. 
To begin, I downloaded the required software and uploaded the installation media to Proxmox VE local storage:

- Kali Linux ISO → [[Download](https://www.kali.org/get-kali/#kali-installer-images)]
- Windows 10 Installation Media → [[Download](https://www.microsoft.com/en-us/software-download/windows10)]
- VirtIO Drivers ISO (for Windows VMs) → [[Download](https://pve.proxmox.com/wiki/Windows_VirtIO_Drivers)]
- pfSense ISO → [[Download](https://www.pfsense.org/download/)]

 ***Outcome***: All necessary software has been downloaded and stored in Proxmox VE.

## Proxmox VE network interfaces preparation. 
- Created a Linux Bridge (VMBR10) with VLAN Aware and Autostart enabled in the Proxmox Network tab.

***Outcome***: A dedicated bridge is now available for configuring the firewall and VLAN networking.

## Virtual Machines (VMs) Creation and Software Installation
- Kali Linux VM: Installed following this [guide](https://www.youtube.com/watch?v=KQVdH79-RA8)
- Windows 10 VM: Installed Windows and VirtIO drivers using this [guide](https://www.youtube.com/watch?v=-KTTmrA3DX8)
- pfSense VM: Installed the firewall using [this guide](https://www.youtube.com/watch?v=yTMvrPAwbPE), setting: `WAN: VMBR0` and `LAN: VMBR10`

## pfSense Configuration.
To configure pfSense, log in to the web interface at 192.168.10.1 from a VM in the LAN. If accessing from WAN, temporarily disable the firewall by running pfctl -d in the pfSense shell.

**pfSense Network Zones**
| Interface | Subnet | Description | 
| :--- | :--- | :--- | 
| VMBR0 | 192.168.8.0/24 | WAN (Home network with physical devices) - DHCP assigned | 
| VMBR10 | 192.168.10.0/24 | LAN (Isolated virtual environment for security research) | 

**pfSense DHCP Configuration**
pfSense will act as a DHCP server for the LAN (VMBR10):
  - Gateway: 192.168.10.1
  - DHCP Range: 192.168.10.10 - 192.168.10.100
  - DNS Servers: 8.8.8.8 (Google) and 4.4.4.4 (Cloudflare)
    
**pfSense Firewall Rules:**

1. Block LAN access to WAN (Home Network)
  - Source: 192.168.10.0/24 (VLAN 10)
  - Destination: 192.168.8.0/24
  - Action: BLOCK
  - Prevents virtual machines from reaching physical devices on the home network.
2. Allow LAN access to external networks (Internet & Inter-VM communication)
  - Source: 192.168.10.0/24 (VLAN 10)
  - Destination: ANY
  - Action: PASS
  - Allows internet access and internal communication between VMs.

 ***Outcome***: pfSense firewall and DHCP are successfully configured, securing the virtual lab network.







