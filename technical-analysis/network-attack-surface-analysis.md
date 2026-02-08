# Network Attack Surface Analysis

### Analysis Scope

This document captures technical observations made during network security monitoring exercises. The focus is on how packet-level inspection and protocol behavior expose different parts of the network attack surface and how these observations support detection and investigation.

Network visibility was explored using Wireshark for packet inspection and Snort for understanding how observed behaviors translate into network-based detection logic.

### ICMP and DNS-Based Reconnaissance Indicators

ICMP traffic was examined to observe reachability and probing behavior at the network layer. ICMP packets were isolated using:
```wireshark
icmp
```
Viewing ICMP traffic independently helped surface echo requests that appeared without corresponding application-layer communication. This pattern is commonly associated with host discovery activity rather than routine diagnostic usage.

DNS analysis focused on identifying anomalous query behavior rather than simple name resolution success or failure. Suspicious activity was isolated using:
```wireshark
dns.qry.name.len > 15 and !mdns
```
Filtering on query length highlighted unusually long domain names that stood out from typical DNS resolution traffic. Excluding multicast DNS traffic reduced background noise and made these anomalies more visible. In several cases, repeated communication involving multiple source IPs and a single DNS server suggested coordinated or non-standard usage.

Further inspection targeted known tunneling indicators embedded within DNS traffic:
```wireshark
dns contains "dnscat"
```
This filter exposed explicit tool-related artifacts, allowing suspicious DNS activity to be identified without relying solely on behavioral inference.

### HTTP Request, Response, and Enumeration Patterns

HTTP traffic analysis focused on server responses and requests. Authentication and authorization boundaries were isolated by filtering for response codes:
```wireshark
http.response.code == 401 || http.response.code == 403
```
Repeated authentication failures against specific resources indicated deliberate access attempts rather than accidental user behavior.

Enumeration behavior became clearer when client error responses were grouped together:
```wireshark
http.response.code == 404 || http.response.code == 405
```
These responses revealed repeated probing of unavailable or restricted endpoints.

Inspection of full request paths provided additional context into targeting behavior:
```wireshark
http.request.full_uri contains "admin"
```
This highlighted repeated attempts to access administrative interfaces, which appeared without successful follow-up, suggesting focused probing.

User-Agent values were then used to distinguish automated tooling from browser-based traffic:
```wireshark
http.user_agent contains "sqlmap" ||
http.user_agent contains "nmap" ||
http.user_agent contains "wfuzz" ||
http.user_agent contains "nikto"
```
Once isolated, these sessions consistently aligned with aggressive request patterns and high failure rates, making them strong candidates for prioritization during analysis.

#### FTP Authentication and Credential Exposure

FTP traffic exposed authentication behavior directly at the packet level. Credential exchange was isolated using:
```wireshark
ftp.request.command == "PASS"
```
This revealed cleartext passwords in transit, making credential reuse patterns immediately visible without reconstructing full sessions.

Failed login attempts were further identified through:
```wireshark
ftp.response.code == 530
```
Repeated authentication failures against specific usernames suggested deliberate login attempts rather than misconfiguration or user error.

### Encrypted Traffic Visibility Boundaries

As traffic transitioned to encrypted protocols, payload inspection was no longer possible. Analysis shifted to session establishment behavior using:
```wirshark
tls.handshake.type == 1 || tls.handshake.type == 2
```
Handshake-level inspection emphasized the limitations of packet analysis under encryption and reinforced the importance of correlating network metadata with other telemetry sources.

### From Packet Analysis to Detection (Snort)

To complement packet inspection, Snort was examined to understand how observed network behaviors are translated into detection logic. Snort was executed in IDS mode using:
```bash
sudo snort -c /etc/snort/snort.conf
```
Reviewing Snort execution modes and rule structure clarified how repeated authentication failures, protocol misuse, and anomalous traffic patterns identified during packet analysis can be operationalized into alerts.

### Consolidated Observation

Across protocols, meaningful signals emerged through repeated behavior and response patterns. Cleartext protocols offered high visibility into misuse, while DNS and encrypted traffic required structural and contextual analysis. In all cases, understanding how traffic behaved over time proved more valuable than inspecting individual events.