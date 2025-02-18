# Description:
ICMP (Internet Control Message Protocol) is used for diagnostics and network management. (ping/traceroute)

## Successful Ping (Echo request and reply)

![2025-02-17 03_21_32-Window](https://github.com/user-attachments/assets/dc399b32-fb10-4162-b7c1-7f1bfef9edf3)

1. Send `ping` from machine at `192.168.8.120` to `google.com`
2. Protocol DNS resolve domain name to IPv4/IPv6 addresses by query router A/AAAA which used as DNS server.
3. Router response to query A/AAAA to previos query with IPv4/IPv6 addresses to google.com
4. `192.168.8.120` send `Echo (ping) request` to `142.250.75.14` and `142.250.75.14` response with `Echo (ping) reply` to `192.168.8.120`.

