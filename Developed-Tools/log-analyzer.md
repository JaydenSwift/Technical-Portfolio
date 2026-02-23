# **Log Analyzer: Cross-Language Security Parsing Tool**

The **Log Analyzer App** is a desktop utility designed to streamline the parsing and analysis of complex log files. Developed as a hybrid application using **C\# (WPF)** for the front-end and **Python** for the back-end logic, it provides a toolkit for log analysts.

## **Vision**

The goal of this project was to gain experience with a new programming language (**C\#**) and to build a practical tool for handling the log data often encountered in security exercises. By combining the UI capabilities of WPF with the powerful text-processing libraries of Python, the Log Analyzer offers a smooth experience for dissecting unstructured data.

## **Technical Architecture**

The application utilizes a modular, hybrid architecture to leverage the strengths of different ecosystems.

### **1\. The C\# / WPF Front-End**

The user interface is built using Windows Presentation Foundation (WPF), providing a modern, responsive desktop experience.

* **Modern GUI:** Leverages XAML for a clean, tabbed layout that separates analysis, pattern testing, and settings.  
* **Visualization:** Includes a dedicated ChartViewer to transform raw log statistics into visual images (pie charts, bar graphs).  
* **Pattern Builder:** A built-in PatternBuilderWindow allows users to visually construct and test parsing patterns without leaving the application.

### **2\. The Python Backend**

The main processing of log parsing is handled by Python scripts, due to its performance in string manipulation and data processing.

* **Log Parser (log\_parser.py):** A script that reads raw log files and converts them into structured JSON/Objects based on user-defined patterns.  
* **Pattern Tester (C\_pattern\_tester.py):** Allows users to validate their regex patterns against live sample text before applying them to massive log datasets.

## **Core Features**

### **Advanced Log Parsing**

The app allows users to load massive datasets and apply specific patterns to extract meaningful fields (IPs, Status Codes, Timestamps). This effectively turns thousands of lines of raw text into a searchable, filterable collocation.

### **Pattern Testing & Building**

One of the most powerful features of the app is the integrated pattern environment:

* **Pattern Tester:** Users can paste a single log line and test their regex in real-time.  
* **Pattern Storage:** Common patterns (Syslog, Apache, Auth) are stored in patterns.json for quick recall.

### **Visual Analytics & Export**

Beyond simple parsing, the tool helps identify trends:

* **Visual Reports:** Instantly visualize the frequency of specific events or IP hits.  
* **Export Options:** Once parsed, data can be exported to multiple formats (CSV, JSON) for further investigation or reporting via the ExportOptionsWindow.

## **Technical Stack & Implementation**

* **Frontend:** C\# / WPF (.NET Framework/Core)  
* **Backend:** Python 3  
* **Data Storage:** JSON for configuration and pattern persistence.

## **Learning Outcomes**

This project represents a major milestone in my development journey, specifically:

* **Inter-process Communication:** Learning how to successfully pass data between a C\# GUI and a Python script.  
* **XAML & C\# Development:** Having experience with developing a modern-looking application made in a multi-functional language.   
* **Security Context:** Designing a tool specifically for security workflows, focusing on the speed and accuracy required for incident response.

[**Demo Video**](https://www.linkedin.com/posts/jayden-swift_development-python-coding-activity-7396982578714624000-cC9s?utm_source=share&utm_medium=member_desktop&rcm=ACoAAETLal8BnV5SG_MPz5gKsj1YkIG9OACUzpc)
