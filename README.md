# Network Security Project

This write up is an initial (design) phase to the implementation phase - whose links will be available below.  
Kindly make sure you go through this just to get the understanding of the setup before moving on to the various test/ implementation phases.

### Operating Systems
The VM's are to run on the Windows 11 Host Operating System (OS).
#### Virtual Machines (VM) and their Network Configurations
|VM | Purpose| Network Configuration|
|-----|-----|-----------------------|
|Metasploitable 2 |An intentionally vulnerable VM. | Host-Only Network - eth0.|
| Kali 2024.1 | Penetration Testing and Security analysis| NAT Network (vmnet8)- eth0 and Host-Only Network (vmnet1)- eth1 |
|Ubuntu 22.04 |Hosting applications to secure vulnerable VM|NAT Network (vmnet8)- ens33 and Host-Only Network (vmnet1)- ens37|  

*Network Segmentation:* For enhanced security, the network design has been segmented into two distinct sub-networks:
1.	Host-Only Network, configured to facilitate internal communication between all VMs.  
  a.	Network Adaptor Name: ‘vmnet1’  
  b.	Subnet: ‘192.168.10.0/24’  
  c.	static IP assignment  

2.	NAT Network: configured to enable only the Kali and Ubuntu VMs to connect to the internet.  
  a.	Network Adaptor Name: ‘vmnet8’  
  b.	Subnet: ‘192.168.152.0/24’  
  c.	DHCP IP assignment.
  
*Virtualization Software (VMware Workstation Pro):* This acts as middleware between the host OS and the VMs, providing the platform upon which the VMs are installed.


## Prototype Network and System Security Architecture
This illustrates the Prototype Network and System security architecture and defined below:  
<picture>![image](https://github.com/user-attachments/assets/e0e8baeb-55dd-467b-9c99-4f3950825a2d)
</picture>
  
## Network Topology

Based on the two IP blocks assigned for the demonstration, I used Zenmap in Kali to draw out the topology of each of the IP blocks as seen in figure 1 and 2.  
<picture>![image](https://github.com/user-attachments/assets/5be1ced5-14c9-4da6-8b4e-fc4e120b5838)</picture>
<p align="center">
    <strong>Figure 1:Network Topology on dynamic IP (192.168.152.0/24) - NAT interface</strong>
</p>
  
<picture>![image](https://github.com/user-attachments/assets/1cbbec14-9381-4071-99b2-a3c875ceb3ed)
</picture>
<p align="center">
    <strong>Figure 2:Network Topology on the static IP (192.168.10.0/24) - Host-only interface. </strong>
</p>

## Demonstration of network security and overall system
This demonstration consists of two parts, based on the prototype network and security architecture provided in the earlier chapter. The processes involved are outlined below.

### Confirming IPs of VMs
The initial step involved verifying the configuration of the virtual machines (VMs) to ensure they were set to receive IP addresses using the command ‘ip a’ and ‘ifconfig’ as seen in figure 5 and 6.  
<picture> ![image](https://github.com/user-attachments/assets/9f6e3068-3bbd-4f92-8c18-d9b43aaf2e4a)</picture>  
<p align="center">
    <strong>Figure 5The Kali and Ubuntu VMs are confirmed to be on the two sub-networks(different interfaces).</strong>
</p>
  
<picture>![image](https://github.com/user-attachments/assets/5e8bca6a-3ebd-4b13-9e2f-08bb4f7c9ae2)</picture>
<p align="center">
    <strong>Figure 6: Metasploitable 2 is assigned a static IP.</strong>
</p>

## Communication between VMs Using the Ping Command.
To verify the VMs' connectivity and demonstrate the isolation of the intended subnetwork from external access, the 'ping' command is used in the test. The results, along with accompanying screenshots, are detailed in figure 7 -11. 
The '-I' flag is to specify the interface to use for testing. The '-c 4' or '-4' option specifies the use of IPv4.  
### Pinging to Test DHCP Configured IPs
<picture>![image](https://github.com/user-attachments/assets/e2da7d69-73ce-4907-9c29-6cfc6085bac7)
</picture>
<p align="center">
    <strong>Figure 7: I conducted pings test on the DHCP interfaces between the Kali and Ubuntu VM to confirm their reachability.</strong>
</p>

### Pinging to test Configured ‘Host-Only’ - Static IPs
<picture>![image](https://github.com/user-attachments/assets/4ccfb73c-ecb3-4d8d-ab68-414142d161bb)
</picture> 
<p align="center">
    <strong>Figure 8: I conducted pings test on the interfaces set up for static IPs, between both Kali and Ubuntu, and confirmed their reachability.</strong>
</p>
  
<picture>![image](https://github.com/user-attachments/assets/c43a89d9-3207-4743-8532-2296f03a635e)
</picture>
<p align="center">
    <strong>Figure 9 Using the vulnerable VM to ping the static IP configured interface from Ubuntu.</strong>
</p>

## Pinging between two different subnetworks

To proof for a proper network segmentation towards a secured network, I tested the connection between the assigned static IP on Metasploitable 2 and a dynamic IP address on the Kali, both on different interface and failed due to proper segmentation; as shown in figure 10 and 11.  
<picture>![image](https://github.com/user-attachments/assets/96a27c3b-1765-44f9-bee5-24f4b71b7123)</picture>
<p align="center">
    <strong>Figure 10: Pinging the Kali DHCP IP configured interface from my Metasploitable 2-configured static IP interface and failed.</strong>
</p>
  
<picture>![image](https://github.com/user-attachments/assets/33491a59-44f5-4f40-b96c-316523c4b38d)</picture>
<p align="center">
    <strong>Figure 11: Pinging the Kali DHCP configured interface from my Ubuntu configured static IP interface and failed.</strong>
</p>




