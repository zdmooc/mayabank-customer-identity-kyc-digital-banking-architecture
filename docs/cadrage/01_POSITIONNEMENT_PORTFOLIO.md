# Positionnement Portfolio — Customer / KYC / Digital

## Rôle

Ce dépôt est un **spécialiste d’architecture Solution / SI bancaire**.

Il couvre une zone jusqu’ici insuffisamment représentée dans le portefeuille : le passage d’un besoin métier Customer/KYC/Digital vers une architecture fonctionnelle, applicative, data, intégration et technique.

## Relation aux actifs existants

```text
                         cadrage_202682030
                               |
                 +-------------+-------------+
                 |                           |
      Common Technical Platform     Enterprise Architecture
                 |                   TOGAF / ArchiMate / HOPEX
                 |                           |
      +----------+-----------+               |
      |          |           |               |
     API       Kafka        MQ               |
      |          |           |               |
      +----------+-----------+---------------+
                         |
                         v
          Customer / Identity / KYC / Digital
                         |
          +--------------+--------------+
          |              |              |
       B2C/B2B        Selfcare      Onboarding
          |              |              |
          +--------------+--------------+
                         |
                  Core / Legacy
```

## Frontières

### Ce dépôt possède

- Customer / Party / Relationship ;
- Identity business model ;
- KYC/KYB lifecycle ;
- consent and preference management ;
- onboarding orchestration ;
- customer lifecycle ;
- Customer 360 / mastership ;
- selfcare journeys ;
- product-specific NFR ;
- architecture transition.

### Ce dépôt consomme

- OpenShift/Kubernetes ;
- GitOps ;
- CI/CD ;
- observability ;
- common IAM runtime ;
- messaging ;
- API management ;
- common data services.

### Ce dépôt référence

- DORA ;
- TOGAF ;
- ArchiMate ;
- HOPEX ;
- Oracle/Exadata ;
- cloud architecture patterns ;
- legacy modernization patterns.

## Positionnement cible

**P1 MISSION-ALIGNED / SOLUTION & BUSINESS ARCHITECTURE SPECIALIST**

Valeur attendue : démontrer qu’un architecte peut partir du métier et construire le lien jusqu’à la technologie, sans réduire l’architecture à Kubernetes ou à un catalogue de produits.
