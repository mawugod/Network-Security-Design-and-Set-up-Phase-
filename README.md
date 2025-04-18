# 🔐 Network Security Project – Design Phase

This write-up outlines the **initial design and setup phase** of a virtualized network security environment.  
Please review this document thoroughly before proceeding to the testing and implementation stages - provided in the links at the end.


---

## 🧰 Host Environment

- **Host OS:** Windows 11
- **Virtualization Platform:** VMware Workstation Pro

VMware serves as the hypervisor layer, allowing multiple isolated virtual machines (VMs) to run concurrently for testing and simulation purposes.

---

## 🖥️ Virtual Machines & Network Configuration

| Virtual Machine     | Purpose                                      | Network Configuration                                                                 |
|---------------------|----------------------------------------------|----------------------------------------------------------------------------------------|
| Metasploitable 2    | Vulnerable target machine                    | Host-Only Network (`vmnet1`) — `eth0`                                                 |
| Kali Linux 2024.1   | Penetration testing & security assessment    | NAT (`vmnet8`) — `eth0`<br>Host-Only (`vmnet1`) — `eth1`                               |
| Ubuntu 22.04        | Hosting secure applications                  | NAT (`vmnet8`) — `ens33`<br>Host-Only (`vmnet1`) — `ens37`                             |

---
 

## 🌐 Network Segmentation Strategy

To simulate real-world network isolation and segmentation, two distinct subnets are used:

### 🔸 Host-Only Network – `vmnet1`
- **Subnet:** `192.168.10.0/24`
- **IP Assignment:** Static
- **Purpose:** Internal communication between all VMs

### 🔹 NAT Network – `vmnet8`
- **Subnet:** `192.168.152.0/24`
- **IP Assignment:** DHCP
- **Purpose:** Internet access for Kali and Ubuntu only

---

## 🧱 Prototype Security Architecture

The diagram below outlines the prototype system and network security architecture.

<p align="center">
  <img src="https://github.com/user-attachments/assets/e0e8baeb-55dd-467b-9c99-4f3950825a2d" alt="Security Architecture Diagram">
</p>

---

## 🛰️ Network Topology Mapping

Zenmap (on Kali Linux) was used to visualize the topology of both subnetworks.

<p align="center">
  <img src="https://github.com/user-attachments/assets/5be1ced5-14c9-4da6-8b4e-fc4e120b5838" alt="Dynamic IP Topology">
  <br>
  <strong>Figure 1: Network topology – Dynamic IPs (`192.168.152.0/24` via NAT)</strong>
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/1cbbec14-9381-4071-99b2-a3c875ceb3ed" alt="Static IP Topology">
  <br>
  <strong>Figure 2: Network topology – Static IPs (`192.168.10.0/24` via Host-Only)</strong>
</p>

---

## 📋 VM IP Configuration Validation

Using `ip a` and `ifconfig`, IP configurations were confirmed for all VMs.

<p align="center">
  <img src="https://github.com/user-attachments/assets/9f6e3068-3bbd-4f92-8c18-d9b43aaf2e4a" alt="Kali and Ubuntu IPs">
  <br>
  <strong>Figure 5: Kali and Ubuntu VMs configured on two separate subnets</strong>
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/5e8bca6a-3ebd-4b13-9e2f-08bb4f7c9ae2" alt="Metasploitable IP">
  <br>
  <strong>Figure 6: Metasploitable 2 configured with a static IP</strong>
</p>

---

## 🔗 VM Communication Test – `ping`

### 🧪 DHCP Interface Connectivity

The `ping` command was used to test connectivity between DHCP-assigned interfaces (`vmnet8`) of Kali and Ubuntu.

<p align="center">
  <img src="https://github.com/user-attachments/assets/e2da7d69-73ce-4907-9c29-6cfc6085bac7" alt="Kali to Ubuntu via NAT">
  <br>
  <strong>Figure 7: Successful ping between Kali and Ubuntu via NAT interfaces</strong>
</p>

---

### 🧪 Static Interface Connectivity

Communication was also tested across the static interfaces (`vmnet1`).

<p align="center">
  <img src="https://github.com/user-attachments/assets/4ccfb73c-ecb3-4d8d-ab68-414142d161bb" alt="Kali to Ubuntu Static">
  <br>
  <strong>Figure 8: Ping between Kali and Ubuntu via Host-Only (static IPs)</strong>
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/c43a89d9-3207-4743-8532-2296f03a635e" alt="Ubuntu to Metasploitable">
  <br>
  <strong>Figure 9: Ubuntu pinging Metasploitable via static interface</strong>
</p>

---

## 🔒 Inter-Subnet Isolation Testing

To validate network segmentation, pings were attempted between VMs on separate subnets. These attempts **failed as expected**, confirming isolation.

<p align="center">
  <img src="https://github.com/user-attachments/assets/96a27c3b-1765-44f9-bee5-24f4b71b7123" alt="Metasploitable to Kali NAT Failure">
  <br>
  <strong>Figure 10: Failed ping from Metasploitable (static) to Kali (DHCP/NAT)</strong>
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/33491a59-44f5-4f40-b96c-316523c4b38d" alt="Ubuntu to Kali NAT Failure">
  <br>
  <strong>Figure 11: Failed ping from Ubuntu (static) to Kali (DHCP/NAT)</strong>
</p>

---

## Links to the various Phases of the Implementation - Attack and Defend
1. 🧨 Exploiting the FTP vsftpd 2.3.4 Service Vulnerability - https://tinyurl.com/Exploiting-FTP-vsftpd-Service
