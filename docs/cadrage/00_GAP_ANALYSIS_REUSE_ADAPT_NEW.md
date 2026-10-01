# I00 — Gap Analysis & matrice REUSE / ADAPT / NEW

## 1. Pourquoi ce dépôt existe

Le portefeuille MayaBank couvre déjà fortement :

- paiements instantanés et Wero ;
- card processing ;
- IBM MQ / JMS ;
- Kafka / EDA ;
- API Management ;
- OpenShift / Kubernetes ;
- Azure ;
- Oracle / Exadata ;
- résilience / DORA ;
- TOGAF / ArchiMate / HOPEX ;
- Data / AI / Agentic AI.

Le gap identifié porte sur la **Connaissance Client et les parcours digitaux bancaires de bout en bout** :

- Customer / Party / Relationship ;
- Customer 360 / Golden Record ;
- entrée en relation ;
- identité client ;
- KYC / KYB ;
- consentement / préférences ;
- screening et contrôles AML comme frontières ;
- parcours B2C / B2B ;
- selfcare ;
- architecture de transition entre digital/API/EDA et legacy bancaire.

Ce gap est distinct des plateformes techniques existantes et justifie un spécialiste d’architecture SI.

## 2. Signal déclencheur

Un signal de mission public du marché freelance demande un Architecte SI Banque sur le périmètre **Connaissance Client & Digital**, avec architecture générale/détaillée, NFR, urbanisme, Java/API, Kafka, MQ, Oracle, cloud et coexistence avec des technologies legacy.

Classification : **MARKET_SIGNAL**.

Aucune architecture interne d’un client n’est déduite de ce signal.

## 3. REUSE / ADAPT / NEW

| Domaine | Actif existant | Décision | Justification |
|---|---|---|---|
| OpenShift/Kubernetes | OpenShift blueprints / shared platform | REUSE | plateforme d’exécution commune |
| GitOps | Argo CD assets | REUSE | ne pas dupliquer la chaîne GitOps |
| CI/CD | GitHub Actions + cible GitLab | REUSE | portabilité plutôt que duplication |
| SonarQube / quality gates | plateforme commune | REUSE | shared capability |
| Observabilité | OTel / Prometheus / Grafana | REUSE | instrumentation standard |
| IAM technique | Keycloak / IAM repositories | ADAPT | devient CIAM côté client |
| OAuth2/OIDC | API Management / IAM | REUSE | standards communs |
| API Gateway / API lifecycle | mayabank-api-management-architecture | REUSE | spécialiste existant |
| Kafka / EDA | Kafka/DDD + plateformes paiements | REUSE | eventing générique |
| IBM MQ / JMS | IBM MQ Native HA | REUSE | intégration critique/legacy |
| PostgreSQL | plateforme commune | REUSE | service générique |
| Oracle / Exadata | Exadata specialist | REFERENCE | data/HA patterns |
| TOGAF | référentiel TOGAF | REUSE | méthode |
| ArchiMate | masterbook ArchiMate | REUSE | modélisation |
| HOPEX | masterbook HOPEX | REUSE | gouvernance/référentiel |
| DORA | masterbook DORA | REUSE | résilience transverse |
| Customer master model | absent | NEW | cœur fonctionnel |
| Customer 360 | absent | NEW | cœur fonctionnel/data |
| KYC / KYB orchestration | absent | NEW | capacité métier principale |
| Consent management | partiel | NEW/ADAPT | capacité client et conformité |
| Digital onboarding | absent | NEW | parcours métier |
| Selfcare B2C/B2B | absent | NEW | parcours digital |
| Customer lifecycle | absent | NEW | bounded context produit |
| Legacy customer coexistence | patterns génériques existants | ADAPT | appliquer strangler/façade/event interception au domaine client |

## 4. Ce que le dépôt ne doit pas contenir

Le dépôt ne doit pas devenir :

- une nouvelle plateforme OpenShift ;
- un deuxième dépôt Kafka ;
- un deuxième dépôt IBM MQ ;
- une nouvelle usine CI/CD ;
- un clone de plateforme IAM générique ;
- une reproduction d’un SI bancaire réel ;
- une collection de produits installés uniquement pour afficher des logos.

## 5. Ce que le dépôt doit posséder

Le dépôt est propriétaire de :

- modèle de capacités Customer/KYC/Digital ;
- parcours d’entrée en relation ;
- parcours selfcare ;
- modèle informationnel client ;
- règles de mastership / golden record ;
- bounded contexts Customer / Identity / KYC / Consent / Document / Relationship ;
- contrats d’intégration de référence ;
- états de dossier KYC ;
- exigences NFR propres au produit ;
- architecture de transition AS-IS → TARGET ;
- décisions d’architecture ;
- scénarios d’échec fonctionnels et techniques ;
- cartographies ArchiMate/C4 adaptées au domaine.

## 6. Decision Gate

### Critère 1 — capacité absente ?

**OUI.** Le portefeuille ne contient pas de spécialiste Customer/KYC/Digital de bout en bout.

### Critère 2 — signal marché réel ?

**OUI.** Une mission récente cible explicitement ce domaine.

### Critère 3 — duplication évitable ?

**OUI.** Les services techniques sont consommés depuis les dépôts/platforms existants.

### Critère 4 — isolation métier justifiée ?

**OUI.** Customer/KYC/Digital possède ses propres capacités, données, parcours, exigences et trajectoires legacy.

## 7. Décision

**APPROUVER** le dépôt :

`zdmooc/mayabank-customer-identity-kyc-digital-banking-architecture`

Positionnement :

**P1 MISSION-ALIGNED / SOLUTION & BUSINESS ARCHITECTURE SPECIALIST**

Le dépôt est d’abord un **référentiel d’architecture SI**. Un runtime ne sera créé que pour fermer un gap de preuve précis.

## 8. Critères de sortie I00

- [x] gap métier explicite ;
- [x] signal déclencheur identifié ;
- [x] matrice REUSE / ADAPT / NEW ;
- [x] frontières de responsabilités ;
- [x] classification des dépendances ;
- [x] roadmap I00→I12 ;
- [ ] synchronisation complète du dépôt P0 `cadrage_202682030`.

