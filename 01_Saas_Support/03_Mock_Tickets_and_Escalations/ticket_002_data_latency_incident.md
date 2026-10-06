# Mock Incident Log: Ticket #002 - Webhook Latency & Local App Desync

This simulation showcases how I combine data log evaluation with advanced infrastructure diagnostics to solve complex data-delivery delays between the cloud layer and client-side web applications.

## Case Details
* **Priority:** P2 (Medium-High Impact - Core operation degraded but functional)
* **Client:** Apex Distribution QLD (Brisbane Regional Hub)
* **Symptom:** Live operational dashboards are experiencing extreme rendering lag. Task completions and state changes are taking up to 45 seconds to display on the local monitoring layout.

---

## The Conversation Log

### Customer Initial Intake
> **From:** Sarah Jenkins (Operations Lead, Apex Distribution)
> **Message:** "Our warehouse floor monitor dashboard is completely lagging out. When a picker updates a task status, it doesn't show up on the main office feed for nearly a minute. We've updated our browser and restarted our local machines but it's killing our shift productivity. Is your system struggling today?"

### Support Response (Isolating Network vs. Cloud State)
> **From:** SaaS Support Engineering (Tier 1)
> **Message:** "Hi Sarah, thank you for flag down these timing details with us. I completely understand how a 45-second delay disrupts your warehouse floor flow when tracking active distribution states. 
> 
> Our main cloud services are operating normally, which suggests a delivery or event-queue breakdown between our servers and your local layout. To help me isolate this immediately, could you let me know if this delay occurs on machines connected to your main warehouse Wi-Fi network, or if it also impacts devices connected via separate mobile broadband networks?"

### Customer Update
> **From:** Sarah Jenkins
> **Message:** "We tested it on a 5G mobile hotspot tablet and the sync is almost instant! But all our main desk workstations on the local building network are still heavily lagging."

---

## Technical Root Cause Isolation & Data Analysis

1. **Rule Out System Outage:** Internal cloud metrics and replication queues are running healthy. 
2. **Network Perimeter Triage:** The client's hotspot test confirmed our application layer is functioning smoothly when hitting external commercial networks. This isolates the problem down to the client’s localized business network path or internal traffic inspector rules.
3. **Log Analysis & Latency Metrics:** Analyzed the incoming payload event history for their Organization ID. Identified that webhooks are successfully dispatching from our environment in `< 120ms`. However, client connection streams (`EventSource` / WebSockets) show massive dropouts and retry spikes.
4. **Root Cause:** A deep traffic inspection or proxy rule on the client's internal local firewall is throttling long-polling data connections, causing event streams to back up and queue heavily before rendering.

### Business Continuity Workaround Provided
While Sarah’s local IT infrastructure manager investigated their corporate firewall parameters, I provided an immediate configuration shift within our app settings to bypass the queue delay:

> **From:** SaaS Support Engineering (Tier 1)
> **Message:** "Hi Sarah, thank you for performing that quick test! Because the mobile network updates instantly, we have isolated the bottleneck to an internal security or data inspection policy on your warehouse corporate firewall.
> 
> To get your team up and running normally right away while your local IT lead adjusts those network rules, I have adjusted an internal configuration flag for your account profile on our end. This temporarily switches your live board engine from a continuous WebSocket event stream over to an aggressive polling sequence every 5 seconds. This will bypass the blocked firewall queue completely and keep your warehouse updates accurate within a brief 5-second window."

---

## High-Fidelity Escalation Ticket (Internal Network Context)
*The following highly descriptive diagnostic ticket was logged to keep a history of the client-side infrastructure limitation:*

```text
SUBJECT: P2 Incident Resolution Note - Client-Side Network Firewall Inspection - Apex Distribution
ORGANIZATION ID: org_apex_44012
RESOLUTION CODE: App Configuration Override (Stream to Poll Fallback)

TECHNICAL ANOMALY SUMMARY:
Client experienced severe dashboard rendering delays (~45s) restricted to their internal building ISP path. Cloud-layer webhook execution history verified normal backend transaction cycles (<120ms execution times).

DIAGNOSTIC BLOCK ANALYSIS:
WebSocket connection logs captured extensive connection drop patterns:
"WebSocket connection to 'wss://://platform.com' failed: WebSocket opening handshake timed out"
Local firewall was dropping packets during active deep-packet inspection (DPI) routines, preventing the web app from maintaining persistent event handshakes.

REMEDIAL ACTION LOGGED:
1. Switched Org ID org_apex_44012 to fallback 5-second HTTP polling interface via admin console.
2. Verified dashboard synchronization stabilized to an acceptable <5s delta across all local building endpoints.
3. Delivered exact port configuration requirements (Allow outbound port 443 TCP persistent connections) to client's internal network engineer to achieve a permanent resolution.
```
