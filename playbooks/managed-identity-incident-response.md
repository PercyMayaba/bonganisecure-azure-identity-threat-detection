# Managed Identity Security Incident Response Playbook

## 1. Purpose

This playbook defines a structured response process for investigating suspicious Azure managed identity activity.

It focuses on user-assigned and system-assigned managed identities, including unexpected authentication, privilege changes, identity creation and deletion.

## 2. Potential Security Alerts

Investigate alerts involving:

- Unexpected managed identity creation.
- Unexpected identity modification.
- Unauthorised RBAC role assignments.
- Unexpected Contributor or Owner permissions.
- Unusual managed identity authentication activity.
- Unexpected workload resource attachments.
- Repeated failed identity management operations.
- Unexpected managed identity deletion.

## 3. Initial Triage

When an alert is received:

1. Identify the affected managed identity.
2. Record the identity's principal ID and resource ID in the secure incident record.
3. Identify the associated subscription and resource group.
4. Establish the alert timestamp and detection source.
5. Identify the initiating caller, where applicable.
6. Check whether the activity corresponds to an approved change.

Do not assume that unusual activity automatically indicates compromise.

## 4. Authentication Investigation

Inspect Microsoft Entra managed identity sign-in logs.

Review:

- Sign-in timestamp.
- Managed identity name.
- Target resource.
- Source IP address, where available.
- Authentication result.
- Failure details.
- Sign-in frequency and patterns.

Compare observed activity against expected workload behaviour.

Investigate unexpected authentication destinations or unusual activity patterns.

## 5. RBAC Privilege Investigation

Inspect Azure role assignments for the affected identity.

Prioritise investigation of:

- Owner
- Contributor
- User Access Administrator
- Role Based Access Control Administrator
- Custom roles with sensitive permissions

Determine:

- Who assigned the role.
- When the assignment occurred.
- Which scope was affected.
- Whether the assignment was approved.
- Whether the permission is still required.

## 6. Activity Log Investigation

Review Azure Activity Log records for:

- Managed identity write operations.
- Managed identity delete operations.
- Role assignment creation.
- Role assignment deletion.
- Resource attachment changes.
- Failed management operations.

Record the caller, operation, status, timestamp, affected resource and correlation ID.

Distinguish management-plane activity from workload authentication activity.

## 7. Containment Considerations

If compromise is reasonably suspected:

1. Escalate to the appropriate security or cloud operations team.
2. Identify all workloads associated with the identity.
3. Assess the impact of disabling or detaching the identity.
4. Review and restrict unnecessary permissions through an approved change.
5. Consider disabling the identity only after assessing workload dependencies.
6. Preserve relevant logs and evidence.

Do not immediately delete a managed identity. It may disrupt legitimate workloads.

## 8. Eradication and Recovery

Following confirmation of malicious activity:

- Remove unauthorised role assignments.
- Correct inappropriate resource attachments.
- Review affected workload configurations.
- Address the underlying credential or identity compromise.
- Restore only approved permissions.
- Validate workload functionality after remediation.

## 9. Post-Incident Review

Document:

- Incident timeline.
- Root cause.
- Affected identities and resources.
- Permissions involved.
- Actions taken.
- Business impact.
- Lessons learned.
- Preventive recommendations.

## 10. Application to the BonganiSecure Investigation

The investigation of `mi-bongani-day1` identified:

- No managed identity sign-ins in the selected 30-day period.
- Reader access at the BonganiSecurity resource-group scope.
- No attached resources identified by the resource attachment query.
- Empty governance tags.
- Management operations initiated by the investigating Azure account.
- One failed write operation caused by missing subscription resource-provider registration.
- Current `Microsoft.ManagedIdentity` provider status: Registered.

No compromise was established.

The findings were treated as investigation and governance observations, not confirmed security incidents.

## 11. Operational Limitations

This playbook is a documented response procedure.

No automated incident response, Sentinel playbook, identity disabling action or production containment process was deployed as part of this project.
