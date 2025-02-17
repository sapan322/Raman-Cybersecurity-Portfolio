# Description  
Wireshark is a free and open-source packet analyzer used for network troubleshooting, analysis, protocol development, and education.  

## Installation Preparation on Debian  

Before installing Wireshark, update the system packages:  
- `sudo apt update && sudo apt upgrade -y`  

## Installation Process  

1. Install Wireshark:  
   - `sudo apt install wireshark -y`  
2. (Optional) Allow non-root users to capture packets:  
   - `sudo dpkg-reconfigure wireshark-common`  
   - Select **Yes** when prompted.  
   - Add your user to the `wireshark` group:  
     - `sudo usermod -aG wireshark $USER`  
   - Log out and log back in for the changes to take effect.  

## Lessons Learned  
- How to install Wireshark on Debian.  
- How to enable packet capturing for non-root users.  
