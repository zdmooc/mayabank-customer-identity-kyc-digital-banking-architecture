# ADR-002 — Customer 360 comme read model explicable

- **Statut** : ACCEPTED
- **Date** : 2026-10-01

## Décision

Customer 360 est une **vue consolidée**, et non automatiquement le système maître de tous les attributs.

Chaque donnée critique conserve :
- source ;
- fraîcheur ;
- propriétaire ;
- règle de mastership ;
- provenance.

Un read model matérialisé peut être utilisé lorsque la latence/ disponibilité le justifie.

## Motif

Éviter qu’une vue pratique devienne un maître implicite et masque les conflits entre systèmes.
