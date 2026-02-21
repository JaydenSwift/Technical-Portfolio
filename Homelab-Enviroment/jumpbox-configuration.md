# **Secure Access Architecture: The Jumpbox "Airlock"**

This document shows the implementation of a secure "Airlock" access model using a dedicated **Debian Jumpbox**. By leveraging VLAN segmentation and SOCKS5 port forwarding, I have established a secure path for management traffic that isolates the lab from the primary home network.

## **The "Airlock" Concept**

The primary security objective was to remove all direct routing between my personal workstation and the production/testing subnets. Instead, all administrative traffic is through a  "Jump" host residing in a Demilitarized Zone (DMZ).

## **Implementation Phases**

### **1\. Virtual Infrastructure**

To support hardware-level isolation, I configured the internal Proxmox bridge (vmbr1) as **VLAN-aware**.

* **Provisioning:** Deployed a minimal Debian 12 (Bookworm) instance.  
* **Network Tagging:** Assigned **VLAN Tag 10** to the Jumpbox virtual interface to logically separate its traffic at the Layer 2 level within the Proxmox environment.

### **2\. Network Gatekeeping (pfSense)**

The pfSense firewall acts as the enforcement point for the Jumpbox's "Airlock" functionality.

* **Interface Assignment:** Created a dedicated new interface (VLAN 10\) with a gateway IP of 10.0.1.1.  
* **Firewall Logic:**  
  * **Ingress Rule:** Allows only my Home PC IP to reach the Jumpbox via SSH (Port 22).  
  * **Egress Rule:** Allows the Jumpbox to reach specific lab subnets (Core Services/Victim Net) on necessary ports (SSH, HTTP, HTTPS).  
  * **Deny-by-Default:** All old rules allowing the Home PC to talk directly to the lab were removed.

## **Secure Tunneling (SOCKS5 Proxy)**

To access internal web GUIs (like Bookstack or Vaultwarden) without exposing them, I implemented an **SSH SOCKS5 Proxy**. Creating a secure tunnel that moves browser traffic directly into the isolated management segment.

### **The Tunnel Mechanism**

I utilize a dynamic port forwarding command that establishes a local listener on my workstation:

\# \-D 1080: Creates a dynamic SOCKS proxy on port 1080  
\# \-C: Enables compression for faster GUI response  
\# \-N: Prevents opening a remote shell (tunnel only)  
ssh \-D 1080 \-C \-N user@192.168.10.50

### **Browser Integration (FoxyProxy)**

To make the tunnel seamless, I configured **FoxyProxy** to use "Proxy by Patterns."

* **Configuration:** Redirects all traffic destined for lab IPs (10.0.x.x) through the localhost:1080 tunnel.

## **Operational Impact**

* **Zero Exposure:** Internal services (Vaultwarden, SIEM) remain entirely invisible to the home network.  
* **Auditable Access:** All administrative actions are funnelled through a single point (the Jumpbox), simplifying log monitoring and access control.  
* **Efficiency:** One-click desktop shortcuts allow for rapid "tunnel-up" operations, maintaining security without sacrificing usability.