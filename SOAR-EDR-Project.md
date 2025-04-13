# SOAR EDR Project

## Objective

The SOAR EDR project focused on building an advanced detection and response (DNR) workflow by integrating an EDR platform (Lima Charlie) with security automation (Tines) and communication tools (Slack). The objective was to detect credential theft tools on an endpoint, trigger real-time alerts, prompt analyst decisions, and automate threat containment actions, demonstrating a full security operations pipeline from detection to response.

## Skills Learned

- Designing security workflows and playbooks using diagramming tools (Draw.io).
- Cloud deployment and secure configuration of Windows server instances (Vultr).
- Endpoint Detection and Response (EDR) sensor installation and management (Lima Charlie).
- Writing and tuning custom detection rules based on telemetry events.
- Integrating SIEM/EDR platforms with security automation (Tines) and communication channels (Slack).
- Building conditional logic workflows for automated threat containment.
- Real-time event handling, webhook management, and user-driven automated responses.

## Tools Used

- **Lima Charlie**: EDR platform for telemetry collection, detection, and response actions.
- **Tines**: Security orchestration and automation platform for playbook development.
- **Slack**: Alerting and communication channel for real-time incident notification and response.
- **Vultr**: Cloud provider for provisioning Windows endpoints.
- **Draw.io**: Used to design and plan workflow diagrams.
- **PowerShell**: Used for agent installation and endpoint configuration scripting.

## Steps

*Ref 1: Project Planning and Workflow Design*
- Developed a complete detection and response playbook design using Draw.io, outlining data flows between Lima Charlie, Tines, and Slack.
- Mapped event telemetry (process creation, command-line activity) to detection triggers and automated isolation actions.

*Ref 2: Deploying and Configuring the EDR Endpoint*
- Provisioned a Windows Server instance on Vultr with minimal resources (1 CPU, 2GB RAM) for EDR simulation.
- Hardened the server by configuring firewall rules to allow only RDP traffic and removing unnecessary ports.
- Installed Lima Charlie agent via PowerShell and enrolled the machine into the Lima Charlie organization.
- Verified endpoint connectivity, telemetry collection, and sensor health via the Lima Charlie dashboard.

*Ref 3: Creating Custom Detection Rules in Lima Charlie*
- Analyzed live telemetry using Lima Charlie's Timeline view to identify unique artifacts of the "lasagna" credential access tool.
- Created custom detection rules using event fields such as `file_path` ends with `lasagna.exe` and `command_line` contains `lasagna`.
- Validated detection rules using sample events and the "Test Event" feature to ensure accurate detection without false positives.

*Ref 4: Integrating Lima Charlie with Tines and Slack*
- Set up a Slack workspace and configured incoming webhooks to a dedicated `#alerts` channel.
- Built Tines workflows to ingest Lima Charlie webhook alerts, parse detection fields, and format incident messages for Slack.
- Configured outputs in Lima Charlie to forward critical detection events to Tines using secured webhooks.

*Ref 5: Building the Automated Response Playbook*
- Developed a conditional response flow in Tines that triggered when a detection matched the custom rules.
- Created a user prompt in Slack asking "Do you want to isolate the machine?".
- If "YES" was selected, automatically issued an isolate command through Lima Charlie’s API targeting the infected endpoint.
- If "NO" was selected, generated a "monitor and investigate" notification without isolation.
- Confirmed automated Slack alerts and email notifications included enriched metadata (e.g., process command line, IP address, hostname, event timestamp).

*Ref 6: Testing and Verifying the Automation Workflow*
- Simulated attacks by executing the "lasagna" tool on the endpoint.
- Verified that custom detection rules triggered within seconds.
- Observed end-to-end alert propagation from Lima Charlie to Tines to Slack.
- Tested both "YES" (automatic isolation) and "NO" (manual investigation) paths of the playbook.
- Confirmed endpoint isolation by checking Lima Charlie’s sensor status and Slack confirmation messages.

*Ref 7: Conclusion and Further Resources*
- Successfully designed, deployed, and validated an automated threat detection and response workflow.
- Gained hands-on experience with modern EDR tooling, automation platforms, and real-world SOC practices.
- Identified areas for expanding the playbook with threat intelligence enrichment and incident ticketing systems.

## Challenges and Solutions

- **Firewall Configuration Errors**: Initial misconfiguration of firewall rules blocked sensor communications; resolved by selectively allowing outbound HTTPS traffic.
- **Webhook Parsing Issues**: Early alert payloads were improperly formatted; corrected by adjusting Tines' extraction of nested event fields.
- **Detection Rule Tuning**: Fine-tuned criteria to avoid false positives from legitimate administrative tools on the endpoint.

## Insights and Observations

- User-driven prompts balance automation speed with human oversight, reducing the risk of false isolation.
- Building detection rules based on file paths and command-line arguments is effective but must be combined with additional indicators (e.g., hash values) for high-confidence detections.

## Future Improvements

- Extend playbook to support multi-stage attacks (e.g., credential access followed by lateral movement).
- Integrate threat intelligence feeds to automatically prioritize and enrich alerts.
- Add ServiceNow integration for automatic incident ticket creation upon confirmed isolation.

## Security Precautions

- All activities conducted on isolated, cloud-based infrastructure.
- Restricted remote access via strict firewall rules.
- Regular VM snapshots taken to allow quick restoration after testing dynamic response workflows.

