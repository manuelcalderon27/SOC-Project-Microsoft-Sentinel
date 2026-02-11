# 🛡️ SOC Project – Microsoft Sentinel (SIEM)


## 📌 Overview

This project simulates a Security Operations Center (SOC) environment using Microsoft Sentinel in Azure.
It focuses on collecting, enriching, detecting, and visualizing malicious authentication activity from Windows Security logs.

The project demonstrates hands-on experience with:

SIEM implementation

Log ingestion and enrichment

Threat detection

KQL development

SOC investigation workflows


## 🏗️ Architecture

- Internet (Attackers)  
- Azure Windows VM  
- Log Analytics Workspace  
- Microsoft Sentinel (SIEM)  
- Workbooks & Incidents



# Architecture Diagram

<img width="661" height="1141" alt="Diagrama sin título drawio" src="https://github.com/user-attachments/assets/f2957c11-d278-439f-9bb7-44856207b1a2" />

## Components:

Azure Virtual Machine (Windows)

Network Security Group (NSG)

Log Analytics Workspace

Microsoft Sentinel

Watchlists (GeoIP)

Sentinel Workbooks


## 🎯 Objectives

Simulate brute-force login attempts

Collect Windows Security Event Logs

Detect suspicious authentication behavior

Enrich logs with geographic context

Visualize attacker activity using dashboards

Practice SOC monitoring and investigation


## 🔍 Data Sources

Windows Security Event Logs:

4625 – Failed logon attempts

4624 – Successful logons

4688 – Process creation

Logs are ingested using Azure Monitor Agent (AMA) into Log Analytics.


## 🧪 Attack Simulation

A Windows virtual machine is intentionally exposed to the Internet in a controlled lab environment

External login attempts generate Event ID 4625

These events are used as detection data


## 📊 Detection & Analysis
Example Detection Query (Failed Logons)
SecurityEvent
| where EventID == 4625
| summarize FailedLogons = count() by IpAddress
| order by FailedLogons desc


Used to identify:

Brute-force attempts

Repeated authentication failures

High-risk source IPs


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


## 📈 Visualization

Sentinel Workbooks are used to create:

Failed login tables

Time-based charts

Geographic maps of attacker IPs

Visualizations support SOC monitoring and threat hunting


## 🚨 Incident Handling

Alerts and incidents are generated based on detection logic.
SOC activities performed:

Alert triage

Event correlation

Basic investigation

Documentation of findings


## 🛠️ Tools & Technologies

Microsoft Sentinel (SIEM)

Azure Log Analytics

Kusto Query Language (KQL)

Windows Event Logs

Azure Monitor Agent

Azure Virtual Machines

Network Security Groups

MITRE ATT&CK (conceptual mapping)


## 🧠 Skills Demonstrated

SIEM deployment and configuration

Log ingestion, correlation, and enrichment

KQL query development

Threat detection (brute-force attacks)

Security dashboards and geographic visualizations

SOC workflows (monitoring, investigation, documentation)


## Microsoft Sentinel Dashboard

Failed Login Events(KQL)
<img width="958" height="376" alt="image" src="https://github.com/user-attachments/assets/cceb7bc3-16eb-49f0-8d76-e662321ef7f3" />

<img width="616" height="648" alt="image" src="https://github.com/user-attachments/assets/0bdd6e3f-5c21-4b96-bd7c-15b6c2d3e69f" />


Failed login events are enriched using a custom GeoIP watchlist and the ipv4_lookup function to correlate attacker IPs with geographical data (country and coordinates), enabling map-based visualization and attack source analysis.

<img width="900" height="395" alt="image" src="https://github.com/user-attachments/assets/9a601043-e1d1-4e6e-9ace-abcb412370c1" />



## Attacker Map Visualization

<img width="554" height="384" alt="image" src="https://github.com/user-attachments/assets/3487268a-a2de-4180-9ef2-35f8d2372872" />


## ▶️ How to Reproduce

Deploy a Windows VM in Azure

Enable Azure Monitor Agent (AMA)

Connect VM to Log Analytics Workspace

Enable Microsoft Sentinel

Upload GeoIP watchlist

Import Workbook JSON

Run KQL detection queries

Observe failed login events


⚠️ Disclaimer

This project was developed for educational and learning purposes in a controlled lab environment.
Any real user accounts, IP addresses, hostnames, or other identifiers originally present in the logs and screenshots have been anonymized before publication.
No sensitive personal data or production system information is intentionally disclosed.


👤 Author

Manuel Calderon
Entry-Level SOC Analyst
https://www.linkedin.com/in/manuel-calderon-restrepo-abb201231/
https://github.com/manuelcalderon27
