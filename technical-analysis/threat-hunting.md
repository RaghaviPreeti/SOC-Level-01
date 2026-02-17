# Threat Hunting Principles & Methodologies

## Overview
Threat hunting is the proactive process of searching through networks and datasets to detect and isolate advanced threats that evade existing security solutions. Unlike reactive monitoring, threat hunting starts with the assumption that a breach has already occurred.

---

## 1. The Threat Hunting Loop
A successful hunt follows a repeatable, structured cycle to ensure consistent results and continuous improvement.

1. **Hypothesis Generation**: Formulate a theory based on threat intelligence, new vulnerabilities, or observed attacker TTPs (e.g., "Attackers are using DNS tunneling for C2 exfiltration").
2. **Data Collection & Tooling**: Identify the necessary logs (Sysmon, Auditd, Firewall) and tools (SIEM, EDR) required to test the hypothesis.
3. **Investigation & Analysis**: Execute queries to look for patterns, outliers, or "weak signals" that indicate malicious activity.
4. **Action & Automation**: If a threat is discovered, trigger Incident Response. If not, use the findings to improve automated detection rules to prevent future occurrences.

---

## 2. Core Frameworks & Models

### The Pyramid of Pain
This model ranks indicators of compromise (IoCs) by the "pain" they cause an attacker when a defender denies them.
* **Hash Values (Easy)**: Easily changed by the attacker; low impact to block.
* **IP Addresses/Domain Names (Simple)**: Still relatively easy for attackers to rotate.
* **TTPs - Tactics, Techniques, & Procedures (Tough)**: Focuses on *how* the attacker operates. This is the goal of professional threat hunting as it forces the attacker to reinvent their entire methodology.

### MITRE ATT&CK Mapping
The MITRE ATT&CK framework is used to categorize adversary behavior into a matrix of tactics and techniques.
* **Purpose**: Helps hunters identify visibility gaps and prioritize hunting missions based on real-world threat actor behaviors.

---

## 3. Common Hunting Hypotheses
Below are examples of hunting missions based on the SOC Level 01 curriculum:

* **Hunting for Persistence**: Searching for unusual Scheduled Tasks (Event ID 4698) or new Systemd services that run from `/tmp` or hidden directories.
* **Hunting for Living off the Land (LotL)**: Identifying instances where `certutil.exe` or `curl.exe` are used to download executable files from the internet.
* **Hunting for Discovery Activity**: Identifying internal hosts running multiple discovery commands (`whoami`, `netstat`, `ipconfig`) in a short time window.

---

## 4. Key Metrics for Success
The goal of threat hunting is to reduce the "window of opportunity" for an attacker.
* **Mean Time to Detect (MTTD)**: Reducing the time it takes to discover a threat.
* **Mean Time to Respond (MTTR)**: Reducing the time from detection to successful remediation.

---

## 5. Summary of Defensive Mindset
* **Assume Breach**: Never assume your perimeter is 100% effective.
* **Know Your Normal**: You cannot find "abnormal" behavior if you do not understand the baseline of your environment.
* **Continuous Improvement**: Every hunt should result in a better-informed defense, whether a threat was found or not.