# I01 — Cadrage Solution

**Statut : COMPLET — 2026-10-01**

## 1. Problème à résoudre

Une banque doit permettre à un prospect ou à un client — personne physique ou personne morale — d’entrer en relation, de prouver son identité, de fournir les informations réglementaires nécessaires, de donner ses consentements, de suivre son dossier puis de gérer sa relation en selfcare.

Le SI doit simultanément :

- offrir un parcours digital fluide ;
- satisfaire les obligations de connaissance client et de vigilance ;
- éviter les doubles identités et incohérences ;
- conserver une traçabilité probante ;
- intégrer des systèmes legacy ;
- maîtriser les données personnelles ;
- continuer à fonctionner en cas de panne partielle ;
- permettre une évolution progressive sans Big Bang.

## 2. Objectif d’architecture

Construire une architecture de référence qui relie :

```text
Intentions métier
   ↓
Capacités
   ↓
Services fonctionnels
   ↓
Applications / bounded contexts
   ↓
Données
   ↓
API / événements / messaging
   ↓
Plateforme technique
   ↓
NFR / sécurité / résilience / exploitation
```

## 3. Scope

### In scope

- prospects et clients personnes physiques ;
- personnes morales et représentants ;
- Party / Customer / Relationship ;
- digital onboarding ;
- KYC / KYB ;
- identification et vérification d’identité ;
- bénéficiaires effectifs ;
- screening comme capacité intégrée ;
- consentements et préférences ;
- Customer 360 ;
- gestion de dossier ;
- documents et preuves ;
- selfcare ;
- intégration core/legacy ;
- événements métier ;
- audit ;
- NFR, résilience et observabilité.

### Hors scope initial

- moteur AML transaction-monitoring complet ;
- moteur de fraude paiement complet ;
- core banking complet ;
- crédit / scoring de solvabilité ;
- sanctions engine propriétaire ;
- reproduction d’un produit KYC du marché ;
- architecture interne d’un établissement réel.

Ces capacités apparaissent comme **interfaces ou plateformes externes** lorsqu’elles sont nécessaires au parcours.

## 4. Parties prenantes

| Partie prenante | Attente principale |
|---|---|
| Client / Prospect | parcours simple, rapide et compréhensible |
| Conseiller | vue fiable, dossier explicable, reprise manuelle |
| Conformité AML/CFT | CDD, bénéficiaires effectifs, risques, preuves |
| Sécurité | authentification, fraude identité, secrets, traçabilité |
| DPO / Privacy | minimisation, finalités, rétention, droits |
| Métier Customer | qualité et cohérence de la donnée client |
| Architecture SI | cohérence fonctionnelle/applicative/technique |
| Data / MDM | mastership, golden record, qualité, lineage |
| Exploitation / SRE | observabilité, SLO, recovery |
| Core Banking | création et synchronisation de la relation |
| API / Integration | contrats, compatibilité, orchestration |
| Plateforme | exécution, IAM technique, GitOps, observabilité |

## 5. Exigences fonctionnelles majeures

### FR-01 — Créer un prospect
Le système doit créer une identité prospect sans créer immédiatement un client bancaire définitif.

### FR-02 — Vérifier l’identité
Le système doit pouvoir collecter et vérifier les attributs nécessaires selon le contexte et le niveau de risque.

### FR-03 — Identifier le bénéficiaire effectif
Pour une personne morale, le parcours doit gérer les personnes physiques qui contrôlent ou détiennent la structure selon les règles applicables.

### FR-04 — Évaluer le contexte de relation
Le système doit capturer la finalité et la nature attendue de la relation.

### FR-05 — Orchestrer KYC/KYB
Le dossier doit avoir un cycle de vie explicite et auditable.

### FR-06 — Gérer les consentements
Chaque consentement doit avoir finalité, version, canal, date, statut et preuve.

### FR-07 — Produire un Customer 360
Les systèmes consommateurs doivent disposer d’une vue client cohérente sans rendre chaque application maître de toutes les données.

### FR-08 — Publier les changements
Les changements significatifs doivent pouvoir être exposés via API et/ou événements.

### FR-09 — Supporter le selfcare
Le client doit pouvoir consulter et modifier les attributs autorisés sans contourner les règles de contrôle.

### FR-10 — Gérer les exceptions
Un cas ambigu, incomplet ou à risque doit pouvoir passer en revue humaine.

## 6. NFR de départ

| NFR | Cible d’architecture |
|---|---|
| Disponibilité | services critiques conçus pour éviter le SPOF |
| Traçabilité | correlation ID et audit métier bout en bout |
| Sécurité | OIDC/OAuth2, MFA selon risque, least privilege |
| Confidentialité | chiffrement transit/repos + minimisation |
| Intégrité | versioning, optimistic locking/idempotence |
| Performance | parcours interactifs avec budgets de latence |
| Résilience | retries bornés, timeouts, circuit breaking, reprise |
| Exploitabilité | logs structurés, métriques, traces, business telemetry |
| Portabilité | contrats indépendants d’un seul fournisseur |
| Évolutivité | bounded contexts et événements versionnés |
| Auditabilité | décision et provenance des données reconstituables |
| Accessibilité | parcours digital compatible exigences d’accessibilité |

Les valeurs chiffrées SLA/RTO/RPO restent `DISCOVERY_REQUIRED` tant qu’un contexte client réel ne les fixe pas.

## 7. Contraintes

- coexistence probable avec un core bancaire legacy ;
- hétérogénéité des canaux ;
- données client réparties ;
- réglementations multiples ;
- dépendances à des fournisseurs externes d’identité/screening ;
- nécessité de migration progressive ;
- absence volontaire de dépendance à un produit propriétaire unique dans le référentiel.

## 8. Questions de discovery

1. Quel système est maître de Party, Customer, Address, Contact et Relationship ?
2. Prospect et Client partagent-ils le même identifiant ?
3. Existe-t-il un MDM ou un référentiel tiers ?
4. Qui décide qu’un KYC est acceptable ?
5. Où sont conservées les preuves ?
6. Quelles données sont nécessaires par segment/clientèle ?
7. Quels parcours sont synchrones ?
8. Quels traitements peuvent être asynchrones ?
9. Quels systèmes legacy doivent être mis à jour ?
10. Quels événements doivent être publiés ?
11. Quelles règles de rapprochement/dédoublonnage existent ?
12. Quels RTO/RPO/SLA sont contractuels ?
13. Quels fournisseurs tiers participent au parcours ?
14. Comment le client exerce-t-il ses droits sur ses données ?
15. Quel est le modèle de délégation pour les personnes morales ?

## 9. Truth boundaries

- réglementation citée : `PUBLIC_VERIFIED` lorsqu’une source officielle est enregistrée ;
- architecture MayaBank : `REFERENCE_ARCHITECTURE` ;
- client réel : `DISCOVERY_REQUIRED` ;
- runtime : `RUNTIME_PROVEN` uniquement après preuve exécutable.

## 10. Critères de sortie I01

- [x] problème et objectif ;
- [x] scope / hors-scope ;
- [x] parties prenantes ;
- [x] FR principales ;
- [x] NFR initiales ;
- [x] contraintes ;
- [x] questions de discovery ;
- [x] truth boundaries.
