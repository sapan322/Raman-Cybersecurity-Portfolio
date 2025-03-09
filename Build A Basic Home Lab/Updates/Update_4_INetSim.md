## Update #4: **INetSim** set up.
[INetSim](https://www.inetsim.org/) is a software suite designed to simulate common internet services in a controlled lab environment, allowing for the analysis of malware network behavior.

### Installation & Configuration:
1. **Modify INetSim Configuration on Remnux:**
- Uncomment the `start_service` dns line to enable DNS server simulation.
- Set the service bind address to 0.0.0.0 to listen on all interfaces.
- Define the DNS default IP as the IP assigned by pfSense DHCP.
2. **Start INetSim**:
- Run `sudo inetsim` on Remnux.
- If INetSim reports port conflicts, identify the conflicting service using netstat and resolve the issue.
3. **Configure FlareVM Networking**:
- Set the Preferred DNS Server to the IP address of Remnux in FlareVM’s network adapter settings.
4. **Verification & Snapshot**:
- Test INetSim functionality by attempting to resolve domain names and downloading simulated files.
- Once verified, create a VM snapshot for future rollback.

***Outcome***:
- Malware executed on FlareVM will interact with INetSim’s simulated services on Remnux, allowing for safe analysis of network indicators without external exposure.
