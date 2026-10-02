# I12 — Acceptance Record

**Date : 2026-10-01**  
**Statut : PASS / PORTFOLIO READY**

## Baseline terminée

- I00 — Decision Gate / Gap analysis ;
- I01 — Cadrage Solution ;
- I02 — Customer / KYC / KYB ;
- I03 — parcours B2C/B2B/selfcare ;
- I04 — architecture fonctionnelle ;
- I05 — architecture applicative ;
- I06 — information/data ;
- I07 — CIAM/sécurité/privacy ;
- I08 — API/EDA/MQ/legacy ;
- I09 — architecture technique ;
- I10 — NFR/résilience/DORA ;
- I11 — AS-IS/GAP/TARGET/DAA/ADR ;
- I12 — pack portfolio.

## Decision Gate runtime

**NO HEAVY RUNTIME REQUIRED FOR BASELINE.**

Le portefeuille possède déjà des preuves exécutables OpenShift, Kafka, IBM MQ, PostgreSQL, observabilité et résilience dans les spécialistes existants.

Un vertical Customer/KYC sera ajouté uniquement si une mission exige une preuve métier exécutable.

## Claim autorisé

`REFERENCE_ARCHITECTURE / PORTFOLIO_READY`

## Claims non autorisés

- architecture interne d’un établissement ;
- conformité réglementaire certifiée ;
- runtime KYC production ;
- HA multi-site prouvée ;
- produit KYC réel du marché intégré.


## Extension post-baseline — Shared Platform consumer contract

**Date : 2026-10-02**  
**Statut : IMPLEMENTED / STATIC_VALIDATED**

Le produit possède maintenant un contrat concret de consommation de la plateforme commune sous `platform-consumption/`.

Cette extension ne change pas le Decision Gate initial : aucun runtime lourd Customer/KYC n'est requis pour fermer la baseline.

Claim additionnel autorisé :
`STATIC_CONSUMER_CONTRACT_VERIFIED`.

CI observée : `validate-platform-consumer` run `37036053381` — **SUCCESS**.

Claim toujours non autorisé :
`CUSTOMER_KYC_CRC_RUNTIME_PROVEN` tant qu'une exécution dédiée n'a pas été observée.
