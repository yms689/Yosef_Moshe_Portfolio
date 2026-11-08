flowchart TB
    %% Subgraphs & Zones
    subgraph WAN_EXT ["🌐 External / Perimeter"]
        FW["pfSense / OPNsense Firewall<br/>(VLAN Routing, NAT, Snort/Suricata IDS)"]
    end

    subgraph LAB_INFRA ["🖥️ Virtualized Infrastructure (KVM / libvirt)"]
        direction TB
        subgraph AD_FOREST ["Active Directory & Core Services (VLAN 10)"]
            DC["DC01: Samba / Windows AD DC<br/>Kerberos, LDAP, DNS, GPO"]
            HOST_WIN["Client-Win11 (Domain Joined)"]
            HOST_LNX["Client-Ubuntu (SSO / SSSD)"]
        end

        subgraph ATTACK_SIM ["Adversary Emulation Zone (VLAN 30)"]
            KALI["Kali Linux & Metasploit"]
            ART["Atomic Red Team Automation"]
        end
    end

    subgraph TELEMETRY ["📡 Telemetry & Log Ingestion Pipeline"]
        direction TB
        AGENTS["Endpoints / Sysmon / Wazuh Agent / Winlogbeat"]
        DC -->|WinEventLog 4624, 4688, 4720| AGENTS
        HOST_WIN -->|Sysmon Process Creation & Network Connect| AGENTS
        HOST_LNX -->|Auditd & Auth.log| AGENTS
        FW -->|NetFlow & Syslog| AGENTS
    end

    subgraph DETECTION_SOC ["🛡️ Detection Engineering & SIEM Layer"]
        SIEM["Wazuh Manager & Indexer / OpenSearch<br/>(Ruleset Engine & Correlation)"]
        DASH["Wazuh Dashboard / Grafana<br/>(Real-time SOC Telemetry & Metrics)"]
        AGENTS -->|Encrypted Forwarding (1514/TCP)| SIEM
        SIEM --> DASH
    end

    subgraph AI_SOC ["🤖 Autonomous AI SOC & Orchestration"]
        ALERT_HOOK["Alert Webhook / Event Stream"]
        AI_ENGINE["AI SOC Engine (Python Async Pipeline)<br/>• Prompt Engineering & Context Enrichment<br/>• MITRE ATT&CK Auto-Mapping<br/>• False Positive Filtering"]
        REPORT_GEN["Automated Response & Incident Reporting<br/>• Markdown & PDF Executive Summaries<br/>• Actionable Containment Playbooks"]
        
        SIEM -->|Alert Trigger Level >= 8| ALERT_HOOK
        ALERT_HOOK --> AI_ENGINE
        AI_ENGINE --> REPORT_GEN
    end

    %% Network relationships
    FW --- AD_FOREST
    FW --- ATTACK_SIM
    KALI -.->|Adversary Emulation / TTPs| DC
    ART -.->|Atomic Tests (T1059, T1003, T1078)| HOST_WIN

    %% Styling
    classDef firewall fill:#e1f5fe,stroke:#0288d1,stroke-width:2px;
    classDef infra fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px;
    classDef soc fill:#e8f5e9,stroke:#388e3c,stroke-width:2px;
    classDef ai fill:#fff3e0,stroke:#f57c00,stroke-width:2px;

    class FW firewall;
    class DC,HOST_WIN,HOST_LNX,KALI,ART infra;
    class SIEM,DASH,AGENTS soc;
    class ALERT_HOOK,AI_ENGINE,REPORT_GEN ai;
