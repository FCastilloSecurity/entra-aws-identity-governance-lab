# Microsoft Entra PIM Privileged Access Validation

## Objective

Validate just-in-time privileged access in Microsoft Entra Privileged Identity Management (PIM) using a synthetic lab identity. The test demonstrates eligible assignment, time-limited activation, least-privilege administration, audit verification, deactivation, and cleanup.

## Lab scenario

A synthetic user, **Maya Chen**, received a time-bound eligible assignment for the **Groups Administrator** role. The role was activated for one hour and used to create a temporary security group named `NorthStar-PIM-Validation`.

All activity occurred in a personal lab tenant with synthetic identities and non-production data.

## Validation workflow

| Stage | Action | Result |
|---|---|---|
| Eligible assignment | Assigned Groups Administrator as eligible for a limited period | User could request activation without receiving permanent standing privilege |
| Activation | Activated the role for one hour through PIM | Groups Administrator appeared as an active, time-limited assignment |
| Controlled action | Created the `NorthStar-PIM-Validation` security group | Group creation succeeded while the role was active |
| Audit verification | Reviewed the Entra audit event for **Add group** | Event showed Success and identified the synthetic user as the initiating actor |
| Least-privilege test | Attempted to open tenant audit logs as the Groups Administrator | Access was denied because the role does not include audit-log reader permissions |
| Deactivation | Deactivated the active Groups Administrator assignment | Temporary privileged access ended before its scheduled expiration |
| Revocation | Removed the eligible role assignment | The user could no longer reactivate the role |
| Cleanup | Deleted the temporary validation group | The environment was returned to its pre-test state |

## Security findings

- PIM replaced permanent administrative access with an eligible, just-in-time assignment.
- The activated role allowed the required group-management task without granting broader directory visibility.
- The audit-log access denial confirmed a meaningful least-privilege boundary.
- Entra audit records provided traceability for assignment, activation, the administrative action, and cleanup.
- Deactivation and removal demonstrated the complete privileged-access lifecycle rather than leaving residual access in the tenant.

## Evidence-handling standard

Public evidence must exclude or redact tenant IDs, object IDs, correlation IDs, session IDs, token identifiers, IP addresses, credentials, recovery information, and other live environment identifiers. Portfolio screenshots should preserve only the control, role, synthetic actor, status, and relevant timestamps needed to support the finding.

## Interview summary

I configured a synthetic user with an eligible Groups Administrator assignment in Microsoft Entra PIM, activated the role for a limited period, performed a controlled group-management action, and verified the action in Entra audit logs. I also confirmed that the role could not read audit logs, which demonstrated least privilege. I then deactivated the role, removed the eligible assignment, and deleted the test group.

## Limitations

- This validation was performed in a controlled portfolio tenant, not a production environment.
- Microsoft Entra ID P2 trial licensing was used for PIM capabilities.
- Risky-user and risky-sign-in behavior was not artificially generated because Identity Protection risk detections are service-driven and should not be fabricated for portfolio evidence.
