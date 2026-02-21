# **Automated Lab Creation**

This project shows the transition of my homelab from manual virtual machine deployment to an automated workflow. By combining **Terraform** with **Cloud-Init**, I can now create preconfigured, network-ready environments in seconds rather than minutes. Manual deployment can be efficient. So to ensure my "Victim Net" remains consistent, I implemented a workflow to simplify the processes.

## **Security & RBAC Prep**

Before deploying automation, I implemented the **Principle of Least Privilege** for the automation application itself. I avoided using the Proxmox root account, instead creating a dedicated API user and role.

* **Custom Role:** TerraformProv — restricted to specific VM and Datastore operations.  
* **Authentication:** Utilized Proxmox API Tokens with privilege separation disabled to allow the Terraform provider to manage resources securely.

## **The Template Image (Cloud-Init)**

To enable rapid scaling, I built an **Ubuntu 24.04 (Noble)** Cloud-Init template. This template image is stripped of unnecessary hardware drivers and pre-configured to listen for initialization instructions.

### **The Configuration Snippet (lab-init.yaml)**

I use Cloud-Init snippets to inject security configurations during the first boot. This ensures every machine starts with:

* A dedicated automation user.  
* Pre-authorized SSH Ed25519 keys.  
* The qemu-guest-agent enabled for host-to-guest communication.

## **Terraform Configuration (main.tf)**

The configuration below clones the template image and dynamically assigns network interfaces to specific VLANs (e.g., VLAN 20 for the Victim Net).

resource "proxmox\_virtual\_environment\_vm" "Ubuntu-Victim" {  
  name      \= "ubuntu-target-01"  
  node\_name \= "pve"  
  vm\_id     \= 901

  clone {  
    vm\_id \= 1000 \# ID of the Ubuntu Template  
  }

  network\_device {  
    bridge  \= "vmbr1"  
    vlan\_id \= 20 \# Isolated Victim Subnet  
  }

  initialization {  
    user\_data\_file\_id \= "local:snippets/lab-init.yaml"  
    ip\_config {  
      ipv4 {  
        address \= "10.0.2.10/24"  
        gateway \= "10.0.2.1"  
      }  
    }  
  }  
}

## **Agentless Security Monitoring**

For legacy systems or minimal environments (like **Metasploitable**) where installing a Wazuh agent is not available, I implemented an **Agentless Syslog Pipeline**.

### **The Monitoring Path**

1. **Wazuh Manager:** Enabled a Remote Syslog listener on UDP port 514\.  
2. **Network Security:** Configured pfSense to allow Syslog traffic from the isolated VLAN 20 to the Wazuh Manager in the management segment.  
3. **Endpoint Forwarding:** Configured the target machine's syslog.conf to forward all logs to the SIEM:  
   \*.\* @\[WAZUH\_MANAGER\_IP\]

## **Operational Impact**

* **Deployment Speed:** Drastically reduced the time to deploy a networked Linux environment.  
* **Consistency:** Every machine is guaranteed to have the same security patches and SSH keys upon creation.  
* **Scalability:** I can scale the "Victim Net" for red-team exercises by simply adjusting the count variable in Terraform.