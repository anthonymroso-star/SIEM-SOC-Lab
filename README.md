# Enterprise SIEM Deployment & Incident Response Lab  
A practical workshop focused on setting up a professional-grade Security Operations Center (SOC) system using Wazuh software to track, detect, and record simulated real-world cyberattacks.
  
> **Compliance & Data Sanitization Note:** Where applicable, files, scripts, logs, IP addresses, hostnames, and architecture identities within this repository have been sanitized and anonymized in alignment with relevant security frameworks and responsible-disclosure guidelines.  
## Lab Architecture Overview  
* **Host Endpoint:** Ubuntu Host OS (Simulating a localized corporate production server).  
* **SIEM Central Engine:** Wazuh Indexer & Manager v4.7.5 deployed via Docker Compose.  
* **Storage Architecture:** Dedicated RAID 1 scratch drive array (`/mnt/**_storage`) isolation for volatile data logging.  
* **Attacker Asset:** Kali Linux (Utilizing Hydra for multi-threaded dictionary attacks).  
  
## Key Skills Demonstrated  

| Core Capability | Technical Implementation & Approach |
| :--- | :--- |
| **SIEM Engineering** | Multi-node Docker architecture orchestration, custom volume pathing, and container timezone synchronization. |
| **Endpoint Hardening** | XML configuration modification, operating system definitions and system metrics auditing. |
| **DFIR** | Digital forensics, Log analysis, Search queries, signature analysis, and NIST CSF 2.0 documentation. |
| **SIEM & Telemetry** | Tracked SIEM authentication rule triggers generated directly from system logs. |
| **Framework Alignment** | Mapped active containment and recovery actions directly to NIST CSF 2.0 core functions. |
| **Incident Response** | Analyzed raw alert JSON data structures to isolate attacker IPs and targeted usernames. |
| **Threat Mitigation** | Executed manual UFW firewall blocks to immediately drop malicious source network traffic. |
| **Infrastructure Awareness** | Protected OS disk space by isolating Docker log directories on local RAID storage. |
| **Technical Communication** | Structured technical forensic findings into professional executive incident reports. |

  
##  Repository Contents  
* `/documentation`: Holds official incident reports mapped to global security compliance models.  
* `/config`: Snippets of hardened agent config profiles (`ossec.conf`).  
* `/artifacts`: Forensic JSON logs captured during live exploit execution.  
  
## Featured Project Walkthroughs  
1. **[Incident Response: (NIST SP 800-61 r2) Detection & Forensic Triaging](./documentation/INC-2026-0709A.md)**  
2. **[SIEM Alert Analysis SSH Brute-Force Attack NIST-CSF-2.0](./documentation/NIST-core_INC-Resp.md)**  
3. **[Incident Final Report: INC-2026-0709A Bruteforce / Credential Stuffing](./documentation/final-report.md)**  
  
## Active Milestones  
  
  
* [Security Configuration Assessment (SCA) Hardening Walkthrough](documentation/sca-hardening.md)
