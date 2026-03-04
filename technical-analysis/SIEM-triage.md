# SIEM Triage Analysis

[cite_start]This document details the investigative reasoning used to process alerts within a SIEM environment[cite: 32].

### **The 5-Step Triage Framework**
1. [cite_start]**Intake:** Assigning ownership within the SIEM dashboard[cite: 20].
2. [cite_start]**Scoping:** Defining the timeline and identifying affected assets (Source/Destination)[cite: 32, 40].
3. [cite_start]**Contextualization:** Examining raw logs surrounding the alert to identify the "root cause"[cite: 32, 43].
4. [cite_start]**Enrichment:** Validating findings against external threat intelligence (VirusTotal/AbuseIPDB)[cite: 31, 75].
5. [cite_start]**Verdict:** Determining if the activity is a True Positive or a False Positive[cite: 41].

### **Correlation Strategy**
[cite_start]Visibility is achieved by linking disparate logs, such as connecting a web server 404 error spike to a subsequent suspicious process execution on the host[cite: 52, 53].