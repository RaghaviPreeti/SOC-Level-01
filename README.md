# TryHackMe SOC Level 01 – Documentation

This repository contains my analysis, and investigation reports from the **TryHackMe SOC Level 01** path. It covers the transition from basic defensive concepts to practical SIEM triage, malware analysis, and incident investigation.

## Content & Lab Coverage
The documentation is organized into the following technical areas:

### **Security Frameworks**
* Working with **MITRE ATT&CK**, the **Cyber Kill Chain**, and the **Pyramid of Pain** to categorize threats.
* Understanding SOC workflows, alert triage, and escalation procedures.

### **Network & Web Monitoring**
* Analyzing traffic with **Wireshark**, and **Tcpdump**.
* Monitoring for web attacks (SQLi, XSS, Command Injection) via server logs.
* Using **Snort** for intrusion detection.

### **Endpoint & Host Analysis**
* Detecting threats on Windows and Linux using **Sysmon**, **Event Viewer**, and **Journalctl**.
* Identifying **Living-off-the-Land (LotL)** techniques and malicious PowerShell/Bash scripts.

### **SIEM & Malware Analysis**
* Querying and dashboarding within **Splunk** and **Elastic Stack**.
* Performing static and dynamic analysis on malware samples to find C2 IPs and persistence.

## Files

- **[SOC_Level_01_Report.pdf](SOC_Level_01_Report.pdf)**  
  Consolidated documentation outlining key concepts, insights, and analytical observations.
- **`/Case_Reports`**: Report for the capstone challenges.