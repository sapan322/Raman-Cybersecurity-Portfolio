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
- [Update #1: Sysmon and Sysinternals Suite installation on Windows 10 VM ](https://github.com/sapan322/Raman-Cybersecurity-Portfolio/blob/main/Build%20A%20Basic%20Home%20Lab/README.md#update-1-sysmon-and-sysinternals-suite-installation-on-windows-10-vm)
- [Update #2: FLARE-VM installation.](https://github.com/sapan322/Raman-Cybersecurity-Portfolio/blob/main/Build%20A%20Basic%20Home%20Lab/README.md#update-2-flare-vm-installation)
- [Update #3: REMnux installation.](https://github.com/sapan322/Raman-Cybersecurity-Portfolio/blob/main/Build%20A%20Basic%20Home%20Lab/README.md#update-3-remnux-installation) 


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



## Update #1: **Sysmon** and **Sysinternals Suite** installation on Windows 10 VM
**Sysmon** is a Windows system service and device driver that logs detailed information about process creation, network connections, and file changes. It helps detect malicious activity, investigate security incidents, and analyze malware behavior.

**Sysinternals Suite** is a collection of powerful system diagnostic and troubleshooting tools developed by Microsoft. These utilities cover process management, file system analysis, networking, system monitoring, and advanced security operations.

**Installation**:
- Install Sysmon on the Windows VM using this [guide](https://www.youtube.com/watch?v=uJ7pv6blyog).
- Download the latest Sysinternals Suite from [Microsoft's official website.](https://learn.microsoft.com/en-us/sysinternals/downloads/sysinternals-suite) and extract files.
- Update VM snapshot: **Baseline**

***Outcome***: 
- Sysmon is installed and actively monitoring system events.
- Sysinternals Suite is available for advanced system diagnostics and security analysis.

## Update #2: **FLARE-VM** installation.
[**FLARE-VM**](https://github.com/mandiant/flare-vm) is a collection of installation scripts for Windows that enables quick deployment and maintenance of a reverse engineering environment on a virtual machine (VM).

**Installation**:
- Create a VM for Windows 10, install the OS and VirtIO drivers using this [guide](https://www.youtube.com/watch?v=-KTTmrA3DX8)
- Disable Windows Updates using this [guide](https://www.windowscentral.com/how-stop-updates-installing-automatically-windows-10)
- Disable Tamper Protection and Windows Defender via GPO using this [guide](https://superuser.com/questions/1757339/how-to-permanently-disable-windows-defender-real-time-protection-with-gpo/1757341#1757341)
- Install FLARE-VM via PowerShell using the following resources:
  - [Official Github page](https://github.com/mandiant/flare-vm)
  - [LetsDefend - Flare-VM Installation](https://www.youtube.com/watch?v=BiSdnusy2AQ)
  - [Grant Collins - Build a Malware Analysis Lab](https://www.youtube.com/watch?v=rmSIm3BKu3Y&t=2311s)
- Create a VM snapshot to preserve the clean state.

***Outcome***:
- The installed FLARE-VM provides a fully configured reverse engineering environment with essential analysis tools.

## Update #3: **REMnux** installation.
[**REMnux**](https://remnux.org/) is a Linux-based distribution that provides a collection of free tools for reverse engineering and analyzing malicious software.

**Installation**:
-  Create a VM for REMnux using an OVF (Open Virtualization Format) image by following this [guide](https://www.itsfullofstars.de/2019/07/import-ova-as-proxmox-vm/).
-  Import the OVA as a Proxmox VM using the following resources:
  - [Tobias Hofmann - Import OVA as Proxmox VM](https://www.itsfullofstars.de/2019/07/import-ova-as-proxmox-vm/)
  - [syncbricks - Proxmox: How to Import OVA](https://www.youtube.com/watch?v=4lYulcTd5yc)
- Configure the VM in Proxmox using this official [guide](https://docs.remnux.org/install-distro/get-virtual-appliance)
- Verify functionality and create a VM snapshot.

***Outcome***:
- The installed REMnux provides a fully equipped environment for malware analysis, eliminating the need to manually find, install, and configure essential tools.
