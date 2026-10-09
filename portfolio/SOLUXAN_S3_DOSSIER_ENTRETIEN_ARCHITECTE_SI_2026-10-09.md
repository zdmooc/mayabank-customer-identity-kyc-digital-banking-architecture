# SOLUXAN-S3 — Dossier d'entretien Architecte fonctionnel / domaine / applicatif bancaire

**Date : 2026-10-09**  
**Statut : INTERVIEW_PACK_READY / REFERENCE_ARCHITECTURE / APPLICATION_SENT**  
**Client final : non communiqué.** Ce document est un pack MayaBank issu des spécialistes existants, pas un dossier validé par une banque.

## 0. Résumé direction — une page

**Contexte d'illustration** : une banque veut fiabiliser l'entrée en relation digitale Customer/KYC malgré des applications multiples, données clientes dispersées et dépendances au core legacy.

**Problème** : risque de doubles identités, traitement KYC insuffisamment traçable, réponse legacy incertaine, données Customer360 difficiles à expliquer.

**Valeur attendue** (pas gains constatés) : maîtriser le cycle de vie des dossiers, limiter les incidents d'intégrité, préserver la conformité et moderniser le SI sans rupture.

**Décision instruite** : après discovery, privilégier un processus d'architecture progressive (Option B), avec API facade, bounded contexts, gouvernance de données et synchronisation contrôlée au core ; l'option A reste transitoire, C requiert justification.

**Risques à porter au Board** : mastership, double création, contrôles KYC, confidentialité, audit, reprise, migration et coûts encore inconnus.

**Demandes au Board** : valider l'étude détaillée, désigner owners métier/data/technique, établir volumes/SLA/budget et fixer les gates de transition. **Ce n'est pas une approbation d'un véritable comité bancaire.**

## 1. Dossier de preuves — lecture ordonnée, 10 minutes

| Temps | Montrer | Argument d'architecture | Source |
|---|---|---|---|
| 00–01 | objectif / contraintes bancaires | métier, conformité, legacy et continuité | [I01](../docs/cadrage/I01_CADRAGE_SOLUTION.md) |
| 01–02 | parcours B2C/B2B | différence prospect / identity / customer / consent | [I03](../docs/journeys/I03_PARCOURS_DIGITAUX.md) |
| 02–03 | capacités / F01…F12 | découpler le besoin métier du produit technique | [I04](../docs/architecture/I04_ARCHITECTURE_FONCTIONNELLE.md) |
| 03–04 | BPMN 2.0 et exception UNKNOWN | issue métier, revue, inquiry et reprise | [HOPEX BPMN](https://github.com/zdmooc/hopex-aquila-enterprise-architecture-masterbook/blob/main/07-business-process-analysis/models/MAYABANK_CUSTOMER_KYC_BPMN_TRACEABILITY_2026-10-09.md) |
| 04–05 | exigences → applications → tests | FR→F→Service→contrat→AC, pas un diagramme isolé | [S1](../docs/traceability/SOLUXAN_S1_EXIGENCES_PROCESSUS_APPLICATION_TESTS_2026-10-09.md) |
| 05–06 | architecture applicative | responsabilités BFF, Onboarding, KYC, Consent, Customer360 | [I05](../docs/architecture/I05_ARCHITECTURE_APPLICATIVE.md) |
| 06–07 | intégration/core | API/EDA/MQ, ACL, Outbox, idempotence, pas de retry aveugle | [I08](../docs/integration/I08_API_EDA_MQ_LEGACY.md) |
| 07–08 | dossier de choix A/B/C | démontrer le trade-off, décision conditionnelle | [S2 Dossier choix](../docs/decision/SOLUXAN_S2_DOSSIER_CHOIX_ARCHITECTURE_CUSTOMER_LEGACY_2026-10-09.md) |
| 08–09 | cas logiciel prouvé | Payment UNKNOWN / idempotence, **dans un autre produit synthétique** | [S2 Deep Dive](../docs/decision/SOLUXAN_S2_CAS_LOGICIEL_PAIEMENT_UNKNOWN_2026-10-09.md) |
| 09–10 | trajectoire et questions | dépendances, livrables, gate migration et prochaines validations | [I11](../docs/transition/I11_ASIS_GAP_TARGET_DAA.md) |

## 2. Trois angles de la même démonstration selon rôle réel

- **Architecte fonctionnel** : FR-01…10, processus/BPMN, acteurs, objets de données, exceptions, contrôle et critères d'acceptation. Ne pas transformer les concepts métier en produits.
- **Architecte de domaine** : découpage Customer/KYC/Payments, ownership, capability map, interfaces, catalogue applicatif, transitions, gouvernance et architecture board.
- **Architecte applicatif / logiciel** : bounded contexts, état durable, APIs et événements, outbox/inbox, authorization, cohérence des données, résilience et tests. Référence au code et aux preuves CRC d'Instant Payments, sans confondre POC et client.
- **Chef de projet SI** (si confirmé uniquement) : dépendances, RACI, wave plan, RAID, jalons, interlocuteurs, arbitrages, critères de livraison ; ne pas se présenter comme directeur de programme non exercé.

## 3. Questions et réponses de fond pour l'entretien

1. **Pourquoi pas une refonte Big Bang ?** Sans patrimoine/volumes/interfaces mesurés, la réduction globale du risque n'est pas établie. Option B transfère l'ownership par capacité avec gates vérifiables.
2. **Qu'est-ce qui est fonctionnel et qu'est-ce qui est applicatif ?** Fonctionnel = capacité et service attendu ; applicatif = composant responsable, interfaces et données ; infra = environnement d'exécution.
3. **Pourquoi Customer360 n'est pas maître ?** Vue consolidée avec sources et fraîcheur ; les maîtres se choisissent par attribut et phase de transformation.
4. **Qui prend la décision KYC ?** Le système met à disposition preuves, politique versionnée et workflows ; la délégation métier/humaine se définit avec Compliance. Aucune règle bancaire réelle n'est inventée.
5. **Pourquoi BPMN ?** Décrire les décisions, exceptions et responsabilités du processus, puis relier chaque activité aux applications et aux contrôles ; ce n'est pas l'architecture runtime elle-même.
6. **Quand API vs event vs MQ ?** API synchrone pour interaction immédiate, events pour changement découplé, MQ pour intégration transactionnelle avec legacy ; choix selon NFR et contrats.
7. **Core ne répond pas après une création ?** UNKNOWN/PENDING_CONFIRMATION, clé stable, inquiry/reconcile ; jamais de création doublée sur simple timeout.
8. **Quels tests prouvent la logique ?** AC-01…10 Customer = spécifications non exécutées ; Instant Payments I18 possède des preuves CRC applicatives limitées au mono-nœud.
9. **Comment conduire l'atelier de cadrage ?** Clarifier objectifs, périmètre, parties prenantes, invariants, inventaire existant, risques et contraintes d'intégration ; valider la cartographie avec owners.
10. **Comment décider en comité ?** options comparées, coûts/risques sourcés, objections et signatures ; B est orientation de référence, pas décision client.
11. **Quel rôle joue OpenShift ?** Moyen d'exécution éventuel ; le besoin Soluxan cible d'abord les processus, applications, données et décisions logicielles.
12. **Et si le domaine réel n'est pas Customer/KYC ?** Démontrer la méthode et pivoter vers Payments/ISO20022, Risk Credit/Decision ou API Integration selon la qualification ; ne pas inventer de connaissance métier spécifique.

## 4. Qualification Soluxan : ce qu'il faut demander dès le contact

- Client final / métier bancaire : banque de détail, paiements, crédit, CIB, Customer/KYC ?
- Missions exactes : architecture de domaine, conception applicative, suivi de delivery, gouvernance SI ?
- Livrables : DAA, urbanisation, ADR, cartographie, processus, roadmap, specs fonctionnelles, coordination équipe ?
- SI : monolithes/legacy, Java/.NET, CRM/core, règles, API/messaging et contraintes réglementaires ?
- Présence Brest : jours par semaine, déplacements, hébergement et prise en charge des frais ?
- Gouvernance : responsable métier, Architecture Board et droits de décision ?
- Démarrage, durée, TJM et scénario de reconduction ?

**Candidature :** 2026-10-09 09:14 sur Collective (Soluxan) ; **750 € HT/jour proposé**. Réponse/entretien **non encore confirmé**. Aucun nouveau message recruteur n'est supposé.

## 5. Inventaire des livrables — Definition of Done de S1/S2/S3

| Gate | Livrable | Statut |
|---|---|---|
| S1 | [Matrice FR → F → App → AC](../docs/traceability/SOLUXAN_S1_EXIGENCES_PROCESSUS_APPLICATION_TESTS_2026-10-09.md) | DOCUMENTED |
| S1 | [Scénarios Gherkin](../docs/traceability/SOLUXAN_S1_CAS_USAGE_ET_CRITERES_GHERKIN_2026-10-09.md) | SPECIFIED, NOT_EXECUTED |
| S2 | [Options A/B/C + Board](../docs/decision/SOLUXAN_S2_DOSSIER_CHOIX_ARCHITECTURE_CUSTOMER_LEGACY_2026-10-09.md) | REFERENCE_DECISION |
| S2 | [Deep dive payment UNKNOWN](../docs/decision/SOLUXAN_S2_CAS_LOGICIEL_PAIEMENT_UNKNOWN_2026-10-09.md) | CROSS_REPO_EVIDENCE_LINKED |
| S3 | [BPMN 2.0 Customer](https://github.com/zdmooc/hopex-aquila-enterprise-architecture-masterbook/blob/main/07-business-process-analysis/models/mayabank-customer-kyc-onboarding-soluxan-2026-10-09.bpmn) | MODELLED, NOT_EXECUTED |
| S3 | [Process → applications / RACI / KPI](https://github.com/zdmooc/hopex-aquila-enterprise-architecture-masterbook/blob/main/07-business-process-analysis/models/MAYABANK_CUSTOMER_KYC_BPMN_TRACEABILITY_2026-10-09.md) | DOCUMENTED |
| S3 | Ce dossier 10 min + questions + discovery | INTERVIEW_PACK_READY |

## 6. Frontières de preuve et exclusions

- Documenter ne veut pas dire mesurer ou valider un SI bancaire client.
- `REFERENCE_ARCHITECTURE` et spécifications Gherkin ne sont pas des tests Customer/KYC `RUNTIME_PROVEN`.
- BPMN `isExecutable=false` n'implique ni déploiement Camunda ni import natif HOPEX.
- Les preuves CRC de Payments/I18 sont des tests applicatifs **mono-nœud**, pas HA/multi-site ou production.
- Les informations Soluxan et l'option B sont des hypothèses / signaux, en attente de qualification.
- Aucune création de dépôt, plateforme, installation Kubernetes ou déploiement client.
