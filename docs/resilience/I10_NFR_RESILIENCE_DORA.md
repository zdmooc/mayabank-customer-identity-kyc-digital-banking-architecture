# I10 — NFR, résilience, DORA et observabilité

**Statut : COMPLET — 2026-10-01**

## 1. Principes NFR

Les exigences non fonctionnelles sont des exigences d’architecture, pas une annexe tardive.

Catégories :
- disponibilité ;
- continuité ;
- intégrité ;
- performance ;
- sécurité ;
- confidentialité ;
- auditabilité ;
- maintenabilité ;
- observabilité ;
- capacité ;
- réversibilité ;
- testabilité.

## 2. Services critiques

Classification de référence :

| Service | Criticité indicative | Dégradation acceptable |
|---|---|---|
| Authentification client | élevée | très limitée |
| Consultation Customer 360 | élevée | read-only dégradé possible |
| Onboarding | élevée | reprise différée possible |
| KYC decision | élevée | file/revue différée possible selon cas |
| Screening externe | dépendance critique | circuit + attente explicite |
| Consent ledger | élevée | aucune perte silencieuse |
| Notification | moyenne | asynchrone/retry |
| Analytics | faible à moyenne | différable |

Les valeurs exactes sont `DISCOVERY_REQUIRED`.

## 3. Failure model

### Provider identité indisponible
- circuit breaker ;
- état `IDENTITY_PENDING` ;
- reprise ;
- autre fournisseur uniquement si politique autorise ;
- pas de validation implicite.

### Screening indisponible
- dossier suspendu ;
- retry borné ;
- alerting ;
- revue ou attente selon politique.

### Kafka indisponible
- transaction locale + Outbox ;
- pas de perte de mutation ;
- publication différée.

### MQ/Core indisponible
- statut `CORE_SYNC_PENDING` ;
- retry contrôlé ;
- inquiry/reconciliation ;
- pas de double création.

### Customer 360 indisponible
- accès source si prévu ;
- réponse partielle explicitement marquée ;
- pas de donnée inventée.

## 4. RTO / RPO

Aucune valeur n’est inventée.

Le processus d’architecture doit produire :

| Capability | RTO | RPO | Dépendances | Mode dégradé |
|---|---|---|---|---|
| CIAM | TBD | TBD | IdP/DB | selon design |
| Onboarding | TBD | TBD | KYC/providers | reprise dossier |
| Customer Registry | TBD | TBD | DB | lecture limitée |
| Consent | TBD | TBD | DB | blocage mutation |
| Customer 360 | TBD | TBD | events/sources | stale/partial explicite |
| Integration Core | TBD | TBD | MQ/core | pending/reconcile |

## 5. DORA — traduction architecture

Le règlement DORA est traité comme une qualité transverse :

- cartographier les fonctions et services critiques ;
- identifier dépendances ICT et tiers ;
- tester résilience et recovery ;
- mesurer incidents ;
- gérer risques fournisseurs ;
- prévoir exit/reversibility ;
- maintenir des preuves.

Le dépôt ne revendique aucune certification DORA.

## 6. Observabilité

### Technical telemetry
- traces OpenTelemetry ;
- RED metrics API ;
- saturation/resources ;
- queue lag ;
- DB pool ;
- provider latency/error.

### Business telemetry
- onboarding started/completed ;
- KYC pending/approved/rejected ;
- manual review backlog ;
- core sync pending ;
- duplicate candidate rate ;
- consent changes ;
- periodic review overdue.

## 7. SLO candidates

Les indicateurs candidats :
- disponibilité APIs ;
- p95/p99 latence ;
- succès login ;
- temps de traitement onboarding ;
- délai de publication event ;
- backlog Outbox ;
- backlog KYC ;
- échecs sync legacy ;
- âge maximal Customer 360.

Seuils = `DISCOVERY_REQUIRED`.

## 8. Chaos / tests

Tests à prévoir si runtime futur :
- provider timeout ;
- DB restart ;
- broker unavailable ;
- duplicate message ;
- delayed message ;
- MQ timeout-after-effect ;
- stale read model ;
- revoked token ;
- node/pod replacement ;
- expired document;
- concurrent customer update.

## 9. Critères de sortie I10

- [x] criticité ;
- [x] failure model ;
- [x] RTO/RPO method ;
- [x] DORA ;
- [x] observabilité ;
- [x] SLO candidates ;
- [x] test catalogue.
