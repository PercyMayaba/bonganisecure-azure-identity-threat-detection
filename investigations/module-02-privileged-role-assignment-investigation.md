# Module 2: Privileged Role Assignment Threat Investigation

## 1. Investigation Objective

Investigate Azure RBAC role assignments to identify unexpected privilege grants, suspicious administrative activity and potential privilege escalation.

## 2. Investigation Environment

- Platform: Microsoft Azure
- Data source: Azure Activity Logs
- Investigation window: Last 90 days
- Analysis method: Azure CLI
- Detection language: Kusto Query Language (KQL)

## 3. Privileged Access Review

The following findings were observed during the role assignment review:

| Role / control | Observation |
|---|---|
| Subscription Owner | User's own account |
| Additional Owner principals | None observed |
| Managed identity Owner assignment | None observed |
| Security Admin | User's account at management-group scope |

The user's Owner and Security Admin assignments are separate permissions with different scopes.

## 4. Role Assignment Creation Investigation

The Azure Activity Log query returned successful `roleAssignments/write` operations.

The caller investigation showed the user's own account as the initiating identity.

No unexpected caller was identified in the inspected results.

## 5. Role Assignment Deletion Investigation

The query for `roleAssignments/delete` returned no matching records within the queried 90-day window.

This does not establish that no deletions occurred outside the available logging scope or retention period.

## 6. Detection Engineering

A KQL detection template was created:

`detections/privileged-role-assignment-detection.kql`

The query identifies successful role assignment write operations in an AzureActivity table.

**Execution status:** Source query committed to GitHub; not executed against a live Log Analytics or Microsoft Sentinel workspace.

## 7. Security Recommendations

1. Monitor successful and failed privileged role assignment operations.
2. Alert on unexpected Owner and User Access Administrator grants.
3. Review role assignment scope before granting permissions.
4. Investigate changes initiated by unfamiliar principals.
5. Maintain a record of approved privileged access changes.
6. Apply least-privilege principles to users and workload identities.
7. Consider privileged identity management capabilities where licensing permits.

## 8. Evidence Limitations

The investigation uses live Azure Activity Log results and role assignment inventory observations.

The available evidence does not independently establish that every role assignment was authorised.

No Sentinel deployment or production alerting configuration is claimed.

## 9. Conclusion

This module demonstrates Azure RBAC inventory review, privileged role assignment investigation, Activity Log analysis and the development of a reusable KQL detection template.
