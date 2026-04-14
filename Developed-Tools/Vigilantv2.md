# **Vigilant: Local Tool to Cross-Platform Security Application**

This summary shows the improvements made to my security compliance application, Vigilant. With this new version, it still retains the core functionality of the previous version, covering system info like CPU and memory usage, as well as other information, including user accounts and running processes. The improvement focuses on multi-node management and cross-platform capabilities. 

## **Comparison: Vigilant 1.x vs. 2.0**

| Feature | Original Version (1.x) | New Version (2.0) |
| :---- | :---- | :---- |
| **Architecture** | Single System CustomTkinter Desktop App | Streamlit Web Application \+ Remote Agents |
| **Reach** | Local Machine Only | Multi-Node Remote Management |
| **OS Support** | Primarily Windows-focused | Windows \+ Debian/Ubuntu |
| **Connectivity** | Local System Calls / Registry | Remote Protocols (SSH, WinRM) |
| **Audit Logic** | Embedded Python Checks | XML-Based Benchmarks |
| **Visualizations** | Static Text Labels | Dynamic ECharts |

## **Technical Improvements**

### **GUI Enhancements**

With CustomTkinter, the interface was very limited and was unreliable when too many elements were active. This led to the migration to a modern web interface.

* **Streamlit Interface:** The GUI is fully built with the Streamlit library in Python, providing a highly customizable website that is extremely easy to create and implement.  
* **Visualization:** Charts, tables, and graphs have greatly improved with this version due to the core features of Streamlit as well as its extension Echarts, which wasn’t an option with the local version of Vigilant.

### **Implementation of Cross-Platform Ability**

The constraints of the Windows focus logic limited the first version. Version 2.0 was refactored to handle multiple operating systems in a streamlined process.

* **Linux Integration:** To make Linux machines accessible to Vigilant, SSH interaction was implemented using Paramiko, allowing for remote systemctl service auditing and file analysis.  
* **Windows Orchestration:** To improve on the previous logic for Windows machines, pywinrm is used, enabling remote PowerShell execution without requiring a persistent GUI session on the target.

### **Agent Interaction**

The compliance and system information collection logic was upgraded from hard-coded checks to an agent-focused model.

* **Remote Execution:** Each system connected to Vigilant has an agent file present, which collects the system information and is used as a tool to execute compliance checks on the system.  
* **XML Delivery:** The Implementation of a base64-chunking upload method was also needed to bypass WinRM command-length limitations when delivering large audit benchmarks and simplify execution for Linux machines.

## **Convenience**

### **1\. Centralized Monitoring**

The transition to a web-based dashboard allows for a "Single Pane of Glass" view. There is no longer a need to RDP/SSH into individual machines to verify their security posture.

### **2\. Continuous Monitoring**

This new version of Vigilant includes an auto-refresh feature, providing updated system information at a customizable interval.

### **3\. Constant Access**

With the nature of the script, it can be deployed using systemd. Making Vigilant active as long as its server is as well, so it won’t require an active terminal.

[**Demo Video**](https://www.linkedin.com/posts/jayden-swift_cybersecurity-python-streamlit-activity-7449904963398402048-LC6o?utm_source=share&utm_medium=member_desktop&rcm=ACoAAETLal8BnV5SG_MPz5gKsj1YkIG9OACUzpc)