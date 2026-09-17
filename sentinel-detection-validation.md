# Identity detection and incident-response validation

## Outcome

On September 17, 2026, a controlled incorrect-password test against the synthetic Maya Chen account produced four Microsoft Entra sign-in records with error 50126. A custom Microsoft Sentinel scheduled rule detected the activity, created a Medium-severity alert, and generated incident #1 in Microsoft Defender. The incident and alert were assigned to the lab administrator and resolved as **Informational, expected activity — Security testing**.

This extends the Entra-to-AWS identity governance lab with monitoring and incident triage. Successful Entra federation events for the AWS enterprise application were also observed in SigninLogs. The incorrect-password test itself targeted **My Apps**; it did not demonstrate AWS CloudTrail ingestion or AWS-native threat detection.

## Data collection

- Installed the Microsoft Entra ID solution from Sentinel Content hub.
- Enabled Sign-In Logs and Audit Logs for the lab Log Analytics workspace.
- Verified records in both SigninLogs and AuditLogs.
- Resolved a diagnostic-setting configuration error by granting the lab administrator Log Analytics Contributor at the workspace scope. The missing linked-resource action was workspaces/sharedKeys/action.

All activity used a personal lab tenant and synthetic test identities, separate from employer systems.

## Detection logic

See [KQL query](../detections/repeated-incorrect-passwords.kql) and [sanitized rule export](../detections/repeated-incorrect-passwords.json).

The query selects error 50126 for one synthetic user, groups records by user principal name and source IP, and returns a result when the group contains at least three events. It also retains the earliest and latest TimeGenerated values and application names.

| Setting | Validated lab configuration |
|---|---|
| Rule | NorthStar - Repeated incorrect passwords |
| Severity | Medium |
| Run frequency | 5 minutes |
| Lookback | 15 minutes |
| Alert threshold | More than zero query result rows |
| Event grouping | Single alert |
| Suppression | Enabled for 1 hour |
| Account entity | FullName mapped to UserPrincipalName |
| IP entity | Address mapped to IPAddress |
| Incident creation | Enabled |
| Alert grouping | Disabled |

The result threshold is zero because the query already enforces the three-attempt threshold. One summarized result row represented four underlying failures.

## Test and investigation

1. Confirmed successful sign-ins from the synthetic user reached the workspace.
2. Performed a small, manual incorrect-password test against that account.
3. Checked Entra sign-in logs and then workspace records for the resulting failures.
4. Verified four records with ResultType 50126 and the description indicating invalid username or password.
5. Confirmed the scheduled rule produced an alert and Defender linked it to incident #1.
6. Reviewed the account, source IP, application, and aggregated attempt count.
7. Assigned the incident to the lab administrator, moved it to In Progress, and resolved the authorized test as Security testing.
8. Verified the live alert was also Resolved with zero active alerts in the incident.

Earlier records with error 50140 described the “Keep me signed in” interruption. They were excluded from the incorrect-password detection.

## Evidence retained

The private evidence set contains the raw sign-in screenshots, the aggregated alert, incident history, original rule export, and an incident PDF. The PDF confirms the resolved incident and Security testing classification, but its alert table showed New. The live alert history independently confirmed resolution; the export discrepancy was not treated as proof that the alert remained open.

Raw screenshots and the PDF are not included here because they contain live identifiers. Public documentation uses sanitized descriptions. The exported PDF alone does not preserve the four raw failures or the complete investigation note.

## Limitations and lessons

- This rule is scoped to one lab user. It is not a production-wide brute-force or password-spray detector.
- Error 50126 identifies invalid credentials; the rule does not establish malicious intent or account compromise.
- Grouping by user and IP can miss attempts distributed across multiple IP addresses.
- One-hour suppression pauses the rule after an alert and can hide subsequent matching activity during that period.
- A 15-minute lookback must be considered alongside ingestion delays; this test does not establish a reliable detection-latency guarantee.
- Entra source timestamps and workspace TimeGenerated values differed. Workspace times were not treated as exact authentication times.
- MITRE ATT&CK tactics and techniques were not configured in the exported rule.
- No automated containment or remediation playbook was used.
- Cloud costs and trial expiration still require review before continued operation.

## Reuse

The public JSON is adapted from the validated export: the account is replaced with maya.chen@example.com, the rule identifier is replaced with a placeholder GUID, and enabled is set to false. Replace these placeholders and review the destination workspace before deployment. The original live rule was enabled when exported; this repository copy does not alter that live rule.

## Skills demonstrated

Entra log integration, KQL filtering and aggregation, scheduled Sentinel analytics, entity mapping, detection testing, Defender incident triage, classification, evidence handling, and documentation of detection limitations.
