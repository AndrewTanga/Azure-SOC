# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://github.com/AndrewTanga/Azure-SOC/assets/93886645/1013ced8-8698-4bab-8679-d55eeed50c8a)


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

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint


## Attack Maps Before Hardening / Security Controls
NSG-malicious-allowed-in
![nsg-malicious-allowed-in](https://github.com/AndrewTanga/Azure-SOC/assets/93886645/ce9455d9-12f8-4daa-9752-c22eb2f5238a)
Linux-ssh-auth-fail
![Linux-ssh-auth-fail](https://github.com/AndrewTanga/Azure-SOC/assets/93886645/bcbf1950-9b91-4b1f-b404-deb99d08d192)
Windows-rdp-auth-fail
![windows-rdp-auth-fail](https://github.com/AndrewTanga/Azure-SOC/assets/93886645/492f511d-2843-4617-9278-3aef3a211429)

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-02-21 13:54:26
Stop Time 2024-02-22 13:54:26

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 18971
| Syslog                   | 4943
| SecurityAlert            | 2
| SecurityIncident         | 303
| AzureNetworkAnalytics_CL | 2323

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2024-02-23 22:09:48
Stop Time	2024-02-24 22:09:48

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 6760
| Syslog                   | 0
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

