# Building a SOC + Honeynet in Azure (Live Traffic)
![Building a SOC + Honeynet in Azure (Live Traffic)](https://github.com/mquijivix/Azure-SOC--Honeynet/assets/173574799/ce2cc6b6-e44c-4deb-aef0-f23b46f11b0f)


## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Before Hardening : Security Controls](https://github.com/mquijivix/Azure-SOC--Honeynet/assets/173574799/727867e4-63b9-43f0-b151-b9f450c4f1d5)


## Architecture After Hardening / Security Controls
![Architecture After Hardening : Security Controls](https://github.com/mquijivix/Azure-SOC--Honeynet/assets/173574799/a6e6e675-bd58-40b4-942d-68250d8461e3)


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
![BEFORE_NSG_Malicious_Allowed](https://github.com/mquijivix/Azure-SOC--Honeynet/assets/173574799/305b4d0c-55e8-4246-88c4-fa8bb953b63d)

![BEFORE_linux_ssh_auth_fail](https://github.com/mquijivix/Azure-SOC--Honeynet/assets/173574799/5f3e8e67-fe09-4d44-8723-8f8669e4b263)

![BEFORE_windows_rdp_auth_fail](https://github.com/mquijivix/Azure-SOC--Honeynet/assets/173574799/016b9f27-00a4-433d-8abb-cc2f63a62bf5)


## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-06-14 23:45:38,
Stop Time  2024-06-15 23:45:38

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 17367
| Syslog                   | 6016
| SecurityAlert            | 2
| SecurityIncident         | 222
| AzureNetworkAnalytics_CL | 1090

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2024-06-17 08:54.64,
Stop Time 2024-06-18 08:54.64

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 4586
| Syslog                   | 1
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
