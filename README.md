<h1>Hi, I'm Michael! <br/><a href="https://www.linkedin.com/in/michael-musoke/">Cybersecurity Professional</a></h1>

<h2>👨‍💻 Sentinel Configuration and Investigation</h2>

- <b> Sentinel </b>
  - [Sentinel](https://github.com/Muts256/Sentinel)

Simulation of a Security Operations Center (SOC) environment by deploying a honeynet in Microsoft Azure, forwarding security logs into Microsoft Sentinel, and detecting a brute force attack in real time. The project demonstrates the end-to-end SOC workflow, including log ingestion, threat detection using KQL queries, incident creation and investigation, and enrichment with external threat intelligence sources such as Pulsedive.

## Summary:
  - Objective: Investigate a simulated data breach using Microsoft Sentinel and KQL.
  - Tools: Microsoft Sentinel, Log Analytics Workspace, KQL, Azure VM logs.
  - Focus: Identify anomalies, trace attacker behaviour, reconstruct timeline.
  - Key Skills Demonstrated: Log analysis, incident investigation, IOC correlation, threat hunting logic.
  - Outcome: Discovered Brute force indicators and developed analytic queries for detection.

### Tools & Technologies:
  - Microsoft Sentinel.
  - KQL (Kusto Query Language).
  - Azure Log Analytics Workspace.
  - Azure VM Security Logs.
  - Pulsedive Threat Intelligence Integration.
  - SecurityAlert, SecurityEvent, Syslog datasets.

## Sentinel Configuration

### Topology

![image alt](https://github.com/Muts256/SNC-Public/blob/345cc5e9441fc31ef50218313c8e0ffcb9e05b34/Images/Sentinel/Se1.png)

### What is Sentinel?
Microsoft Sentinel is a cloud-native Security Information Event Management (SIEM) system. It collects, normalises, and analyses security logs/events from across your environment (On-prem, Cloud, third party)
Sentinel is also a Security Orchestration Automation and Response (SOAR). It automates responses with playbooks, so incidents can be contained or remediated quickly without manual effort.

### Create a Resource group (RG)
In an Azure account under a subscription, create a resource group. A resource group is a logical container that holds related Azure resources.

![image alt](https://github.com/Muts256/SNC-Public/blob/345cc5e9441fc31ef50218313c8e0ffcb9e05b34/Images/Sentinel/Se2.png)

### Create a Virtual Network (Vnet)

A Vnet is a logically isolated network inside Azure where you can securely run and connect Azure resources. Works the same way as an on-premises network but hosted in Azure.
Ensure the VNet is created under the same resource group and region created in the previous create resource group step

![image alt](https://github.com/Muts256/SNC-Public/blob/345cc5e9441fc31ef50218313c8e0ffcb9e05b34/Images/Sentinel/Se3.png)

### Create a Virtual Machine (VM)
This is the VM that will intentionally be left unprotected to entice attackers, i.e. the Honeypot. Search for Virtual Machine and click on Virtual Machines to begin creating it.
Create the VM in the resource group and region as the RG and Vnet


Fill in the required details. In the size use at least 2vcpus. Anything less will be painfully slow. Ensure to confirm you have a license; otherwise, the process will fail

![image alt](https://github.com/Muts256/SNC-Public/blob/345cc5e9441fc31ef50218313c8e0ffcb9e05b34/Images/Sentinel/Se5.png)

Click Create when the validation of the VM is complete.

Once the VM has been deployed. Navigate to the Network Security Group (NSG) and create a rule that will allow any traffic from any source

![image alt](https://github.com/Muts256/SNC-Public/blob/345cc5e9441fc31ef50218313c8e0ffcb9e05b34/Images/Sentinel/Se10.png)


Log in to the VM using RDP. Turn off the firewall.

![image alt](https://github.com/Muts256/SNC-Public/blob/345cc5e9441fc31ef50218313c8e0ffcb9e05b34/Images/Sentinel/Se12.png)

Test that ICMP are being responded to 

![image alt](https://github.com/Muts256/SNC-Public/blob/345cc5e9441fc31ef50218313c8e0ffcb9e05b34/Images/Sentinel/Se14.png)

In Azure, in the search type Log Analytics to create a workspace. This is a requirement for Sentinel log ingestion. Create the workspace and add to Sentinel

![image alt](https://github.com/Muts256/SNC-Public/blob/345cc5e9441fc31ef50218313c8e0ffcb9e05b34/Images/Sentinel/Se19.png)



In the Microsoft Sentinel page, click on the content hub tab and select Windows Security Events

![image alt](https://github.com/Muts256/SNC-Public/blob/345cc5e9441fc31ef50218313c8e0ffcb9e05b34/Images/Sentinel/Se21.png)

On the Windows security Events page, click install

![image alt](https://github.com/Muts256/SNC-Public/blob/345cc5e9441fc31ef50218313c8e0ffcb9e05b34/Images/Sentinel/Se22.png)

This installs the Azure Monitor Agent

![image alt](https://github.com/Muts256/SNC-Public/blob/345cc5e9441fc31ef50218313c8e0ffcb9e05b34/Images/Sentinel/Se23.png)

Select the Azure Monitor Agent then click on Manage to install the Data Connectors.

![image alt](https://github.com/Muts256/SNC-Public/blob/c230cd4bd37cc83b1030ccc097ef15cd5fd20850/Images/Sentinel/Se24.png)






### Investigation Steps:
  - Ingested event logs from a Windows device into Sentinel.
  - Identify Brute-Force Attempts.
  - Used KQL to detect repeated failed authentication attempts.
  - Built an Analytic Detection Rule by mapping the indicators to the MITRE ATT&CK framework.
  - Generated a ticket, assigned it to a SOC member for investigation.
  - Configured additional indicators from Theart Intelligence feeds (PulseDive) for faster and wider detection surface area.



























Recommendations:
  - Enforce MFA for all externally accessible systems.
  - Implement geo-based access restrictions.
  - Add brute-force detection rules with thresholding.
  - Monitor for suspicious command-line activity.
  - Enable Sentinel Fusion correlation for multi-stage attack detection.

Lessons Learned:
  - Configure Cyber Threat Intelligence feeds to enhance detection accuracy, Proactive defence.
  - Configure Sentinel, Log Analytics workspaces, and Analytic rules for log ingestion and anomaly detection, respectively.
  - Using the KQL to filter noise for quick investigation.
  - The value of centralised monitoring.
  - Incident handling workflow.
  <h4>For the details, open the pdf attached</h4>

<h2> 🤳 Connect with me:</h2>

[<img align="left" alt="michael-musoke | LinkedIn" width="22px" src="https://cdn.jsdelivr.net/npm/simple-icons@v3/icons/linkedin.svg" />][linkedin]

[linkedin]: https://linkedin.com/in/michael-musoke
