# Roadmap — I00 à I12

## Objectif global

Construire en français un référentiel démontrable d’**Architecture SI Banque — Connaissance Client / KYC / Digital**, aligné sur le cadrage MayaBank.

| Itération | Objectif | Livrables principaux |
|---|---|---|
| I00 | Decision Gate & gap analysis | REUSE/ADAPT/NEW, frontières, dépendances, portfolio sync |
| I01 | Cadrage Solution | parties prenantes, scope, FR/NFR, contraintes, hypothèses, truth boundaries |
| I02 | Domaine Customer/KYC | glossaire, capability model, KYC/KYB/AML boundaries, lifecycle |
| I03 | Parcours digitaux | onboarding B2C/B2B, selfcare, consentement, exceptions |
| I04 | Architecture fonctionnelle | capability map, services fonctionnels, interactions |
| I05 | Architecture applicative | bounded contexts, composants, APIs, responsabilités |
| I06 | Architecture information/data | Customer 360, MDM, golden record, lineage, qualité, rétention |
| I07 | CIAM & sécurité | OIDC/OAuth2, MFA, consent, RBAC/ABAC, privacy, threat model |
| I08 | Intégration & legacy | API, Kafka, MQ, COBOL/CICS, strangler, anti-corruption layer |
| I09 | Architecture technique | consommation plateforme commune, déploiement, réseau, secrets, observabilité |
| I10 | NFR & résilience | disponibilité, RTO/RPO, performance, DORA, SLO, incident/recovery |
| I11 | Transition | AS-IS → GAP → TARGET, scénarios, DAA, ADR, roadmap de migration |
| I12 | Portfolio-ready | board A3, pack entretien, pitch, scénario de démo, index de preuves |

## Règles d’exécution

1. Pas de runtime inutile avant que l’architecture fonctionnelle/applicative soit stable.
2. Un runtime n’est construit que pour prouver une propriété précise.
3. Les briques techniques communes sont réutilisées.
4. Toute hypothèse client est explicitement marquée.
5. Les diagrammes doivent exister en source versionnée.
6. Les décisions importantes sont consignées en ADR.
7. Chaque itération ferme un ensemble de critères de sortie.

## Gate après I08

À la fin d’I08, décider si un vertical exécutable apporte une valeur réelle.

Vertical candidat :

```text
Client Web
   ↓
BFF / API
   ↓
Onboarding
   ↓
Identity / KYC
   ↓
Decision
   ↓
Customer creation
   ↓
Kafka event
   ↓
Legacy adapter
```

Ce vertical reste optionnel tant qu’un besoin d’entretien ou une preuve manquante ne l’exige pas.
