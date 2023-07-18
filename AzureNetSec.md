# Building a SOC + Honeynet in Azure (w/ Live Traffic)
![image](https://github.com/M4riSec/AzureNetSec/assets/103901296/7a452124-3203-4eb5-8047-69f4605494fd)




## Introduction

This project showcases a honeynet I built in Microsoft Azure where each resource ingests log sources into a Log Analytics workspace. Microsoft Sentinel conducts analysis on this workspace to inform our SIEM on alerts and events of the environment while also generating attack maps for a better understanding of the big picture.
	The environment is setup insecurely, operating for 24 hours generating alerts and incidents. Then, measured to show a baseline of activity.
The environment is hardened by applying security controls and operates another 24 hours to show the impact of running a secure environment.
	The metrics I used are as follows:
- Windows Event Logs (SecurityEvent)
- Linux Event Logs (Syslog)
- Log  Analytics Alerts Triggered (SecurityAlert)
- Incidents created by Sentinel (SecurityIncident)
- Malicious Flows allowed into our Honeynet (AzureNetworkAnalytics_CL)
	

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://github.com/M4riSec/AzureNetSec/assets/103901296/019119d6-0753-40c2-82ae-ad539bf4d3e0)


## Architecture After Hardening / Security Controls
![Architecture Diagram](https://github.com/M4riSec/AzureNetSec/assets/103901296/c11c83fd-833e-4dd7-80fc-5f6f8063207a)


The architecture of the honeynet in Azure consists of the following:

- Virtual Network (VNet)
- Network Security Group/Firewall ![image](https://github.com/M4riSec/AzureNetSec/assets/103901296/2bc35730-9b4d-43b6-8cd3-8aaf4678df38)
- Virtual Machines (1 windows w/ a SQL server, 1 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the Insecure metrics, all resources were deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls blocking zero incoming or outgoing traffic, and all other resources were deployed with public endpoints visible to the Internet.

For the Secure metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoints.

## Attack Maps Before Hardening / Security Controls
### Malicious Traffic through NSG's + Firewalls
![image](https://github.com/M4riSec/AzureNetSec/assets/103901296/50b7ec53-67d9-44cd-a15c-71326b360528)<br>
### Linux Syslog Auth Failures
![image](https://github.com/M4riSec/AzureNetSec/assets/103901296/824bb4b9-6199-494a-ba97-20dec3aa8c3c)<br>
### Windows RDP/SMB Auth Failures
![image](https://github.com/M4riSec/AzureNetSec/assets/103901296/18d29d97-6a85-424a-9e09-c58f6c13d9a1)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-03-15 17:04:29
Stop Time 2023-03-16 17:04:29

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 19470
| Syslog                   | 3028
| SecurityAlert            | 10
| SecurityIncident         | 348
| AzureNetworkAnalytics_CL | 843

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-03-18 15:37
Stop Time	2023-03-19 15:37

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8778
| Syslog                   | 25
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
