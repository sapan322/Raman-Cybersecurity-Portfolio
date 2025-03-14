# Building a Basic Virtual Home Lab

A **virtual home lab** is essential for conducting cybersecurity operations such as malware analysis, reverse engineering, indicators of compromise (IOC) identification, vulnerability testing, tool evaluation, malware development, and system hardening.

A virtual lab provides a **controlled** environment for performing potentially risky actions without exposing the main network or physical machines. By using virtualization, you can deploy virtual machines (VMs), create snapshots, control network flow via a virtual firewall, execute malware, analyze network traffic, test software updates, and experiment with security configurations.

This document outlines the steps **I followed** to build my Proxmox-based Virtual Home Lab, including software installations, network configurations, and system preparations.


### Requirements

- Hardware with virtualization support
- USB flash drive for installation media
- Internet connection for software downloads and updates

### Software used
- Rufus (for creating bootable media)
- Proxmox VE (virtualization platform)
- pfSense (firewall and router)
- Windows 10 (for testing malware in a controlled environment)
- VirtIO Drivers (for optimizing Windows VM performance)
- Kali Linux (for penetration testing)
- FLARE-VM (for reverse engineering)
- REMnux (for malware analysis)

### Contents:
- [Step 1: Proxmox VE Installation and Configuration](https://github.com/sapan322/Raman-Cybersecurity-Portfolio/tree/main/Build%20A%20Basic%20Home%20Lab#step-1-proxmox-ve-installation-and-configuration)
- [Step 2: Software Preparation](https://github.com/sapan322/Raman-Cybersecurity-Portfolio/tree/main/Build%20A%20Basic%20Home%20Lab#step-2-software-preparation)
- [Step 3: Proxmox VE Network Configuration](https://github.com/sapan322/Raman-Cybersecurity-Portfolio/tree/main/Build%20A%20Basic%20Home%20Lab#step-3-proxmox-ve-network-configuration)
- [Step 4: Virtual Machines (VMs) Creation and Software Installation](https://github.com/sapan322/Raman-Cybersecurity-Portfolio/tree/main/Build%20A%20Basic%20Home%20Lab#step-4-virtual-machines-vms-creation-and-software-installation)
- [Step 5: pfSense Configuration](https://github.com/sapan322/Raman-Cybersecurity-Portfolio/tree/main/Build%20A%20Basic%20Home%20Lab#step-5-pfsense-configuration)
- [Step 6: Final Preparations](https://github.com/sapan322/Raman-Cybersecurity-Portfolio/tree/main/Build%20A%20Basic%20Home%20Lab#step-6-final-preparations)
- [Lessons Learned
](https://github.com/sapan322/Raman-Cybersecurity-Portfolio/tree/main/Build%20A%20Basic%20Home%20Lab#lessons-learned)

### Updates:
- **Sysmon and Sysinternals Suite:** ***Sysmon*** that helps detect malicious activity, investigate security incidents, and analyze malware behavior. And ***Sysinternals Suite*** - a collection of powerful system diagnostic and troubleshooting tools developed by Microsoft. Links:
  - [Update #1: Sysmon and Sysinternals Suite installation on Windows 10 VM ](https://github.com/sapan322/Raman-Cybersecurity-Portfolio/blob/main/Build%20A%20Basic%20Home%20Lab/Updates/Update_1_Sysmon_Sysinternals.md)

- **Malware analysis toolkit:** *FLARE-VM*, *REMnux* with *INetSim* together in VLAN controlled by pfSense create a powerful infrastructure for malware analysis. FLARE-VM for *host based indicators* and Remnux with INetSim to collect *network based indicators* of malware. These tools allow to make professional static and dynamic malware analysis.

![2025-03-14 18_32_55-Window](https://github.com/user-attachments/assets/6a68173b-5bbc-4534-81db-2c78a86a345a)

 Links:  
  - [Update #2: FLARE-VM installation.](https://github.com/sapan322/Raman-Cybersecurity-Portfolio/blob/main/Build%20A%20Basic%20Home%20Lab/Updates/Update_2_FLARE_VM.md)
  - [Update #3: REMnux installation.](https://github.com/sapan322/Raman-Cybersecurity-Portfolio/blob/main/Build%20A%20Basic%20Home%20Lab/Updates/Update_3_REMnux.md)
  - [Update #4: INetSim set up.](https://github.com/sapan322/Raman-Cybersecurity-Portfolio/blob/main/Build%20A%20Basic%20Home%20Lab/Updates/Update_4_INetSim.md)


## Step 1: Proxmox VE Installation and Configuration
I'm already write about it in my "Installation Configuration Guides" - [Proxmox VE](https://github.com/sapan322/Raman-Cybersecurity-Portfolio/tree/main/Installation%20Configuration%20%20Guides/Proxmox%20VE).
I have documented the Proxmox VE installation and configuration process in my [Installation Configuration Guides](https://github.com/sapan322/Raman-Cybersecurity-Portfolio/tree/main/Installation%20Configuration%20%20Guides).

***Outcome***: Proxmox VE is successfully installed and ready for VM deployment.

## Step 2: Software Preparation
To begin, I downloaded the required software and uploaded the installation media to Proxmox VE local storage:

- Kali Linux ISO → [[Download](https://www.kali.org/get-kali/#kali-installer-images)]
- Windows 10 Installation Media → [[Download](https://www.microsoft.com/en-us/software-download/windows10)]
- VirtIO Drivers ISO (for Windows VMs) → [[Download](https://pve.proxmox.com/wiki/Windows_VirtIO_Drivers)]
- pfSense ISO → [[Download](https://www.pfsense.org/download/)]

 ***Outcome***: All necessary software has been downloaded and stored in Proxmox VE.

## Step 3: Proxmox VE Network Configuration
- Created a Linux Bridge (VMBR10) with ***VLAN Aware*** and ***Autostart*** enabled in the Proxmox Network tab.

***Outcome***: A dedicated bridge is now available for configuring the firewall and VLAN networking.

## Step 4: Virtual Machines (VMs) Creation and Software Installation
- Kali Linux VM: Installed following this [guide](https://www.youtube.com/watch?v=KQVdH79-RA8)
- Windows 10 VM: Installed Windows and VirtIO drivers using this [guide](https://www.youtube.com/watch?v=-KTTmrA3DX8)
- pfSense VM: Installed the firewall using [this guide](https://www.youtube.com/watch?v=yTMvrPAwbPE), settings:
  - `WAN: VMBR0`
  - `LAN: VMBR10`

## Step 5: pfSense Configuration
To configure pfSense, log in to the web interface at `192.168.10.1` from a VM in the LAN. If accessing from WAN, temporarily disable the firewall by running `pfctl -d` in the pfSense shell.

**pfSense Network Zones**
| Interface | Subnet | Description | 
| :--- | :--- | :--- | 
| VMBR0 | 192.168.8.0/24 | WAN (Home network with physical devices) - DHCP assigned | 
| VMBR10 | 192.168.10.0/24 | LAN (Isolated virtual environment for security research) | 

**pfSense DHCP Configuration**
pfSense will act as a DHCP server for the LAN (VMBR10):
  - Gateway: 192.168.10.1
  - DHCP Range: 192.168.10.10 - 192.168.10.100
  - DNS Servers: 8.8.8.8 (Google) and 1.1.1.1 (Cloudflare)
    
**pfSense Firewall Rules:**

1. **Block LAN access to WAN (Home Network)**
  - Source: 192.168.10.0/24 (VLAN 10)
  - Destination: 192.168.8.0/24
  - Action: BLOCK
  - Prevents virtual machines from reaching physical devices on the home network.
  - **If any machine from VLAN 192.168.10.0/24 attempts to send a packet to 192.168.8.0/24, it will be blocked to prevent any connection to my physical machines in WAN and router.**
    
2. **Allow LAN access to external networks (Internet & Inter-VM communication)**
  - Source: 192.168.10.0/24 (VLAN 10)
  - Destination: ANY
  - Action: PASS
  - Allows internet access and internal communication between VMs.
  - **If a packet is not blocked by the first rule, it can proceed to the next rule and be allowed to access the internet and communicate within the VLAN.**

 ***Outcome***: pfSense firewall and DHCP are successfully configured, securing the virtual lab network.

 ## Step 6: Final Preparations
 - Allowed ICMP requests in Windows Firewall for connectivity testing.
 - Prepare installed Windows with Sysprep and make Windows template.
 - Clone Windows template and prepare system for usage (activation, software installation, etc.)
 - Create "Baseline" snapshots for Windows and Kali Linux systems.
 - Verified connectivity between machines, firewall rules, internet access, DNS resolution, and DHCP functionality.

## Lessons Learned
   
1. Proper VLAN Configuration is Essential
    - Understanding how VLANs work prevented connectivity issues and ensured isolation between networks.
2. pfSense Provides Strong Network Security
    - Using pfSense as a firewall allowed for granular control over traffic between virtual machines and the home network.
3. Snapshots are Crucial for Testing
    - Taking snapshots before major changes saved time when needing to revert to a clean state.
4. Windows VMs Require Additional Configuration
    - Installing VirtIO drivers was necessary for performance optimization, and using Sysprep made template creation easier.
5. Lab Documentation is Key
    - Keeping structured documentation ensured that configurations and installations were reproducible and easy to debug.

---

***This guide serves as a foundation for my virtual cybersecurity lab, allowing for safe and structured experimentation with security tools and techniques.***
