### Description  
The Address Resolution Protocol (ARP) is a communication protocol used for discovering the link layer address (such as a MAC address) associated with an internet layer address (typically an IPv4 address).  

---

## ARP Resolving Layer 3 (IPv4) Address to Layer 2 (MAC) Address:  

![2025-02-18 17_05_25-proxmox - Proxmox Virtual Environment](https://github.com/user-attachments/assets/2e14240e-1362-4e90-88fc-d14ea65fa8a5)  

1. Running the command `ping 192.168.8.106 -c 1` in the terminal sends an **ICMP echo request** to `192.168.8.106`.  
2. However, communication on **Layer 2** relies on **MAC addresses**. Therefore, our machine broadcasts a request:  
   *"Who has `192.168.8.106`? Tell `192.168.8.106`."*  
   And `192.168.8.106` responds with its **MAC address**.
   
ARP resolves the **IPv4 address** to the corresponding **MAC address**, and now the ICMP request will be sent directly to this MAC address.  

---
