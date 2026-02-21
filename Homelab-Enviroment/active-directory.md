# **Identity Management & Automation (Active Directory)**

This project shows the implementation of a scalable, automated Active Directory (AD) environment. By leveraging PowerShell scripting, I’ve transitioned manual administrative tasks into efficient, repeatable processes suitable for an enterprise-scale infrastructure.

## **Key Project Achievements**

### **1\. Bulk Onboarding**

To simulate a real-world enterprise migration or expansion, I developed automation scripts to implement **1,000+ users** from a CSV.

* **Logic:** A Python generator (gen\_users.py) creates randomized staff data, which is then fed into by a PowerShell user import script.  
* **Dynamic Placement:** Instead of dumping users into a single container, the script dynamically calculates the target Organizational Unit (OU) based on the user's department (IT, Sales, HR, etc.).  
* **Automation Snippet:**  
  \# Dynamic pathing based on CSV department attribute  
  $deptOU \= "OU=$($user.Department),OU=Enterprise\_Users,DC=lab,DC=internal"  
  New-ADUser \-Name "$($user.FirstName) $($user.LastName)" \-Path $deptOU ...

### **2\. OU Hierarchy & GPOs**

I implemented a structured Organizational Unit (OU) hierarchy to enforce the principle of least privilege and simplify Group Policy Object (GPO) application.

* **Structure:** Separated Administrative, Resource, and Departmental accounts into distinct sub-OUs.  
* **Security Baselines:** Leveraged GPOs linked to specific OUs to enforce account lockout policies and password complexity.

### **3\. Simple Offboarding & Incident Response**

In a security breach or termination scenario, speed is critical. I developed an **Offboarding Automation** tool to ensure 100% consistency when removing access.

* **Process:**  
  1. Immediately disables the AD account.  
  2. Strips all security group memberships (excluding the Primary Group).  
  3. Clears attributes.  
  4. Appends an audit note to the description field with the date and executor.  
  5. Relocates the object to a quarantined Disabled\_Users OU.

### **4\. Infrastructure Visibility (HTML Dashboard)**

To provide real-time visibility into the domain’s structure and health, I built a custom reporting tool that converts Active Directory queries into a clean, interactive HTML dashboard.

* **Features:**  
  * **User Inventory:** A searchable table of all domain users.  
  * **OU Health:** Indicators for OUs containing disabled accounts.  
  * **Statistical Breakdown:** Pie charts and bar graphs detailing user-to-group ratios and domain statistics.  
* **Output:** Generates a portable .html file that can be hosted on a local web server.