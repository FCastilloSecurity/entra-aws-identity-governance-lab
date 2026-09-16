# Test Results

| Test ID | Scenario | Expected result | Observed result | Status |
|---|---|---|---|---|
| T01 | SAML sign-in as read-only user | AWS portal opens with `ReadOnlyAccess` only | Expected role displayed | Pass |
| T02 | Read IAM resources | Read operation succeeds | IAM page accessible | Pass |
| T03 | Create IAM user as read-only user | Authorization denied | `iam:CreateUser` denied; no user created | Pass |
| T04 | SAML sign-in as support user | `SupportUser` displayed | Expected role displayed | Pass |
| T05 | SCIM user provisioning | Assigned user created in AWS | Identity created by SCIM | Pass |
| T06 | SCIM group provisioning | Assigned groups created with membership | Groups and users synchronized | Pass |
| T07 | Joiner workflow | New user receives read-only access | User provisioned and added to group | Pass |
| T08 | Mover workflow | Support access removed; read-only retained | New session showed read-only only | Pass |
| T09 | Leaver workflow | User disabled and memberships removed | AWS user disabled and retained for audit | Pass |
| T10 | AWS MFA policy What If | Pilot policy applies to pilot user | MFA policy returned report-only impact | Pass |
| T11 | Emergency What If | MFA policies excluded | No policies returned | Pass |
| T12 | Enforced AWS MFA | AWS sign-in requires MFA | MFA policy evaluated successfully | Pass |
| T13 | Enforced administrator MFA | Global Administrator sign-in requires MFA | Authenticator challenge completed | Pass |

## Session Note

Existing AWS STS sessions can retain previously issued permissions until session expiration. Mover validation therefore used a new browser session to verify the new effective role.
