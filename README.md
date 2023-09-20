# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the Internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
#### NSG Allowed Inbound Malicious Flows
![NSG Allowed Inbound Malicious Flows](https://github.com/kyiez/Azure-SOC/assets/90296943/e8f955ad-e03e-44c2-80f4-43b7bd46fbed)<br>
#### Linux Syslog Auth Failures
![Linux Syslog Auth Failures](https://github.com/kyiez/Azure-SOC/assets/90296943/0d91ad52-4cfe-4172-80d8-38fff54db621)<br>
#### Windows RDP/SMB Auth Failures
![Windows RDP/SMB Auth Failures](https://github.com/kyiez/Azure-SOC/assets/90296943/9d35bb2d-0132-45b9-a025-d669bf45abd0)<br>
#### Microsoft SQL Server Auth Failures
![Microsoft SQL Auth Failures](https://github.com/kyiez/Azure-SOC/assets/90296943/c1be1b13-5822-42db-a33d-a798a7ed54e1)<br>


## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-08-04 20:45
Stop Time 2023-08-05 20:45

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 47595
| Syslog                   | 3147
| SecurityAlert            | 6
| SecurityIncident         | 348
| AzureNetworkAnalytics_CL | 366


## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-08-12 23:52
Stop Time	2023-08-13 23:52

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8349
| Syslog                   | 1
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
