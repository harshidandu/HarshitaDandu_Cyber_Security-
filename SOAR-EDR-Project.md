# Fully Automated SOAR-EDR Incident Response Pipeline
### Project: Real-Time Detection, Triage, and Containment of Credential Theft Tools

---

## 📄 Project Summary
This project simulates a real-world Security Operations Center (SOC) workflow by building an automated detection and response pipeline.

When a malicious tool (`Lasagna`) is executed on a Windows endpoint, the **EDR (Lima Charlie)** detects it and sends an alert. The **SOAR (Tines)** playbook ingests the alert, enriches it, and asks an analyst for permission to act via Slack and Email. If the analyst approves, the SOAR platform automatically sends a command back to the EDR to isolate the compromised machine from the network.

## 🚀 Skills & Technologies at a Glance
This table is designed to be skimmed.

| Category | Skills & Technologies Demonstrated |
| :--- | :--- |
| **SOAR Platform** | **Tines:** Playbook/Story design, Webhook ingestion, JSON parsing, API integration. |
| **EDR Platform** | **Lima Charlie:** Sensor deployment, custom Detection & Response (DNR) rule writing, API-based containment. |
| **Core Skills** | **Incident Response (IR):** Full lifecycle (Detection, Analysis, Containment, Confirmation).<br>**Detection Engineering:** Writing robust, multi-condition rules (hash, file path, command line).<br>**API Integration:** Using REST APIs, Webhooks, and JWT for tool-to-tool communication. |
| **Automation Logic** | Branching logic (If/Else), "Human-in-the-Loop" approval workflows, dynamic data parsing. |
| **Notification** | **Slack** & **Email** integration for automated alerting and ChatOps. |
| **Threat Emulation**| **Lasagna** (Credential Dumping Tool). |
| **Planning** | **draw.io** for workflow diagramming and architecture planning. |

---

## 🗺️ Visual Workflow
This diagram illustrates the complete flow of data and actions, from initial detection to final containment.
-- coming soon ---

---

## ⚙️ How It Works: Step-by-Step
1.  **Detect:** `lasagna.exe` is executed on the Windows VM.
2.  **Alert:** The Lima Charlie EDR agent matches the process against a custom DNR rule and generates a `detection`.
3.  **Ingest:** Lima Charlie sends the full JSON alert to a Tines Webhook URL.
4.  **Triage & Alert:** The Tines Story triggers, parses the JSON, and sends formatted alerts to both Slack and an analyst's Email.
5.  **Decision:** Tines generates a unique "Page" (web link) prompting the analyst to "Approve" or "Deny" network isolation for the machine.
6.  **Contain:** If **"Approved"**, Tines uses the Lima Charlie API and the `Sensor ID` from the alert to execute the `isolate_sensor` command.
7.  **Confirm:** Tines queries the EDR for the sensor's new status and sends a final confirmation message to Slack (e.g., "Machine [hostname] has been isolated.").

---

## 🛠️ Technical Configuration Details

### 1. EDR Configuration (Lima Charlie) 📡
* **Sensor Deployment:** Installed the 64-bit Windows sensor on the VM using the organization's installation key.
* **Custom DNR Rule:** Wrote a robust, multi-condition detection rule to catch the threat. The rule triggers if **any** of the following are true:
    * File path ends with `lasagna.exe`
    * Command line contains `lasagna` or `all`
    * File hash matches the known `Lasagna.exe` hash
* **Output Configuration:** Set up an "Output" to send all `detection` events to the Tines Webhook, linking the EDR to the SOAR.

> 

### 2. SOAR Configuration (Tines) 🚀
* **Webhook Ingestion:** The Story is triggered by the "Webhook" action, which receives the JSON payload from Lima Charlie.
* **Dynamic Field Parsing:** Extracted key data from the JSON to use in other actions (e.g., `{{ retrieve_detection.body.detect.routing.sid }}` to get the Sensor ID for isolation).
* **Secure API Authentication:** Stored the Lima Charlie Organization API Key (JWT) as a secure "Credential" in Tines. This credential was used to authenticate the API call for machine isolation, ensuring no secret keys were hard-coded in the playbook.



---

## 💡 Challenges & Problem-Solving
This section details the key technical and design challenges encountered during the project and the methods used to overcome them.

* **Challenge: Parsing Deeply Nested JSON Payloads**
    * **Problem:** The entire automation hinges on passing data from the alert (like the `Sensor ID`) to the containment action. The API call to isolate the machine was failing, but the alert was arriving correctly.
    * **Investigation:** I used the "Events" tab in Tines to capture and inspect the raw JSON payload from Lima Charlie. I discovered the data was highly nested. My initial attempt to get the Sensor ID (`{{ retrieve_detection.body.sid }}`) was wrong.
    * **Solution:** By manually tracing the JSON structure, I found the correct path was `{{ retrieve_detection.body.detect.routing.sid }}`. In contrast, the command line was in a completely different branch: `{{ retrieve_detection.body.detect.event.COMMAND_LINE }}`. This trial-and-error process emphasized that proficiency in reading and debugging JSON is a fundamental requirement for any SOAR or API integration.

* **Challenge: Designing a Resilient Detection Rule**
    * **Problem:** My first detection rule was too simple: `file_path ends with 'lasagna.exe'`. During testing, I realized an attacker could simply rename the file to `update.exe` and completely evade the detection.
    * **Investigation:** I had to "think like an attacker" and identify other, more stable indicators of compromise.
    * **Solution:** I re-engineered the rule to be layered and far more robust. I added multiple conditions with `OR` logic:
        1.  The `file_path` (catches the default name).
        2.  The `file_hash` (catches the file even if renamed, though it fails if recompiled).
        3.  The `command_line` contains `lasagna` or ends with `all` (catches the *behavior* of the tool, which is much harder for an attacker to change).
    This multi-condition approach creates a high-fidelity detection that is significantly harder to bypass.

* **Challenge: Debugging a Multi-Stage Workflow**
    * **Problem:** When I first built the entire 7-step pipeline and ran a test, it failed. I had no idea *where* in the chain the failure had occurred. It could have been the EDR rule, the EDR-to-SOAR output, the SOAR's JSON parsing, the Slack API, or the SOAR-to-EDR isolation command.
    * **Investigation:** An "all-at-once" testing approach is impossible to debug.
    * **Solution:** I adopted a "unit testing" methodology and tested each link in the chain individually:
        1.  **Test 1:** Run `lasagna.exe` -> Did the alert appear in the **Lima Charlie UI**? (Success)
        2.  **Test 2:** Re-run -> Did the event appear in the **Tines "Events" tab**? (Success - this confirms the LC Output and Tines Webhook are working)
        3.  **Test 3:** Manually re-play the Tines event -> Did the **Slack message** post? (Failure - found a typo in my Slack action).
        4.  **Test 4:** Fix Slack. Re-play event -> Did the **Isolation Page** generate? (Success)
        5.  **Test 5:** Submit Page -> Did the **Isolation API call** work? (Failure - this isolated the JSON parsing error from Challenge 1).
    This methodical, step-by-step process was the only way to efficiently find and fix each point of failure.

* **Challenge: Balancing Automation Speed with Operational Risk**
    * **Problem:** This was a *design* challenge, not a technical one. My first instinct was to build a 100% automated workflow: Detection -> Instant Isolation.
    * **Investigation:** I realized that while fast, this is extremely dangerous. What if my rule had a false positive and flagged a legitimate administrator script on a critical Domain Controller or production database? The automation would instantly take that server offline, causing a catastrophic business outage.
    * **Solution:** I made the deliberate design choice to insert a "human-in-the-loop" step. The Tines "Page" acts as a critical safety brake. It allows automation to do the machine-speed work (detection, data collection, alerting) but reserves the final, high-impact containment decision for a human analyst. This project demonstrates a practical balance between automation speed and operational safety.
