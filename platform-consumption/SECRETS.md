# Secrets consumption contract

The Customer/KYC product does not commit credentials.

For a future runtime:
- create one product OIDC client with least privilege;
- store its credential through the approved shared secret mechanism;
- reference the resulting Kubernetes Secret from workloads;
- never place client secrets, passwords or tokens in this repository;
- use the Shared Platform ExternalSecret template only when External Secrets Operator and an approved SecretStore actually exist.

Current status:
`CONTRACT_ONLY / NO_SECRET_MATERIAL`.
