# Azure Identity Threat Detection Architecture

## 1. Architecture Overview

The BonganiSecure identity threat detection architecture monitors identity-related security activity across Microsoft Entra ID and Azure resources.

The architecture follows five stages:

1. Identity activity generation.
2. Security log collection.
3. Threat detection and analysis.
4. Incident investigation.
5. Response and remediation.

## 2. Identity Activity Sources

### Microsoft Entra ID
- User sign-in activity.
- Authentication failures.
- MFA-related events.
- Privileged directory role changes.
- Service principal activity.

### Azure Resource Manager
- Azure RBAC role assignments.
- Managed identity configuration changes.
- Resource permission modifications.
- Administrative operations.

## 3. Monitoring and Detection Layer

The project uses:

- Entra ID sign-in logs.
- Azure Activity Logs.
- Azure CLI investigations.
- Kusto Query Language (KQL).
- Microsoft Sentinel detection concepts.

Actual log availability depends on subscription configuration, licensing and diagnostic settings.

## 4. Threat Detection Scenarios

| Scenario | Detection objective |
|---|---|
| Repeated sign-in failures | Identify possible password attacks |
| Unusual privileged role assignment | Detect potential privilege escalation |
| Service principal changes | Identify unexpected application identity modifications |
| Managed identity role changes | Detect excessive or unexpected workload permissions |
| Authentication anomalies | Identify suspicious identity activity |

## 5. Incident Response Workflow

1. Identify the alert or suspicious event.
2. Validate the event using available logs.
3. Establish the incident timeline.
4. Identify affected identities and resources.
5. Assess the potential impact.
6. Recommend containment and remediation.
7. Document findings and lessons learned.

## 6. Evidence Classification

**Live evidence:** Events and configurations directly verified in the Azure or Entra environment.

**Simulated evidence:** Synthetic security events used to demonstrate attack detection and investigation.

**Planned integrations:** Future monitoring capabilities not yet deployed.

## 7. Security Boundaries

This project does not claim that Microsoft Sentinel or paid monitoring services have been deployed.

Detection examples and architecture components will be labelled according to their actual implementation status.
