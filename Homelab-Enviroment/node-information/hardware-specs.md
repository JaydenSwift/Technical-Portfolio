# **Homelab Infrastructure: Hardware & Architecture**

This document shows the hardware configuration of my Proxmox homelab. The goal of this build was to create a power-efficient system that is capable of hosting a virtualization environment for testing, practical experience, and network experimentation.

## **Proxmox Virtualization Environment**

The center of my lab is a single-node **Proxmox VE** cluster. I chose Proxmox for its robust features, virtualization, and LXC container support.

* **Role:** Primary Compute & Storage Node  
* **Operating System:** Proxmox VE 9.1

## **Physical Hardware**

I selected the **Lenovo P320 Tower** as the foundation due to its reliability, support for ECC memory, and power efficiency.

| Component | Specification | Notes |
| :---- | :---- | :---- |
| **Manufacturer** | Lenovo | ThinkStation P320 Tower |
| **CPU** | Intel Xeon E3-1245 v6 | 4 Cores / 8 Threads |
| **Memory (Original)** | 16 GB DDR4 UDIMM | Initial setup for testing. |
| **Memory (Current)** | 32 GB DDR4 ECC UDIMM | **Upgrade:** Migrated to ECC for its data integrity and expanded headroom. |
| **Motherboard** | Custom Lenovo P320 | Workstation-grade chipset. |

## **Networking & Connectivity**

The networking setup is designed to handle multiple VLANs and bridge traffic between virtual environments and the physical network.

* **Primary NIC (nic0):** Physical Ethernet interface.  
* **Virtual Bridge (vmbr0):** The main interface facing my home network.  
* **Isolated Bridge (vmbr1):** A secondary bridge used for internal traffic and segmenting the lab from the home network.

## **Storage Architecture**

My storage strategy balances speed for operating systems and high capacity for media/backups.

| Drive Identifier | Capacity | Model | Primary Usage |
| :---- | :---- | :---- | :---- |
| /dev/nvme0n1 | 250 GB | Western Digital SN520 | **Boot & Hot Storage:** Proxmox OS and VM disk images for high IOPS. |
| /dev/sda | 6 TB | Seagate Enterprise (HDD) | **Bulk Storage:** Mass data storage. |
| /dev/sdb | 250 GB | External USB (Seagate) | **Off-Device Backup:** Dedicated to critical VM backups to ensure recovery in the event of an internal failure. |
