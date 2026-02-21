# **Homelab Infrastructure: Hardware & Architecture**

This document outlines the physical and logical hardware configuration of my primary home server node. The goal of this build was to create a power-efficient yet capable virtualization environment for testing, self-hosting, and network experimentation.

## **🖥️ Node Overview: Proxmox Virtualization Environment**

The heart of my lab is a single-node **Proxmox VE** cluster. I chose Proxmox for its robust KVM virtualization, LXC container support, and the flexibility of its Debian-based underpinnings.

* **Role:** Primary Compute & Storage Node  
* **Operating System:** Proxmox VE 8.1.x (No-Subscription)  
* **Optimization:** Configured with intel\_iommu=on for IOMMU grouping, allowing for future PCIe/GPU pass-through to virtual machines.

## **🔩 Physical Hardware**

I selected the **Lenovo P320 Tower** as the foundation due to its workstation-grade reliability, support for ECC memory, and compact form factor.

| Component | Specification | Notes |
| :---- | :---- | :---- |
| **Manufacturer** | Lenovo | ThinkStation P320 Tower |
| **CPU** | Intel Xeon E3-1245 v6 | 4 Cores / 8 Threads @ 3.70GHz |
| **Memory (Original)** | 16 GB DDR4 UDIMM | Initial setup for testing. |
| **Memory (Current)** | 32 GB DDR4 ECC UDIMM | **Upgrade:** Migrated to ECC for data integrity and expanded headroom. |
| **Motherboard** | Custom Lenovo P320 | Workstation-grade chipset. |

## **🛜 Networking & Connectivity**

The networking stack is designed to handle multiple VLANs and bridge traffic between virtual environments and the physical network.

* **Primary NIC (nic0):** Physical Gigabit Ethernet interface.  
* **Virtual Bridge (vmbr0):** The main management interface and gateway for the majority of VM traffic.  
  * **Management IP:** 192.168.0.200/24  
* **Isolated Bridge (vmbr1):** A secondary bridge used for internal traffic and segmenting lab experiments from the primary home network.

## **💾 Storage Architecture**

My storage strategy balances speed for operating systems and high capacity for media/backups.

| Drive Identifier | Capacity | Model | Primary Usage |
| :---- | :---- | :---- | :---- |
| /dev/nvme0n1 | 250 GB | Western Digital SN520 | **Boot & Hot Storage:** Proxmox OS and VM disk images for high IOPS. |
| /dev/sda | 6 TB | Seagate Enterprise (HDD) | **Bulk Storage:** Mass data storage and large media libraries. |
| /dev/sdb | 250 GB | External USB (Seagate) | **Off-Device Backup:** Dedicated to critical VM backups to ensure recovery in the event of an internal disk failure. |

### **Technical Decision: Redundancy vs. Capacity**

By utilizing a high-speed NVMe for the OS and a massive 6TB Enterprise HDD for data, I achieved a "tiered" storage approach. The inclusion of the /dev/sdb external drive provides a manual "air-gap" style backup layer that lives outside the internal SATA/NVMe bus.