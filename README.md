# 🛡️ SOC Project – Microsoft Sentinel (SIEM) Overview

This project simulates a Security Operations Center (SOC) environment using Microsoft Sentinel in Azure. It focuses on collecting, enriching, detecting, and visualizing malicious authentication activity from Windows Security logs. The project now includes the creation and implementation of a custom analytic rule to detect brute-force login attempts, which generates alerts and incidents for SOC investigation.

## 🎯 The project demonstrates hands-on experience with:

SIEM implementation
Log ingestion and enrichment
Threat detection via analytic rules
KQL development
SOC investigation workflows, including alert triage and incident handling

## 🏗️ Architecture

Internet (Attackers)
Azure Windows VM
Log Analytics Workspace
Microsoft Sentinel (SIEM)
Workbooks & Incidents

## 🗺️ Architecture Diagram
<img width="661" height="1141" alt="image" src="https://github.com/user-attachments/assets/a3db3b87-f90c-4a77-86f6-306413e1c20a" />

## ⚙️ Components:

Azure Virtual Machine (Windows)
Network Security Group (NSG)
Log Analytics Workspace
Microsoft Sentinel
Watchlists (GeoIP)
Sentinel Workbooks
Analytic Rules for threat detection

## 🎯 Objectives

Simulate brute-force login attempts
Collect Windows Security Event Logs
Detect suspicious authentication behavior using custom analytic rules
Enrich logs with geographic context
Visualize attacker activity using dashboards
Generate and investigate alerts and incidents
Practice SOC monitoring and investigation

## 📊 Data Sources

Windows Security Event Logs:
4625 – Failed logon attempts
4624 – Successful logons
4688 – Process creation

Logs are ingested using Azure Monitor Agent (AMA) into Log Analytics.

## 🧪 Attack Simulation

A Windows virtual machine is intentionally exposed to the Internet in a controlled lab environment. External login attempts generate Event ID 4625. These events are used as detection data to trigger analytic rules.

## 🔍 Detection & Analysis
### 📌 Analytic Rule Creation

A custom analytic rule named "Brute Force Login Attempts" was created in Microsoft Sentinel to detect multiple failed login attempts from the same IP address within a short time window.

<img width="958" height="376" alt="Analytic Rule General" src="https://github.com/user-attachments/assets/cceb7bc3-16eb-49f0-8d76-e662321ef7f3" />

MITRE ATT&CK Mapping: Credential Access
Severity: Medium
Status: Enabled

## 🧠 Rule Query (KQL):
SecurityEvent
| where EventID == 4625
| summarize FailedAttempts = count() by IpAddress, Account, bin(TimeGenerated, 5m)
| where FailedAttempts > 5

Rule Frequency: Run query every 5 minutes
Rule Period: Look at last 5 minutes of data
Alert Threshold: Trigger if query returns more than 0 results
Event Grouping: Group all events into a single alert
Suppression: Not configured

<img width="616" height="648" alt="Analytic Rule Entity Mapping" src="https://github.com/user-attachments/assets/0bdd6e3f-5c21-4b96-bd7c-15b6c2d3e69f" />

## 🧩 Entity Mapping:

Entity 1: IP (Identifier: Address, Value: IpAddress)
Entity 2: Account (Identifier: Name, Value: Account)

## 🚨 Incident Settings:

Create incidents: Enabled
Alert Grouping: Enabled (Match all entities)
Re-open closed incidents: Disabled
Grouping Period: Match from the last 5 hours
Incident Correlation: Tenant default
Automated Response: Not configured

This rule uses KQL to identify:
Brute-force attempts
Repeated authentication failures
High-risk source IPs

## 🧾 Example Detection Query (Failed Logons)
SecurityEvent 
| where EventID == 4625 
| summarize FailedLogons = count() by IpAddress 
| order by FailedLogons desc
<img width="900" height="395" alt="Failed Attempts Table" src="https://github.com/user-attachments/assets/9a601043-e1d1-4e6e-9ace-abcb412370c1" />

Example of generated incident:
"Brute Force Login Attempts involving multiple users" with details on affected users, IP addresses, and failed attempt counts.

## 🌍 Log Enrichment (GeoIP)

A custom GeoIP watchlist is used to enrich events with country and coordinates.

let GeoIPdatabase = GetWatchlist('geoip');
SecurityEvent 
| where EventID == 4625 
| evaluate ipv4_lookup(GeoIPdatabase, IpAddress, network) 
| project IpAddress, countryname, latitude, longitude

This allows:
Geographic attribution of attacker IPs
Contextual threat analysis
Map-based visualization

<img width="554" height="384" alt="Enriched GeoIP Query Results" src="https://github.com/user-attachments/assets/3487268a-a2de-4180-9ef2-35f8d2372872" />

## 📈 Visualization

Sentinel Workbooks are used to create:
Failed login tables
Time-based charts
Geographic maps of attacker IPs

Visualizations support SOC monitoring and threat hunting. Additional dashboards show incident status, alert severity, and data ingestion metrics.

<img width="958" height="376" alt="image" src="https://github.com/user-attachments/assets/cceb7bc3-16eb-49f0-8d76-e662321ef7f3" /> <img width="616" height="648" alt="image" src="https://github.com/user-attachments/assets/0bdd6e3f-5c21-4b96-bd7c-15b6c2d3e69f" />

Failed login events are enriched using a custom GeoIP watchlist and the ipv4_lookup function to correlate attacker IPs with geographical data (country and coordinates), enabling map-based visualization and attack source analysis.

<img width="900" height="395" alt="image" src="https://github.com/user-attachments/assets/9a601043-e1d1-4e6e-9ace-abcb412370c1" />
🗺️ Attacker Map Visualization
<img width="554" height="384" alt="image" src="https://github.com/user-attachments/assets/3487268a-a2de-4180-9ef2-35f8d2372872" />

## 🚑 Incident Handling

Alerts and incidents are generated based on the analytic rule detection logic.

Example incident:
ID 96: Brute Force Login Attempts involving multiple users
Status: Active, Unassigned, Unclassified
Severity: Medium
Entities: 5 Users, IP Addresses (e.g., 149.50.xx.xx, 146.19.xx.xx)
Alerts: 116 (Brute Force Login Attempts)
Activities: 234
Graph: Shows user and IP correlations

<img width="958" height="376" alt="Incidents List" src="https://github.com/user-attachments/assets/cceb7bc3-16eb-49f0-8d76-e662321ef7f3" /> <img width="616" height="648" alt="Incident Details" src="https://github.com/user-attachments/assets/0bdd6e3f-5c21-4b96-bd7c-15b6c2d3e69f" />

## 🧠 SOC activities performed:

Alert triage
Event correlation (e.g., querying related events by IP and time)
Basic investigation using incident graph and query results
Documentation of findings

<img width="900" height="395" alt="Related Events" src="https://github.com/user-attachments/assets/9a601043-e1d1-4e6e-9ace-abcb412370c1" /> <img width="554" height="384" alt="Dashboards" src="https://github.com/user-attachments/assets/3487268a-a2de-4180-9ef2-35f8d2372872" />

## 🧰 Tools & Technologies

Microsoft Sentinel (SIEM)
Azure Log Analytics
Kusto Query Language (KQL)
Windows Event Logs
Azure Monitor Agent
Azure Virtual Machines
Network Security Groups
MITRE ATT&CK (conceptual mapping)

## 🏆 Skills Demonstrated

SIEM deployment and configuration
Log ingestion, correlation, and enrichment
Analytic rule creation and tuning
KQL query development
Threat detection (brute-force attacks)
Security dashboards and geographic visualizations
SOC workflows (monitoring, investigation, documentation)

## 🔁 How to Reproduce

Deploy a Windows VM in Azure
Enable Azure Monitor Agent (AMA)
Connect VM to Log Analytics Workspace
Enable Microsoft Sentinel
Upload GeoIP watchlist
Create the "Brute Force Login Attempts" analytic rule as described
Import Workbook JSON
Run KQL detection queries
Simulate or observe failed login events
Monitor generated incidents and alerts

## ⚠️ Disclaimer

This project was developed for educational and learning purposes in a controlled lab environment. Any real user accounts, IP addresses, hostnames, or other identifiers originally present in the logs and screenshots have been anonymized before publication. No sensitive personal data or production system information is intentionally disclosed.

### 👤 Author

**Manuel Calderon**
Entry-Level SOC Analyst

https://www.linkedin.com/in/manuel-calderon-restrepo-abb201231/

https://github.com/manuelcalderon27
