# Technical Analysis

This section contains focused technical analysis aligned with the SOC Level 01 scope, examining observable behaviors and detection-relevant artifacts across network, web, and host-level environments.

The analysis emphasizes how visibility is achieved through packet-level inspection, log-based detection, and proactive threat hunting, illustrating how these perspectives complement each other within a SOC context.

## Tools Referenced

* **Wireshark**: Used for packet-level network traffic analysis to examine protocols, flows, and indicators of suspicious behavior.
* **Snort**: Referenced for network-based intrusion detection, rule logic, and understanding how alerts are generated from observed traffic patterns.
* **Sysmon**: Leveraged for granular Windows telemetry, including process creation (Event ID 1) and network connections (Event ID 3).
* **Auditd**: Used for kernel-level Linux auditing to track high-risk syscalls like `execve` and `connect`.
* **Ausearch**: Utilized for parsing and investigating raw Linux audit logs to identify malicious activity.

## Concepts Covered

* **Network & Packet Analysis**: Inspection of traffic at the protocol level to identify anomalous behavior, scanning activity, and man-in-the-middle indicators.
* **Host-Based Threat Detection**: Monitoring for "Living off the Land" (LotL) techniques and unauthorized persistence mechanisms in Windows and Linux environments.
* **Log Filtering and Correlation**: Use of filtering and rule-based logic to isolate relevant events, reduce noise, and surface security-significant activity across multiple telemetry sources.
* **Proactive Threat Hunting**: Applying hypothesis-driven methodologies to search for advanced threats that evade traditional signature-based defenses.

## Analysis Deep Dives

* [**Linux Threat Detection**](./linux-detection.md): Overcoming logging limitations with Auditd and monitoring for Cron/Systemd persistence.
* [**Windows Threat Detection**](./windows-detection.md): Identifying discovery commands and tool transfers using Sysmon and standard Event Logs.
* [**Threat Hunting Principles**](./threat-hunting.md): Exploring the hunting loop, the Pyramid of Pain, and MITRE ATT&CK mapping.

