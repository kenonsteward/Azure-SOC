# Building a SOC + Honeynet in Azure (Live Traffic)
<!--
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)
-->
![Cloud Honeynet / SOC Diagram](https://github.com/kenonsteward/Azure-SOC/assets/90296943/4579a2e9-627b-4107-a4cd-cd434f363f3e)



## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics I will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
<!--
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)
-->
![Architechture Before Hardening](https://github.com/kenonsteward/Azure-SOC/assets/90296943/79dabb5f-f278-43ce-971a-09bffeb1d701)






## Architecture After Hardening / Security Controls
<!--
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)
-->
![Architechture After Hardening _ Security Controls](https://github.com/kenonsteward/Azure-SOC/assets/90296943/6a7f5aa0-2d0e-40dd-aa5d-1ab3209d732a)




The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed and exposed to the Internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources were deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoints.

## Attack Maps Before Hardening / Security Controls
#### NSG Allowed Inbound Malicious Flows
![NSG Allowed Inbound Malicious Flows](https://github.com/kenonsteward/Azure-SOC/assets/90296943/78074ddf-fcc6-412c-90c9-1de1f01a23fc)<br>

#### Linux Syslog Auth Failures
![Linux Syslog Auth Failures](https://github.com/kenonsteward/Azure-SOC/assets/90296943/1f10a9b4-f898-4cb7-afb6-a75ae70accce)<br>

#### Windows RDP/SMB Auth Failures
![Windows RDP/SMB Auth Failures](https://github.com/kenonsteward/Azure-SOC/assets/90296943/a18f5261-b057-497b-ab90-8b5c23e0855f)<br>

#### Microsoft SQL Server Auth Failures
![Microsoft SQL Auth Failures](https://github.com/kenonsteward/Azure-SOC/assets/90296943/57261866-9461-463c-989c-b80e1b4072db)<br>



## Metrics Before Hardening / Security Controls

The following table shows the metrics I measured in my insecure environment for 24 hours:

Start Time 2023-08-04 20:45

Stop Time 2023-08-05 20:45

| Metric                                                         | Count
| ------------------------                                       | -----
| SecurityEvent (Windows VMs)                                    | 47595
| Syslog (Linux VMs)                                             | 3147
| SecurityAlert (Windows Defender for Cloud                      | 6
| SecurityIncident (Sentinel Incidents)                          | 348
| AzureNetworkAnalytics_CL (NSG Inbound Malicious Flows Allowed) | 366


## Attack Maps After Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```


## Metrics After Hardening / Security Controls

The following table shows the metrics I measured in my environment for another 24 hours, but after having applied security controls:

Start Time 2023-08-12 23:52

Stop Time 2023-08-13 23:52

| Metric                                                         | Count  | Change after securing environment  
| ------------------------                                       | -----  | ---- 
| SecurityEvent (Windows VMs)                                    | 8349   | -82.46%
| Syslog (Linux VMs)                                             | 1      | -99.97%
| SecurityAlert (Windows Defender for Cloud                      | 0      | -100.00%
| SecurityIncident (Sentinel Incidents)                          | 0      | -100.00%
| AzureNetworkAnalytics_CL (NSG Inbound Malicious Flows Allowed) | 0      | -100.00%

<!--
|                          | Change after securing environment
| ------------------------ | -----
| Security Events (Windows VMs)            | -82.46%
| Syslog (Linux VMs)                   | -99.97%
| SecurityAlert (Microsoft Defender for Cloud)            | -100.00%
| Security Incident (Sentinel Incidents)         | -100.00%
| NSG Inbound Malicious Flows Allowed | -100.00%
-->

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
