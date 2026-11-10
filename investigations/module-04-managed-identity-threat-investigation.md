# Module 4: Azure Managed Identity Threat Detection and Investigation

## 1. Executive Summary

This investigation assessed the security posture and operational activity of the Azure user-assigned managed identity `mi-bongani-day1`.

The investigation examined managed identity sign-in activity, Azure RBAC assignments, resource attachments, identity metadata and Azure Activity Log events.

The identity was found to be enabled with Reader permissions at the `BonganiSecurity` resource-group scope. No managed identity sign-in records were returned for the selected 30-day period, and no attached resources were identified by the resource attachment query.

Activity Log investigation identified administrative write operations initiated by the investigating Azure account. Two operations succeeded, two were recorded as started and one failed.

The failed operation contained a `MissingSubscriptionRegistration` error for the `Microsoft.ManagedIdentity` resource provider. A subsequent inspection confirmed that the provider is now registered.

No evidence of malicious activity or identity compromise was established.

## 2. Investigation Scope

**Target identity:** `mi-bongani-day1`

**Identity type:** User-assigned managed identity

**Resource group:** `BonganiSecurity`

**Platform:** Microsoft Azure and Microsoft Entra ID

**Investigation areas:**

- Managed identity authentication activity
- Azure RBAC permissions and scope
- Resource attachment status
- Identity metadata and governance
- Azure management-plane activity
- Failed identity management operations
- Resource-provider registration

**Evidence sources:**

- Microsoft Entra managed identity sign-in logs
- Azure Portal Access Control (IAM)
- Azure Cloud Shell and Azure CLI
- Azure Activity Log
- Azure Resource Provider registration status

## 3. Managed Identity Authentication Investigation

The Managed Identity sign-in logs were inspected for the previous 30 days.

**Observed result:** No results returned.

### Security interpretation

No managed identity authentication records were visible in the selected period.

This does not establish that the identity is unused or compromised. The absence of records may indicate that no authentication occurred during the selected period.

Further investigation would be required to establish whether the identity has an active workload dependency.

## 4. Identity Configuration Assessment

The identity was located in Microsoft Entra ID Enterprise Applications.

| Attribute | Observed result |
|---|---|
| Identity name | mi-bongani-day1 |
| Identity type | User-assigned managed identity |
| Enabled for users to sign in | Yes |
| Resource group | BonganiSecurity |
| Identity exists in Azure | Yes |

The identity was confirmed to exist through Azure CLI resource inventory.

## 5. Azure RBAC Permission Review

The identity's Azure role assignment was inspected through Access Control (IAM).

| Attribute | Observed result |
|---|---|
| Assigned role | Reader |
| Principal | mi-bongani-day1 |
| Principal type | User-assigned managed identity |
| Scope | BonganiSecurity resource group |

### Security interpretation

Reader provides resource visibility without granting general resource modification permissions.

The resource-group scope is narrower than subscription-wide access and is consistent with a least-privilege approach where read-only access is sufficient.

The investigation did not perform an application-level authorization test.

## 6. Managed Identity Resource Attachment Investigation

An Azure CLI query was used to identify resources in `BonganiSecurity` with user-assigned managed identities attached.

**Observed result:** Empty output.

### Security interpretation

No resource attachments were identified by this query.

This is a useful indicator for further investigation, but it does not conclusively establish that the identity is orphaned or unused.

A broader dependency review would be required before considering any lifecycle action.

## 7. Identity Metadata and Governance Review

The identity metadata inspection returned:

| Metadata field | Observed result |
|---|---|
| Identity exists | Yes |
| Created time metadata | Null |
| Tags | Empty object `{}` |
| Location | Available in Azure resource output |
| Principal ID | Present in Azure resource output |

### Security finding

The identity has limited governance metadata.

Missing descriptive tags and ownership information can make it difficult to determine the identity's business purpose, responsible team and lifecycle requirements.

### Recommendations

- Record the identity's workload purpose.
- Assign an accountable owner through an appropriate governance process.
- Apply environment and application tags.
- Document the identity's approved resource dependencies.
- Review the identity periodically for continued business need.

No metadata or ownership changes were made during this investigation.

## 8. Azure Activity Log Investigation

Azure Activity Log records were retrieved for the previous 90 days and filtered for the managed identity resource.

The observed caller was the investigating Azure account.

The investigation identified the following operation statuses:

| Status | Number of records |
|---|---:|
| Succeeded | 2 |
| Started | 2 |
| Failed | 1 |
| Total | 5 |

The operation observed was:

`Microsoft.ManagedIdentity/userAssignedIdentities/write`

### Security interpretation

The write operation represents an Azure management-plane operation involving creation or modification of a user-assigned managed identity.

The observed caller was the investigating Azure account.

These events are administrative management activity. They must not be interpreted as proof of workload authentication.

The Started records were not independently confirmed as completed operations.

## 9. Failed Operation Investigation

One failed write operation was inspected in detail.

| Attribute | Observed result |
|---|---|
| Operation | Microsoft.ManagedIdentity/userAssignedIdentities/write |
| Status | Failed |
| Substatus | Conflict |
| Error code | MissingSubscriptionRegistration |
| Affected provider | Microsoft.ManagedIdentity |

### Error message

`MissingSubscriptionRegistration`

Azure reported that the subscription was not registered to use the `Microsoft.ManagedIdentity` resource provider.

### Root cause interpretation

The failure was associated with missing resource-provider registration at the time of the historical operation.

This is a subscription configuration issue rather than evidence of an identity attack.

The investigation did not establish the exact reason for the original registration gap.

## 10. Resource Provider Registration Verification

The current resource-provider registration state was inspected using Azure CLI.

Command:

```bash
az provider show \
  --namespace Microsoft.ManagedIdentity \
  --query registrationState \
  -o tsv
```

**Observed result:**

`Registered`

### Security interpretation

The resource provider is currently registered.

This provides a useful comparison between the historical failure and the current subscription configuration.

The current registration state does not independently establish the exact time at which registration was completed.

## 11. Security Findings Summary

| Finding | Result |
|---|---|
| Managed identity exists | Confirmed |
| Identity enabled | Yes |
| Managed identity sign-ins | No results in selected 30-day period |
| RBAC role | Reader |
| RBAC scope | BonganiSecurity resource group |
| Attached resources | None identified by query |
| Creation metadata | Null |
| Tags | Empty |
| Activity Log caller | Investigating Azure account |
| Successful write operations | 2 |
| Started write operations | 2 |
| Failed write operations | 1 |
| Failure code | MissingSubscriptionRegistration |
| Current provider registration | Registered |

## 12. Recommended Security Improvements

1. Confirm the business purpose and continued requirement for the managed identity.
2. Identify and document all workload dependencies before considering decommissioning.
3. Maintain Reader access only where read-only access is sufficient.
4. Record an accountable identity owner.
5. Apply meaningful governance tags.
6. Monitor managed identity sign-in activity.
7. Alert on unexpected identity creation, modification and deletion operations.
8. Investigate failed resource-provider and identity management operations.
9. Review managed identity role assignments periodically.
10. Maintain an evidence trail for identity lifecycle changes.

## 13. Evidence Limitations

This assessment reflects the Azure configuration and log records visible during the investigation.

Important limitations:

- No managed identity sign-in records were returned for the selected 30-day period.
- The resource attachment query returned no results within the inspected resource group.
- Activity Log events represent management-plane activity, not workload authentication.
- Started operations were not confirmed as completed.
- The historical provider-registration failure does not establish the exact date of registration.
- No live Sentinel analytics rule or automated alert was deployed.
- No identity permissions, credentials or resource configuration were changed during this investigation.

## 14. Conclusion

This investigation demonstrates practical Azure managed identity security assessment, RBAC least-privilege review, workload identity monitoring, management-plane event analysis and resource-provider troubleshooting.

The investigation identified governance improvements and a historical subscription configuration failure while avoiding unsupported conclusions about malicious activity.

The work provides a foundation for implementing more advanced Azure identity threat detection and incident response controls.
