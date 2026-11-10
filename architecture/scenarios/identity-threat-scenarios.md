# Advanced Azure Identity Threat Scenarios

## 1. Purpose

This document defines four identity-based attack scenarios for the BonganiSecure Azure identity security engineering portfolio.

The scenarios demonstrate how attackers may exploit excessive permissions, compromised application identities, workload identities and weaknesses in identity lifecycle governance.

All attack scenarios in this document are simulated. They do not represent confirmed attacks against the Azure tenant.

## 2. Scenario One: Azure RBAC Privilege Escalation

### Scenario

An attacker obtains access to a compromised Azure account with permissions to create role assignments.

The attacker attempts to assign the Owner role to another identity at subscription scope.

### Potential Indicators

- Unexpected `Microsoft.Authorization/roleAssignments/write` operations.
- New Owner or Contributor assignments.
- Privilege changes outside approved maintenance windows.
- Unexpected initiating users or service principals.
- Role assignments at broader scopes than required.

### Investigation

1. Identify the initiating principal.
2. Inspect the role definition.
3. Establish the assignment scope.
4. Review the Azure Activity Log.
5. Compare the change with approved administrative activity.

### Recommended Response

Escalate the event, validate authorisation and remove unauthorised assignments through an approved change process.

## 3. Scenario Two: Suspicious Service Principal Credential Changes

### Scenario

An attacker gains access to an application administrator account and attempts to add a new client secret or certificate to a financial application.

The credential could potentially be used to authenticate as the application.

### Potential Indicators

- Unexpected application credential additions.
- New certificates or client secrets.
- Credential changes initiated by unfamiliar administrators.
- Unusual service principal sign-ins after a credential change.
- Unexpected API permissions or consent grants.

### Investigation

1. Identify the affected application registration.
2. Review directory audit logs for credential changes.
3. Identify the initiating administrator.
4. Inspect the credential creation timestamp and expiry.
5. Review subsequent service principal sign-ins.
6. Assess API permissions and application assignments.

### Recommended Response

Validate the credential change with the application owner. If unauthorised, escalate and follow the approved credential revocation and application recovery process.

## 4. Scenario Three: Managed Identity Privilege Abuse

### Scenario

An attacker compromises an Azure workload that has access to a managed identity.

The attacker attempts to use that identity to access additional Azure resources.

### Potential Indicators

- Unexpected managed identity sign-ins.
- Access to unfamiliar target resources.
- Unexpected Contributor or Owner permissions.
- New identity-to-resource associations.
- Unusual resource management operations.

### Investigation

1. Identify the managed identity.
2. Review managed identity sign-in records.
3. Inspect the associated Azure resource.
4. Review the identity's RBAC assignments.
5. Establish whether the target resource is an approved dependency.
6. Inspect relevant Azure Activity Log events.

### Recommended Response

Assess workload dependencies and business impact before restricting access. Remove excessive permissions through an approved change process and investigate the originating workload.

## 5. Scenario Four: Identity Lifecycle Abuse

### Scenario

An attacker creates an unauthorised identity, modifies its permissions and attempts to use it to maintain access to the cloud environment.

### Potential Indicators

- Unexpected user or service principal creation.
- New managed identities.
- Unusual role assignments.
- Unexpected identity enablement.
- Unapproved group membership changes.
- Identity deletion or modification outside approved processes.

### Investigation

1. Establish the identity creation timestamp.
2. Identify the initiating principal.
3. Review the identity's group memberships and role assignments.
4. Inspect application permissions and credentials.
5. Review subsequent authentication activity.
6. Compare the changes against approved identity lifecycle records.

### Recommended Response

Escalate suspicious changes, preserve evidence and coordinate approved containment and recovery actions.

## 6. Financial Services Risk Context

These scenarios are relevant to financial-services organisations because cloud identities may provide access to:

- Finance applications.
- Customer and transaction data.
- Risk management platforms.
- Financial reporting systems.
- Data analytics workloads.
- Cloud infrastructure and administrative services.

Excessive identity permissions or compromised application credentials may increase the risk of unauthorised data access, service disruption and regulatory exposure.

## 7. Detection Engineering Opportunities

| Scenario | Primary evidence source |
|---|---|
| RBAC privilege escalation | Azure Activity Log |
| Service principal credential changes | Entra directory audit logs |
| Managed identity privilege abuse | Managed identity sign-in logs and Azure Activity Log |
| Identity lifecycle abuse | Entra audit logs and Azure Activity Log |

## 8. Evidence and Simulation Boundaries

These scenarios are simulated threat models for security engineering and detection development.

They are not evidence of actual attacks or compromises.

No attack was executed against the Azure subscription, no credentials were created, and no permissions were deliberately escalated.

## 9. Conclusion

These scenarios demonstrate practical understanding of Azure identity attack paths and the evidence required to investigate them.

They provide a foundation for developing advanced KQL detections, incident response procedures and identity security monitoring controls.
