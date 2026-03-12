# **SOAR Automation Pipeline: Integrated Detection, Response & Forensics**

This project shows the design and implementation of an automated security pipeline. By integrating **Wazuh (SIEM)**, **Shuffle (SOAR)**, **TheHive (Case Management)**, and **Velociraptor (Endpoint Forensics)**, I have created a cohesive system that automates the transition from threat detection to incident response and forensic investigation.

## **Overview**

The pipeline follows a simple loop:

1. Wazuh detects a security event (e.g., a brute-force SSH attack).  
2. Shuffle receives the alert via Webhook and parses the data.  
3. Shuffle queries Velociptor to start a collection on the client and formats the response.  
4. TheHive automatically creates an alert for review, populated with the forensic evidence.

## **Phase 1: Infrastructure & Orchestration (Shuffle & TheHive)**

To maximize resource efficiency on the Proxmox cluster, I deployed **Shuffle** and **TheHive** using **Docker** on a dedicated Ubuntu VM.

* **Optimization:** Adjusted the host parameters and configuration files to support Elasticsearch and OpenSearch, and to ensure the system uses the appropriate amount of RAM.

## **Phase 2: The Ingestion (Wazuh → Shuffle)**

The integration between Wazuh and Shuffle is done through an event-driven webhook that sends alerts only when it meets the alert-level threshold.

* **Wazuh Configuration:** Modified the ossec.conf file to include a custom \<integration\> block. This includes the verification for the webhook and the alert threshold.  
* **Validation:** Implemented a testing workflow using ssh nonexistentuser@\<IP\> to trigger a Rule 5712 (SSHD failure), verifying the "pipe" by monitoring the ossec.log for successful integration execution.

## **Phase 3: Case Management (Shuffle → TheHive)**

To make these alerts easier to manage, I automated the creation of TheHive alerts based on the information from Wazuh

* **Service Authentication:** Created a dedicated **Service Account** in TheHive with an "Analyst" profile, generating an API key for the Shuffle integration.  
* **Data Mapping:** Configured Shuffle to map dynamic Wazuh alert variables ($exec.rule.description, $exec.agent.name, $exec.id) directly into TheHive alert fields. Providing immediate context without going back to the SIEM.

## **Phase 4: Automated Forensics (Shuffle → Velociraptor)**

The final layer of the pipeline automates forensic investigation. When an alert triggers, Shuffle queries **Velociraptor** for endpoint data.

* **API Connection:** Configured Shuffle to interact with Velociraptor’s API using a generated api.config.yaml file.  
* **Automated Evidence Collection:** Upon a detection of suspicious SSH activity, Shuffle executes a custom Velociraptor query to pull recent journal logs from the target host and attach them to the TheHive alert.

## **Operational Impact**

* **Reduced MTTR (Mean Time to Respond):** By automating case creation and forensic log retrieval, analysis can be performed without gathering contextual information.  
* **Awareness:** The pipeline links network-level alerts (Wazuh) with host-level forensics (Velociraptor) and organizational tracking (TheHive).  
* **Scalability:** The infrastructure and workflows are very modular. Making it easy to expand detection types, forensic collections, and detailed alerts.