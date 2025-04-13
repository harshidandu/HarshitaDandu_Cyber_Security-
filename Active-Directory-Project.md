# 🛡️ Active Directory and Red/Blue Team Lab Project

## 🚀 Project Highlights
- Built a fully functional Active Directory lab with domain services, target endpoints, and attacker machines.
- Deployed and configured Splunk SIEM for real-time telemetry collection and analysis.
- Installed Sysmon and Splunk Universal Forwarders to capture detailed endpoint activity.
- Conducted red team simulations using Crowbar (brute force) and Atomic Red Team (adversary simulation).
- Analyzed attack telemetry, tuned detection configurations, and developed troubleshooting skills in a real-world lab environment.

---

## 📚 Table of Contents
- [Objective](#objective)
- [Skills Learned](#skills-learned)
- [Tools Used](#tools-used)
- [Steps](#steps)
  - [Project Planning and Diagramming](#ref-1-project-planning-and-diagramming)
  - [Environment Setup with VirtualBox](#ref-2-environment-setup-with-virtualbox)
  - [Active Directory Domain Controller Configuration](#ref-3-active-directory-domain-controller-configuration)
  - [Sysmon and Splunk Configuration](#ref-4-sysmon-and-splunk-configuration)
  - [Red and Blue Team Exercises](#ref-5-red-and-blue-team-exercises)
  - [Troubleshooting and Optimization](#ref-6-troubleshooting-and-optimization)
  - [Conclusion and Further Learning](#ref-7-conclusion-and-further-learning)
- [Challenges and Solutions](#challenges-and-solutions)
- [Insights and Observations](#insights-and-observations)
- [Future Improvements](#future-improvements)
- [Security Precautions](#security-precautions)

---

## 🎯 Objective

This project focused on designing and deploying a full Active Directory (AD) environment integrated with a centralized SIEM (Splunk) for log collection and security monitoring. The lab included configuring domain services, deploying endpoint agents, simulating attacks using red team tools, and analyzing telemetry to strengthen detection and response capabilities.

---

## 🛠️ Skills Learned

- Designing networked lab environments using virtualization (VirtualBox).
- Installing and configuring Active Directory Domain Services (AD DS) on Windows Server 2022.
- Creating Organizational Units (OUs), user accounts, and administrative policies in Active Directory.
- Setting up Splunk on Ubuntu for centralized log collection and monitoring.
- Deploying Sysmon and Splunk Universal Forwarder for endpoint telemetry.
- Executing red team exercises using Crowbar and Atomic Red Team.
- Analyzing and correlating security telemetry in Splunk.
- Troubleshooting virtual networking, domain join, and data ingestion issues.

---

## 🧰 Tools Used

- **VirtualBox**: Virtualization platform.
- **Windows Server 2022**: Domain Controller.
- **Windows 10**: Target machine.
- **Kali Linux**: Red team attacker machine.
- **Ubuntu Server 22.04**: Splunk SIEM deployment.
- **Sysmon**: Endpoint telemetry collection.
- **Splunk Enterprise & Universal Forwarder**: Log aggregation and analysis.
- **Crowbar**: Brute force attack tool.
- **Atomic Red Team**: Adversary simulation framework.
- **Draw.io**: Lab network diagramming.

---

## 📝 Steps

### Ref 1: Project Planning and Diagramming
- Created a complete lab architecture diagram using Draw.io.
- Planned IP schema, static assignments, and firewall segmentation.

### Ref 2: Environment Setup with VirtualBox
- Installed VirtualBox and verified SHA-256 hashes.
- Deployed Windows Server 2022, Windows 10, Ubuntu (Splunk), and Kali Linux virtual machines.
- Configured NAT and host-only networking for safe internal traffic.

### Ref 3: Active Directory Domain Controller Configuration
- Installed Windows Server 2022 and promoted it to a domain controller (`mydfir.local`).
- Created Organizational Units (IT, HR) and users (e.g., JSmith, TSmith).
- Joined the Windows 10 machine to the domain and validated connectivity.

### Ref 4: Sysmon and Splunk Configuration
- Installed Splunk Enterprise on Ubuntu Server.
- Deployed Sysmon with a tuned configuration to capture process, network, and file telemetry.
- Installed Splunk Universal Forwarder on Windows endpoints to forward logs to the SIEM.
- Created Splunk indexes and verified successful event ingestion.

### Ref 5: Red and Blue Team Exercises
- Set up Kali Linux for red team simulations.
- Used **Crowbar** to conduct brute force RDP attacks against the target machine.
- Deployed **Atomic Red Team** techniques to simulate real-world adversarial behavior.
- Verified that Splunk captured successful and failed login attempts and adversary activities.

### Ref 6: Troubleshooting and Optimization
- Diagnosed and fixed domain join issues by correcting DNS settings.
- Adjusted firewall and Splunk forwarding configurations for complete log ingestion.
- Refined Sysmon configuration to capture additional attack telemetry.

### Ref 7: Conclusion and Further Learning
- Successfully built, attacked, monitored, and tuned a full AD lab environment.
- Developed advanced troubleshooting, telemetry analysis, and security operations monitoring skills.

---

## 🛠️ Challenges and Solutions

- **Network Configuration Errors**: Resolved DNS and IP conflicts by planning static addressing and proper DNS pointing.
- **Splunk Ingestion Failures**: Fixed misconfigured inputs.conf on Universal Forwarders and verified network connectivity.
- **Attack Detection Gaps**: Tuned Sysmon and Splunk forwarding to capture Atomic Red Team simulations properly.
- **Crowbar Attack Visibility**: Ensured successful logging of RDP brute force attempts by tuning event capture.

---

## 🔍 Insights and Observations

- Accurate DNS setup is critical for stable Active Directory environments.
- Sysmon provides deep endpoint visibility but requires fine-tuning.
- Red team simulations provide valuable validation of telemetry and SIEM effectiveness.
- Distinguishing normal vs. abnormal endpoint behavior is key for effective threat hunting.

---

## 🚀 Future Improvements

- Build Splunk dashboards to visualize attack and endpoint activity trends.
- Implement Splunk alerts based on MITRE ATT&CK techniques.
- Expand red team activities to simulate persistence, lateral movement, and exfiltration.
- Integrate open-source threat intelligence feeds into Splunk for richer context.

---

## 🔒 Security Precautions

- Isolated lab environment without internet exposure.
- NAT and host-only networking ensured safe VM communications.
- Regular VM snapshots taken before major changes.
- Hardened firewall configurations and service restrictions applied.

---


