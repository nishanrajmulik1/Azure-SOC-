# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://github.com/user-attachments/assets/cc0aeca7-84e1-43d6-a80f-4787cc289b8d)



## Introduction

In this project, I created a mini honeynet in Azure, where I integrated log sources from various resources into a Log Analytics workspace. This workspace was connected to Microsoft Sentinel, enabling the setup of attack maps, alert triggers, and incident creation. Initially, I monitored security metrics in an unsecured environment over a 24-hour period, then implemented security controls to strengthen the environment. I measured the metrics again for another 24 hours and compared the results. The metrics I will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://github.com/user-attachments/assets/776a7cf4-baa5-44fd-be9a-065a48f3271e)


## Architecture After Hardening / Security Controls
![Architecture Diagram](https://github.com/user-attachments/assets/1bea0ff3-b46b-4be3-b238-5caf9585f784)


The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel
  
In the "BEFORE" metrics, all resources were initially deployed and exposed to the internet. The Virtual Machines had their Network Security Groups and built-in firewalls fully open, and all other resources had public endpoints accessible from the internet—meaning no Private Endpoints were implemented.

In the "AFTER" metrics, I hardened the Network Security Groups by blocking all traffic except from my admin workstation. Additionally, all other resources were secured using their built-in firewalls and Private Endpoints, ensuring limited and controlled access.

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://github.com/user-attachments/assets/af6ff754-c5f6-4751-8e3c-b1afa12f3d3c)<br>
![Linux Syslog Auth Failures](https://github.com/user-attachments/assets/4dc9ae4d-c20a-49a8-a60f-912471b9c70b)<br>
![Windows RDP/SMB Auth Failures](https://github.com/user-attachments/assets/802a8afc-25c1-4173-b189-38b9ab351376)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-10-24T09:11:55.959525Z
Stop Time 2024-10-25T09:11:55.959525Z

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 16630
| Syslog                   | 4592
| SecurityAlert            | 2
| SecurityIncident         | 176
| AzureNetworkAnalytics_CL | 8505

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-03-18 15:37
Stop Time	2023-03-19 15:37

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 9789
| Syslog                   | 50
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was set up in Microsoft Azure, where log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was used to trigger alerts and generate incidents based on the collected logs. Security metrics were initially measured in an unsecured environment, then again after implementing security controls. The results showed a significant reduction in security events and incidents post-implementation, highlighting the effectiveness of the applied security measures.

It's important to note that if the resources in the network had been actively used by regular users, there likely would have been a higher number of security events and alerts during the 24-hour period following the implementation of these security controls.
