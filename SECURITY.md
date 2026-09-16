# Security and Disclosure Policy

This repository must never contain live credentials, passwords, MFA seeds, QR codes, recovery codes, SCIM tokens, private certificates, billing information, personal addresses, or employer data.

Before publishing any evidence, redact:

- Email addresses and tenant domains
- AWS account, organization, identity-store, and instance identifiers
- Entra tenant, object, application, policy, and correlation identifiers
- IP addresses and precise locations
- Request IDs and assumed-role ARNs
- Portal URLs containing unique instance identifiers
- Names or details that are not explicitly synthetic

If a secret is committed accidentally, remove it from the live system immediately, rotate it, and then purge it from repository history. Deleting only the visible file is insufficient.
