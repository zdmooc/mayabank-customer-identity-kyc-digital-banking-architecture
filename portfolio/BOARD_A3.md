# Board A3 — Architecture SI Banque Customer / KYC / Digital

**Statut : PORTFOLIO READY — 2026-10-01**

## 1. Problème

Moderniser les parcours Connaissance Client et Digital sans reproduire la fragmentation du SI ni imposer un Big Bang au core bancaire.

## 2. Cible

```text
Client / Conseiller
        |
   CIAM + API
        |
       BFF
        |
+-------+-------+----------+
|               |          |
Onboarding    Customer    Selfcare
|               |          |
KYC/KYB ---- Relationship  Consent
|               |
Evidence      Customer 360
        \       /
          Events
            |
        Kafka / MQ
            |
      Core CICS/COBOL
        Oracle/Legacy
```

## 3. Principes

- Customer ≠ Identity ≠ CIAM ;
- KYC = case auditable ;
- Customer 360 = vue explicable ;
- API pour interaction, events pour propagation, MQ pour legacy critique ;
- Outbox/idempotence/reconciliation ;
- AS-IS → GAP → TARGET ;
- Strangler, pas Big Bang ;
- plateforme technique partagée ;
- privacy/security/resilience by design.

## 4. Capacités

- Customer / Party ;
- Relationship ;
- KYC/KYB ;
- Consent ;
- Customer 360 ;
- Digital Onboarding ;
- Selfcare ;
- Evidence ;
- Manual Review ;
- Integration Legacy.

## 5. NFR

- auditabilité ;
- disponibilité ;
- intégrité ;
- privacy ;
- sécurité ;
- résilience ;
- observabilité ;
- réversibilité.

## 6. Transformation

```text
T0 Legacy
 → T1 API façade
 → T2 Onboarding/KYC moderne
 → T3 déplacement mastership
 → T4 Customer 360 event-driven
 → T5 décommissionnement ciblé
```

## 7. Différenciation portfolio

Ce dépôt démontre la capacité à partir du **métier bancaire Customer/KYC** et à descendre jusqu’à l’architecture applicative, data, intégration et technique, en réutilisant les spécialistes Kafka, MQ, API, OpenShift, Oracle, DORA et Enterprise Architecture existants.
