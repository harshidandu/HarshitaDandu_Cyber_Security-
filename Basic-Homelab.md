# 🛡️ Home Cybersecurity Lab Project (VirtualBox + Windows 10 + Kali Linux)

[![View Lab Project](https://img.shields.io/badge/View%20Lab%20Project-Cybersecurity-blue?style=for-the-badge&logo=github)](https://github.com/YOUR-USERNAME/YOUR-REPO-NAME)

![VirtualBox](https://img.shields.io/badge/VirtualBox-Installed-blue?style=flat-square&logo=virtualbox)
![Windows 10](https://img.shields.io/badge/Windows%2010-Configured-blue?style=flat-square&logo=windows)
![Kali Linux](https://img.shields.io/badge/Kali%20Linux-Deployed-blue?style=flat-square&logo=kalilinux)

---

## 🚀 Project Highlights
- Built a fully isolated cybersecurity lab using VirtualBox to safely test malware and offensive security tools.
- Installed and configured Windows 10 and Kali Linux VMs for red and blue team exercises.
- Set up secure network configurations (NAT, Internal Network) to protect the host system.
- Verified download integrity with checksum validation and implemented snapshot management for rollback.
- Gained hands-on experience in virtualization, network security, and safe malware analysis practices.

---

## 📚 Table of Contents
- [Objective](#objective)
- [Skills Learned](#skills-learned)
- [Tools Used](#tools-used)
- [Steps](#steps)
  - [Introduction and Objectives](#introduction-and-objectives)
  - [Prerequisites and Requirements](#prerequisites-and-requirements)
  - [Installing and Verifying VirtualBox](#installing-and-verifying-virtualbox)
  - [Creating Virtual Machines (VMs)](#creating-virtual-machines-vms)
  - [Network Configurations for a Safe Lab](#network-configurations-for-a-safe-lab)
  - [Best Practices: Snapshots and Resource Allocation](#best-practices-snapshots-and-resource-allocation)
  - [Conclusion and Next Steps](#conclusion-and-next-steps)
- [Security Precautions](#security-precautions)

---

## 🎯 Objective
This project focused on designing and deploying a personal cybersecurity lab using VirtualBox, safely isolated from production systems.  
The goal was to create a sandboxed environment for testing malware, cybersecurity tools, and offensive techniques without risking the host machine or network.

---

## 🛠️ Skills Learned
- Installing and verifying VirtualBox and virtual machine images.
- Deploying and configuring Windows 10 and Kali Linux virtual machines.
- Isolating lab environments using secure network configurations (NAT, Internal Network).
- Performing checksum validation (SHA-256) to verify software integrity.
- Creating and managing snapshots for safe experimentation and rollback.
- Planning and allocating virtual resources efficiently.

---

## 🧰 Tools Used
- **VirtualBox**: Virtualization platform for creating isolated environments.
- **Windows 10**: Target machine for malware analysis and tool testing.
- **Kali Linux**: Offensive security platform for red team exercises.
- **7-Zip**: Tool for extracting compressed VM images.
- **PowerShell**: For checksum verification (Get-FileHash).

---

## 📝 Steps

### Introduction and Objectives
- Built a virtualized environment to safely test cybersecurity tools, malware samples, and attack techniques.
- Simulated real-world scenarios in a controlled, sandboxed lab.

### Prerequisites and Requirements
- Recommended 8–16 GB of RAM and 250+ GB storage.
- Downloaded required OS images: Windows 10 ISO and Kali Linux VirtualBox image.
- Installed VirtualBox and 7-Zip.

### Installing and Verifying VirtualBox
- Downloaded VirtualBox from the official site.
- Verified download integrity using SHA-256 checksum via PowerShell.
- Installed VirtualBox and dependencies (e.g., Visual C++ runtimes).

### Creating Virtual Machines (VMs)

**Windows 10 Setup:**
- Created a new VM with 4 GB RAM and 1–2 CPUs.
- Attached the Windows 10 ISO and performed a clean OS installation.
- Configured Windows settings for lab use.

**Kali Linux Setup:**
- Downloaded and extracted the pre-built Kali VirtualBox image.
- Imported the Kali VM and adjusted memory and CPU settings.
- Logged in with default Kali credentials for initial configuration.

### Network Configurations for a Safe Lab

**Network Adapter Options:**
- **NAT**: Enabled internet access while isolating VMs from the host network.
- **Internal Network**: Allowed VM-to-VM communication without external connectivity (ideal for malware testing).

**Configured VirtualBox Network Settings:**
- Assigned Internal Network adapters for malware isolation scenarios.
- Used NAT for general tool testing and internet access within VMs.

### Best Practices: Snapshots and Resource Allocation
- Created baseline snapshots after each VM installation.
- Took additional snapshots before major experiments or malware execution.
- Allocated system resources carefully to maintain host stability.
- Used internal-only networking or no-network modes for deep malware analysis.

---

## 🛠️ Security Precautions
- Ensured all VMs operated within isolated VirtualBox environments.
- Used Internal Network settings to prevent malware from reaching external networks.
- Verified download integrity of ISO files and VM images to avoid compromised files.
- Maintained frequent VM snapshots for quick recovery after malware tests or system failures.

---

## 🎯 Conclusion and Next Steps

### Recap
- Successfully deployed a Windows 10 and Kali Linux-based cybersecurity lab.
- Gained practical experience setting up VMs, securing lab networks, and configuring safe testing environments.
- Implemented best practices for network isolation, resource management, and snapshot usage.

### Next Steps
- Expand the lab by adding Splunk for log analysis or setting up a Domain Controller.
- Practice malware analysis, penetration testing, and detection engineering in the isolated environment.
- Document configurations and build portfolio projects based on lab activities.

---
