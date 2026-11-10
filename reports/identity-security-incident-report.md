# Module 7: Identity Security Incident Report

## 1. Document purpose

This report establishes a structured incident reporting process for the BonganiSecure Azure Identity Threat Detection and Response project.

It consolidates evidence from Entra ID sign-in investigations, privileged role assignment reviews, service principal monitoring, managed identity investigations and identity threat detection scenarios.

**Evidence classification**

- **Live evidence:** Observations collected directly from the Microsoft Entra admin centre, Azure Portal or Azure Cloud Shell.
- **Detection templates:** KQL queries prepared for future use in Log Analytics or Microsoft Sentinel. These have not been executed or validated against a deployed monitoring workspace.
- **Simulated scenarios:** Hypothetical attack scenarios used to demonstrate detection and incident response design. No attacks were executed.

## 2. Incident identification

| Field | Details |
|---|---|
| Project | BonganiSecure Azure Identity Threat Detection and Response |
| Module | 7 – Security Operations and Incident Reporting |
| Report type | Investigation and operational event assessment |
| Environment | Azure subscription and Microsoft Entra ID Free |
| Incident reference | BS-ID-2026-001 |
| Investigation status | Documented |
| Confirmed security breach | Not established |
| Monitoring platform | Azure Portal, Entra admin centre and Azure Cloud Shell |

## 3. Executive summary

The investigation examined identity-related activity across the Azure environment, including authentication events, privileged role assignments, service principal configuration and managed identity activity.

The investigation identified a historical failed managed identity resource-provider registration operation. The operation returned the error `MissingSubscriptionRegistration`, indicating that the subscription was not registered to use the `Microsoft.ManagedIdentity` resource provider at the time of that operation.

The operation was initiated using the administrator account associated with the environment. A subsequent check confirmed that the provider registration state is now `Registered`.

The available evidence does not establish malicious activity, unauthorised access or compromise. The event is therefore documented as an operational configuration failure rather than a confirmed security incident.

## 4. Evidence and findings

### 4.1 Entra ID sign-in investigation

The investigation reviewed sign-in activity from the available seven-day period.

Findings:

- Successful sign-in records were observed.
- Security Defaults appeared in the Conditional Access information.
- One successful sign-in showed that the MFA requirement was satisfied by a claim in the token.
- A failed sign-in returned error code `50089`, associated with an expired flow token.
- A subsequent successful sign-in by the same user to the same application was observed.
- Risk fields were not available in the inspected sign-in record.

Assessment:

The observed failed sign-in was consistent with an authentication-flow interruption followed by recovery. The available evidence does not establish an account compromise.

### 4.2 Privileged role assignment investigation

Azure role assignment information was reviewed using Azure CLI and Activity Log queries.

Findings:

- The administrator account had an Owner assignment at subscription scope.
- A Security Admin assignment was observed at management-group scope.
- Successful role assignment write events were observed in the reviewed Activity Log period.
- The inspected successful role assignment write events showed the administrator account as the caller.
- No matching role assignment deletion events were returned by the reviewed 90-day query.

Assessment:

The inspected records did not identify an unexpected caller. The absence of matching deletion events in the selected period does not establish that no such activity occurred outside the reviewed period.

### 4.3 Service principal investigation

The investigation reviewed the `BonganiSecure-Finance-Portal-Lab` enterprise application.

Findings:

- The enterprise application was enabled for users to sign in.
- Assignment was required.
- No app assignments were found.
- No admin-consented permissions were listed.
- The Certificates and secrets page showed zero certificates and zero client secrets.
- No owners were assigned.
- No service principal sign-in results were returned for the selected period.

Assessment:

These observations identify governance and accountability considerations for an application that may be retained. The absence of sign-in results does not prove that the service principal has never been used or is necessarily unused.

### 4.4 Managed identity investigation

The investigation reviewed the user-assigned managed identity `mi-bongani-day1`.

Findings:

- No managed identity sign-in results were returned for the selected 30-day period.
- A Reader role assignment was verified at the `BonganiSecurity` resource-group scope.
- The inspected resource-group attachment query returned no resources using the identity.
- Activity Log records showed managed identity resource management operations initiated by the administrator account.
- A historical resource-provider registration operation failed with `MissingSubscriptionRegistration`.
- A later provider registration check returned `Registered`.

Assessment:

The available evidence does not establish whether the identity is actively used by a workload. The resource-group attachment query was limited to that resource group and is not sufficient to prove that the identity is orphaned.

The historical registration failure is classified as an operational configuration issue, not evidence of malicious activity.

## 5. Incident timeline

| Sequence | Event | Assessment |
|---|---|---|
| 1 | Managed identity write operation initiated | Azure management activity observed |
| 2 | Resource-provider registration failure returned | Operational configuration failure |
| 3 | Further managed identity management activity observed | Administrator-initiated activity |
| 4 | Provider registration state checked | Current state returned `Registered` |
| 5 | Identity RBAC scope reviewed | Reader assignment verified at resource-group scope |

Exact timestamps should be taken from the relevant Azure Activity Log records when producing a case-specific operational record. This report does not invent timestamps.

## 6. Severity and impact assessment

**Provisional classification:** Low – operational configuration issue.

| Assessment area | Finding |
|---|---|
| Confirmed unauthorised access | Not established |
| Confirmed data exposure | Not established |
| Confirmed privilege escalation | Not established |
| Confirmed workload compromise | Not established |
| Service disruption | A resource-provider operation failed |
| Current provider registration | Registered |
| Business impact | No material business impact established from the available evidence |

This classification applies to the observed registration failure only. A different severity would be required if further evidence demonstrated unauthorised access, privilege escalation or service disruption.

## 7. Response and remediation recommendations

The following actions are recommended as operational controls. They are not claims that automated remediation has been deployed.

1. Maintain the current registered state of the `Microsoft.ManagedIdentity` provider.
2. Review whether `mi-bongani-day1` is still required by an Azure workload.
3. Identify and document the business or technical owner of each retained service principal and managed identity.
4. Confirm that managed identity permissions remain restricted to the required resource scope.
5. Investigate unexpected privileged role assignment changes promptly.
6. Review failed and unusual sign-in activity alongside authentication and audit evidence.
7. Validate service principal credential changes against an approved change record.
8. Preserve relevant Activity Log and Entra audit evidence for investigation.
9. Escalate suspected compromise through the organisation's incident response process.

No identity deletion, credential rotation, account disabling or role removal is claimed as part of this Module 7 report.

## 8. Detection and monitoring strategy

The project contains KQL starter templates for:

- Privileged Azure RBAC role assignment changes.
- Service principal credential modifications.
- Managed identity resource and permission changes.
- Unusual identity lifecycle activity.
- Entra ID service principal sign-ins.

These queries are intended for future validation in an appropriately configured Log Analytics workspace or Microsoft Sentinel environment.

**Deployment limitation:** Microsoft Sentinel and a centralised Log Analytics monitoring environment have not been deployed as part of this project. The queries are not represented as tested production detections.

## 9. Lessons learned

The investigation demonstrates several important identity security principles:

- Successful authentication does not necessarily mean a fresh MFA challenge occurred.
- A failed sign-in should be assessed in context rather than automatically treated as an attack.
- Azure Activity Log management operations must be distinguished from workload authentication.
- A managed identity with no sign-in results should not automatically be classified as unused.
- RBAC assignments should be reviewed for both the assigned role and its scope.
- Service principals require ownership, permission and credential governance.
- Detection queries require validation against the actual data schema before operational deployment.
- Incident reports must distinguish observed facts, analyst interpretation and unverified hypotheses.

## 10. Evidence limitations

This investigation has the following limitations:

- Entra ID Free does not provide all advanced identity protection and risk investigation capabilities.
- Some inspected sign-in and service principal log views returned no records for the selected periods.
- Azure Activity Log findings are limited to the available retention and query windows.
- The managed identity attachment check was limited to the `BonganiSecurity` resource group.
- KQL detection templates have not been executed in Microsoft Sentinel or Log Analytics.
- The attack scenarios in Module 6 are simulated and have not been executed against the Azure subscription.
- No automated containment or incident response workflow has been deployed.

These limitations should be considered before using the project as a production security monitoring design.

## 11. Final conclusion

The Module 7 investigation consolidates the identity security evidence collected during the project and demonstrates a structured approach to incident classification, evidence assessment, severity determination, remediation planning and reporting.

The investigation identified an operational managed identity resource-provider registration failure that was subsequently followed by a successful registration-state check.

No confirmed identity compromise or malicious activity was established by the evidence reviewed.

The project demonstrates practical exposure to Microsoft Entra ID investigations, Azure RBAC analysis, managed identity governance, service principal security reviews, KQL detection engineering and incident response documentation.

**Project status:** Module 7 report prepared. Live evidence and simulated detection content are explicitly distinguished.

**Environment:** BonganiSecure Azure security lab.

**Classification:** Portfolio and training documentation. Not a production incident record.
