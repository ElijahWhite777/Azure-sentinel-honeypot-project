# Azure-sentinel-honeypot-project
Azure Cloud Honeypot – Advanced Threat Detection & Brute-Force Analysis



This project demonstrates the end-to-end deployment, configuration, and analysis of a cloud-based honeypot environment within Microsoft Azure. The primary objective was to intentionally expose a vulnerable endpoint to the internet, capture real-world adversarial activity, forward high-value telemetry into a SIEM (Sentinel), and enrich threat data for investigative and attribution purposes.

1. <h2> Cloud Architecture & Environment Design

I designed a segmented Azure environment that isolates the honeypot while maintaining full observability and operational control from the SOC perspective. Key components include:

Resource Group: Centralized governance for all honeypot assets.

Virtual Network (VNet): Dedicated subnet isolating the honeypot VM.

Windows 10/11 VM: Configured as the primary attack surface.

Network Interface (NIC): Attached to VM, fully monitored.

Network Security Group (NSG): Defines inbound/outbound rules to control exposure.

Log Analytics Workspace (LAW): Collects Windows security telemetry.

Azure Monitor Agent (AMA): Installed on the VM to forward event logs.

Data Collection Rule (DCR): Configured to forward all Windows Security Events (4624, 4625, 4672, etc.) to LAW.

Azure Sentinel: Integrated for centralized detection, threat hunting, and alerting.

built-in Sentinel analytics rule is enabled and actively monitors incoming logs from the honeypot VM

📌 ![Azure Architecturedrawio](https://github.com/user-attachments/assets/9f515a10-5070-40de-9ca7-d40d5aac99b4)


2. <h2> Exposure & Attack Surface Configuration

To simulate a realistic attack vector and assess brute-force activity:
This setup deliberately exposes a high-value attack surface while ensuring logs capture all relevant telemetry for SOC analysis.

Network Security Group (NSG): Inbound RDP (TCP/3389) rule set to allow public access.

📌 <img width="1920" height="961" alt="NSG azure photo" src="https://github.com/user-attachments/assets/9681bd0f-5772-4b68-be80-d482b605b399" /> 

Fire wall on VM disabled


<img width="1070" height="890" alt="firewal on vm turned off" src="https://github.com/user-attachments/assets/8150bdad-c8b0-41fe-801f-800d44420fd5" />



3. <h2> SIEM Integration & Log Forwarding

Centralized detection was achieved by forwarding all Windows security telemetry into Azure Sentinel:

Azure Monitor Agent (AMA): Deployed on the VM; validated for heartbeat and event ingestion.

Data Collection Rule (DCR): Configured to collect all Windows Security Events and filter key authentication attempts for brute-force monitoring from VM to Microsoft Sentinel.

Sentinel Integration: Workspace onboarded into Azure Sentinel; advanced hunting queries enabled to analyze potential intrusions.


<img width="1920" height="966" alt="DCR = all windows security events" src="https://github.com/user-attachments/assets/52df3aed-eaf1-4c61-ae9d-7e585ec013e6" />



📌 <img width="1920" height="1008" alt="Sentinel Logs Pane (KQL Environment loaded" src="https://github.com/user-attachments/assets/2a721d24-005f-4d9a-8bae-3edf8ee69aec" />



📌 <img width="1920" height="963" alt="Sentinel Analytics Rule Enabled bonus" src="https://github.com/user-attachments/assets/4845c923-d463-4cc9-8c3e-ab26b9ddaa16" />

This built-in Sentinel analytics rule is enabled and actively monitors incoming logs from the honeypot VM. It detects high-severity, multi-stage attacks and maps to multiple MITRE ATTACK tactics, demonstrating end-to-end threat detection capability in my SOC-level cloud environment.




4. <h2> Threat Hunting & Real Attack Analysis

Within hours of exposing the VM, automated attack activity was detected:

Key Findings:

6 brute-force login attempts targeting exposed RDP port (3389).

Originating IPs traced to foreign sources.

Authentication failures confirmed no successful compromise.

📌 <img width="1920" height="968" alt="Azure brutforce attack Log analytics" src="https://github.com/user-attachments/assets/ceab8eaa-6f4b-424b-b7d2-e0f639caebdf" />



5. <h2> Threat Intelligence Enrichment

To enhance situational awareness and incident context:

Imported a GeoIP Threat Intelligence Watchlist into Sentinel.

Correlated attacker IPs with geographic data.

Visualized attacker locations and linked IPs to known brute-force scanning campaigns.

Geolocation identified Germany as the origin of attacks.

📌 <img width="1888" height="921" alt="hacker attack geo map" src="https://github.com/user-attachments/assets/e84bca93-a297-45cb-923c-7a876cdabec1" />

Watchlist integration within LAW


<img width="1920" height="1008" alt="watch list geo ip integrated with log through query _Getwatchlist" src="https://github.com/user-attachments/assets/f25ec18b-f25c-40ed-b556-8fc62490e782" />


6. <h2> Outcomes & Analyst Value

Outcomes

Deployed a fully functional Azure honeypot capturing live attacker activity in a cloud environment.

Created and leveraged watchlists (malicious IPs, suspicious accounts) to enhance threat detection and alerting.

Integrated honeypot telemetry into Microsoft Sentinel, validating end-to-end SIEM workflow: ingestion → enrichment → detection → alerting.

Developed actionable KQL queries and analytics rules that automatically detect and flag suspicious activity.

Generated actionable threat intelligence, including geolocation, attacker techniques, and attack patterns, for SOC analysis.


Analyst Value

Real-time threat detection: Enables SOC analysts to identify attacks as they happen.

Enhanced alert accuracy: Watchlists provide contextual intelligence, reducing false positives.

Efficient investigation: Centralized logs and queries allow analysts to quickly pivot from alerts to root cause.

Prioritization of high-severity threats: Helps SOC teams focus resources effectively.



