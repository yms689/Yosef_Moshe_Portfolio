# Enterprise Hybrid Home Lab & Autonomous AI SOC Platform

An end-to-end detection engineering, enterprise virtualization, and automated incident triage lab simulating modern adversary tactics, telemetry collection, and LLM-assisted SOC workflows.

---

## ⚡ Overview & Key Capabilities

- **Enterprise Infrastructure Simulation:** Virtualized AD domain environment (KVM/libvirt) with segmented VLANs, DNS, Kerberos, and Linux/Windows endpoints[cite: 1.2].
- **Comprehensive Telemetry Pipeline:** Full Sysmon, Auditd, and Windows Event Log ingestion forwarded to Wazuh SIEM[cite: 1.2].
- **Adversary Emulation:** Repeatable threat scenarios mapped to the MITRE ATT&CK framework using Atomic Red Team and Metasploit[cite: 1.2].
- **Autonomous AI Incident Responder:** Python-powered orchestration layer that consumes high-severity SIEM alerts, enriches entity data, and generates structured triage summaries with actionable remediation steps[cite: 1.2].

---

## 🛠️ Technology Stack

| Layer | Tools & Technologies |
| :--- | :--- |
| **Virtualization & Compute** | KVM, libvirt, QEMU, Samba AD DC, Ubuntu Server, Windows Server / 11 |
| **Network & Perimeter** | pfSense / OPNsense, 802.1Q VLANs, Snort/Suricata IDS |
| **SIEM & Observability** | Wazuh Manager, OpenSearch, Sysmon, Auditd, Winlogbeat |
| **Offensive Simulation** | Atomic Red Team, Metasploit Framework, Kali Linux |
| **Automation & AI Tier** | Python 3.11+, LangChain / LLM APIs, Pydantic, Jinja2 (Report Generation) |

---

## 🔬 Attack Scenarios & Triage Workflow

### 1. Adversary Emulation
Using Atomic Red Team, credential dumping tactics (e.g., **T1003.001 - LSASS Memory Dumping**) and command execution (e.g., **T1059.001 - PowerShell**) are executed against domain-joined endpoints.

### 2. Detection & Correlation
- Sysmon Event ID 10 (Process Access) captures anomalous handle access to `lsass.exe`.
- Wazuh correlates the process hierarchy, user context, and host state, triggering a Level 12 critical alert.

### 3. AI-Assisted Triage & Containment
- The **AI SOC Engine** intercepts the alert payload via webhook.
- The pipeline enriches the alert with active directory host metadata, MITRE sub-technique details, and historical endpoint behavior.
- An LLM analyzes the correlation chain to generate:
  - Incident Severity Assessment (True Positive vs. False Positive determination).
  - Scope of compromise (affected accounts and lateral movement risks).
  - Containment actions (e.g., host network isolation, account revocation).
