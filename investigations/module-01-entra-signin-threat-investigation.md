# Module 1: Entra ID Sign-in Threat Investigation

## 1. Investigation Objective

Investigate Microsoft Entra ID authentication activity to identify failed sign-ins, authentication interruptions, MFA evidence and potential identity security concerns.

## 2. Investigation Environment

- Platform: Microsoft Entra admin centre
- Tenant licence: Microsoft Entra ID Free
- Data source: Entra ID sign-in logs
- Investigation window: Last 7 days
- Evidence type: Live tenant sign-in records

## 3. Successful Sign-in Investigation

A successful sign-in record was examined.

Observed findings:

- Sign-in status: Success
- Conditional Access status: Success
- Policy displayed: Security Defaults
- Grant controls: Empty
- Authentication method: Previously satisfied
- Authentication requirement: Empty
- Additional details: MFA requirement satisfied by claim in the token

### Security interpretation

The successful sign-in contained an MFA claim in the authentication token. This indicates that Entra ID accepted an existing MFA claim. It does not establish that a fresh MFA challenge occurred during that sign-in.

## 4. Failed Sign-in Investigation

A failed sign-in record was examined.

Observed findings:

- Failure reason: Authentication failed due to flow token expired
- Error code: 50089

### Security interpretation

The event is consistent with an expired authentication flow token. Possible explanations include an interrupted authentication process or an expired authentication session.

The event alone does not establish malicious activity.

## 5. Subsequent Authentication

A successful sign-in by the same user to the same application was observed after the failed attempt.

This is consistent with successful recovery from a temporary authentication-flow interruption.

## 6. Identity Risk Visibility

The following risk fields were not available in the inspected sign-in record:

- Risk level during sign-in
- Risk state
- Risk detail

The investigation does not make a conclusion about identity risk based on unavailable fields.

## 7. Findings Summary

| Finding | Result |
|---|---|
| Successful authentication | Observed |
| Failed authentication | Observed |
| Error 50089 | Observed |
| Subsequent successful sign-in | Observed |
| MFA token claim | Observed |
| Fresh MFA challenge | Not established |
| Identity risk fields | Not available |

## 8. Recommended Security Actions

1. Monitor repeated authentication failures.
2. Investigate unusual sign-in patterns and unfamiliar locations.
3. Review authentication methods and Security Defaults configuration.
4. Correlate failed sign-ins with subsequent successful authentication.
5. Investigate unexpected changes to authentication methods.
6. Consider Microsoft Entra ID P2 capabilities where advanced identity risk detection is required.

## 9. Evidence Limitations

This investigation used selected sign-in records from the available tenant.

It does not establish that the tenant experienced an attack or compromise.

Risk fields were unavailable in the inspected record. No paid Identity Protection or Microsoft Sentinel deployment is claimed.

## 10. Conclusion

This investigation demonstrates practical Entra ID sign-in log analysis, authentication failure classification, MFA claim interpretation and evidence-based identity security reporting.
