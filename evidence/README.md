# Evidence Checklist

Store only sanitized screenshots in this directory.

## Recommended evidence files

| Filename | Evidence represented |
|---|---|
| `01-scim-cycle-success.png` | Successful automated provisioning cycle |
| `02-aws-users-created-by-scim.png` | Enabled AWS identities created by SCIM |
| `03-aws-groups-created-by-scim.png` | Provisioned groups and membership counts |
| `04-permission-set-assignments.png` | Group-to-permission-set mapping |
| `05-readonly-portal-role.png` | Read-only role visible in AWS access portal |
| `06-readonly-denied-create-user.png` | Expected authorization denial |
| `07-joiner-provisioned.png` | Joiner identity and group assignment |
| `08-mover-role-changed.png` | Mover role after new sign-in |
| `09-leaver-disabled.png` | Disabled identity with memberships removed |
| `10-ca-what-if-pilot.png` | Pilot MFA policy impact |
| `11-ca-emergency-exclusion.png` | Emergency account exclusion |
| `12-ca-mfa-enforced.png` | Enforced AWS MFA policy success |

## Redaction requirements

Before uploading an image, remove or cover:

- Personal and administrative email addresses
- Tenant and verified-domain names
- AWS account and organization identifiers
- Identity Center instance, store, and portal identifiers
- Application, object, policy, correlation, and request identifiers
- IP addresses, precise locations, and timestamps when unnecessary
- Assumed-role ARNs
- Billing, payment, address, and subscription details
- Authentication QR codes, secrets, tokens, and recovery information

Use opaque redaction boxes rather than blur when possible. Crop screenshots to the smallest area that still proves the control or result.
