# **Vigilant: Windows System Compliance & Analysis Tool**

Vigilant is a Windows system utility designed to provide a comprehensive snapshot of system health while offering the ability for security compliance auditing. Built with **Python** and **CustomTkinter**, it bridges the gap between passive system monitoring and active security verification.

## **Core Features**

The application utilizes a dual-pane interface to provide great visibility without cluttering the user experience.

### **1\. Persistent System Overview (Left Pane)**

A dedicated sidebar provides a basic information of the machine, ensuring metrics are always visible regardless of the active tab:

* **Host Identification:** Node name, OS version.  
* **Resource Monitoring:** Real-time CPU usage and Memory usage.  
* **Network Mapping:** Detailed interface listing including MAC, IPv4, and IPv6 addresses.  
* **Storage Health:** Capacity and free space tracking for primary system drives.

### **2\. Analysis Tabs (Right Pane)**

The main interface is segmented into five functional areas:

* **Processes & Programs:** Searchable and sortable inventories of active execution threads and installed software packages.  
* **Identity Management:** Detailed view of local user accounts and their current status (Enabled/Disabled).  
* **Policy Audit:** Deep dives into Windows system policies, including password age, lockout thresholds, and audit logging configurations.

## **The Compliance Functionality**

The centerpiece of Vigilant is its dynamic **Security Compliance Tab**. Vigilant uses an architecture where the check logic is defined in external XML files.

### **Key Capabilities:**

* **Dynamic Rule Ingestion:** Users can load custom XML-formatted compliance checks to run specific audits without modifying the source code.  
* **Multi-Method Validation:** The checker supports multiple check types:  
  * **Registry:** Verifies keys and values against expected security baselines.  
  * **PowerShell:** Executes secure background commands to verify system states.  
  * **User/Group Audit:** Checks for the presence or status of specific security principals.  
* **Visual Reporting:** Results are color-coded (Pass/Fail/Error) with a bar chart for statistical analysis.  
* **Data Portability:** Audit results can be exported as **JSON** for external reporting.

## **Technical Stack & Implementation**

Vigilant leverages several Python libraries and Windows-specific APIs:

* **UI Framework:** CustomTkinter for a modern interface.  
* **System Metrics:** psutil and platform for hardware and process data.  
* **Windows Integration:** winreg and pywin32 for direct interaction with the Windows Registry and Network Management APIs.  
* **Concurrency:** Utilizes threading to ensure the GUI remains responsive while performing intensive system scans or PowerShell executions.

[**Demo Video**](https://www.linkedin.com/posts/jayden-swift_development-python-customtkinter-activity-7374109934965346304-lAwG?utm_source=share&utm_medium=member_desktop&rcm=ACoAAETLal8BnV5SG_MPz5gKsj1YkIG9OACUzpc)