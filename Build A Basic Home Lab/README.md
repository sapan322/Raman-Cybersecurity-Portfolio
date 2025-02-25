# Description

Malware analysis, reverse engineering, IOCs, vulnerabilities, tools testing, malware crafting, system hardening, and other cybersecurity operations can be successfully and safely achieved in a virtual home lab. Virtual Home Lab allows making risky operations in a controlled environment. Deploy virtual machines, make snapshots, control network flow by virtual firewall, execute malware, analyze traffic, make risky actions, test updates and software, and other activities without exposure to the main network and machines. That's why I decided to make my own virtual home lab in a proxmox environment.

Here i share my steps, links, thoughts and other in home lab creation proccess.

## Requirements

Hardware that support virtualization
Flashdrive
Internet connection for software installation/update

## Software used

Rufus
Proxmox VE
Pfsense 
Windows 
VirtIO Drivers 
Kali Linux 


## Hardware preparation. Proxmox VE installation/configuration proccess.
I'm already write about it in my "Installation Configuration Guides" - [Proxmox VE](https://github.com/sapan322/Raman-Cybersecurity-Portfolio/tree/main/Installation%20Configuration%20%20Guides/Proxmox%20VE). So i leave link to my documentation.

***Step result***: Installed Proxmox VE on laptop for deploy virtual machines, networks, and other interesting stuff.

## Software preparation. 
- Download Kali Linux ISO image to Proxmox VE *local* storage. [[Kali Linux Installer Images]](https://www.kali.org/get-kali/#kali-installer-images)
- Create Windows 10 installation media and upload it to Proxmox VE *local* storage. [[Windows 10 installation media]](https://www.microsoft.com/en-us/software-download/windows10)
- Download VirtIO Drivers ISO image for Windows installation to Proxmox VE *local* storage. [[Windows VirtIO Drivers]](https://pve.proxmox.com/wiki/Windows_VirtIO_Drivers)
- Download pfSense ISO image to Proxmox VE *local* storage. [[pfSense CE software download]](https://www.pfsense.org/download/)

***Step result***: Download all needed software to Proxmox VE storage.

## Proxmox VE network interfaces preparation. 
- Create Linux Bridge called VMBR10 with "VLAN Aware" and "Autostart" enabled in Proxmox Network tab.

***Step result***: Create bridge to use in firewall networking configuration.

## VMs Creation. Software installation.
- Create VM for Kali Linux and install system with this [guide](https://www.youtube.com/watch?v=KQVdH79-RA8)
- Create VM for Windows 10 and install system and install drivers with this [guide](https://www.youtube.com/watch?v=-KTTmrA3DX8)
- Create VM for pfSense and install firewall with this [guide](https://www.youtube.com/watch?v=yTMvrPAwbPE) with VMBR0 as WAN and VMBR10 as LAN in creation/installation proccess.

## pfSense configuration.
*Login to pfSense interface on 192.168.10.1 from VM in LAN, or configure from WAN (*temp. disable firewall in pfsense shell by `pfctl -d`*) get through initial configuration proccess, and change settings for home lab.

pfSense zones:
- VMBR0 **[ 192.168.8.0/24 ]** - WAN my home network where are main physical devices, pfSense get IP address from DCHP. 
- VMBR10 **[ 192.168.10.0/24 ]** - LAN where are virtual machines will communicate with each other and get internet connectiion.

pfSense DHCP:
*pfSense will serve as DHCP server. So Gateway, Range, DNS servers need to be congigured*.
- DHCP LAN configuration: **Gateway** 192.168.10.1 / **DHCP Range** 192.168.10.10-192.168.10.100 / DNS Servers: 8.8.8.8 and 4.4.4.4    

pfSense LAN firewall rules:
*First rule - If any machine from VLAN 192.168.10.0/24 try send packet 192.168.8.0/24 it will be blocked to prevent any connection to my physical machines.*
*If packet not blocked by first rule - it can go anywhere. Allow access to Internet and communication in VLAN beetween machines.*

- 1. Source: 192.168.10.0/24 (VLAN 10) Destination: 192.168.8.0/24 Action: BLOCK
- 2. Source: 192.168.10.0/24 (VLAN 10) Destination: ANY Action: PASS











