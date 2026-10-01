# Threat Model — Customer / KYC / Digital

## Assets critiques

- identités ;
- documents ;
- résultats de vérification ;
- décision KYC ;
- relations et mandats ;
- consentements ;
- tokens/sessions ;
- données Customer 360 ;
- journaux d’audit.

## Trust boundaries

1. navigateur/mobile ↔ edge ;
2. edge ↔ API/BFF ;
3. API ↔ services ;
4. services ↔ providers externes ;
5. services modernes ↔ legacy ;
6. service ↔ event bus ;
7. application ↔ data stores ;
8. production ↔ opérations/admin.

## Scénarios prioritaires

### T01 — prise de contrôle de compte
Impact : accès aux données/modifications frauduleuses.

### T02 — usurpation d’identité lors de l’onboarding
Impact : création d’une relation sous fausse identité.

### T03 — modification illicite de bénéficiaire effectif
Impact : contournement KYB/AML.

### T04 — replay d’une commande Customer
Impact : doublon ou incohérence core.

### T05 — fuite de PII dans logs/events
Impact : exposition massive et non maîtrisée.

### T06 — bypass de revue manuelle
Impact : décision non conforme au workflow.

### T07 — fournisseur KYC compromis/indisponible
Impact : résultat faux ou blocage du parcours.

### T08 — divergence Customer/Core
Impact : informations contradictoires visibles au client/conseiller.

### T09 — privilège B2B excessif
Impact : action d’un utilisateur hors mandat.

### T10 — suppression/altération de preuve
Impact : impossibilité d’expliquer une décision.

## Contrôles transverses

- strong identity ;
- least privilege ;
- segregation of duties ;
- immutable/auditable evidence ;
- schema validation ;
- idempotency ;
- reconciliation ;
- policy versioning ;
- encryption ;
- secrets management ;
- dependency isolation ;
- observability ;
- incident response.
