<h1>Hi, I'm Michael! <br/><a href="https://www.linkedin.com/in/michael-musoke/">Cybersecurity Professional</a></h1>

<h2>👨‍💻 Sentinel Configuration and Investigation</h2>

- <b> Sentinel </b>
  - [Sentinel](https://github.com/Muts256/Sentinel)

Simulation of a Security Operations Center (SOC) environment by deploying a honeynet in Microsoft Azure, forwarding security logs into Microsoft Sentinel, and detecting a brute force attack in real time. The project demonstrates the end-to-end SOC workflow, including log ingestion, threat detection using KQL queries, incident creation and investigation, and enrichment with external threat intelligence sources such as Pulsedive.

Summary 
  - Objective: Investigate a simulated data breach using Microsoft Sentinel and KQL.
  - Tools: Microsoft Sentinel, Log Analytics Workspace, KQL, Azure VM logs.
  - Focus: Identify anomalies, trace attacker behaviour, reconstruct timeline.
  - Key Skills Demonstrated: Log analysis, incident investigation, IOC correlation, threat hunting logic.
  - Outcome: Discovered Brute force indicators and developed analytic queries for detection.

Tools & Technologies
  - Microsoft Sentinel
  - KQL (Kusto Query Language)
  - Azure Log Analytics Workspace
  - Azure VM Security Logs
  - Pulsedive Threat Intelligence Integration
  - SecurityAlert, SecurityEvent, Syslog datasets

Investigation Steps
  - Ingested logs from a Windows device into Sentinel 
  - Identify Brute-Force Attempts
  - Used KQL to detect repeated failed authentication attempts
  - Built an Analytic Detection Rule by mapping the indicators to the MITRE ATT&CK framework
  - Generated a ticket, assigned it to a SOC member for investigation. 

Recommendations
  - Enforce MFA for all externally accessible systems.
  - Implement geo-based access restrictions.
  - Add brute-force detection rules with thresholding.
  - Monitor for suspicious command-line activity.
  - Enable Sentinel Fusion correlation for multi-stage attack detection.

Lessons Learned:
  - Configure Cyber Threat Intelligence feeds to enhance detection accuracy, Proactive defence.
  - Configure Sentinel, Log Analytics workspaces, and Analytic rules
  - Using the KQL to filter noise for quick investigation.
  - The value of centralised monitoring.
  - Incident handling workflow
  <h4>For the details, open the pdf attached</h4>

<h2> 🤳 Connect with me:</h2>

[<img align="left" alt="michael-musoke | LinkedIn" width="22px" src="https://cdn.jsdelivr.net/npm/simple-icons@v3/icons/linkedin.svg" />][linkedin]

[linkedin]: https://linkedin.com/in/michael-musoke
