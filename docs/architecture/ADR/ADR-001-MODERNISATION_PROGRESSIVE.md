# ADR-001 — Modernisation progressive plutôt que Big Bang

- **Statut** : ACCEPTED
- **Date** : 2026-10-01

## Contexte

Le domaine Customer/KYC doit pouvoir coexister avec CRM, référentiels, MQ, CICS/COBOL et bases historiques.

## Décision

Adopter un modèle **Strangler + API façade + Anti-Corruption Layer + Event propagation**, avec déplacement progressif du mastership.

## Conséquences positives

- risque de migration réduit ;
- rollback possible ;
- valeur incrémentale ;
- legacy encapsulé ;
- modernisation par capacité.

## Conséquences négatives

- coexistence temporaire ;
- reconciliation obligatoire ;
- gouvernance mastership plus exigeante ;
- architecture de transition à maintenir.

## Alternatives rejetées

- Big Bang : trop risqué comme baseline générique ;
- simple façade permanente : ne réduit pas suffisamment la dette métier.
