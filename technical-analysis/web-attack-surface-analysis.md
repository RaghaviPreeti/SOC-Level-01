# Web Attack Surface Analysis

### Analysis Scope

This document captures technical observations from web security monitoring activities. The focus is on how application-layer behavior exposes attack surface and what indicators become visible through request patterns, logs, and host-level telemetry.

The analysis covers common web attack indicators, web shell behavior, and application-layer denial-of-service patterns as they appear during monitoring.

---

### Common Indicators of Web-Based Attacks

Web-based attacks often blend into legitimate traffic, making detection dependent on subtle inconsistencies rather than obvious protocol violations. Indicators typically emerge through repeated patterns in request metadata.

Observed indicators included:
- Unusual or non-browser User-Agent strings
- Requests originating from unexpected or inconsistent IP addresses
- Abnormal query strings containing encoded or command-like parameters
- Missing or inconsistent referrer headers suggesting direct access to resources

When correlated over time, these indicators helped distinguish automated or malicious interaction from normal user behavior.

---

### Role of Web Application Firewalls

Web Application Firewalls (WAFs) were introduced as a defensive layer for inspecting and filtering HTTP requests at the application boundary. WAFs provide protection against known attack patterns by enforcing request validation and rule-based blocking.

The analysis highlighted that while WAFs reduce exposure to common attacks, they are most effective when paired with monitoring and log analysis. Application-specific abuse and evasive behavior often require contextual review beyond generic rule matches.

---

### Web Shell Detection Indicators

Web shell detection centered on identifying persistence mechanisms rather than one-time exploitation attempts. Web shells often attempt to remain low-noise and resemble legitimate application components, making detection dependent on correlating multiple weak signals.

Key indicators included:
- Suspicious User-Agent strings inconsistent with typical browser traffic
- Query strings containing encoded commands or unusual parameters
- Direct access to scripts without expected referrer context

These indicators were subtle in isolation but became significant when observed repeatedly or across multiple sessions.

---

### Log, File System, and Host-Level Correlation

Detection of web shells extended beyond application logs. Correlating web activity with host-level telemetry provided stronger confirmation of compromise.

Analysis incorporated:
- Web server logs to identify abnormal request patterns
- File system changes indicating unauthorized script creation or modification
- Network activity initiated by web server processes
- Host-level audit logs revealing unexpected command execution

Using multiple telemetry sources reduced false positives and strengthened confidence during investigations.

---

### Application-Layer DoS and DDoS Patterns

Denial-of-service attacks at the application layer were examined to understand how attackers exhaust resources without relying on high traffic volume. These attacks often appear protocol-compliant, making them difficult to detect through volume-based thresholds alone.

Observed attack patterns included:
- Slow request techniques that tie up server connections
- High-rate HTTP request floods targeting application endpoints
- Cache bypass attempts forcing repeated origin server processing
- Oversized query payloads designed to increase parsing and memory usage
- Login and form abuse targeting authentication logic
- Input validation abuse exploiting inefficient application handling

Detection relied on monitoring request behavior, processing cost, and response patterns rather than raw request counts.

---

### Consolidated Observation

Web security monitoring demonstrated that effective detection at the application layer requires correlating request metadata, behavioral patterns, and host-level evidence. Malicious activity is rarely exposed through a single indicator; instead, confidence emerges through repeated inconsistencies across multiple layers of visibility.
