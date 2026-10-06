# Mock Incident Log: Ticket #003 - CSV Ingestion Failure & Structural Data Desync

This case profile documents my capability to apply data validation practices to spot database write errors, resolve complex structural file bugs, and clearly guide users through system file recoveries.

## Case Details
* **Priority:** P1 (High Impact - Bulk operational failure)
* **Client:** National Fleet Logistics (Brisbane Corporate Office)
* **Symptom:** A batch update file containing 1,500 new staff entries crashed midway. Active records have loaded into the UI with corrupted properties, rendering their team notification system non-functional.

---

## The Conversation Log

### Customer Initial Intake
> **From:** David Vance (HR Director, National Fleet Logistics)
> **Message:** "I just used our system tool to upload our new 1,500 staff accounts using our standard internal file layout, and the process threw an error message halfway through. Now, half our roster records look corrupted. Phone values are missing digits, and notifications are failing across our entire Brisbane center. We need this fixed now."

### Support Response (Calming and Scoping)
> **From:** SaaS Support Engineering (Tier 1)
> **Message:** "Hi David, thank you for sharing those details with me. I recognize how critical it is to have your 1,500 staff records matching accurately, especially when it halts team updates across your Brisbane operation. Let's get this sorted out for you.
> 
> To isolate where the sync dropped, could you reply directly with a copy of the exact CSV import document you utilized for this batch step? I will analyze the layout mechanics right away."

---

## Technical Root Cause Isolation & Data Profiling

1. **File Inspection:** Analyzed David's attached file using basic data verification practices. Identified that the mobile contacts column was formatted as a raw integer data value rather than a text string. As a result, the spreadsheet engine automatically dropped the leading `0` from every single entry (e.g., `0475305573` became `475305573`).
2. **Log Verification:** Checked internal server telemetry fields. Located a string of database script exception warnings thrown during the import window:
   `Database Exception: Validation failed for field 'sms_payload_phone': String length must be exactly 10 digits for AU regional codes. Received length: 9.`
3. **Root Cause:** The application backend properly rejected the rows because a 9-digit sequence violated the regional safety schema. The upload system failed to handle the validation cleanly, creating an inconsistent data desync loop across the user dashboard.

### Resolution & Sanitation Steps Conducted
I cleanly sanitized the client's file source layout to completely re-establish service operations:

> **From:** SaaS Support Engineering (Tier 1)
> **Message:** "Hi David, I have successfully identified the structural issue. When saving your internal data file, the application converted your contact column to a standard number row, which automatically stripped the leading zeros off your staff phone strings. When our platform system read the 9-digit lines, it halted the process to protect your data rules.
> 
> I have completely cleaned and repaired your source file. I converted the column formatting strictly to a Text type rule, and restored the leading `0` padding to all 1,500 staff entries. 
> 
> Please navigate to *Admin Panel > Import Management*, click 'Clear Pending Batch Log' to remove the corrupted 9-digit rows, and upload this corrected file I have attached. This will populate your roster fields cleanly and restore your team notification channels immediately."

---

## High-Fidelity Tier 2 / Development Escalation Ticket
*The following bug containment log was issued to engineering to introduce better user file validation safety filters:*

```text
SUBJECT: Bug Report: Batch Import Engine Ingestion Validation Bypass - Data Desync Fault
ENVIRONMENT: Production App Ingestion Engine
REPLICABLE SOURCE: System automatically processes numeric integer columns, causing character truncation.

DEFECT DESCRIPTION:
The bulk import mechanism does not actively force string-casting validations on incoming telephone fields prior to database row creation. If a user uploads an unpadded integer string, the leading zero drops, triggering an `HTTP 500 / Validation Failure` mid-transaction on the schema constraint layer. This creates a partial ingestion state and table desync.

REQUESTED REMEDIAL ENHANCEMENT:
Introduce an upfront regex validator rule (`/^0[0-9]{9}$/`) inside the frontend import module to intercept files, check lengths, and alert users to use explicit string padding before database writes execute.
```
