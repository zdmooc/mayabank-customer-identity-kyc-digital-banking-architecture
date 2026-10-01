# I11 — AS-IS → GAP → TARGET / DAA

**Statut : COMPLET — 2026-10-01**

## 1. Méthode

Une mission réelle commence par la découverte, pas par la cible.

```text
AS-IS
  ↓
Evidence / facts
  ↓
Gaps
  ↓
Options
  ↓
Trade-offs
  ↓
TARGET
  ↓
Transition states
  ↓
Implementation roadmap
```

## 2. AS-IS générique de référence

Exemple synthétique, non attribué à un client :

- portail digital historique ;
- CRM ;
- référentiel client ;
- KYC partiellement manuel ;
- documents dispersés ;
- API hétérogènes ;
- IBM MQ vers mainframe ;
- COBOL/CICS ;
- Oracle ;
- batch de synchronisation ;
- plusieurs sources Customer ;
- logs techniques non corrélés.

Classification : `REFERENCE_ARCHITECTURE`.

## 3. Gaps typiques

| Gap | Conséquence |
|---|---|
| plusieurs maîtres Customer | incohérence |
| KYC sans machine d’état | faible traçabilité |
| sync batch uniquement | fraîcheur faible |
| aucun event contract | couplage |
| front connaît le legacy | dette |
| pas d’ACL | modèle moderne contaminé |
| pas d’idempotence | doublons |
| consent dispersé | preuve fragile |
| Customer 360 opaque | conflits masqués |
| observabilité technique seule | incidents métier invisibles |

## 4. Options d’architecture

### Option A — Façade
Conserver le legacy maître et standardiser API/façades.

Avantages :
- risque réduit ;
- rapide.

Limites :
- dette structurelle conservée.

### Option B — Modernisation progressive
Créer bounded contexts modernes et déplacer le mastership par capacité.

Avantages :
- trajectoire incrémentale ;
- séparation claire.

Limites :
- coexistence complexe.

### Option C — Refonte complète
Remplacer les composants principaux.

Avantages :
- cible homogène.

Limites :
- risque, coût, cutover, migration.

## 5. Cible de référence

La cible retenue pour MayaBank est **Option B — modernisation progressive**.

Motifs :
- compatible avec un SI bancaire legacy ;
- évite le Big Bang ;
- permet des preuves incrémentales ;
- facilite rollback ;
- respecte la stratégie portefeuille.

## 6. États de transition

### T0
Legacy dominant.

### T1
API façade + observabilité.

### T2
Onboarding/KYC modernes ; legacy synchronisé.

### T3
Customer Registry moderne pour certains attributs.

### T4
Customer 360 event-driven.

### T5
Retrait progressif des fonctions legacy devenues inutiles.

## 7. DAA — structure

Un Dossier d’Architecture Applicative/Solution doit contenir :

1. contexte et objectifs ;
2. scope/hors-scope ;
3. exigences ;
4. AS-IS ;
5. gaps ;
6. options ;
7. critères ;
8. décision ;
9. architecture fonctionnelle ;
10. architecture applicative ;
11. architecture data ;
12. intégration ;
13. sécurité ;
14. NFR ;
15. exploitation ;
16. transition ;
17. risques ;
18. ADR ;
19. questions ouvertes ;
20. critères de sortie.

## 8. Critères d’arbitrage

- valeur métier ;
- conformité ;
- sécurité ;
- résilience ;
- intégration ;
- complexité ;
- exploitabilité ;
- réversibilité ;
- time-to-market ;
- coût ;
- dépendance fournisseur ;
- migration/cutover.

Aucun score fictif n’est appliqué sans données.

## 9. Risques

- duplication Party/Customer ;
- cutover incomplet ;
- propagation d’erreurs legacy ;
- faux positifs matching ;
- dépendance fournisseur KYC ;
- exposition PII ;
- backlog de revue ;
- incohérence événements/core ;
- règles métier non inventoriées ;
- responsabilités de mastership non tranchées.

## 10. Critères de sortie I11

- [x] méthode ;
- [x] AS-IS générique ;
- [x] gaps ;
- [x] options ;
- [x] cible ;
- [x] transitions ;
- [x] structure DAA ;
- [x] arbitrage ;
- [x] risques.
