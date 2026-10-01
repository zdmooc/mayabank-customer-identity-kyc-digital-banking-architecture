# Classification des dépendances — Customer / KYC / Digital

## Règle

Chaque dépendance suit l’une des catégories du cadrage MayaBank :

- `CONSUME_SHARED`
- `DEDICATED_FOR_TEST`
- `SPECIALIZED_PLATFORM`
- `PRODUCT_OWNED`
- `REFERENCE_ONLY`

## Matrice initiale

| Composant / capacité | Classification | Propriétaire de référence |
|---|---|---|
| OpenShift / Kubernetes | CONSUME_SHARED | Common Platform |
| Namespace / Project Factory | CONSUME_SHARED | Common Platform |
| Argo CD | CONSUME_SHARED | Common Platform |
| CI/CD contracts | CONSUME_SHARED | Common Platform |
| SonarQube | CONSUME_SHARED | Common Platform |
| Registry / Artifacts | CONSUME_SHARED | Common Platform |
| OpenTelemetry | CONSUME_SHARED | Common Platform |
| Prometheus / Grafana | CONSUME_SHARED | Common Platform |
| Secrets / Vault patterns | CONSUME_SHARED | Common Platform |
| Keycloak/OIDC runtime | CONSUME_SHARED | IAM/Common Platform |
| CIAM policies et journeys | PRODUCT_OWNED | Customer/KYC Product |
| API Management | SPECIALIZED_PLATFORM | API Management |
| Kafka standard eventing | CONSUME_SHARED | Shared Technical Platform |
| Kafka HA specialist tests | SPECIALIZED_PLATFORM | Kafka specialist |
| IBM MQ | SPECIALIZED_PLATFORM | IBM MQ specialist |
| PostgreSQL standard | CONSUME_SHARED | Shared Data Services |
| Customer schema / DB ownership | PRODUCT_OWNED | Customer/KYC Product |
| Oracle/Exadata patterns | REFERENCE_ONLY | Oracle specialist |
| Customer 360 | PRODUCT_OWNED | Customer/KYC Product |
| KYC case state | PRODUCT_OWNED | Customer/KYC Product |
| Consent ledger | PRODUCT_OWNED | Customer/KYC Product |
| Document metadata | PRODUCT_OWNED | Customer/KYC Product |
| Core banking legacy | REFERENCE_ONLY | External/Core domain |
| COBOL/CICS adapters | PRODUCT_OWNED / REFERENCE_ONLY | Integration boundary |
| DORA method | REFERENCE_ONLY | DORA masterbook |
| TOGAF/ArchiMate/HOPEX | REFERENCE_ONLY | EA repositories |

## Exception DEDICATED_FOR_TEST

Une instance dédiée n’est créée que si le composant lui-même est l’objet de la preuve :

- test de HA PostgreSQL ;
- test de Kafka HA ;
- test de MQ Native HA ;
- test de failover CIAM ;
- test de performance/isolation nécessitant un environnement dédié.

Sinon, le produit consomme la capacité commune.

## Conséquence

Le dépôt KYC doit rester lisible comme **produit métier et architecture Solution**, et non comme une plateforme technique autonome.
