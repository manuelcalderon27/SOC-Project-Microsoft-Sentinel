🛡️ SOC Lab – Microsoft Sentinel (SIEM)

📌 Overview

This project simulates a Security Operations Center (SOC) environment using Microsoft Sentinel in Azure.
The goal is to collect, analyze, and visualize security events generated from a Windows virtual machine exposed to the Internet, simulating real-world attack activity.

The project demonstrates hands-on experience with:

SIEM implementation

Log collection

Threat detection

KQL querying

SOC workflows

🏗️ Architecture

Flow:

Internet (Attackers)
        ↓
Azure Windows VM
        ↓
Log Analytics Workspace
        ↓
Microsoft Sentinel
        ↓
Workbooks & Incidents


Components:

Azure Virtual Machine (Windows)

Log Analytics Workspace

Microsoft Sentinel (SIEM)

Network Security Group (NSG)

Workbooks (dashboards)

🎯 Objectives

Simulate malicious login attempts (brute-force activity)

Collect Windows Security Event Logs

Detect suspicious authentication behavior

Visualize attacker activity using maps and dashboards

Practice SOC analysis and incident handling

🔍 Data Sources

Windows Security Event Logs

Event ID 4625 (failed logon attempts)

Event ID 4624 (successful logons)

Event ID 4688 (process creation)

Logs are ingested into Log Analytics via Azure Monitor Agent (AMA).

🧪 Attack Simulation

The virtual machine is intentionally exposed to the Internet (controlled environment)

External login attempts generate failed authentication events (Event ID 4625)

These events are used as detection data for analysis

📊 Detection & Analysis
Example KQL Query
SecurityEvent
| where EventID == 4625
| summarize FailedLogons = count() by IpAddress
| order by FailedLogons desc


Used to identify:

Brute-force attempts

Repeated login failures

High-risk source IPs

🌍 Visualization

Workbooks built in Microsoft Sentinel

Visualized attacker activity by:

IP address

Country/Region

Time of activity

Included geolocation maps of attacker IPs

🚨 Incident Handling

Alerts and incidents created based on failed login thresholds

Performed:

Alert triage

Basic investigation

Event correlation

Documentation

🛠️ Tools & Technologies

Microsoft Sentinel (SIEM)

Azure Log Analytics

Kusto Query Language (KQL)

Windows Event Logs

Azure Virtual Machines

Azure Monitor Agent

Network Security Groups (NSG)

MITRE ATT&CK (conceptual mapping)

🧠 Skills Demonstrated

SIEM deployment and configuration (Microsoft Sentinel)

Log ingestion, correlation, and enrichment

KQL query development (including ipv4_lookup and watchlists)

Threat detection (brute-force attacks)

Security dashboards and geographic visualizations

SOC workflows (monitoring, investigation, documentation)


Architecture diagram

<img width="661" height="1141" alt="Diagrama sin título drawio" src="https://github.com/user-attachments/assets/f2957c11-d278-439f-9bb7-44856207b1a2" />


Sentinel dashboard

KQL queries

Failed login events
<img width="958" height="376" alt="image" src="https://github.com/user-attachments/assets/cceb7bc3-16eb-49f0-8d76-e662321ef7f3" />

<img width="416" height="348" alt="image" src="https://github.com/user-attachments/assets/0bdd6e3f-5c21-4b96-bd7c-15b6c2d3e69f" />


Failed login events are enriched using a custom GeoIP watchlist and the ipv4_lookup function to correlate attacker IPs with geographical data (country and coordinates), enabling map-based visualization and attack source analysis.

<img width="899" height="386" alt="image" src="https://github.com/user-attachments/assets/97f2f0a5-6450-4afd-8bbe-ad6a68a33601" />


Map visualization

<img width="554" height="384" alt="image" src="https://github.com/user-attachments/assets/3487268a-a2de-4180-9ef2-35f8d2372872" />

📈 Learning Outcomes

Understanding of SOC end-to-end flow

Practical experience with Microsoft Sentinel

Improved log analysis and detection logic

Exposure to cloud-based security monitoring

⚠️ Disclaimer

This project was created for educational and learning purposes in a controlled lab environment.
No production systems or real user data were used.

👤 Author

Manuel Calderón
Entry-Level SOC Analyst
https://www.linkedin.com/in/manuel-calderon-restrepo-abb201231/
https://github.com/manuelcalderon27
