# MayaBank — Architecture Connaissance Client, KYC & Banque Digitale

> Référentiel d’architecture SI bancaire en français — Customer / KYC / KYB / Digital Onboarding / Selfcare B2C-B2B / CIAM / API / EDA / Legacy Modernization.

## Statut

**BASELINE I00→I12 : COMPLETE / PORTFOLIO READY**

Ce dépôt est créé à la suite d’un signal de mission concret portant sur un rôle d’Architecte SI Banque dans le domaine **Connaissance Client & Digital**.

Il ne reproduit l’architecture interne d’aucun établissement bancaire. Il constitue une **architecture de référence MayaBank**, synthétique, indépendante et démontrable.

## Objectif

Compléter le portefeuille MayaBank sur une capacité jusque-là insuffisamment couverte :

- Connaissance Client / Customer 360 ;
- entrée en relation digitale ;
- KYC / KYB ;
- AML / screening comme frontières d’intégration ;
- consentements et préférences ;
- CIAM / identité client ;
- parcours B2C et B2B ;
- selfcare ;
- architecture fonctionnelle, applicative, data et technique ;
- intégration API / événements / messaging ;
- coexistence et modernisation du legacy bancaire ;
- NFR, résilience, sécurité, observabilité et DORA ;
- trajectoire AS-IS → GAP → TARGET.

## Positionnement dans le portefeuille MayaBank

```text
cadrage_202682030                         [P0 — pilotage]
        |
        +--> shared-platform-services-openshift [P0 — plateforme commune]
        |
        +--> plateformes spécialistes
        |      +--> API Management
        |      +--> Kafka / EDA
        |      +--> IBM MQ
        |      +--> Oracle / Exadata
        |      +--> IAM / Keycloak patterns
        |
        +--> mayabank-customer-identity-kyc-digital-banking-architecture
               [BUSINESS / SOLUTION ARCHITECTURE SPECIALIST]
```

Le dépôt KYC **consomme et référence** les capacités techniques communes. Il ne doit pas recréer une plateforme complète par produit.

## Règle REUSE / ADAPT / NEW

| Capacité | Décision |
|---|---|
| OpenShift/Kubernetes | REUSE / CONSUME_SHARED |
| GitOps / Argo CD | REUSE / CONSUME_SHARED |
| Observabilité OTel/Prometheus/Grafana | REUSE / CONSUME_SHARED |
| IAM technique / OIDC / OAuth2 | REUSE + ADAPT au CIAM |
| Kafka / EDA | REUSE |
| IBM MQ / JMS | REUSE / SPECIALIZED_PLATFORM |
| API Management | REUSE / SPECIALIZED_PLATFORM |
| Oracle / Exadata | REFERENCE_ONLY |
| Customer 360 / modèle client | NEW / PRODUCT_OWNED |
| KYC / KYB / onboarding | NEW / PRODUCT_OWNED |
| Consentements / préférences | NEW / PRODUCT_OWNED |
| Parcours B2C/B2B / selfcare | NEW / PRODUCT_OWNED |
| Architecture de transition legacy | ADAPT + NEW |

Voir : [Gap Analysis REUSE / ADAPT / NEW](docs/cadrage/00_GAP_ANALYSIS_REUSE_ADAPT_NEW.md).

## Couches d’architecture couvertes

```text
Métier / capacités
      ↓
Architecture fonctionnelle
      ↓
Architecture applicative
      ↓
Architecture de l’information et des données
      ↓
Architecture d’intégration
      ↓
Architecture technique
      ↓
Sécurité / NFR / résilience / observabilité
      ↓
AS-IS → GAP → TARGET
      ↓
Trajectoire / DAA / ADR
```

## Principes

1. **Capability before product** : raisonner en capacités avant les produits.
2. **REUSE / ADAPT / LINK avant duplication**.
3. **Produit métier ≠ plateforme technique complète**.
4. **API + événements + messaging** selon le besoin, pas par préférence technologique.
5. **Legacy coexistence before big-bang replacement**.
6. **Security & privacy by design**.
7. **Customer mastership explicite** : ownership, golden record, synchronisation et qualité de données doivent être décidés.
8. **Toute revendication runtime exige une preuve**.
9. **Aucune architecture interne client n’est inférée**.
10. **AS-IS → GAP → TARGET** avant toute recommandation de transformation.

## Classification de vérité

| Label | Sens |
|---|---|
| `PUBLIC_VERIFIED` | Information vérifiée sur une source publique primaire ou fiable |
| `MARKET_SIGNAL` | Signal de mission / recrutement public |
| `REFERENCE_ARCHITECTURE` | Choix d’architecture MayaBank |
| `RUNTIME_PROVEN` | Preuve réellement exécutée et référencée |
| `DISCOVERY_REQUIRED` | Donnée à établir en mission |
| `INFERRED` | Hypothèse explicitement identifiée comme telle |

## Roadmap

| Itération | Contenu | Statut |
|---|---|---|
| I00 | Gap analysis, REUSE/ADAPT/NEW, frontières, inscription portfolio | **COMPLET** |
| I01 | Scope, parties prenantes, exigences, truth boundaries | **COMPLET** |
| I02 | Customer / KYC / KYB / AML / consentement | **COMPLET** |
| I03 | Parcours onboarding, B2C, B2B, selfcare | **COMPLET** |
| I04 | Capability Map + architecture fonctionnelle | **COMPLET** |
| I05 | Architecture applicative + bounded contexts | **COMPLET** |
| I06 | Customer 360 / MDM / architecture data | **COMPLET** |
| I07 | CIAM / IAM / sécurité / RGPD | **COMPLET** |
| I08 | API / Kafka / MQ / COBOL-CICS / legacy modernization | **COMPLET** |
| I09 | Architecture technique et consommation plateforme commune | **COMPLET** |
| I10 | NFR / résilience / DORA / observabilité | **COMPLET** |
| I11 | AS-IS → GAP → TARGET / trajectoire / DAA / ADR | **COMPLET** |
| I12 | Pack professionnel A3 / pitch / démo d’architecture | **COMPLET** |

## Livrable final attendu

Un dossier d’architecture SI bancaire capable de démontrer, de manière cohérente et traçable :

- la compréhension du domaine Customer/KYC/Digital ;
- la conception d’un parcours d’entrée en relation ;
- le passage du métier au fonctionnel, à l’applicatif, à la donnée et à la technique ;
- l’intégration d’un SI moderne avec un core bancaire legacy ;
- la maîtrise des NFR et de la résilience ;
- la construction d’une trajectoire de transformation réaliste ;
- la production de décisions d’architecture documentées.

---

**Langue de travail : français.**

## Statut final de la baseline

- **I00→I12 : COMPLET**
- **Architecture : REFERENCE_ARCHITECTURE**
- **Portfolio : PORTFOLIO_READY**
- **Runtime lourd dédié : non requis pour la baseline**
- **Shared Platform consumer profile : IMPLEMENTED / STATIC_VALIDATED** (`platform-consumption/`)
- **Prochaine extension runtime : uniquement sur signal mission ou gap de preuve**

Voir [I12 Acceptance](docs/evidence/I12_ACCEPTANCE.md), [Mission Alignment](portfolio/MISSION_ALIGNMENT.md) et [Shared Platform Consumption Profile](platform-consumption/README.md).

---

## Complément Soluxan — S1/S2/S3 du 09/10/2026

**Status : REFERENCE_ARCHITECTURE / INTERVIEW_PACK_READY.** Les itérations S1/S2/S3 ajoutent une traçabilité complète FR-01…FR-10 / AC-01…AC-10, un dossier de choix Customer↔core legacy A/B/C, un cas logiciel Payment UNKNOWN lié aux preuves existantes, un modèle BPMN 2.0 documentaire dans HOPEX et un pack entretien de 10 minutes. **Ce pack ne change pas la baseline I00→I12**, ne déclenche aucun runtime Customer/KYC et ne prouve aucune architecture client.

- [Index des preuves S1/S2/S3](docs/INDEX.md#soluxan--architecture-fonctionnelle--applicative-bancaire-s1s3-2026-10-09)
- [Dossier d'entretien Soluxan](portfolio/SOLUXAN_S3_DOSSIER_ENTRETIEN_ARCHITECTE_SI_2026-10-09.md)

