# **Centralized File Management: The "Landing Pad"**

This document shows the implementation of a centralized file management system, known as my **"Landing Pad."** By deploying a lightweight **File Browser** instance in a Proxmox LXC, I’ve created a central repository for scripts and other files that is accessible across the lab.

## **The Objective: Friction in File Transfer**

In a multi-segmented lab, moving scripts from a management machine to isolated machines or servers often involves a tedious process. My goal was to create a central pool that could be interacted with via both a web GUI and a command-line interface, ensuring file transfers no longer take more time than necessary.

## **Architecture & Implementation**

### **1\. Lightweight Provisioning (LXC)**

I utilized a **Debian 12 Linux Container (LXC)** for this service due to its lightweight.

* **Resources:** 512MB RAM / 8GB Disk.  
* **Network:** Assigned a static IP (10.0.0.102) to ensure that automated curl scripts across the lab remain persistent.

### **2\. Proxmox Bind Mounts**

To ensure the data persists even if the container is deleted, I mapped a directory from the Proxmox physical host directly into the container using **Bind Mounts**. This allows the "Landing Pad" to sit on the high-capacity storage pool while the container remains small.

* **Host Path:** /tank/landing-pad.  
* **Container Path:** /mnt/landing-pad.  
* **Configuration:**  
  \# Executed on Proxmox Host to link the storage  
  pct set 105 \-mp0 /tank/landing-pad,mp=/mnt/landing-pad

### **3\. File Browser Deployment**

I chose **File Browser** for its simplicity and powerful CLI capabilities.

* **Installation:** Installed via a curl command and initialized the database at /etc/filebrowser.db.  
* **Configuration:** Bound the service to 0.0.0.0:8080 to allow lab access and set the root directory to the bind-mounted /mnt/landing-pad.  
* **Service Persistence:** Created a systemd unit file to ensure the application starts automatically on boot.

## **Workflow: Interacting with the "Landing Pad"**

The usefulness of this setup is how different machines in the lab interact with it based on their context.

### **A. Web GUI**

From my Jumpbox or Management PC, I use the web interface to upload new PowerShell automation scripts or security tools. The interface provides a clean, folder-based view of the /tank/landing-pad directory.

### **B. Console-Only Interaction**

For "Victim" nodes or minimal Linux servers without a GUI, I developed a standardized interaction method using curl. Which allows me to pull tools directly into a terminal session without needing to configure SMB or NFS shares on every machine.

## **Impact**

* **Reduced Friction:** Drastically decreased the time spent configuring a machine with the necessary tools.  
* **Data Integrity:** Because the data lives in a bind mount, it is included in host-level snapshots and backups.  
* **Security:** By using a centralized landing point, I can audit exactly what scripts are being distributed across the lab segments from a single location.