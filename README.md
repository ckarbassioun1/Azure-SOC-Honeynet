# Building a SOC + Honeynet in Azure (Live Traffic)
![Final Diagram](https://github.com/user-attachments/assets/652d0bdf-a08f-41cd-b94e-1920a17d16c2)



## Introduction

In this project, I built a mini honeynet in Azure and ingested log sources from various resources into a Log Analytics workspace. This central repository (Log Analytics Workspace) is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, applied security controls to harden the environment, measured metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture before image](https://github.com/user-attachments/assets/33286752-9a7b-4e20-b771-5ae134681a7e)


## Architecture After Hardening / Security Controls
![Architecture after hardening](https://github.com/user-attachments/assets/e5fe1ea6-4d96-44a1-b973-dff095359600)


The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint.

## Attack Maps Before Hardening / Security Controls
![Before Screenshot for nsg-malicious-allowed-in workbook map](https://github.com/user-attachments/assets/16817a1c-ef03-489c-93b2-3e46639b3be8)
![Before Screenshot for linux-ssh-auth-fail workbook map](https://github.com/user-attachments/assets/fc88341a-8dbd-4f29-a971-e20a3b4c5e1e)
![Before Screenshot for windows-rdp-auth-fail workbook map](https://github.com/user-attachments/assets/7d897488-ea07-4de5-b0f9-827e292e9f39)


## Metrics Before Hardening / Security Controls

The following table shows the metrics I measured in the insecure environment for 24 hours:

Start Time 2024-08-06 03:22:15 -
Stop Time 2024-08-07 03:43:29

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 3053
| Syslog                   | 1528
| SecurityAlert            | 10
| SecurityIncident         | 116
| AzureNetworkAnalytics_CL | 263

## Attack Maps After Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics I measured in the secure environment for another 24 hours, after we have applied security controls:

Start Time 2024-08-08 15:05:46 -
Stop Time	2024-08-09 15:37:12

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 158
| Syslog                   | 17
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
