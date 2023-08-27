# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

During this project, I built a mini honeynet in Azure and ingested log sources from various resources into a Log Analytics workspace, which was then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured various security metrics in the insecure environment for 24 hours, applied security controls to harden the environment, then measured the metrics for an additional 24 hours. The results of the project are displayed below. The metrics shown below are as follows:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

The architecture of the mini honeynet in Azure consisted of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed and exposed to the internet. The Virtual Machines had their Network Security Groups and built-in firewalls wide open, and all other resources were deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic, with the exception of my admin workstation, and all other resources were protected by their built-in firewalls and Private Endpoint.

## Attack Maps Before Hardening / Security Controls<br>
![image](https://github.com/AjayBery/Azure-SOC/assets/143306721/299f4599-8430-414c-8f52-b103e652c957)<br>
![image](https://github.com/AjayBery/Azure-SOC/assets/143306721/43b2934a-f61f-4a94-9ac0-a456288d36f2)<br>
![image](https://github.com/AjayBery/Azure-SOC/assets/143306721/71c954ce-7a3f-4b5a-9b61-08ed34e75cd5)<br>
![image](https://github.com/AjayBery/Azure-SOC/assets/143306721/7c9355ed-52d4-40d3-a077-2445349ce301)<br>



## Metrics Before Hardening / Security Controls

The following table shows the metrics I measured in my insecure environment for 24 hours:<br>
Start Time 2023-08-21 19:31<br>
Stop Time 2023-08-22 19:31<br>

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 59194
| Syslog                   | 2874
| SecurityAlert            | 22
| SecurityIncident         | 376
| AzureNetworkAnalytics_CL | 1511

## Attack Maps Before Hardening / Security Controls

```All map queries returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics I measured in my environment for another 24 hours, AFTER security controls were applied:<br>
Start Time 2023-08-26 01:58<br>
Stop Time	2023-08-27 01:58<br>

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 10624
| Syslog                   | 1
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0


| Metric                                        | Change after security environment
| ----------------------------------------------| ---------------------------------
| Security Events (Windows VM)                  | -82.05%
| Syslog (Linux VMs)                            | -99.97%
| Security Alert (Microsoft Defender for Cloud) | -100.00%
| Security Incident (Sentinel Incidents)        | -100.00%
| NSG Inbound Malicious Flows Allowed           | -100.00%


## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then remeasured after security measures were implemented. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied. This demonstrated their effectiveness.

It is also worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
