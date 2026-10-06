# Technical Documentation: Tier 1 LAN & Core Services Resolution

### 1. Localized DHCP & APIPA Failures
* **Symptom:** Workstation displays "No Internet Connection" and pulls an IP address of `169.254.4.12`.
* **Root Cause Identification:** The workstation failed to reach the local DHCP server and self-assigned an Automatic Private IP Address (APIPA).
* **Resolution Workflow:**
  * Run `ipconfig /release` to drop the existing invalid lease.
  * Run `ipconfig /renew` to force negotiation with the DHCP server.
* *If renewal fails:* Verify physical layer connection (switch port link lights), check VLAN assignment on the managed switch, or check the DHCP pool exhaustion status on the security gateway.

### 2. DNS Resolution Anomalies
* **Symptom:** User can ping external public targets (e.g., `ping 8.8.8.8` succeeds) but cannot resolve domain names (e.g., `ping google.com` fails).
* **Remediation:** Flush local DNS resolver cache and register records via Command Prompt:
  * Run `ipconfig /flushdns`
  * Run `ipconfig /registerdns`
* Cross-check static DNS server assignments on the client's network interface adapter properties. Verify external forwarders are operational on the local DNS server.
