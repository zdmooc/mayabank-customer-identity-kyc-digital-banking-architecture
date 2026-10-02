# Customer/KYC — Shared Platform Consumption Profile

**Consumer:** `mayabank-customer-identity-kyc-digital-banking-architecture`  
**Role:** business/product architecture consumer  
**Evidence level:** `IMPLEMENTED / STATIC_VALIDATED`

## Purpose

This profile turns the existing REUSE/CONSUME_SHARED architecture decision into a concrete deployable contract without creating a second technical platform inside the Customer/KYC product.

## Shared capabilities consumed

- OpenShift/Kubernetes conventions: `CONSUME_SHARED`;
- GitOps / Argo CD: `CONSUME_SHARED`;
- OIDC issuer / identity contract: `CONSUME_SHARED`;
- OpenTelemetry collector: `CONSUME_SHARED`;
- secrets / PKI integration contract: `CONSUME_SHARED`;
- quality gates: `CONSUME_SHARED`;
- API Management: `SPECIALIZED_PLATFORM`;
- Kafka/eventing: `SPECIALIZED_PLATFORM / CONSUME_SHARED WHEN REQUIRED`;
- IBM MQ: `SPECIALIZED_PLATFORM`.

## Product-owned capabilities

- Customer 360/domain model;
- KYC/KYB/onboarding state;
- consent and preference business rules;
- product API/event contracts;
- customer/KYC data ownership and schema decisions;
- business SLOs and functional controls.

## Concrete target contract

```text
Shared Keycloak/OIDC -------------------+
Shared OTel ----------------------------+--> Customer/KYC workloads
Shared GitOps / security conventions ---+
                                         |
API Management / Kong ------------------>+--> product APIs
```

The supplied manifests define only the consumer-side contract. They do not install Keycloak, OTel, Argo CD, Kafka, MQ, databases or secret-management products.

## Validate

The repository workflow `.github/workflows/validate-platform-consumer.yml` validates:
- YAML syntax;
- Kustomize render;
- required shared endpoints;
- no embedded client secret;
- namespace/network-policy consistency;
- least-privilege ServiceAccount/Role/RoleBinding surface;
- secret-material absence.

## Truth boundary

This profile is **not** a Customer/KYC runtime proof. It proves that the product repository has an explicit, deployable and CI-validated Shared Platform consumption contract.

A runtime claim requires an observed deployment and evidence markers from the target environment.


## Secrets

See `SECRETS.md`. No secret values are present in this profile.
