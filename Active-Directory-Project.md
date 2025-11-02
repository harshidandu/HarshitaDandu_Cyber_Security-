# Active Directory 2.0: Automated SIEM-SOAR Incident Response
### Project: Real-Time Detection and Automated Disablement of Unauthorized Logins

---

## 📄 Project Summary
This project demonstrates a complete, end-to-end Security Operations Center (SOC) workflow. I built a cloud-based Active Directory lab, configured a SIEM to detect threats, and built a SOAR playbook to automatically respond to them.

The scenario is an **unauthorized successful RDP login** to a domain-joined machine. The **SIEM (Splunk)** detects this login, based on its source IP, and sends an alert to the **SOAR (Shuffle)**. The SOAR playbook automatically enriches the alert, notifies an analyst via Slack, and sends an email with "Yes/No" options. If the analyst clicks "Yes," the SOAR platform connects back to the Domain Controller and **automatically disables the compromised user account in Active Directory**, closing the loop on the incident.

## 🚀 Skills & Technologies at a Glance
This table is designed for a quick review of the technical skills demonstrated in this project.

| Category | Skills & Technologies Demonstrated |
| :--- | :--- |
| **SIEM (Security Info. & Event Mgmt.)** | **Splunk Enterprise:** Installation, configuration, and administration on Ubuntu.<br>**Splunk Query Language (SPL):** Writing custom alert queries to detect threats.<br>**Splunk Universal Forwarder (UF):** Deploying agents on Windows to forward event logs.<br>**Index Creation:** Configuring custom Splunk indexes (`my myani-ad`). |
| **SOAR (Security Orchestration)** | **Shuffle:** Playbook/Workflow design, Webhook ingestion, API integration.<br>**Automated Response:** Building logic to disable users in Active Directory via LDAP.<br>**Human-in-the-Loop:** Designing a workflow that requires analyst approval (via Email) before taking a destructive action. |
| **Directory & Endpoint** | **Active Directory:** Installation, promotion of Domain Controller (`my ani.local`).<br>**User & Computer Management:** Creating users (`JSmith`), joining machines to the domain.<br>**Windows Server 2022:** Deployment and configuration.<br>**Ubuntu Server:** Deployment and command-line administration. |
| **Cloud & Networking** | **Vultr (Cloud):** Provisioning and managing 3 virtual machines (DC, Test, Splunk).<br>**VPC (Virtual Private Cloud):** Configuring a private network for internal communication.<br>**Firewall Configuration (Multi-Layer):** Managing **Vultr** (cloud firewall) and **UFW** (host firewall) rules.<br>**Core Protocols:** RDP, SSH, DNS, LDAP, TCP/IP. |
| **Core Security Skills** | **Incident Response (IR):** Full lifecycle (Detection, Analysis, Containment, Confirmation).<br>**Detection Engineering:** Writing rules to detect specific TTPs (Logon Type `7` & `10`).<br>**Network Troubleshooting:** Debugging DNS and network connectivity issues. |
| **Notification & ChatOps**| **Slack:** Integrating for real-time security alerts.<br>**Email:** Using email as a "human-in-the-loop" decision vector. |

---

## 🗺️ Visual Workflow
This diagram illustrates the complete flow of data and actions, from initial detection to final containment. -- coming soon --



---

## ⚙️ How It Works: Step-by-Step
1.  **Detect:** An attacker RDPs into the "Test Machine" from an unauthorized IP.
2.  **Forward:** The Splunk Universal Forwarder on the Test Machine sends the Windows Security Event Log (Event ID `4624`, Logon Type `7` or `10`) to the Splunk Server.
3.  **Alert:** The Splunk SPL query, running every minute, finds the event (where `source_network_address` is not on the allowlist) and fires an alert.
4.  **Ingest:** The Splunk alert is configured to send a **Webhook** to Shuffle.
5.  **Triage (SOAR):** The Shuffle playbook triggers, parses the alert, and sends a notification to the `#alerts` Slack channel.
6.  **Decision (SOAR):** Shuffle sends an **email** to the SOC analyst asking: "Would you like to disable the user?" with "Yes" and "No" links.
7.  **Contain (SOAR):** The analyst clicks "Yes." Shuffle receives this response and executes its Active Directory module, connecting to the Domain Controller over port `389` (LDAP) to disable the user (`JSmith`).
8.  **Confirm (SOAR):** Shuffle sends a final confirmation message to Slack: "Account: J Smith has been disabled."

---

## 🛠️ Technical Configuration Details

### 1. SIEM Configuration (Splunk) 📡
* **Log Ingestion:** Deployed Splunk Universal Forwarders (UFs) on both the DC and Test Machine.
* **`inputs.conf`:** Edited `inputs.conf` on the UFs to monitor the Security Event Log:
    ```ini
    [WinEventLog://Security]
    disabled = 0
    ```
* **Service Fix:** The UF service was changed to run as the **Local System Account** to grant it the necessary permissions to read the Security Event Logs.
* **Firewalls:** Opened port `8000` (Splunk Web), `9997` (Splunk Ingestion), and `22` (SSH) on both the **Vultr cloud firewall** and the **Ubuntu `ufw` host firewall**.
* **Splunk Alert (SPL):** This query finds all successful RDP logins (Logon Type 7 or 10) that do *not* come from an authorized IP range (e.g., `40.x.x.x`) and formats them.

    ```sql
    index="my ani-ad" event_code=4624 (logon_type=7 OR logon_type=10) source_network_address!="40.*"
    | stats count by _time, computername, source_network_address, user, Logon_Type
    ```


### 2. SOAR Configuration (Shuffle) 🚀
* **Workflow:** Built a workflow that is triggered by the Splunk Webhook.
    `Webhook -> Slack (Alert) -> User Input (Email) -> [IF YES] -> Active Directory (Disable User) -> Slack (Confirm)`
* **AD Integration:** The most critical step was configuring the Active Directory app in Shuffle. This required troubleshooting to find the correct settings:
    * **Server:** The Domain Controller's **Public IP** address.
    * **Port:** `389` (LDAP).
    * **Used SSL:** `false` (to force an unencrypted LDAP connection for this lab).
    * **Base DN:** `CN=users,DC=my ani,DC=local`


---

## 💡 Challenges & Problem-Solving
This section details the key technical problems encountered and the methods used to solve them.

* **Challenge: DNS Failure on Domain Join**
    * **Problem:** The Windows Test Machine could not join the `my ani.local` domain, even though it was on the same VPC. The error was "The specified domain... cannot be contacted."
    * **Investigation:** This error is almost always a DNS issue. The Test Machine didn't know how to resolve the name `my ani.local` to an IP address.
    * **Solution:** I manually configured the Test Machine's VPC network adapter (Ethernet instance 02). I set its **Preferred DNS server** to the **private IP address of the Domain Controller** (`10.22.96.4`). This immediately resolved the issue, as Active Directory relies on DNS to function.

* **Challenge: VPC Network Misconfiguration**
    * **Problem:** After enabling the Vultr VPC on all three machines, the two Windows VMs could not ping the Splunk server (or each other) on their private IPs (e.g., `10.22.96.x`).
    * **Investigation:** Checking `ipconfig` on the Windows VMs showed their VPC-facing network adapters had self-assigned `169.x.x.x` (APIPA) addresses. They were not correctly pulling the static IPs from Vultr.
    * **Solution:** I manually configured the "Ethernet instance 02" adapter properties on both Windows VMs. I set their **static IPv4 addresses** and subnet masks to match the IPs assigned by Vultr (e.g., `10.22.96.3` / `255.255.255.240`). This forced them onto the correct private network, and all hosts could immediately ping each other.

* **Challenge: Splunk Ingestion & UI Failure (Multi-Layer Firewalls)**
    * **Problem:** After installing Splunk, I could not access the Web UI on port `8000` or receive logs on port `9997`, even after adding rules to the **Vultr cloud firewall**.
    * **Investigation:** I realized there are *two* firewalls: the Vultr firewall (at the cloud level) and the `ufw` (uncomplicated firewall) running *on the Ubuntu OS itself*.
    * **Solution:** I had to open the ports in both places. After adding the Vultr rules, I SSH'd into the Splunk server and ran `ufw allow 8000` and `ufw allow 9997`. The UI and log ingestion began working immediately. This shows an understanding of multi-layer network security.

* **Challenge: Splunk Logs Not Forwarding from Windows**
    * **Problem:** The Universal Forwarder (UF) was installed and the service was running, but no Security event logs were arriving in Splunk.
    * **Investigation:** I checked the `splunkd.log` on the Windows machines and saw "access denied" errors when the UF service tried to read the Security log.
    * **Solution:** The default "Log On As" account for the Splunk UF service did not have sufficient permissions. I solved this by opening `services.msc`, finding the "SplunkForwarder" service, and changing its **Log On** properties to use the **Local System Account**, which has the necessary privileges.

* **Challenge: SOAR-to-Active Directory Authentication Failure**
    * **Problem:** The Shuffle workflow could not connect to the Domain Controller to disable the user. All attempts failed.
    * **Investigation:** I had to troubleshoot the connection parameters. The initial attempts using the private IP failed (as Shuffle is an external cloud service). Using the public IP with SSL also failed.
    * **Solution:** The final working configuration required two specific settings:
        1.  Connecting to the DC's **Public IP** address, which required me to open port `389` (LDAP) on the Vultr firewall.
        2.  Setting the **"Used SSL"** option in the Shuffle configuration to **`false`**. This forced an unencrypted LDAP connection, which is insecure for production but necessary for this lab setup and demonstrates troubleshooting the connection protocol.
