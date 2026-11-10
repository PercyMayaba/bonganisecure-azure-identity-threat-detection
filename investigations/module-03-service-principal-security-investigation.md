# Module 3: Service Principal Security Investigation

## 1. Investigation Objective

Assess the security posture of an Azure enterprise application and its associated service principal, focusing on authentication activity, access assignments, permissions, credentials and ownership.

## 2. Investigation Environment

- Platform: Microsoft Entra ID
- Application: BonganiSecure-Finance-Portal-Lab
- Data sources: Enterprise Applications, App registrations and Service principal sign-in logs
- Evidence type: Live tenant configuration observations

## 3. Service Principal Sign-in Investigation

The Service principal sign-in logs returned no results for the selected investigation period and filters.

This does not establish that the service principal is unused or inactive.

## 4. Application Configuration

| Control | Observed result |
|---|---|
| Enabled for users to sign in | Yes |
| Assignment required | Yes |
| Assigned users or groups | None found |
| Admin-consented API permissions | None found |

### Security interpretation

The application is enabled, but assignment is required and no user or group assignments were found.

No admin-consented API permissions were listed during the investigation.

## 5. Credential Security Review

The associated application registration displayed:

| Credential type | Observed count |
|---|---|
| Certificates | 0 |
| Client secrets | 0 |

No certificates or client secrets were listed in the inspected registration.

This does not prove that no other authentication mechanisms or credential paths exist.

## 6. Application Ownership Review

The application registration displayed no assigned owners.

### Security concern

The absence of designated owners creates an application governance and accountability gap.

Recommended action: identify an accountable application owner before retaining the application for operational use.

## 7. Findings Summary

| Finding | Result |
|---|---|
| Service principal sign-in records | No results |
| Application enabled | Yes |
| Assignment required | Yes |
| User/group assignments | None |
| Admin-consented API permissions | None |
| Certificates | 0 |
| Client secrets | 0 |
| Assigned application owners | None |

## 8. Recommended Security Actions

1. Confirm whether the Finance application is still required.
2. Assign an accountable owner if the application is retained.
3. Review the service principal's effective permissions.
4. Investigate any future service principal sign-in activity.
5. Maintain an approved credential lifecycle process.
6. Review application assignments periodically.
7. Decommission the application through an approved process if it is no longer required.

## 9. Evidence Limitations

This assessment reflects the configuration visible during the investigation.

No successful or failed service principal sign-ins were available in the selected log view.

No application compromise is established.

No credentials, permissions or application settings were changed during this investigation.

## 10. Conclusion

This module demonstrates practical Microsoft Entra application identity assessment, service principal monitoring, credential inventory review, access assignment analysis and application ownership governance.
