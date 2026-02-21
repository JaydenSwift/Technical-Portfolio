# **Homelab Network Architecture**

This document explains the network design of my Proxmox homelab. The architecture is designed with an isolated mindset, utilizing a firewall to segment management traffic, services, and the “victim” environment used for security experience.

## **Network Topology Overview**

The lab sits behind a standard home router gateway (192.168.X.X), but the internal logic is entirely managed by a virtualized **pfSense Router**. This allows for total isolation between the home network and the lab environments.

### **Key Subnets & Segments**

| Segment | Subnet | Description |
| :---- | :---- | :---- |
| **Main Lab Net** | 10.0.0.0/24 | Primary segment for core services and tools. |
| **DMZ** | 10.0.1.0/24 | The "Demilitarized Zone" contains the JumpBox for secure external access. |
| **Victim Net** | 10.0.2.0/24 | An isolated sandbox for vulnerable machines and testing. |

## **Security & Perimeter Control**

### **pfSense Router**

The pfSense instance acts as the central point of the network. It handles:

* **Network Routing:** Controlling traffic flow between the DMZ, Victim Net, and Core services.  
* **Firewalling:** Strict filtering to prevent "Victim" machines from reaching the home network.  
* **Logging:** Forwarding traffic logs to the security monitoring system.

### **Access Control**

To maintain high security, direct access to the lab from my home network is restricted. A **JumpBox** residing in the DMZ (10.0.1.0/24) acts as the single point of entry, ensuring that management traffic is easily controlled.

## **Security Monitoring (SIEM)**

A dedicated security stack is deployed to monitor the environment in real-time:

* **Wazuh (SIEM/XDR):** Collects and analyzes logs from the pfSense firewall and various victim endpoints.  
* **Ubuntu-Blue:** Serves as a defensive workstation for log analysis and incident response.

## **Internal Services & Workloads**

The environment is a mix of full Virtual Machines (VMs) for standard operations and Linux Containers (LXCs) for lightweight services.

### **Core Services (LXCs)**

These services provide the lab's infrastructure support:

* **Bookstack:** Centralized documentation and knowledge base.  
* **Vaultwarden:** Self-hosted credential management.  
* **File Browser:** Web-based file management for easy file sharing.

### **Penetration Testing Lab**

A dedicated **Victim Net** is populated with various operating systems for vulnerability assessment and securing:

* **Attackers:** Kali Linux.  
* **Targets:** Ubuntu-Victim, Debian-Victim, Metasploitable, and Windows-Victim.

## **Storage Integration**

The network architecture accounts for the physical storage mapped in the hardware specifications:

* **HDD-Bulk:** Used for cold storage like log archives.  
* **USB-Backup:** Targeted by the Proxmox backup scheduler for automated continuity assurance.
