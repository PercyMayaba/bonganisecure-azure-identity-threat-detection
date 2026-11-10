# BonganiSecure Azure Identity Threat Detection and Response

## Project Overview

This project demonstrates practical Azure identity security monitoring, threat detection, investigation and incident response.

It focuses on identifying suspicious authentication activity, excessive privileges, unexpected role assignments, service principal risks and managed identity security events.

The project combines Microsoft Entra ID, Azure Activity Logs, Azure RBAC and Kusto Query Language (KQL) to develop an identity-focused security operations workflow.

## Project Objectives

- Investigate suspicious Entra ID sign-in activity.
- Detect unusual privileged role assignments.
- Identify potentially risky service principal configurations.
- Monitor managed identity and workload identity activity.
- Develop KQL queries for identity threat detection.
- Document incident investigation and response procedures.

## Technology Stack

- Microsoft Entra ID
- Microsoft Azure
- Azure Activity Logs
- Azure RBAC
- Kusto Query Language (KQL)
- Microsoft Sentinel concepts
- GitHub

## Project Modules

| Module | Description | Status |
|---|---|---|
| 1 | Entra ID sign-in threat investigation | Planned |
| 2 | Privileged role assignment detection | Planned |
| 3 | Service principal security monitoring | Planned |
| 4 | Managed identity threat detection | Planned |
| 5 | KQL detection engineering | Planned |
| 6 | Identity incident response playbooks | Planned |
| 7 | Final security assessment | Planned |

## Evidence and Validation

Real Azure configuration and log evidence will be distinguished from synthetic investigation data.

Simulated attacks will be used where live security events are unavailable. No simulated detection will be presented as a real security incident.

## Cost and Safety Controls

- Prioritise existing Azure subscription resources.
- Avoid unnecessary billable infrastructure.
- Do not expose credentials, secrets or personal identifiers.
- Check licensing and pricing before enabling paid security services.

## Career Relevance

This project develops practical skills relevant to:

- Azure IAM Engineering
- Cloud Security Engineering
- Identity Threat Detection
- Security Operations (SOC)
- Identity Governance and Administration
- Cloud Incident Response

## Project Completion Status

This project developed an Azure identity threat detection and response framework using Microsoft Entra ID, Azure RBAC, managed identities, service principals, Azure Activity Logs and security investigation techniques.

### Completed Modules

1. Entra ID Sign-in Threat Investigation
2. Privileged Role Assignment Threat Investigation
3. Service Principal Security Investigation
4. Managed Identity Threat Investigation
5. Identity Security Monitoring Architecture
6. Advanced Identity Threat Detection
7. Security Operations and Incident Reporting

### Evidence and Implementation Boundaries

The project combines live investigations of the available Azure and Microsoft Entra environment with simulated threat scenarios and detection engineering.

**Live investigation evidence includes:**
- Entra ID sign-in log observations.
- Azure RBAC role and scope reviews.
- Service principal configuration reviews.
- Managed identity configuration and activity investigations.
- Azure Activity Log investigation and verification of Microsoft.ManagedIdentity provider registration.

**Detection engineering includes:**
- KQL detection query templates.
- Privilege escalation and suspicious role assignment scenarios.
- Service principal credential-change monitoring.
- Managed identity privilege-abuse scenarios.
- Identity lifecycle anomaly detection.
- Incident response and reporting procedures.

KQL queries are documented as detection templates and have not been validated through a deployed Microsoft Sentinel or Log Analytics environment. Threat scenarios are simulations and were not executed against the Azure subscription.

### Professional Outcome

This project demonstrates practical understanding of cloud identity security, identity governance, least-privilege RBAC, workload identity, security monitoring, threat investigation and incident response.

It is designed as a portfolio project supporting progression toward Azure IAM Engineering and Azure Cloud Security Engineering roles.
