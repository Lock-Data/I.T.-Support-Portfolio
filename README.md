# SaaS Support & IT Engineering Portfolio
**Lachlan Matthew** | *CompTIA A+ Certified* |  *Data Analytics Professional Certificate:*  | *Network+ (In Progress)*


Welcome to my technical portfolio. This repository explicitly demonstrates my analytical troubleshooting workflows, systems documentation, and lifecycle management capabilities across two major technical tracks:


1. **📁 01_SaaS_Application_Support:** Core application troubleshooting, API data latency triage, multi-tenant incident response, and customer-facing Knowledge Base generation.
2. **📁 02_MSP_Infrastructure_Support:** Managed Service Provider frameworks covering Active Directory management, local area networking (DHCP/DNS resolution), and VoIP system optimization.


# Technical Triage Philosophy

When a client’s business-critical infrastructure, network, or core cloud service experiences an incident, I follow a disciplined 3-step operational workflow to minimize downtime and drive swift resolution.

---

## 1. Scope and Isolate the Impact
I systematically determine the fault boundaries to separate localized issues from systemic infrastructure failure.
* **Endpoint vs. Environment:** Check if the issue is restricted to a single local workstation profile or workstation hardware.
* **Identity & Access Layer:** Isolate authentication blocks within **Active Directory** or **Office 365** licensing.
* **Network & Connectivity:** Verify if a drop is local to a single endpoint, a localized **DHCP/DNS** assignment failure, or a systemic site-wide routing/switch failure.

## 2. Protect Business Continuity
My immediate priority is maintaining client productivity. While conducting root-cause analysis, I pivot to establish immediate, functional workarounds:
* **Cloud & Collaboration Failovers:** Rerouting users to browser-based web apps if local Outlook or **SharePoint** sync engines fail.
* **Network & VoIP Resilience:** Failing over to alternative network pathways or provisioning softphone/mobile routing solutions for **IP PBX/VoIP** system drops.
* **Workstation Contingencies:** Deploying secondary access routes or remote support sessions to keep client-facing operations functional.

## 3. Analyze, Document, and Escalate
If a Tier 1 resolution isn't immediately achievable, I gather high-fidelity technical telemetry to ensure a seamless escalation to Tier 2 or Senior Systems Engineers:
* **Log Gathering:** Collect local Event Viewer logs, network packet traces, or cloud tenant health diagnostics.
* **Reproducible Documentation:** Clearly detail environmental baselines, symptoms, and the precise troubleshooting actions already performed.
* **High-Fidelity Tickets:** Deliver cleanly structured escalation tickets to ensure Tier 2 can pick up exactly where I left off without data gaps.
