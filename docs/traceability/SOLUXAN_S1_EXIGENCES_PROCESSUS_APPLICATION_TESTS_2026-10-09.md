# SOLUXAN-S1 — Matrice de traçabilité métier → applicatif → critères d'acceptation

**Statut : DOCUMENTED / REFERENCE_ARCHITECTURE** · **Date : 2026-10-09**  
**Périmètre :** MayaBank Customer/KYC/Digital, scénario bancaire fictif, et non architecture du client Soluxan.  
**Responsables métier :** rôles de référence, à confirmer en discovery ; aucun nom de personne réelle n'est attribué.

## Objectif et frontière

Donner à un architecte fonctionnel/de domaine une chaîne navigable **exigence → capacité → activité de processus → service fonctionnel → composant applicatif → donnée maître → contrat → contrôle/test**. Les exigences FR-01…FR-10 proviennent de [I01](../cadrage/I01_CADRAGE_SOLUTION.md), les fonctions F01…F12 de [I04](../architecture/I04_ARCHITECTURE_FONCTIONNELLE.md), les applications de [I05](../architecture/I05_ARCHITECTURE_APPLICATIVE.md). Il s'agit de **critères d'acceptation à exécuter**, pas de tests Customer/KYC runtime déjà réussis.

| Exigence | Capacité métier | Activité de parcours | Service fonctionnel | Application responsable | Source / donnée | Interface / événement de référence | Acceptation |
|---|---|---|---|---|---|---|---|
| FR-01 Prospect | Customer lifecycle | P01 saisir prospect | F01 Party, F02 Lifecycle | Party/Customer Service | Party + prospect_id, maître Customer à découvrir | CreateProspect API, ProspectCreated | AC-01 |
| FR-02 Identity | Identity verification | P02 collecter et vérifier | F04 Identity Verification | Onboarding + fournisseur identité | identité et attestation chez owner défini | IdentityVerificationRequested / IdentityVerified | AC-02 |
| FR-03 KYB UBO | Relationship/KYB | P03 déclarer représentant/UBO | F03 Relationship, F05 KYC Case | Relationship + KYC Case | graph Party/Relationship et bénéficiaires effectifs | UboDeclared / case update (CONCEPT) | AC-03 |
| FR-04 Purpose | Customer due diligence | P04 saisir objet relation | F05 KYC Case | KYC Case Service | dossier + profil/provenance | CaseProfileUpdated (CONCEPT) | AC-04 |
| FR-05 KYC case | Decision and review | P05 screening, décision, revue | F05 KYC Case, F06 Screening, F07 Manual Review | KYC Case + Manual Review | case_id, policy_version, décision et audit | ScreeningRequested/Completed, ManualReviewRequested, KycApproved/Rejected | AC-05 |
| FR-06 Consent | Consent management | P06 choisir/retrait consentement | F08 Consent | Consent Service | consent_id/finality/version/horodatage/preuve | ConsentGranted / ConsentWithdrawn | AC-06 |
| FR-07 Customer 360 | Unified customer view | P07 agréger les données | F10 Customer 360 | Customer 360 read model | ownership par attribut, fraîcheur, provenance | Customer 360 query, CustomerUpdated | AC-07 |
| FR-08 Change events | Customer integration | P08 publier changement | F12 Change, F01 Party | Party Service + Outbox adapter | mutation et outbox atomiques | CustomerUpdated v1 (REFERENCE) | AC-08 |
| FR-09 Selfcare | Self-service | P09 modification client | F12 Change + F08 Consent | BFF + Party/Consent + workflow approbation | attributs sensibles, droits et preuve | ChangeRequest API / CustomerUpdated | AC-09 |
| FR-10 Exceptions | KYC case / risk | P10 suspendre/reprendre | F07 Manual Review, F05 Case | KYC Case + Manual Review | état REVIEW/REQUEST_INFO + audit | ManualReviewRequested, CaseStatusQuery | AC-10 |

**Attention aux contrats :** `UboDeclared` et `CaseProfileUpdated` sont ici **CONCEPT** ; aucun schéma AsyncAPI public/runtime n'est déclaré existant pour eux. Les autres noms reprennent des événements de référence I04, sans réclamer de publication exécutée en Customer/KYC.

## Critères d'acceptation vérifiables (S1)

| ID | Given / When / Then attendu | Preuve requise pour passer le gate |
|---|---|---|
| AC-01 | GIVEN identité prospect absente WHEN demande validée THEN `prospect_id` est stable, pas de customer bancaire créé prématurément | API response + datastore + audit |
| AC-02 | GIVEN fournisseur indisponible WHEN vérification demandée THEN état en attente/reprise, sans identité auto-validée | code retour fournisseur + transition de cas |
| AC-03 | GIVEN UBO/mandat incomplet WHEN décision KYB demandée THEN blocage ou revue, pas d'APPROVED implicite | case + graph relationships + journal |
| AC-04 | GIVEN finalité manquante WHEN dossier soumis THEN il reste incomplet avec motif versionné | dossier + validation / erreur |
| AC-05 | GIVEN screening ambigu WHEN résultat reçu THEN statut REVIEW, décideur autorisé et piste d'audit | case state, policy version, audit de décision |
| AC-06 | GIVEN consentement accordé puis retiré WHEN lecture THEN dernier état = WITHDRAWN avec historique et finalité inchangée | consent ledger / horodatages |
| AC-07 | GIVEN valeurs contradictoires WHEN vue Customer360 appelée THEN conflit + sources/fraîcheur visibles, pas d'écrasement du maître | read model + source mapping |
| AC-08 | GIVEN retry message après commit WHEN publication rediffusée THEN mutation métier unique et consommateur idempotent | outbox/inbox, correlation_id |
| AC-09 | GIVEN modification d'attribut sensible WHEN session non réauthentifiée THEN requête non appliquée / étape MFA imposée | décision d'autorisation + audit |
| AC-10 | GIVEN validation humaine ou retour tardif WHEN case en REVIEW THEN transition autorisée seulement et aucun double Customer | state history + idempotency key |

## Cas négatifs / non-régression indispensables

- **EX-01** : timeout du Core après envoi → `UNKNOWN/PENDING_CONFIRMATION`, **pas de retry de création aveugle**, inquiry puis reconciliation ; AC-01, AC-08, AC-10.
- **EX-02** : réponse screening hors ordre → ignorée ou rattachée à la version/correlation adéquate ; AC-05.
- **EX-03** : duplication `CustomerUpdated` → consommation idempotente ; AC-08.
- **EX-04** : retrait de consentement alors que read model est en retard → statut explicable, freshness affichée, retrait non perdu ; AC-06, AC-07.
- **EX-05** : révocation mandat pendant action Selfcare → réévaluation des droits, refus et audit ; AC-03, AC-09.
- **EX-06** : fournisseur de documents indisponible → dossier interrompu avec reprise possible, pas d'APPROVED sans preuve ; AC-02, AC-05.
- **EX-07** : double décision concurrente (revue humaine/retour externe) → verrou de version / transitions déterministes ; AC-05, AC-10.
- **EX-08** : conflit MDM vs core legacy → alerte data stewardship, pas d'écrasement silencieux ; AC-07, AC-08.

## Mapping NFR / contrôle

| Contrôle | Exigences | Mesure attendue | Owner à confirmer |
|---|---|---|---|
| Données privées / finalité / consentement | FR-02, FR-06, FR-07, FR-09 | audit consent et accès ; durée de rétention à découvrir | DPO + Customer Data |
| Séparation des pouvoirs / revue | FR-03, FR-05, FR-10 | reviewers habilités, logs et SoD | Compliance / KYC |
| Idempotence / intégrité | FR-01, FR-08, FR-10 | un seul Customer malgré retries | Product owner + Dev Lead |
| Continuité / reprise / TIMEOUT | FR-02, FR-05, FR-08 | modes dégradés et récupération des cas | Application Architect + Ops |
| Cohérence / lineage | FR-07, FR-08 | mastership, provenance, fraicheur | Data / MDM Owner |

## Gate S1 et limite de vérité

- [x] Tous les FR-01…FR-10 ont une chaîne de traçabilité et un AC-01…10.
- [x] Contrats de référence vs concepts nouveaux explicitement séparés.
- [x] Exigences négatives EX-01…08 rattachées aux critères d'acceptation.
- [x] Interfaces et données relient [I03](../journeys/I03_PARCOURS_DIGITAUX.md), [I05](../architecture/I05_ARCHITECTURE_APPLICATIVE.md), [I08](../integration/I08_API_EDA_MQ_LEGACY.md).
- [ ] **NON RÉALISÉ :** exécution Customer/KYC de ces dix tests sur un runtime ; client réel, maîtres de données, métriques RTO/RPO et objectifs restent `DISCOVERY_REQUIRED`.
