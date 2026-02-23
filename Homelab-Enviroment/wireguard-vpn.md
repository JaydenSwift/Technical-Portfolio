# **WireGuard VPN & Hardened SSH**

This project shows the implementation of added security for my remote access pipeline to the homelab. By combining a **WireGuard VPN** tunnel with **Identity-Based SSH** authentication, I removed the need for unnecessary exposure and improved access speeds.

## **Architecture Strategy**

The access method now relies on three layers of security:

1. **The Perimeter:** A WireGuard VPN tunnel that acts as the only entry point into the network.  
2. **The Airlock:** A JumpBox that serves as the internal management hub.  
3. **The Identity:** Cryptographic SSH keys (Ed25519) that replace manual password entry.

## **WireGuard VPN**

I chose WireGuard for its ease of use and implementation, for both the firewall and client devices.

### **pfSense Server Configuration**

* **Tunneling:** Created a tunnel on UDP port 51820 with a unique subnet (10.0.50.0/24) to isolate VPN clients from the primary LAN.  
* **Firewall Configuration:** Opened port 51820 only to allow the encrypted handshakes.  
  * **WireGuard Rule:** The rules placed on the WireGuard interface make sure that, once connected to the VPN, clients can only access the **JumpBox IP**.

## **Hardened SSH Identity Management**

To increase speeds and security, I opted to transfer from traditional login credentials to using ssh keys on the JumpBox machine.

### **1\. Key Generation & Provisioning**

I utilized the ed25519 algorithm.

* **Provisioning:** The public key was manually placed into the JumpBox authorized\_keys file, with permissions enforced to ensure proper access.

### **2\. Operational Efficiency**

To simplify my workflow, I implemented a local SSH configuration file. This allows for "One-Click" access and automated SOCKS5 proxying.

**Configuration Snippet (\~/.ssh/config):**

Host jumpbox  
    HostName 10.0.1.x  
    User lab\_admin  
    IdentityFile \~/.ssh/id\_ed25519  
    DynamicForward 8080 \# Automated SOCKS5 Tunneling

## **One-Click Access Workflow**

With that SSH config file i could create a desktop shortcut to trigger the management session instantly:

powershell.exe \-NoExit \-Command "ssh jumpbox"

The command initializes the SSH session through the VPN tunnel, starts the SOCKS5 proxy, and provides an immediate terminal on the JumpBox machine within my DMZ.

