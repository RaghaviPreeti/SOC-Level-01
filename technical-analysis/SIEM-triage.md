# SIEM Triage Analysis

This document details the investigative reasoning used to process alerts within a SIEM environment.

### **The 5-Step Triage Framework**
1. **Intake:** Assigning ownership within the SIEM dashboard.
2. **Scoping:** Defining the timeline and identifying affected assets (Source/Destination).
3. **Contextualization:** Examining raw logs surrounding the alert to identify the "**root cause**".
4. **Enrichment:** Validating findings against external threat intelligence (VirusTotal/AbuseIPDB).
5. **Verdict:** Determining if the activity is a True Positive or a False Positive.

### **Correlation Strategy**
Visibility is achieved by linking disparate logs, such as connecting a web server 404 error spike to a subsequent suspicious process execution on the host.