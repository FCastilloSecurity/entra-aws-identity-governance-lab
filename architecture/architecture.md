# Architecture

```mermaid
flowchart TD
    subgraph Entra["Microsoft Entra ID"]
        Users["Synthetic users"]
        Groups["Security groups"]
        CA["Conditional Access + MFA"]
        App["AWS enterprise application"]
        Users --> Groups
        CA --> App
    end

    subgraph AWS["AWS"]
        IIC["IAM Identity Center"]
        PS["Permission sets"]
        Account["AWS account"]
        IIC --> PS
        PS --> Account
    end

    App -->|"SAML authentication"| IIC
    App -->|"SCIM lifecycle provisioning"| IIC
    Groups --> App
```

## Trust and Data Flows

| Flow | Source | Destination | Purpose |
|---|---|---|---|
| SAML 2.0 | Microsoft Entra ID | AWS IAM Identity Center | Federated user authentication |
| SCIM | Microsoft Entra provisioning service | AWS IAM Identity Center | Create, update, disable, and group identities |
| Group assignment | Entra security groups | AWS permission-set assignments | Role-based access control |
| Conditional Access | Entra policy engine | AWS and administrative sign-ins | Enforce MFA and preserve emergency access |
