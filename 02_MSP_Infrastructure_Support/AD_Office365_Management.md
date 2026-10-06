# Incident Playbook: Enterprise Account Lockouts & User Provisioning

### 1. Persistent Active Directory Lockouts
* **Scenario:** A user is locked out of their system repeatedly every 15 minutes after a required domain password rotation.
* **Triage Pipeline:**
  1. **Identify:** Utilize AD Event Viewer (Event ID 4740) on the Domain Controller to identify the source caller machine name.
  2. **Isolate:** Inspect the user's secondary corporate mobility devices (iPad/iPhone) or saved credential managers (Windows Credential Manager).
  3. **Remediation:** Remove expired cached passwords causing automated authentication failures. Clear bad password count in AD UC, verify replication across domain controllers, and unlock the account.

### 2. User Offboarding Standard Operating Procedure (SOP)
* **Workflow:** Automated ticket received for an urgent employee termination.
  1. Reset the user's Active Directory and Microsoft 365 passwords immediately to revoke active sessions.
  2. Convert the mailbox to a Shared Mailbox to preserve data without consuming an active M365 license.
  3. Delegate Full Access and Send As permissions to the departing employee's direct manager.
  4. Move the Active Directory object to the "Disabled Users" Organizational Unit (OU) and strip all security group memberships.
