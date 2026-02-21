# **Centralized Logging Implementation**

# **(pfSense → Logstash → Wazuh)**

This project shows the implementation of a centralized security logging system. The objective was to visualize raw firewall logs from **pfSense** within a **Wazuh (SIEM)** dashboard to enhance network visibility and response capabilities.

## **Overview**

The logging follows a standard ingestion pattern to ensure data is structured and searchable:

1. **Source:** pfSense Firewall (UDP Syslog).  
2. **Ingestion & Transformation:** Logstash (running on Ubuntu) acting as a receiver and parser.  
3. **Indexing:** Wazuh Indexer.  
4. **Visualization:** Wazuh Dashboard (Custom Index Patterns).

## **Implementation Workflow**

### **1\. The Processing Pipeline (pfsense.conf)**

I put in place a pipeline to transform raw, unstructured text into structured information.

* **Input:** Configured a UDP listener on port 5140 to receive incoming firewall logs.  
* **Filter:**  
  * Utilized **Grok patterns** to identify the syslog header.  
  * Applied a **CSV filter** to dissect the comma-separated pfSense payload into searchable fields such as src\_ip, dest\_ip, proto, and action.  
* **Output:** Directed data to the Wazuh Indexer (port 9200\) to be displayed in the manager.

### **2\. Firewall Configuration**

On the pfSense side, I enabled remote logging via **Status \> System Logs \> Settings**, pointing the logged traffic to the Ubuntu server's IP. Sending all logged firewall rules that I deemed important to flag an issue.

## **Troubleshooting & Validation**

Building a log pipe requires verifying every point in the process. I utilized several Linux networking utilities to ensure data is flowing properly:

* **Traffic Arrival:** Used tcpdump to confirm that UDP packets were physically hitting the Ubuntu interface.  
* **Service Listening:** Used netstat to verify Logstash was actively bound to port 5140\.  
* **Log Verification:** Constantly checked the log file to ensure Logstash was processing the pfSense logs.

## **Visualization & Insights**

The culmination of this project is a custom **pfSense Dashboard** within Wazuh, providing:

* **Traffic Distribution:** Pie charts visualizing "Blocked vs. Allowed" traffic by interface.  
* **Real-time Monitoring:** A dedicated "Discover" view filtered by pfsense-\* index patterns, allowing for immediate investigation of specific IP addresses.