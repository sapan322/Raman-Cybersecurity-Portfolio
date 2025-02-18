# Description:  
ICMP (Internet Control Message Protocol) is used for network diagnostics and management, commonly utilized in tools like `ping` and `traceroute`.  

---

## Successful Ping (Echo Request and Reply)  

![2025-02-17 03_21_32-Window](https://github.com/user-attachments/assets/dc399b32-fb10-4162-b7c1-7f1bfef9edf3)  

1. A `ping` request is sent from `192.168.8.120` to `google.com`.  
2. The DNS resolves `google.com` to an IPv4/IPv6 address by querying the router, which acts as a DNS server.  
3. The router responds with the resolved IPv4/IPv6 addresses.  
4. `192.168.8.120` sends an **ICMP Echo Request** to `142.250.75.14`, and `142.250.75.14` responds with an **ICMP Echo Reply** to `192.168.8.120`.  

---

## No Response Ping  

![2025-02-18 10_16_49-proxmox - Proxmox Virtual Environment](https://github.com/user-attachments/assets/3da2a46c-c84f-4251-af0c-266629598439)  

1. A `ping` request is sent from `192.168.8.120` to `192.168.8.101`.  
2. The ARP protocol attempts to resolve the IP address to a MAC address.  
3. The host at `192.168.8.101` does not respond to the ICMP Echo Request, which could indicate:  
   - The device is offline.  
   - ICMP requests are blocked by a firewall.  
   - ICMP is disabled on the target machine.  

---


## Traceroute (Using ICMP Packets)  

*Traceroute determines the route to a destination by using the TTL (Time To Live) value in packets. Each router along the path decreases the TTL by 1, and when TTL reaches 0, the router discards the packet and sends a "TTL exceeded" message back to the sender. This allows us to identify each hop along the route.*  

### How Traceroute Works:  

1. Execute the command `traceroute -I google.com` in the terminal.  
2. Three packets are sent with **TTL = 1**. The first router drops these packets and replies with **"TTL exceeded"**.  
3. The **TTL exceeded** message is received from the intermediate router (your home router/switch).  
4. The **TTL exceeded** message is received from the next intermediate router that received packets with **TTL = 2**.  
5. Finally, the destination receives the ICMP request and sends the **ICMP Echo Reply** back to the source.  

### Wireshark Capture Example  

![ICMP](https://github.com/user-attachments/assets/bce50ab7-cd6d-4c1c-9379-d7010e1ba267)  

This packet capture in Wireshark shows the **ICMP "TTL exceeded" messages** received from each router along the route to the destination.  

---
