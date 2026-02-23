# **Real-Time Monitoring with Pulse**

This document shows the implementation of **Pulse**, a e monitoring dashboard, to provide real-time information for my Proxmox lab. The goal of this implementation was to establish a single view for CPU, memory, and storage usage across all machines and containers.

## **Deployment Strategy**

To ensure a clean and quick install, I utilized the **Proxmox Helper Scripts** library to deploy Pulse within a Linux Container (LXC). This method ensures that the container is pre-configured with the correct resources and dependencies.

### **Technical Specifications**

* **Base OS:** Debian/Alpine (Minimal)  
* **Compute:** 1 vCPU / 1GB RAM  
* **Storage:** 4GB Boot Disk  
* **Network:** Static information assigned through the “Advanced Install” option.

### **Installation Workflow**

The deployment was executed through the Proxmox host shell, pulling the community script to automate the container build process:

\# Automated Pulse LXC Provisioning  
bash \-c "$(wget \-qLO \- \[https://github.com/community-scripts/ProxmoxVE/raw/main/ct/pulse.sh\](https://github.com/community-scripts/ProxmoxVE/raw/main/ct/pulse.sh))"

## **Security & Integration (The Proxmox Link)**

A critical aspect of this integration was establishing a secure link between the Pulse dashboard and the Proxmox lab.

### **API Token Authentication**

Rather than using the primary root@pam account for Pulse’s management I created a dedicated user and I implemented **API Token-based authentication**:

1. **RBAC Configuration:** Created a dedicated monitoring role in Proxmox with Audit permissions.  
2. **Token Generation:** Generated a unique Token ID and Secret specifically for the Pulse service.  
3. **Encrypted Handshake:** Linked Pulse to the Proxmox API endpoint (https://\<IP\>:8006) using the token, ensuring the dashboard can "read" cluster data without having "write" or "delete" privileges.

## **Operational Insights**

Once linked, the Pulse dashboard provides immediate visibility into:

* **Node Uptime:** Tracking the stability of the physical Lenovo P320 host.  
* **LXC/VM Load:** Identifying which virtual machines are consuming the most IOPS, memory, or storage usage.  
* **Storage Latency:** Monitoring the health of the various storage devices.  
* **Network Throughput:** Visualizing traffic across the `vmbr0` and `vmbr1` bridges.

