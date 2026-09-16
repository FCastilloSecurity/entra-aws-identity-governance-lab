# Troubleshooting Notes

## SCIM email attribute was empty

**Symptom:** Provisioning could not populate the AWS work-email attribute reliably.

**Cause:** The synthetic cloud-only users had no Exchange-backed `mail` value.

**Resolution:** Updated the SCIM mapping to use `userPrincipalName` for `emails[type eq "work"].value`.

**Lesson:** Validate source attributes before assuming default gallery mappings fit cloud-only identities.

## Groups were unavailable for enterprise-application assignment

**Symptom:** The application assignment pane allowed users but not groups.

**Cause:** The tenant lacked the required Entra premium licensing.

**Resolution:** Activated a time-limited P2 trial and assigned licenses only to relevant lab identities.

**Lesson:** Confirm feature licensing early and document trial-expiration dependencies.

## Administrator Conditional Access policy did not appear in What If

**Symptom:** What If returned no matching policy for the IAM administrator.

**Cause:** The policy selected `Groups Administrator` instead of `Global Administrator` and targeted agent resources instead of cloud resources.

**Resolution:** Corrected the role to `Global Administrator` and the target to `All resources (formerly All cloud apps)`.

**Lesson:** Similar UI labels can materially change policy scope; validate every assignment using What If before enforcement.

## Sign-in continuation showed Conditional Access as not applied

**Symptom:** The final successful AWS sign-in event displayed Conditional Access as not applied.

**Cause:** The MFA policy was evaluated successfully on the immediately preceding interrupted event. The success event was the post-MFA continuation.

**Resolution:** Correlated the paired events and reviewed the Conditional Access and authentication details of the interrupted event.

**Lesson:** Analyze the complete authentication sequence rather than a single terminal event.

## Lockout-safe policy migration

Security Defaults were not disabled until both custom policies had been validated in report-only mode. A separate emergency administrator session was opened and verified before the administrator policy was enforced. The emergency account remained excluded through a dedicated group.
