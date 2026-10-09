# SOLUXAN-S2 — Dossier de choix d'architecture : Customer/KYC ↔ Core bancaire legacy

**Version : 1.0 · 2026-10-09**  
**Statut : REFERENCE_DECISION_PACK / PROPOSITION MayaBank ; NON validé par un comité client**  
**Décision sollicitée :** autoriser l'instruction de l'option B avec **gates factuels**, pas autoriser un déploiement réel.  
**Références :** [I01 exigences](../cadrage/I01_CADRAGE_SOLUTION.md), [I05 applications](../architecture/I05_ARCHITECTURE_APPLICATIVE.md), [I08 legacy](../integration/I08_API_EDA_MQ_LEGACY.md), [I11 transitions](../transition/I11_ASIS_GAP_TARGET_DAA.md), [ADR-001](../architecture/ADR/ADR-001-MODERNISATION_PROGRESSIVE.md), [S1 traçabilité](../traceability/SOLUXAN_S1_EXIGENCES_PROCESSUS_APPLICATION_TESTS_2026-10-09.md).

## 1. Executive decision brief

**Problème de référence fictif :** un parcours d'entrée en relation Customer/KYC digital cohabite avec un CRM et un core legacy ; plusieurs systèmes peuvent posséder des fragments d'information client, et le résultat d'une création transmise au core via messaging peut être incertain.

**Enjeu métier :** améliorer fluidité, auditabilité et délai d'entrée en relation **sans créer deux clients**, tout en maintenant des contrôles KYB/KYC et une reprise sûre lors d'une défaillance du core.

**Décision de référence recommandée :** **B — modernisation incrémentale par capacité**, sous réserve de cartographier les propriétaires de données, les interfaces, les règles métiers existantes et d'obtenir un accord sur la stratégie de réconciliation. **A** peut être une étape de transition ; **C** n'est pas la baseline recommandée faute de preuve permettant de maîtriser un Big Bang.

**Non-décision :** ni fournisseur, ni cible RTO/RPO, ni estimation budget, ni client bancaire final, ni date de décommissionnement ne sont inventés.

## 2. Architecture de départ — hypothèse (à vérifier, pas AS-IS client)

```text
Canaux Web / Conseiller
   ├── CRM historique → référentiel client historique
   └── orchestrations KYC semi-manuelles
                └── batch/API/MQ → Core CICS/COBOL
Pièces justificatives + décisions + consentements : dispersion possible
```

**Inventaire documentaire préalable** : référentiel appli, contrat core, key ownership Party/Customer, catalogue d'erreurs/timeout, version des règles KYC, cycle de vie des consentements, règles de migration, volumétries et journaux d'exploitation. Sans inventaire vérifié, la cible reste une hypothèse.

## 3. Comparaison des options

| Critère | A — Façade + master legacy | B — Bounded contexts + mastership progressif | C — Refonte totale |
|---|---|---|---|
| Mise en valeur rapide du digital | Bonne sur périmètre restreint | Progressive par parcours | Retardée par la bascule globale |
| Risque de cutover | Limité à l'intégration | Progressif, avec états de coexistence | Fort tant que migration non prouvée |
| Dette structurelle | Faible réduction | Réduction par capacité | Potentiel fort après bascule réussie |
| Cohérence Customer/Core | Legacy maître, synchronisation simple en théorie | **Point de vigilance :** mastership attribué, dual-write évité, reconcile obligatoire | Risques concentrés en migration et retour arrière |
| Évolution KYC/consent | Couplage possible | Responsabilités explicites | Refonte de tous les processus |
| Réversibilité | Bonne pour les façades | Par vague si gate et rollback définis | Faible après cutover massif |
| Coût / effort | À établir sur sources réelles | À établir après découpage des vagues | À établir après inventaire |
| Choix de référence | Étape transitoire possible | **À approfondir prioritairement** | À réexaminer uniquement avec justification métier/technique solide |

**Aucun score pondéré ni gain chiffré fictif.** Décision réelle conditionnée aux données observées, aux contraintes réglementaires, à l'existant, aux NFR, et aux arbitrages entre métiers, sécurité, data et architecture.

## 4. Architecture cible proposée — option B

```text
Client / Conseiller
   → BFF / API Management (exposition et contrôle, pas règles KYC)
      → Onboarding (orchestration de parcours)
      → Party/Customer (mastership d'attributs à répartir)
      → KYC Case (décision versionnée, état et revue humaine)
      → Consent (finalité, preuve, retrait)
      → Relationship / Evidence
            └─ transaction locale + Outbox → Kafka / broker
                                               ↓
                                       ACL / mapping / retry contrôlé
                                               ↓
                                         IBM MQ / API legacy
                                               ↓
                                        CICS/COBOL / Core
                                               ↑
                                     Inquiry + reconciliation
Customer360 read model ← événements/changes avec provenance + fraîcheur
```

### Règles d'architecture imposées

1. **Une autorité unique par attribut et phase** : `customer_id`, Party/Relationship et droits, pas de double master implicite.
2. **One intent / one identifier** : `customer_creation_id` reste constant en reprise ; duplications consommateur traitées par Inbox/dedup.
3. **Timeout ≠ échec** : réponse core ambiguë classée `PENDING_CONFIRMATION/UNKNOWN`, query/inquiry puis réconciliation ; interdiction de rejouer une création à l'aveugle.
4. **API / événements / MQ selon besoin** : orchestration interactive via API, propagation via événements, compatibilité transactions historiques via MQ.
5. **Security & Privacy** : données minimisées, finalités et consentements versionnés, SoD sur revue humaine, audit.
6. **Tests d'acceptation fonctionnels** : S1 AC-01…10 / EX-01…08 avant changement de responsabilité maître.

## 5. Plan de transition conditionnel

| Wave | Capacité | Donnée maître cible | Gate d'entrée | Gate de sortie / rollback |
|---|---|---|---|---|
| T0 — Discovery | catalogue applicatif et data | legacy maître (hypothèse) | inventaire validé | owners, contraintes, contrats et NFR établis |
| T1 — Façade | lecture statut + API contrôlée | inchangé | contrat core validé | observabilité/erreurs + retour simple à chemin legacy |
| T2 — Onboarding/KYC | case, revue, policy version | Customer encore legacy | intégration + SoD + tests de reprise | AC-02/04/05/10 ; fallback flux historique |
| T3 — Party/Customer | mastership d'attributs ciblés | transféré par attribut et population | data stewardship + double-lecture contrôlée | AC-01/07/08, reconcile sans doublon ; rollback uniquement si maîtrise des écritures |
| T4 — Customer360 | vue réconciliée | read model, jamais maître universel | événements versionnés + lineage | AC-06/07 et contrôles retard/rejeu |
| T5 — Sunset | retrait fonction legacy | ownership cible confirmé | absence de consumers résiduels | analyse d'impact, sauvegarde/rollback, accord exploitation |

Les dates, volumes, ressources, engagements et coût restent **DISCOVERY_REQUIRED**.

## 6. Registre des risques et contrôles

| Risque | Cause / scénario | Contrôle proposé | Validation attendue |
|---|---|---|---|
| Double création Customer | timeout core + retry | clé stable + inquiry + réconciliation | EX-01 / AC-01 / AC-08 |
| Conflit mastership | attribut modifié dans 2 systèmes | source of truth par attribut + règles d'arbitrage | EX-08 / AC-07 |
| KYC non probant | policy non versionnée / documents manquants | policy_version, evidence provenance, revue habilitée | AC-05 / EX-06 |
| Consentement perdu | projection asynchrone obsolète | ledger versionné + freshness | EX-04 / AC-06 |
| Dépendance éditeur | screening externe indisponible | case suspendue et reprise, pas validation implicite | AC-02 / EX-02 |
| Retour arrière incomplet | migration d'ownership trop large | vagues, rollback test et gel conditionnel | gate T3 |
| Chaîne de tests insuffisante | contrats non testés | Gherkin, contrats API/EDA, scénarios négatifs, perf | preuves S1/QA |

## 7. Dossier Architecture Board — fiche d'arbitrage

**Décision demandée** : autoriser la **discovery + étude détaillée B**, avec A comme étape de réduction du risque et C comme contre-option documentée.  
**Sponsors/reviewers à identifier** : sponsor métier Customer, responsable conformité/KYC, data owner, architecte de domaine, architecte applicatif, sécurité, exploitant/core owner, delivery lead.  
**Prérequis obligatoires à produire dans un contexte réel** :
- capability/process map confirmée par métier ;
- inventaire des applications/interfaces/data maîtres ;
- impacts sécurité et compliance, NFR, RTO/RPO, volumétrie, charge ;
- coût et effort des 3 options par données documentées ;
- score / pondération **définis par les décideurs**, pas inférés ;
- plan de migration, rollback vérifié, critères de decommissionnement ;
- décisions/objections consignées avec propriétaire et date.

**Verdict d'architecture de référence** : B **candidate préférée, non décision client**.  
**Validation board** : NOT_REQUESTED / NOT_CLIENT_APPROVED.
