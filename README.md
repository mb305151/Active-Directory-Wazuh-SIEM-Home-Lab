# Active Directory & Wazuh SIEM Home Lab 🛡️

## 📌 Description
This project demonstrates an Active Directory lab integrated with Wazuh SIEM for real-time security monitoring and log analysis. It features AD DS management, GPO audit policies, and custom detection rules mapped to MITRE ATT&CK for detecting credential attacks and log tampering.

---

## 🏗️ Architecture & Topology

```mermaid
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

    DC -->|Agent Telemetry| WAZUH
    WIN10 -->|Agent Telemetry| WAZUH
    Browser -.->|HTTPS| WAZUH
    SSH -.->|SSH| WAZUH
