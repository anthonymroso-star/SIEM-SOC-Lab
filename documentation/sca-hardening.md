#  Security Configuration Assessment (SCA) Hardening Walkthrough

### 1. Vulnerability Identification
During an automated compliance audit using the Wazuh SCA engine, the server failed **CIS Benchmark Control 28638: Ensure SSH root login is disabled**. Leaving this active allowed external brute-force vectors directly against the administrative root account.

* **Initial State:** `Failed (Red)`
* **Audit Detection Command:** `sshd -T`

### 2. Remediation Implementation
To secure the asset, administrative access rules were updated by modifying the master SSH daemon configuration file on the target host:

```bash
sudo nano /etc/ssh/sshd_config
```

The configuration directive was updated to explicitly deny root authentication sessions across the network interface while preserving standard user access alignment:

```text
PermitRootLogin no
```

The authentication daemon configuration tables were then safely reloaded in production without dropping active administrator session states:

```bash
sudo systemctl reload sshd
sudo systemctl restart wazuh-agent
```

### 3. Verification & Telemetry Confirmation
* **Dashboard Verification:** The Wazuh Manager received updated node telemetry packets during the subsequent 12-second assessment scan, successfully flipping **Control 28638** to `Passed (Green)`.
* **Manual Verification:** Network connection tests targeting `root@x.x.x.x` were explicitly executed, confirming proper server-side rejection logs:
  ```text
  root@x.x.x.x: Permission denied (publickey,password).
  ```
