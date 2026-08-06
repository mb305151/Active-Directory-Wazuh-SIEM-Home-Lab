This project demonstrates the deployment and configuration of an Active Directory domain environment integrated with a Wazuh SIEM instance for real-time security monitoring, event log aggregation, and custom threat detection.

The lab simulates an enterprise network environment with centralized auditing, Group Policy enforcement, and active SOC alert monitoring.

🏗️ Architecture & Topology
Domain Controller: Windows Server (DC-Server.ad.lab.local)

Workstation: Windows 10 Enterprise (Win10-Workstation)

SIEM / Telemetry: Wazuh Server & Dashboard (Ubuntu Server)

Networking: VirtualBox Isolated Lab Network (Host-Only / NAT Network)

🚀 Key Features & Implementation
Active Directory Domain Services (AD DS):

Configured ad.lab.local domain infrastructure.

Defined Organizational Units (OUs), domain users (jkowalski), and RBAC structures.

Advanced Audit Policies (GPO):

Enabled detailed auditing for Process Creation, Logon/Logoff failures (Event ID 4625), and Event Log Tampering (Event ID 1102 / 104).

SIEM Integration (Wazuh):

Deployed Wazuh agents on domain endpoints.

Verified telemetry pipelines for authentication failures and privilege operations.

Custom Detection Rules:

Developed custom detection logic (local_rules.xml) to elevate log-tampering events (Clear-EventLog) to Critical Severity (Level 15) mapped to MITRE ATT&CK T1070 (Indicator Removal).

🎯 Simulated Attack Scenarios & Detections
Authentication Failures & Brute Force: Simulated network-based password spraying via PowerShell; correlated multiple failed logons into High Severity alerts.

Defense Evasion / Log Clearing: Triggered event log purges on Windows hosts, confirming real-time rule match and Critical dashboard alerting.

graph TD
    subgraph Host["💻 Physical Host PC"]
        Browser["Przeglądarka Web<br/>(Dashboard 8443)"]
        SSH["Klient SSH<br/>(Port 2222)"]
    end

    subgraph Network["🌐 VirtualBox Lab Network (192.168.10.0/24)"]
        DC["<b>DC-Server</b><br/>Windows Server<br/>192.168.10.10<br/><i>(AD DS + Wazuh Agent)</i>"]
        WIN10["<b>Win10-Workstation</b><br/>Windows 10<br/>192.168.10.20<br/><i>(Wazuh Agent)</i>"]
        WAZUH["<b>Wazuh-Server</b><br/>Ubuntu Server<br/>192.168.10.6<br/><i>(Manager + Indexer)</i>"]
    end

    %% Połączenia
    DC -->|Agent Telemetry| WAZUH
    WIN10 -->|Agent Telemetry| WAZUH
    Browser -.->|HTTPS| WAZUH
    SSH -.->|SSH| WAZUH
