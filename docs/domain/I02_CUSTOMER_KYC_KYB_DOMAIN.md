# I02 — Domaine Customer / KYC / KYB

**Statut : COMPLET — 2026-10-01**

## 1. Concepts principaux

### Party
Entité identifiée dans le SI : personne physique ou organisation.

### Prospect
Party engagée dans un parcours avant création d’une relation bancaire définitive.

### Customer
Party ayant au moins une relation client reconnue par la banque.

### Relationship
Lien entre une Party et la banque, ou entre plusieurs Parties : titulaire, représentant, mandataire, bénéficiaire effectif, dirigeant, etc.

### Identity
Ensemble d’attributs permettant d’identifier une Party et d’en vérifier certains éléments.

### KYC
Processus de connaissance et de vigilance client appliqué à une personne physique ou, selon le contexte, au détenteur/dirigeant d’une organisation.

### KYB
Extension du raisonnement de connaissance à une personne morale : existence, structure, représentants, bénéficiaires effectifs, activité.

### Consent
Autorisation explicite liée à une finalité, une version et une durée/condition.

### Evidence
Élément de preuve : document, résultat de vérification, décision, horodatage, source, journal d’audit.

## 2. Bounded contexts

```text
Identity
  |
  +--> Customer Registry
  |
  +--> KYC/KYB Case
  |
  +--> Consent
  |
  +--> Document & Evidence
  |
  +--> Screening Adapter
  |
  +--> Relationship
  |
  +--> Customer 360
```

### Identity Context
Responsable des identifiants, attributs d’identité et états de vérification.

### Customer Registry
Responsable du cycle Prospect → Customer et des identifiants internes.

### KYC/KYB Case
Responsable du dossier, de ses états, contrôles, décisions et raisons.

### Consent
Responsable du ledger de consentements.

### Document & Evidence
Responsable des métadonnées, versions, empreintes et liens de preuve.

### Screening Adapter
Façade vers sanctions/PEP/adverse media ou services équivalents. Le moteur externe n’est pas reproduit.

### Relationship
Responsable des rôles et liens Party↔Party et Party↔Bank.

### Customer 360
Vue composée ou matérialisée selon décision d’architecture ; n’est pas nécessairement le maître de toutes les données.

## 3. Machine d’état KYC de référence

```text
DRAFT
  ↓
IDENTITY_PENDING
  ↓
IDENTITY_VERIFIED
  ↓
CDD_PENDING
  ↓
SCREENING_PENDING
  ↓
REVIEW_REQUIRED ───→ REJECTED
  │
  └────────────────→ APPROVED
                        ↓
                   CUSTOMER_ACTIVE
                        ↓
                   PERIODIC_REVIEW
```

États complémentaires possibles :
- `EXPIRED`
- `SUSPENDED`
- `INCOMPLETE`
- `MANUAL_REVIEW`

Les états exacts sont `REFERENCE_ARCHITECTURE`, pas une norme réglementaire.

## 4. Mesures CDD structurantes

Le Règlement (UE) 2024/1624 prévoit notamment, dans les mesures de vigilance à l’égard de la clientèle, l’identification et la vérification du client, l’identification des bénéficiaires effectifs, la compréhension de l’objet/nature de la relation ainsi que des vérifications relatives aux sanctions ciblées.

Conséquence d’architecture :

```text
Identity Verification
      +
Beneficial Ownership
      +
Purpose / Nature
      +
Screening / Sanctions
      +
Risk & Decision
      =
Traceable KYC Case
```

## 5. Remote onboarding

Les lignes directrices EBA sur l’onboarding client à distance sont en vigueur depuis le **2 octobre 2023**. Elles sont technologiquement neutres et demandent aux établissements de disposer de politiques/processus robustes et sensibles au risque, ainsi que d’évaluer la fiabilité des solutions d’onboarding à distance.

Conséquences :

- ne pas coupler le parcours à un fournisseur unique ;
- versionner la politique appliquée ;
- conserver la provenance et la preuve ;
- prévoir des contrôles de qualité et des fallbacks ;
- permettre une revue humaine ;
- surveiller la performance du fournisseur.

## 6. Personne morale / KYB

Modèle minimal :

```text
Organisation
   |
   +--> Registration / Legal form
   +--> Registered address
   +--> Activities
   +--> Representatives
   +--> Ownership
            |
            +--> Beneficial Owners (Party)
```

L’architecture doit permettre de distinguer :
- la personne morale ;
- ses représentants ;
- les personnes qui la contrôlent ;
- les relations de délégation.

## 7. Invariants métier

1. Une preuve n’est jamais écrasée silencieusement.
2. Une décision KYC est liée à la version des données et règles utilisées.
3. Un changement sensible doit pouvoir déclencher une réévaluation.
4. Une Party peut avoir plusieurs relations.
5. Un Customer 360 ne doit pas masquer l’origine des données.
6. Un statut externe inconnu ne doit pas devenir automatiquement « OK ».
7. Une revue manuelle doit laisser une trace explicable.
8. La création du client bancaire doit être idempotente.
9. L’absence de réponse d’un système legacy doit être traitée comme ambiguïté, pas comme succès.
10. Les données collectées doivent rester liées à une finalité.

## 8. Critères de sortie I02

- [x] concepts ;
- [x] bounded contexts ;
- [x] machine d’état ;
- [x] CDD ;
- [x] remote onboarding ;
- [x] KYB ;
- [x] invariants.
