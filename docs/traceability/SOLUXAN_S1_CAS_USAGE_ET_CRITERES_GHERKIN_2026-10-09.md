# SOLUXAN-S1 — Scénarios métier et critères Gherkin de référence

**Type : ACCEPTANCE_SPECIFICATION / NOT_EXECUTED**. Complète la [matrice S1](SOLUXAN_S1_EXIGENCES_PROCESSUS_APPLICATION_TESTS_2026-10-09.md). Les scénarios servent de support à un atelier PO/BA/QA/Architecte, non de preuve de conformité ou d'exécution.

## UC-01 — Entrée en relation B2C avec revue humaine

```gherkin
Feature: Entrée en relation Customer/KYC

  Scenario: Parcours nominal
    Given un prospect avec identifiant de demande stable
    And une preuve d'identité recevable selon une politique versionnée
    When la décision KYC est APPROVED
    And les consentements requis pour les finalités concernées sont tracés
    Then une seule identité Customer est créée par la fonction maître désignée
    And le dossier conserve la version de politique et la provenance des preuves

  Scenario: Screening ambigu
    Given un dossier KYC à l'état SCREENING_PENDING
    When le fournisseur retourne un résultat ambigu
    Then le dossier passe en MANUAL_REVIEW
    And aucun Customer définitif n'est créé par cet événement
    And une tâche est confiée à un reviewer habilité
    And la décision finale est auditable

  Scenario: Retour core inconnu
    Given une création Customer transmise au legacy avec customer_creation_id stable
    When la réponse du legacy expire après l'envoi
    Then le dossier est PENDING_CONFIRMATION ou UNKNOWN
    And aucune nouvelle création aveugle n'est émise
    And une inquiry ou une reconciliation détermine le résultat
    And un retry client conserve le même identifiant métier

  Scenario: Rejeu d'événement
    Given un CustomerUpdated déjà appliqué et journalisé
    When le même event_id est reçu à nouveau
    Then le consommateur ne crée aucune seconde mutation métier
    And la corrélation et l'accusé sont conservés
```

## UC-02 — Consentement et Selfcare

```gherkin
Feature: Consentement et changement des données sensibles

  Scenario: Retrait de consentement
    Given un consentement versionné actif avec finalité explicite
    When le client retire ce consentement
    Then le nouvel état WITHDRAWN est persisté avec la preuve de retrait
    And l'historique d'accord reste reconstituable
    And la vue Customer360 indique sa date de fraîcheur

  Scenario: Mandat révoqué
    Given un utilisateur délégué à une entreprise
    And son mandat a été révoqué avant validation de la modification
    When il tente de modifier un attribut sensible
    Then l'autorisation est refusée
    And aucune modification Customer n'est commise
    And un audit du refus est disponible
```

## Atelier et décision

1. **Business Owner** : confirme les cas d'usage et variantes bancaires.
2. **Compliance/DPO** : confirme contrôles, SoD, finalités et règles de rétention.
3. **Data Owner** : confirme Customer mastership, identifiants, golden record.
4. **Architecte Solution** : relie applications, contracts, timeouts et ADR.
5. **QA / Dev** : dérive tests exécutables, jeux de données synthétiques, assertions et evidence IDs.
6. **Ops** : valide runbooks, SLO et alertes sur exceptions.

**Aucune étape ci-dessus n'est présentée comme une validation réelle des interlocuteurs d'une banque.**
