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



In the Microsoft Sentinel page, click on content management then content hub tab and select Windows Security Events

![image alt](https://github.com/Muts256/SNC-Public/blob/345cc5e9441fc31ef50218313c8e0ffcb9e05b34/Images/Sentinel/Se21.png)

On the Windows security Events page, click install

![image alt](https://github.com/Muts256/SNC-Public/blob/345cc5e9441fc31ef50218313c8e0ffcb9e05b34/Images/Sentinel/Se22.png)

This installs the Azure Monitor Agent

![image alt](https://github.com/Muts256/SNC-Public/blob/345cc5e9441fc31ef50218313c8e0ffcb9e05b34/Images/Sentinel/Se23.png)

Select the Azure Monitor Agent then click on Manage to install the Data Connectors.

![image alt](https://github.com/Muts256/SNC-Public/blob/c230cd4bd37cc83b1030ccc097ef15cd5fd20850/Images/Sentinel/Se24.png)

Next, create a Data Collection Rule. This rule will instruct the agent to collect logs from the Event logs on the selected windows device

In the configuration tab, Click on Create Collection Rule

![image alt](https://github.com/Muts256/SNC-Public/blob/345cc5e9441fc31ef50218313c8e0ffcb9e05b34/Images/Sentinel/Se26.png)

In the Basic tab, fill in the rule name selec the same resource group as in the VM creation

![image alt](https://github.com/Muts256/SNC-Public/blob/345cc5e9441fc31ef50218313c8e0ffcb9e05b34/Images/Sentinel/Se27.png)

In the Resources tab select the VM created earlier

![image alt](https://github.com/Muts256/SNC-Public/blob/345cc5e9441fc31ef50218313c8e0ffcb9e05b34/Images/Sentinel/Se28.png)

Click on Review + create to create the rule.

Navigate to the logs tab in Sentinel. Note: The logs may take a while to be ingested from the VM.

To test if the logs are ingested write to query to display some data

```
SecurityEvent
| where EventID == "4625"

```
This qurey will display all the failed logins that have been recorded.

![image alt](https://github.com/Muts256/SNC-Public/blob/345cc5e9441fc31ef50218313c8e0ffcb9e05b34/Images/Sentinel/Se33.png)

### Plotting the IP address on a Map

To plot the IP address of the general area these IPs are originating from, we need to create a Watchlist.
Click on the Sentinel instance, under configure, click  on Watchlist create a new watchlist using the watchlist wizard

![image alt](https://github.com/Muts256/SNC-Public/blob/345cc5e9441fc31ef50218313c8e0ffcb9e05b34/Images/Sentinel/Se35.png)

Navigate to this GitHub, download the CSV file to your local device
```
https://raw.githubusercontent.com/joshmadakor1/lognpacific-public/refs/heads/main/misc/geoip-summarized.csv 
```
This csv will help in giving a general estimation of the origin/location of the IP Address that will be captured in the event logs.

Import the csv into the watchlist instance. Click on Source then Drag and drop or browse to the location of the csv file. Ensure network is selected in the SearchKey field.

![image alt](https://github.com/Muts256/SNC-Public/blob/345cc5e9441fc31ef50218313c8e0ffcb9e05b34/Images/Sentinel/Se37.png)

Click Review and create.

Wait for the csv file to be imported

![image alt](https://github.com/Muts256/SNC-Public/blob/345cc5e9441fc31ef50218313c8e0ffcb9e05b34/Images/Sentinel/Se39.png)


On Microsoft Sentinel to create a workbook. Click on Add a workbook.

![image alt](https://github.com/Muts256/SNC-Public/blob/944309b8538cfda11bf3315ce5dcb05450fb484d/Images/Sentinel/Se40.png)

On the right hand side click on edit remove all the contents in the workbook.

![image alt](https://github.com/Muts256/SNC-Public/blob/345cc5e9441fc31ef50218313c8e0ffcb9e05b34/Images/Sentinel/Se41.png)

Click on Edit again Give the workbook a title and location. Click on save

![image alt](https://github.com/Muts256/SNC-Public/blob/345cc5e9441fc31ef50218313c8e0ffcb9e05b34/Images/Sentinel/Se42.png)

Click on Open in Azure. Click on edit. Select advanced query. Copy and paste the map.json contents

Navigate to this website 
```
https://drive.google.com/file/d/1ErlVEK5cQjpGyOcu4T02xYy7F31dWuir/view?usp=drive_link
```
copy the map.json content to the advanced query and click done editing 

```
{
        "type": 3,
        "content": {
        "version": "KqlItem/1.0",
        "query": "let GeoIPDB_FULL = _GetWatchlist(\"geoip\");\nlet WindowsEvents = SecurityEvent;\nWindowsEvents | where EventID == 4625\n| order by TimeGenerated desc\n| evaluate ipv4_lookup(GeoIPDB_FULL, IpAddress, network)\n| summarize FailureCount = count() by IpAddress, latitude, longitude, cityname, countryname\n| project FailureCount, AttackerIp = IpAddress, latitude, longitude, city = cityname, country = countryname,\nfriendly_location = strcat(cityname, \" (\", countryname, \")\");",
        "size": 3,
        "timeContext": {
                "durationMs": 2592000000
        },
        "queryType": 0,
        "resourceType": "microsoft.operationalinsights/workspaces",
        "visualization": "map",
        "mapSettings": {
                "locInfo": "LatLong",
                "locInfoColumn": "countryname",
                "latitude": "latitude",
                "longitude": "longitude",
                "sizeSettings": "FailureCount",
                "sizeAggregation": "Sum",
                "opacity": 0.8,
                "labelSettings": "friendly_location",
                "legendMetric": "FailureCount",
                "legendAggregation": "Sum",
                "itemColorSettings": {
                "nodeColorField": "FailureCount",
                "colorAggregation": "Sum",
                "type": "heatmap",
                "heatmapPalette": "greenRed"
                }
        }
        },
        "name": "query - 0"
}
```
A map showing the general area from which the attacking Ips are originating will be displayed.

![image alt](https://github.com/Muts256/SNC-Public/blob/345cc5e9441fc31ef50218313c8e0ffcb9e05b34/Images/Sentinel/Se43.png)

---

### Create a Scheduled Rule

A scheduled rule is an automated query or detection logic that runs at regular time intervals to search for specific patterns, anomalies, or threats in your data

In the Analytics tab, Click creare a new Schedule rule

![image alt](https://github.com/Muts256/SNC-Public/blob/345cc5e9441fc31ef50218313c8e0ffcb9e05b34/Images/Sentinel/Se44.png)

In the MITRE ATT&CK section select all the associated tactics, techniques, and sub-techniques

![image alt](https://github.com/Muts256/SNC-Public/blob/345cc5e9441fc31ef50218313c8e0ffcb9e05b34/Images/Sentinel/Se46.png)

Set the query logic

![image alt](https://github.com/Muts256/SNC-Public/blob/345cc5e9441fc31ef50218313c8e0ffcb9e05b34/Images/Sentinel/Se45.png)


### Query Explained

SecurityEvent
  - This specifies the table you're querying.
  - In Azure Log Analytics, SecurityEvent contains Windows security event logs.
  - These include things like logon attempts, privilege use, account management, etc.

| where EventID == 4625
  - Filters the data to only include rows (events) where EventID is 4625.
  - 4625 is the Windows Event ID for a failed logon attempt (due to a bad password, unknown user, etc.).

| project TimeGenarated, EventID, Computer, IpAddress, Account, LogonType
  - This selects (projects) only the specific columns you want to see in the results:
  - TimeGenerated: When the event was logged

Set how often the rule will run. for testing purposes I set it to every 5 mins

![image alt](https://github.com/Muts256/SNC-Public/blob/0f6a3a603176cb35bc3c93aea247e7915f3e2af3/Images/Sentinel/Se47.png)

Save the rule

Navigate to the incidents tab in MDE. An incident/s should have been created if violation or EventID 4625 was detected

![image alt](https://github.com/Muts256/SNC-Public/blob/345cc5e9441fc31ef50218313c8e0ffcb9e05b34/Images/Sentinel/Se50.png)

Click on the incidents then click on open incident page

![image alt](https://github.com/Muts256/SNC-Public/blob/08d929184427034bd911651411e24c21f5ad6326/Images/Sentinel/Se52.png)

Assign the incident to an analyst

![image alt](https://github.com/Muts256/SNC-Public/blob/345cc5e9441fc31ef50218313c8e0ffcb9e05b34/Images/Sentinel/Se51.png)

The analyst can the Add task and invesigate the incident, Making notes and documenting the actions taken to completion. 

![image alt](https://github.com/Muts256/SNC-Public/blob/cf0772582c5d1cda6ca58fd069f18df54cdac76d/Images/Sentinel/Se56.png)


---

## Threat Intelligence 

**Threat Intelligence (TI)** refers to the collection, analysis, and application of data related to existing and emerging cyber threats. This includes information such as:

- Malicious IP addresses and domains
- Malware hashes
- Attack techniques and tactics
- Threat actor behavior and infrastructure

The primary goal of threat intelligence is to provide **actionable insights** that help security teams anticipate, identify, and respond to threats more effectively.

### Why Threat Intelligence Matters in a SOC

#### 1. Enhanced Detection Accuracy
By enriching alerts and logs with threat intelligence feeds, SOC analysts can determine whether suspicious activity is associated with known malicious actors or infrastructure. This reduces false positives and ensures alerts carry meaningful context.

#### 2. Proactive Defense
Threat intelligence enables SOC teams to stay ahead of attackers by identifying emerging **tactics, techniques, and procedures (TTPs)**, often mapped to frameworks such as **MITRE ATT&CK**. This supports proactive defensive measures before attacks fully materialize.

#### 3. Faster Incident Response
During an incident, threat intelligence provides valuable context around **indicators of compromise (IoCs)**, allowing analysts to quickly prioritize and respond to high-risk threats. For example, identifying an IP address as part of a known botnet can accelerate containment decisions.

#### 4. Strategic Insights
Beyond day-to-day operations, threat intelligence informs long-term security strategy by highlighting adversary groups targeting the organization’s industry, common attack vectors, and potential gaps in defensive controls.

#### 5. Integration with SOC Tooling
Modern **SIEM and SOAR platforms**, such as **Microsoft Sentinel** and **Microsoft Defender XDR**, integrate directly with threat intelligence sources (e.g., **MISP**, **Pulsedive**). This integration ensures real-time threat data enhances automated detection, threat hunting, and response workflows.



### Connenting Threat Intellligence Feeds - Pulsedive

In the Data Connector open content hub in MDE. In the search type Threatintelligence

![image alt](https://github.com/Muts256/SNC-Public/blob/345cc5e9441fc31ef50218313c8e0ffcb9e05b34/Images/Sentinel/Se57.png)

Select ThreatIntelligence (NEW) 

![image alt](https://github.com/Muts256/SNC-Public/blob/345cc5e9441fc31ef50218313c8e0ffcb9e05b34/Images/Sentinel/Se58.png)


Install the Data Connector

![image alt](https://github.com/Muts256/SNC-Public/blob/0c83d8ffbcc2756e0abe8bdf4ff32a1677778634/Images/Sentinel/Se70.png)

When the installation is complete Click on Manage to configure the connectors

![image alt](https://github.com/Muts256/SNC-Public/blob/821b19934fbc53b312f091be320d57a6f96c34d4/Images/Sentinel/Se71.png)

Open an account on the PulseDive offical website
```
https://pulsedive.com/
```

Navigate to the test data data

![image alt](https://github.com/Muts256/SNC-Public/blob/345cc5e9441fc31ef50218313c8e0ffcb9e05b34/Images/Sentinel/Se63.png)

On the same page there will the information that is required to connect to the feed server, this include the username and Password (API Key)

![image alt](https://github.com/Muts256/SNC-Public/blob/345cc5e9441fc31ef50218313c8e0ffcb9e05b34/Images/Sentinel/Se64.png)

On the Threat intelligence page Open connector page

![image alt](https://github.com/Muts256/SNC-Public/blob/fec4b3ea3218e4601e4dc4abe340d400e51b4ddc/Images/Sentinel/Se72.png)

Fill in the required details for configuring the connection

![image alt](https://github.com/Muts256/SNC-Public/blob/345cc5e9441fc31ef50218313c8e0ffcb9e05b34/Images/Sentinel/Se65.png)

If details are correct you should receive notification the the connection is successful

![image alt](https://github.com/Muts256/SNC-Public/blob/61bb14162e0d1e777dc86ce76e03547f1d888ec4/Images/Sentinel/Se73.png)

After a while, data from Pulsedive will be ingested into Sentinel.

![image alt](https://github.com/Muts256/SNC-Public/blob/345cc5e9441fc31ef50218313c8e0ffcb9e05b34/Images/Sentinel/Se68.png)


Pulsedive integration with Microsoft Sentinel enriches security events with real-time threat intelligence on malicious IPs, domains, and indicators. This contextual enrichment improves detection accuracy by reducing false positives and helping analysts quickly identify known threat infrastructure. Automated ingestion of Pulsedive data also strengthens proactive threat hunting and accelerates incident response within the SOC.

---

### Summary of the Investigation Steps:
  - Ingested event logs from a Windows device into Sentinel.
  - Identify Brute-Force Attempts.
  - Used KQL to detect repeated failed authentication attempts.
  - Built an Analytic Detection Rule by mapping the indicators to the MITRE ATT&CK framework.
  - Generated a ticket, assigned it to a SOC member for investigation.
  - Configured additional indicators from Theart Intelligence feeds (PulseDive) for faster and wider detection surface area.

### Recommendations:
  - Enforce MFA for all externally accessible systems.
  - Implement geo-based access restrictions.
  - Add brute-force detection rules with thresholding.
  - Monitor for suspicious command-line activity.
  - Enable Sentinel Fusion correlation for multi-stage attack detection.

### Lessons Learned:
  - Configure Cyber Threat Intelligence feeds to enhance detection accuracy, Proactive defence.
  - Configure Sentinel, Log Analytics workspaces, and Analytic rules for log ingestion and anomaly detection, respectively.
  - Using the KQL to filter noise for quick investigation.
  - The value of centralised monitoring.
  - Incident handling workflow.
  <h4>For the details, open the pdf attached</h4>

<h2> 🤳 Connect with me:</h2>

[<img align="left" alt="michael-musoke | LinkedIn" width="22px" src="https://cdn.jsdelivr.net/npm/simple-icons@v3/icons/linkedin.svg" />][linkedin]

[linkedin]: https://linkedin.com/in/michael-musoke
