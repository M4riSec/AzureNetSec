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

## Metrics Before Hardening Environment

The following table shows the metrics we measured in our insecure environment for 24 hours:<br>
>Start Time 2023-07-11 21:13:29<br>
>Stop Time 2023-07-12 21:13:29

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 127111
| Syslog                   | 1263
| SecurityAlert            | 23
| SecurityIncident         | 193
| AzureNetworkAnalytics_CL | 2360

## Attack Maps After Hardening / Security Controls

```All map queries returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:<br>
>Start Time 2023-07-15 01:28:25<br>
>Stop Time 2023-07-16 01:28:25

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 582
| Syslog                   | 24
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a honeynet was engineered in Microsoft Azure and log sources were aggregated into a Log Analytics workspace. Microsoft Sentinel was activated to trigger alerts and generate incidents based on the ingested logs. Additionally, activity was measured in the insecure environment before security controls were applied, and then again after implementing security controls. It is noteworthy that the number of security events and incidents were reduced by a minimum of 98% after the security controls were established, demonstrating their effectiveness.

It's also noteworthy that if the resources within the network were heavily utilized by regular users, more security events and alerts would have been generated within the same secure 24-hour period following the implementation.
