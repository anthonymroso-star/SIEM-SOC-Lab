# Cybersecurity Incident Report
**Incident Reference:** INC-2026-0709A  
**Date / Time:** 2026-07-09 03:15:00 BST  
**Severity Level:**  High  
**Target Asset:** `**server` (Ubuntu Host OS)  
**Classification:** Brute Force / Credential Stuffing  

---

## Part 1: NIST CSF 2.0 Mapping
The incident lifecycle and infrastructure architecture align directly with the **NIST Cybersecurity Framework (CSF) 2.0** functions:

| NIST CSF Function | Activity |
| :--- | :--- |
| **Identify (ID)** | Mapping `**server` as a critical asset and tracking its baseline architecture limitations. |
| **Protect (PR)** | Moving Docker data to the `/mnt/**_1storageA` RAID 1 array to protect the OS partition from log flooding. |
| **Detect (DE)** | Wazuh SIEM firing alerts (Rule 5710/5712) by harvesting data from `/var/log/auth.log`. |
| **Respond (RS)** | Executing forensic analysis on the dashboard to extract the attacker's IP and target usernames. |
| **Recover (RC)** | Hardening the SSH configuration profile and validating system availability after the attack. |

---

## Part 2: Incident Response Phases

### Phase 1: Detection
* **Indicator of Compromise (IoC):** A massive vertical spike of hits appeared on the `wazuh-alerts-*` timeline within a tight 10-second window.
* **Telemetry Source:** The local Wazuh agent on `**server` captured authentication failures within `/var/log/auth.log` and shipped them securely over TLS.
* **SIEM Signatures Triggered:**
  * `Rule ID 5710`: sshd: Attempt to login using a non-existent user
  * `Rule ID 5712`: SSHD brute force trying to get access (Triggered as Hydra's rapid multi-threading crossed the frequency threshold).

### Phase 2: Analysis
Expanding the JSON alert data structure on the Wazuh Discover screen revealed the following forensic facts:
* **Attacker IP (`data.srcip`):** `x.x.x.x` *(Sanitised)*
* **Target Account (`data.dstuser`):** `fakeuser3`
* **Tactics, Techniques, and Procedures (TTPs):** Automated dictionary attack via port 22 (SSH) utilising a multi-threaded tool (Hydra) guessing users against the `rockyou.txt` wordlist.

### Phase 3: Containment
To isolate the threat and stop the automated tool from consuming critical server resources, the following containment strategies were implemented:

* **Short-Term Network Isolation:** Applied a temporary local firewall block on the Ubuntu server using Uncomplicated Firewall (UFW) to drop all traffic from the malicious source IP:
  ```bash
  sudo ufw deny from x.x.x.x to any port 22
  ```
* **SIEM Active Response (Automated Mitigation):** For future mitigation, the Wazuh `active-response` module is being configured. This will automatically execute a host-deny or firewall script to drop the source IP the exact instant `Rule 5712` is triggered.

### Phase 4: Eradication & Recovery
Because the attacker only generated failed logins (`Rule 5710`), no system breach occurred. No malware eradication or account password resets were required.
* **System Health Check:** Monitored the storage pool at `/mnt/**_1storageA` to verify that the high volume of logs generated did not exhaust disk space or cause an OS denial of service.
* **Service Validation:** Verified that legitimate user shell connections remained responsive, stable, and unimpacted.

### Phase 5: Post-Incident Activity & System Hardening
To ensure this specific scenario cannot be repeated, the security team recommends the following three system-hardening actions using Security Configuration Assessment (SCA) guidelines:

1. **Disable Root and Password Authentication:** Force the server to strictly require SSH Cryptographic Keys instead of weak text passwords.
2. **Change the Default Port:** Shift SSH off public port 22 to a non-standard high port (e.g., `2222`) to eliminate 99% of automated internet scan scripts.
3. **Install System Rate-Limiter:** Implement a local system rate-limiter (such as `fail2ban`) to block any IP address that fails 3 login attempts for 24 hours.
