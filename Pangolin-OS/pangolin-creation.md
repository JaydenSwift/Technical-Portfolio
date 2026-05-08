# **Engineering a Custom Security-Focused Operating System: Pangolin OS**

**Objective:** To create a minimal-footprint Linux distribution tailored for IT management, and cybersecurity compliance. With this project, I aim to further understand the foundation of Linux components and build a full operating system for my needs.

## **Architecture Overview**

Pangolin OS uses a fully curated Arch Linux base. With this base and a minimal methodology, the OS guarantees a minimized attack surface and efficient capabilities. The environment is also intentionally designed for stability across hypervisors or VM managers and implements a script to automate the configuration and installation of the system.

## **Implementation Phases**

### **Phase 1: OS build & Package Management** 

To ensure the integrity and reproducibility of the OS, the build process was done using archiso within a Linux subsystem (WSL2).

* **Purpose:** With these tools in use, the core build of the OS is done through a simple process, allowing for more attention to the usability of the system in terms of establishing the needed packages and file layout.   
* **Technical Implementation:** Curated a strict dependency tree. The OS was pre-loaded with a few security and compliance tools, including nmap, tcpdump, and Wireshark, while stripping out non-essential packages.

### **Phase 2: User Interface & Virtualization Stability**

For the graphical environment, a balance between clean UI capabilities and efficient execution within virtual environments was needed. Therefore, the system was built with a stack of X11 and AwesomeWM.

* **Purpose:** With the implemented display server and window manager, the UI can run without issues on multiple platforms due to the compatibility of X11. The addition of AwesomeWM allows for the creation of a modern interface through simple configuration and gives great availability to increase efficiency through shortcuts.  
* **Technical Implementation:** Created a Lua-based window manager configuration (rc.lua) focused on keyboard-driven shortcuts and ease of use.

\-- Snippet: Window management  
awful.rules.rules \= {  
    { rule \= { },  
      properties \= {   
                     manager \= true,  
                     border\_width \= 0, \-- Minimized chrome for maximum terminal visibility  
                     focus \= awful.client.focus.filter,  
                     raise \= true,  
                     keys \= clientkeys,  
                     screen \= awful.screen.preferred,  
                     placement \= awful.placement.no\_overlap+awful.placement.no\_offscreen  
     }  
    },  
}

### **Phase 3: Automated Configuration**

To deploy the OS without human error and unnecessary repetitive tasks, a script was created to handle the core installation of Arch Linux.

* **Purpose:** The interaction between archiso and the archinstall script makes package implementation and service activation cumbersome therefore, the script ensures that once the system is rebooted after install the OS is ready for use.  
* **Technical Implementation:** Developed a bash script called pangolin-setup that handles disk partitioning, base system installation, and the post-install injection of the curated packages and UI configuration.

## **Impact**

* **Attack Surface Reduction:** By utilizing a completely custom Arch build, all services and applications are intentional and known. The OS runs only what is explicitly required for the task of system management.  
* **Quick Implementation:** Deploying Pangolin OS immediately gives a fully configured toolset, eliminating the time spent provisioning and updating security tools on a fresh installation.  
* **Elimination of Configuration Errors:** The combination of the archinstall profiles and the pangolin-setup deployment script ensures that, wherever it’s deployed, the security posture and toolset remain consistent.
