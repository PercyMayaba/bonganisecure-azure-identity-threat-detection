# BonganiSecure Identity Security Monitoring Framework

## 1. Overview

This framework defines a centralised identity security monitoring architecture for Azure and Microsoft Entra ID environments.

It brings together identity authentication monitoring, privileged access monitoring, service principal security and managed identity threat detection.

The design uses a financial-services environment as its reference scenario.

## 2. Monitoring Objectives

The framework aims to:

- Detect suspicious authentication patterns.
- Identify unexpected privileged role assignments.
- Monitor service principal authentication activity.
- Detect unexpected managed identity modifications.
- Identify excessive permissions and governance weaknesses.
- Support structured security incident investigation and response.

## 3. Monitoring Architecture

### Identity Sources

**Microsoft Entra ID**
- User sign-in logs.
- Service principal sign-in logs.
- Managed identity sign-in logs.
- Directory audit logs.
- Privileged role changes.

**Azure Resource Manager**
- Azure Activity Log.
- RBAC role assignments.
- Managed identity lifecycle operations.
- Resource configuration changes.

### Centralised Monitoring Layer

Proposed integration:

- Azure Monitor.
- Log Analytics workspace.
- Microsoft Sentinel.

These services provide a potential central location for collecting, querying and analysing identity security events.

They are proposed architectural components, not deployed components of this project.

### Detection Layer

KQL detection templates cover:

- User authentication anomalies.
- Privileged role assignment changes.
- Service principal sign-in activity.
- Managed identity creation, modification and deletion.

### Response Layer

Security analysts use documented investigation and incident response procedures to:

- Validate suspicious events.
- Identify affected identities.
- Assess permissions and business impact.
- Escalate confirmed incidents.
- Recommend containment and remediation.

## 4. Financial Services Security Scenario

The reference organisation is BonganiSecure Financial Services.

Identity monitoring focuses on access to:

- Finance applications.
- Risk management systems.
- Financial reporting workloads.
- Cloud infrastructure.
- Privileged administration functions.

The design recognises that financial services environments require strong identity governance, least privilege, traceable administration and reliable audit evidence.

## 5. Threat Detection Use Cases

| Use case | Security objective |
|---|---|
| Unusual user sign-in | Identify potentially suspicious authentication |
| Privileged role assignment | Detect unexpected administrative privilege changes |
| Service principal authentication | Monitor application identity activity |
| Managed identity modification | Detect unexpected workload identity changes |
| Excessive RBAC permissions | Identify unnecessary privilege |
| Identity lifecycle changes | Monitor creation, disabling and deletion |

## 6. Evidence and Investigation

The monitoring framework uses the following evidence sources:

| Evidence source | Investigation purpose |
|---|---|
| Entra user sign-in logs | Authentication investigation |
| Service principal sign-in logs | Application identity monitoring |
| Managed identity sign-in logs | Workload identity monitoring |
| Azure Activity Log | Subscription management activity |
| Azure RBAC assignments | Privilege and scope review |
| Directory audit logs | Directory and identity changes |

## 7. Detection Engineering

KQL is used to query collected security events.

Detection logic should:

1. Identify relevant event types.
2. Apply a defined investigation window.
3. Extract the initiating identity.
4. Identify the affected resource.
5. Distinguish successful and failed operations.
6. Produce actionable investigation fields.
7. Support incident escalation.

Detection rules should be tested against representative events before production deployment.

## 8. Incident Response Workflow

1. Alert generation.
2. Initial triage.
3. Identity and resource identification.
4. Authentication and privilege investigation.
5. Evidence preservation.
6. Risk assessment.
7. Approved containment.
8. Recovery and validation.
9. Post-incident reporting.

## 9. Architecture Security Principles

The framework follows these principles:

- Least privilege.
- Zero Trust.
- Identity-first security.
- Separation of administrative responsibilities.
- Continuous monitoring.
- Evidence-based investigation.
- Controlled incident response.
- Application and workload identity governance.

## 10. Implementation Status

### Completed and evidenced

- Microsoft Entra user sign-in investigation.
- Privileged role assignment investigation.
- Service principal security investigation.
- Managed identity security investigation.
- Azure RBAC scope assessment.
- KQL detection templates.
- Incident response documentation.

### Planned integrations

- Log Analytics workspace.
- Microsoft Sentinel.
- Automated analytics rules.
- Automated incident creation.
- Security orchestration and response.

No production SIEM or automated response capability is claimed as deployed.

## 11. Conclusion

This framework provides an architectural foundation for identity-centric security monitoring in Azure.

It combines practical Entra ID and Azure investigations with detection engineering, RBAC governance and incident response procedures.

The design demonstrates an understanding of how identity security monitoring supports the protection of financial-services cloud environments.
