# SOC Automation Lab Project

[![View SOC Lab Project](https://img.shields.io/badge/View%20Project-SOC%20Automation-blue?style=for-the-badge&logo=github)](https://github.com/YOUR-USERNAME/YOUR-REPO-NAME)

---

## 📚 Table of Contents
- [Objective](#objective)
- [Skills Learned](#skills-learned)
- [Tools Used](#tools-used)
- [Steps](#steps)
  - [Project Overview and Diagramming](#ref-1-project-overview-and-diagramming)
  - [Setting Up Infrastructure and Virtual Machines](#ref-2-setting-up-infrastructure-and-virtual-machines)
  - [Installing and Configuring Components](#ref-3-installing-and-configuring-components)
  - [Building the Automation Workflow](#ref-4-building-the-automation-workflow)
  - [Testing the Automation Pipeline](#ref-5-testing-the-automation-pipeline)
  - [Troubleshooting and Debugging](#ref-6-troubleshooting-and-debugging)
  - [Finalization](#ref-7-finalization)
- [Troubleshooting Challenges and Solutions](#troubleshooting-challenges-and-solutions)
- [Insights and Observations](#insights-and-observations)
- [Future Improvements](#future-improvements)
- [Security Precautions](#security-precautions)

---

## 🎯 Objective

The SOC Automation Lab project focused on building a scalable, hands-on Security Operations Center (SOC) environment.  
The goal was to integrate a SIEM (Wasa/Wazuh), a SOAR platform (Shuffle), and a case management system (TheHive) to automate telemetry ingestion, threat enrichment, case creation, and active response workflows.

This end-to-end project deepened understanding of SOC operations, incident detection, investigation, and automated response orchestration.

---

## 🛠️ Skills Learned

- Designing and diagramming security architecture.
- Deploying and configuring SIEM, SOAR, and case management platforms.
- Building and tuning custom alert rules.
- Integrating APIs (VirusTotal, Wazuh, TheHive, Shuffle).
- Creating automation workflows using SOAR platforms.
- Troubleshooting data parsing, API calls, and active response execution.
- End-to-end security telemetry ingestion and automated incident response.

---

## 🧰 Tools Used

- **Wasa (Wazuh variant)**: SIEM/XDR platform for telemetry ingestion and alert generation.
- **TheHive**: Case management and incident response platform.
- **Shuffle**: SOAR platform for orchestration and automation.
- **Sysmon**: Endpoint telemetry tool for detailed event logging.
- **Windows 10**: Log generation and attack simulation endpoint.
- **Kali Linux**: Red team attack simulation platform.
- **VirusTotal**: Threat intelligence API.
- **Draw.io**: Diagramming and architecture planning tool.
- **VirtualBox**: Virtualization platform for lab deployment.

---

# 📝 Steps

## Ref 1: Project Overview and Diagramming

- Defined project goals: build a functional SOC lab integrating telemetry, enrichment, automation, and case management.
- Created an architecture diagram with **draw.io**:
  - Windows 10 (Sysmon logs) → Wasa (SIEM) → Shuffle (SOAR) → TheHive (Case Management).
  - Data flows color-coded: telemetry (gray), alerts (blue), active response (red).

---

## Ref 2: Setting Up Infrastructure and Virtual Machines

- Installed **VirtualBox** and verified installation integrity with SHA-256 checksums.
- Deployed virtual machines:
  - **Windows 10 Client** (Sysmon agent installed).
  - **Kali Linux** (for offensive testing).
  - **Ubuntu servers** (for Wasa, TheHive, and Shuffle).
- Configured safe networking (NAT, Internal Network).

---

## Ref 3: Installing and Configuring Components

- **Sysmon**: Installed and configured on Windows 10 VM to capture key event types.
- **Wasa (Wazuh)**: Installed on Ubuntu; configured to ingest Sysmon events and forward alerts to Shuffle.
- **TheHive**: Deployed on Ubuntu; configured case management backend and user accounts.
- **Shuffle**: Configured webhook triggers, built parsing workflows, integrated VirusTotal API and TheHive API.

---

## Ref 4: Building the Automation Workflow

- Created a **Shuffle workflow**:
  - **Trigger**: Webhook input from Wasa.
  - **Parsing**: Extracted SHA-256 hashes using Regex.
  - **Enrichment**: Queried VirusTotal for file reputation.
  - **Case Management**: Created incident cases in TheHive automatically.
  - **Analyst Notification**: Sent email with key incident details.
  - **Optional Active Response**: Allowed analyst-initiated blocking actions.

---

## Ref 5: Testing the Automation Pipeline

- Simulated attacks with **mimikatz** on Windows 10.
- Verified Sysmon telemetry ingestion into Wasa.
- Checked alert forwarding to Shuffle and successful workflow execution:
  - Data parsing.
  - Threat enrichment.
  - Case creation in TheHive.
  - Email notifications.
  - (Optional) Execution of active response commands.

---

## Ref 6: Troubleshooting and Debugging

- **No Telemetry**: Fixed Sysmon configuration and Wasa ingestion pipeline issues.
- **Webhook Errors**: Corrected Shuffle webhook URLs and permissions.
- **Regex Parsing Issues**: Tuned Regex patterns to accurately extract hashes.
- **VirusTotal API Failures**: Verified API key permissions and endpoint URLs.
- **Active Response Failures**: Ensured correct agent control syntax and endpoint ID targeting.

---

## Ref 7: Finalization

- Saved clean snapshots of all VMs for rollback safety.
- Exported Shuffle workflows for backup.
- Documented lab configuration, architecture diagrams, and troubleshooting notes.

---

# 🔥 Troubleshooting Challenges and Solutions

- **Challenge**: No alerts were forwarded from Wasa to Shuffle.
  - **Solution**: Fixed webhook URL misconfiguration and adjusted output filter settings in Wasa.

- **Challenge**: VirusTotal enrichment failed during API calls.
  - **Solution**: Updated API keys and ensured correct endpoints in Shuffle workflow.

- **Challenge**: Active response commands failed.
  - **Solution**: Verified agent IDs and payload formatting for agent control commands.

- **Challenge**: Regex patterns missed critical data fields.
  - **Solution**: Refined parsing expressions using test samples and validation in Shuffle.

---

# 🔍 Insights and Observations

- Full SOC automation is achievable with open-source tools and careful integration planning.
- Building modular workflows improves flexibility when incident response playbooks evolve.
- Accurate data parsing is crucial for successful enrichment and case creation.
- Integrating external threat intelligence (VirusTotal) greatly enhances alert triage and prioritization.

---

# 🚀 Future Improvements

- Expand the lab to simulate multi-stage attacks (e.g., privilege escalation, lateral movement).
- Integrate additional threat intelligence sources (e.g., AbuseIPDB, AlienVault OTX).
- Create dynamic dashboards in TheHive for real-time incident tracking.
- Implement automated ticket generation into Jira or ServiceNow.
- Build proactive threat hunting workflows triggered by telemetry anomalies.

---

# 🔒 Security Precautions

- All lab activities conducted on isolated, internal-only networks.
- Used NAT and Internal Network modes to prevent malware escape.
- Enforced strict API key management (limited permissions, rotated keys regularly).
- Regular snapshots created to allow fast rollback after attack simulation tests.

---

✅ **Final Result**:  
Successfully designed, deployed, and validated a real-world SOC lab integrating detection, enrichment, case management, and response automation — demonstrating practical skills in modern cybersecurity operations.

