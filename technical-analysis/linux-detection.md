# Linux Threat Detection & Security Monitoring

## Overview
This documentation outlines the methodology for detecting malicious activity on Linux systems by leveraging the Linux Auditing System (`auditd`) and analyzing system-level logs. It covers common attacker techniques for bypassing standard logging and how to implement robust monitoring for persistence.

---

## 1. Limitations of Standard Logging
Standard Bash history is a common target for attackers attempting to hide their tracks.

* **Leading Spaces**: Attackers can prevent a command from being recorded in `.bash_history` by simply adding a space before the command (e.g., `  whoami`).
* **Script Obfuscation**: Pasting multiple commands into a script hides the individual actions from the user's history file.
* **Alternative Shells**: Switching to shells like `/bin/sh` can bypass history tracking entirely if not properly configured.

---

## 2. Advanced Auditing with Auditd
To overcome the limitations of Bash history, the Linux Audit Framework (`auditd`) is used to track system calls in real-time.

### Core Audit Rules (`/etc/audit/rules.d/audit.rules`)
| Monitoring Goal | Audit Rule Example | Key Identifier |
| :--- | :--- | :--- |
| **Process Execution** | `-a always,exit -S execve -F exe=/usr/bin/wget` | `proc_wget` |
| **File Access** | `-a always,exit -S openat -F path=/etc/shadow` | `shadow_access` |
| **Network Connections** | `-a always,exit -S connect -F exe=/usr/bin/python3` | `net_python` |
| **Configuration Changes** | `-w /etc/ssh/sshd_config -p wa` | `ssh_config` |

### Investigation Workflow
When a rule is triggered, the `ausearch` utility is used to parse the logs for better readability.
* **Command**: `ausearch -i -k proc_wget`
* **Output Analysis**: Focus on `type=SYSCALL` to see the command-line arguments and `type=CWD` to identify the directory where the tool was executed.

---

## 3. Detecting Persistence Mechanisms
Persistence allows an attacker to maintain access through system reboots.

### A. Cron Job Persistence
Attackers often add malicious entries to the crontab to execute payloads at specific intervals.
* **Detection Goal**: Monitor changes in `/etc/crontab`, `/etc/cron.d/*`, and `/var/spool/cron/*`.
* **Audit Command**: `ausearch -i -x crontab` to look for execution of the crontab editing command.

### B. Systemd Service Persistence
Creating a fake system service is a common tactic for high-level persistence.
* **Detection Goal**: Monitor for file creations in `/lib/systemd/system/` and `/etc/systemd/system/`.
* **Suspicious Pattern**: Look for services with unusual descriptions or those running binaries from hidden directories (e.g., `~/.hidden-directory/`).

---

## 4. Post-Exploitation Discovery Commands
If a breach is suspected, the following discovery commands are often run by attackers and should be monitored:

* **User Discovery**: `id`, `whoami`, `cat /etc/passwd`.
* **Network Discovery**: `ip a`, `netstat -tnlp`, `ss -tnlp`.
* **System Discovery**: `uname -a`, `hostname`, `lsmod`.

---

## 5. Summary of Defensive Controls
1. **Enable Granular Auditing**: Deploy `auditd` rules that target high-risk syscalls (`execve`, `connect`, `openat`).
2. **Centralize Logs**: Forward audit logs to a SIEM for correlation with network-level events.
3. **Monitor Persistence**: Implement file integrity monitoring (FIM) on sensitive system directories like `/etc/systemd/` and `/var/spool/cron/`.