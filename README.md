# **Technical Portfolio: Infrastructure & Security Engineering**

A collection of projects focused on building, securing, and automating complex environments. This portfolio documents a journey through systems administration, security operations (SecOps), and custom tool development.

## **Vision**

The goal of this environment is to simulate enterprise-grade security challenges within a controlled homelab. By bridging the gap between **Infrastructure as Code (IaC)** and **Defensive Security**, this lab serves as a testing ground for automation, incident response, and custom security software.

## **Core Pillars**

### **1\. Infrastructure & Secure Networking**

The foundation of this portfolio is a segmented virtualization environment built on **Proxmox**.

* **Airlock Access Model:** Implementation of a "Jumpbox" architecture to isolate management traffic from the primary home network using VLAN segmentation and SOCKS5 SSH tunneling.  
* **Virtual Perimeter:** Centralized routing and firewalling via **pfSense**, managing multiple subnets including a DMZ, Core Services, and an isolated "Victim Net."  
* **Centralized Management:** A "Landing Pad" file repository for lab-wide script distribution and auditing.

### **2\. Security Tooling & Development**

Hybrid applications designed to solve specific security workflows, utilizing **C\# (WPF)**, **Python**, and **PowerShell**.

* **Security Parsing:** Tools built to transform unstructured log data into actionable intelligence through custom regex engines and visual analytics.  
* **Compliance Auditing:** System utilities that bridge the gap between passive monitoring and active security verification, using XML-based rule ingestion for dynamic compliance checks.  
* **Identity Management:** Full-lifecycle automation for Active Directory, ranging from bulk user onboarding to rapid offboarding scripts for incident response.

### **3\. Automation & Orchestration (IaC)**

Moving away from manual configuration toward a "rebuildable" infrastructure.

* **Environment Provisioning:** Using **Terraform** paired with **Cloud-Init** to deploy pre-hardened, network-ready Linux environments in seconds.  
* **Centralized Logging (SIEM):** A data pipeline routing traffic from firewalls through **Logstash** and finally into **Wazuh** for security monitoring and threat hunting.

## **Repository Structure**

**├── Developed-Tools/                      \# Custom security applications & scripts**

**│   ├── log-analyzer.md                   \# C\# & Python hybrid parsing tool**

**│   └── vigilant-application.md           \# Windows compliance auditing tool**

**├── Homelab-Enviroment/              \# Infrastructure documentation & IaC**

**│   ├── assets/                                  \# Dashboards, network diagrams & screenshots**

**│   ├── node-information/                \# Hardware specs & network topology details**

**│   ├── active-directory.md              \# AD automation & identity management**

**│   ├── file-browser.md                    \# Centralized "Landing Pad" implementation**

**│   ├── jumpbox-configuration.md \# Secure SSH/SOCKS5 access architecture**

**│   ├── logstash-intergration.md    \# Centralized logging & pipeline config**

**│   └── terraform-automation.md   \# IaC workflows for "Victim Net" creation**

**└── README.md                             \# Portfolio overview & vision**

## **Tech Stack**

* **Hypervisors:** Proxmox VE  
* **Networking:** pfSense, VLAN Tagging, SOCKS5  
* **Languages:** Python (CustomTkinter), C\# (WPF), PowerShell, Bash  
* **Monitoring/SIEM:** Wazuh, Logstash, Syslog  
* **Automation:** Terraform, Cloud-Init, API Integration

*Developed and maintained by Jayden Swift.*
