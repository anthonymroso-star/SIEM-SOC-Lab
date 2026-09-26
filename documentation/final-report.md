# INCIDENT FINAL REPORT  
**Reference:** INC-2026-0709A  
**Classification:** Bruteforce / Credential Stuffing  
**Target:** **Server (Ubuntu Host OS)  
  
### Executive summary  
The lab experienced a security incident on 9 July 2026, at 3:15 a.m. BST, during which an external asset attempted to gain unauthorised access via automated password guessing. No customer data or personally  identifiable information (PII) was exposed or compromised. The financial impact of the incident is £0, as the attack was safely contained within a controlled environment. The incident is now closed and a thorough forensic database investigation has been completed.  
  
### Timeline  
* **03:13 a.m. BST** — Attacker initiated a reconnaissance connection to test network responsiveness on port 22. Initial telemetry captured brief connection drops due to script timeouts.  
* **03:15 a.m. BST** — Attacker deployed a high-speed, automated dictionary attack tool. The host immediately began logging a high volume of failed authentication attempts.  
* **03:16 a.m. BST** — The security analyst reviewed the real-time SIEM dashboard, concentrating on isolating the malicious source IP address and identifying the targeted system usernames to confirm containment.  
  
### Investigation  
The security analyst received the authentication alerts and utilised the central console to investigate the live event.  
  
The root cause of the incident was identified as an exposed SSH utility service running on standard port 22 with password authentication enabled. This configuration allowed the attacker to run a brute-force utility (Hydra) using known wordlists. The tool automatically flooded the host with rapid credential-stuffing attempts against non-existent accounts.  
  
After confirming the automated volume, the team analysed the log data. The logs indicated that the attacker generated a massive vertical spike of hits before dropping the network connection.  
  
### Response and remediation  
The organization successfully intercepted the attack traffic through the local security pipeline. As the automated tool only generated failed login events, no system breach or credential compromise occurred on the asset.  
  
After the analyst reviewed the associated alerts, the scope was clear. There was a single log source showing an exceptionally high volume of sequential invalid user queries originating from the specific attacker source IP x.x.x.x  
  
### Recommendations  
To prevent future recurrences, we are taking the following actions:  
* Disable standard password authentication and enforce cryptographic SSH key pairs.  
* Implement rate-limiting tools to automatically block source IPs after three consecutive failures.  
* Shift the SSH service from default port 22 to a non-standard high port to evade automated public scan tools.
