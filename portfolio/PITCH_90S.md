# Pitch 90 secondes — Architecte SI Banque Customer/KYC/Digital

Je traite le domaine Connaissance Client comme une chaîne d’architecture complète, et pas comme un simple formulaire KYC.

Je commence par distinguer Party, Prospect, Customer, Identity, Relationship, Consent et dossier KYC. Je modélise ensuite les parcours B2C et B2B, y compris les bénéficiaires effectifs, les cas de revue humaine et le selfcare.

Au niveau applicatif, je sépare les bounded contexts Customer, KYC, Consent, Relationship, Evidence et Customer 360. Les canaux passent par BFF/API Management ; les événements servent à propager les changements ; IBM MQ reste pertinent pour certaines intégrations transactionnelles avec le legacy.

Je traite explicitement l’idempotence, les timeouts ambigus, l’Outbox et la reconciliation pour éviter les doublons et divergences avec le core bancaire.

Je ne propose pas un Big Bang. Je pars de l’AS-IS, mesure les gaps puis construis une trajectoire Strangler : façade, nouveaux parcours, déplacement progressif du mastership et retrait contrôlé du legacy.

Enfin, j’intègre CIAM, privacy, sécurité, observabilité, RTO/RPO, dépendances tiers et DORA dès le dossier d’architecture.

Le résultat est une architecture Solution de bout en bout : métier, fonctionnel, applicatif, data, intégration, technique et transformation.
