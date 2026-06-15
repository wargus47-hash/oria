# ORIA — ROADMAP 24 MOIS & BACKLOG COMPLET
## *MVP → V1 → V2 → V3 · User Stories · KPIs · Version 1.0*

---

## 1. VUE D'ENSEMBLE — ROADMAP 24 MOIS

```
M0    M1    M2    M3    M4    M5    M6    M7    M8    M9    M10   M11   M12
│─────│─────│─────│─────│─────│─────│─────│─────│─────│─────│─────│─────│
│           MVP (M0-M6)          │          V1 (M6-M12)              │
│  Design + Dev + Alpha Testing  │  Lancement + Itération + 100 prat │
│────────────────────────────────│───────────────────────────────────│

M12   M13   M14   M15   M16   M17   M18   M19   M20   M21   M22   M23   M24
│─────│─────│─────│─────│─────│─────│─────│─────│─────│─────│─────│─────│
│          V2 (M12-M18)              │          V3 (M18-M24)              │
│  Consolidation + 200 praticiens    │  Scale + International + 300 prat  │
│────────────────────────────────────│────────────────────────────────────│
```

---

## 2. MVP (M0 → M6) — "Le Cœur Fonctionnel"

### Objectif MVP

Avoir une application fonctionnelle permettant à 20 praticiens early adopters d'accompagner leurs participants à travers les 8 phases, avec Mandala Vivant et Dossier d'Âme.

**Critère de succès MVP :**
- 20 praticiens actifs
- 50 participants en parcours
- 3 Dossiers d'Âme générés
- NPS praticien > 50
- Taux de rétention praticien M1 > 80%

---

### Jalons MVP

**M0 : Préparation**
- Wireframes validés (Figma)
- Design System ORIA créé (couleurs, typographie, composants)
- Architecture technique validée
- Environnement de développement configuré
- Schéma de base de données initial

**M1-M2 : Foundation**
- Auth complète (inscription, connexion, refresh token)
- Onboarding praticien
- Gestion participants (invitations, liste, profil)
- Parcours de phases (structure, validation)

**M3-M4 : Core Features**
- Journal participant (écriture, partage)
- Exercices — 3 phases complètes (0, 1, 2)
- Notes de séance praticien
- Messagerie basique (async)
- Mandala Vivant v1 (structure, couleurs, illumination)

**M5 : Completion**
- Exercices — toutes les phases (3, 4, 5, 6, 7)
- Attribution des Archétypes
- Ressources (médiathèque basique)
- Dossier d'Âme PDF (génération automatique)
- Export Mandala

**M6 : Alpha Testing + Launch**
- Tests avec 20 early adopters
- Corrections bugs
- Stripe intégré (paiement)
- Lancement plan SOLO

---

### Features MVP — Liste complète

```
AUTH
  ✓ Inscription praticien (avec validation manuelle Admin)
  ✓ Inscription participant (via invitation)
  ✓ Login / Logout
  ✓ Refresh token
  ✓ Réinitialisation mot de passe
  ✓ Vérification email

PRATICIEN — GESTION PARTICIPANTS
  ✓ Inviter un participant par email
  ✓ Liste des participants (vue tableau)
  ✓ Fiche participant (vue détaillée)
  ✓ Archiver un participant
  ✓ Notes internes sur le participant

PRATICIEN — PARCOURS
  ✓ Vue des phases du participant
  ✓ Valider une phase
  ✓ Activer la Phase 2.5 (Désert)
  ✓ Débloquer la phase suivante

PRATICIEN — SÉANCES
  ✓ Créer une note de séance
  ✓ Champs structurés (date, phase, observations, tags)
  ✓ Partie privée / partageable
  ✓ Partager une note au participant

PARTICIPANT — ESPACE PERSONNEL
  ✓ Dashboard participant
  ✓ Vue du parcours (phases)
  ✓ Exercices de la phase active
  ✓ Journal (écriture, partage, historique)
  ✓ Messagerie avec praticien

MANDALA VIVANT
  ✓ Rendu SVG interactif (6 anneaux + centre)
  ✓ États visuels (locked / active / validated)
  ✓ Choix de couleur participant
  ✓ Illumination sur validation praticien
  ✓ Animation d'illumination
  ✓ Symboles praticien (bibliothèque basique)
  ✓ Export PNG

DOSSIER D'ÂME
  ✓ Construction progressive (par phase)
  ✓ Génération PDF (template ORIA standard)
  ✓ Stockage S3
  ✓ Téléchargement participant

RESSOURCES
  ✓ Médiathèque globale (articles, exercices PDF)
  ✓ Filtrage par phase
  ✓ Upload praticien (fichiers perso)

ADMINISTRATION
  ✓ Validation manuelle des comptes praticiens
  ✓ Gestion des utilisateurs (admin)
  ✓ Logs d'audit basiques

PAIEMENT
  ✓ Stripe — plan SOLO (mensuel + annuel)
  ✓ Portail client Stripe
  ✓ Webhooks (suspension si paiement échoué)
```

---

## 3. V1 (M6 → M12) — "La Plateforme Pro"

### Objectif V1

Atteindre 100 praticiens actifs payants. Lancer le plan PRO et la certification Niveau 1. Améliorer la rétention participant.

**Critère de succès V1 :**
- 100 praticiens actifs (dont 30% PRO)
- 10 certifications Niveau 1 délivrées
- 500 participants actifs
- MRR > 7 500 €
- Taux de rétention praticien M6 > 85%

---

### Features V1

```
PRATICIEN — AMÉLIORATIONS
  ○ Dashboard analytique (stats participants, phases, temps)
  ○ Agenda intégré (disponibilités + prise de rendez-vous)
  ○ Intégration Google Calendar
  ○ Modèles de notes de séance (templates par phase)
  ○ Synthèses automatiques (résumé structuré de la phase)
  ○ Co-branding Dossier d'Âme (logo praticien — plan PRO)
  ○ Export données participants (CSV/JSON)

PARTICIPANT — AMÉLIORATIONS
  ○ Notifications push mobile (PWA)
  ○ Rappels d'exercices
  ○ Historique des séances (chronologie)
  ○ Ressources audio guidés (lecteur intégré)
  ○ Téléchargement offline des audios (PWA)
  ○ Onboarding amélioré (5 étapes guidées)

MANDALA — V2
  ○ Bibliothèque de symboles étendue (+30 symboles)
  ○ Labels personnalisés pour chaque anneau
  ○ Export carte numérique (format 1080x1080)
  ○ Partage lien public (durée limitée)
  ○ Animations raffinées (micro-interactions)

CERTIFICATION — NIVEAU 1
  ○ Plateforme de formation en ligne (modules vidéo)
  ○ Exercices et quiz par module
  ○ Suivi de progression candidat
  ○ Validation superviseur (workflow)
  ○ Génération badge de certification
  ○ Page praticien publique (profil certifié)

PAIEMENT
  ○ Plan PRO (119 €/mois)
  ○ Plan annuel avec réduction
  ○ Participants supplémentaires (facturation à l'usage)
  ○ Dossiers d'Âme supplémentaires (plan SOLO)
  ○ Paiement certification (990 €)

SÉCURITÉ & CONFORMITÉ
  ○ Chiffrement at-rest des données sensibles
  ○ 2FA praticien (TOTP)
  ○ Politique confidentialité complète
  ○ Export données (droit portabilité RGPD)
  ○ Suppression de compte complète

ADMIN
  ○ Dashboard admin complet
  ○ Métriques business (MRR, churn, NPS)
  ○ Gestion des certifications
  ○ Support intégré (chat admin → praticien)
```

---

## 4. V2 (M12 → M18) — "La Communauté"

### Objectif V2

Atteindre 200 praticiens. Lancer la supervision. Renforcer la communauté. Premier plan ÉCOLE.

**Critère de succès V2 :**
- 200 praticiens (dont 50% PRO)
- 3 superviseurs actifs
- 2 clients ÉCOLE
- MRR > 18 000 €
- Certification Niveau 2 ouverte

---

### Features V2

```
SUPERVISION
  ○ Dashboard superviseur
  ○ Vue anonymisée des parcours praticiens supervisés
  ○ Sessions de supervision (planification, comptes-rendus)
  ○ Certification Niveau 2 (modules + validation)
  ○ Forum privé superviseurs

COMMUNAUTÉ PRATICIENS
  ○ Espace communauté (forum segmenté par niveau)
  ○ Annuaire des praticiens certifiés (public)
  ○ Système de parrainage (tracking + attribution)
  ○ Événements (webinaires, supervisions de groupe)
  ○ Bibliothèque communautaire (ressources partagées par praticiens)

PLAN ÉCOLE
  ○ Dashboard multi-praticiens (vue école)
  ○ Gestion des praticiens du groupe
  ○ Statistiques agrégées école
  ○ Templates Dossier d'Âme co-brandés école
  ○ Formation interne (modules uploadés par l'école)

MANDALA — V3
  ○ Mode d'affichage "Evolution" (animation timeline)
  ○ Comparaison mandala à différentes dates
  ○ Thèmes de couleur prédéfinis (palette ORIA officielle)

DOSSIER D'ÂME — V2
  ○ Templates multiples (classique, poétique, épuré)
  ○ Sections éditables (praticien peut personnaliser le contenu)
  ○ Envoi par email au participant directement depuis l'app
  ○ QR code sur le dossier (lien vers le mandala numérique)

INTÉGRATIONS
  ○ Calendly (alternative à l'agenda intégré)
  ○ Outlook / Office 365 Calendar
  ○ Zoom (lien séance automatique)
  ○ Webhook API (plan ÉCOLE)
```

---

## 5. V3 (M18 → M24) — "Le Scale"

### Objectif V3

300 praticiens, expansion internationale, version anglophone.

**Critère de succès V3 :**
- 300 praticiens (dont 60% PRO ou ÉCOLE)
- Version EN opérationnelle avec 30 praticiens anglophones
- MRR > 26 000 €
- Certification Niveau 3 (Superviseur) ouverte
- 1 partenariat école formation internationale

---

### Features V3

```
INTERNATIONALISATION
  ○ Version anglaise complète (UI + contenu + exercices)
  ○ Version espagnole (UI)
  ○ Support multilingue du Dossier d'Âme
  ○ Adaptation culturelle des ressources

MOBILE NATIVE (optionnel selon traction)
  ○ Évaluation React Native vs PWA améliorée
  ○ Si décision native : app iOS + Android (journal + mandala + exercises)

API PUBLIQUE
  ○ API REST documentée (plan ÉCOLE)
  ○ Webhooks configurables
  ○ SDK JavaScript (pour intégration partenaires)

ANALYTICS AVANCÉS
  ○ Rapport d'impact praticien (data anonymisées)
  ○ Heatmap d'utilisation des exercices
  ○ Prédiction de churn praticien (ML simple)

CERTIFICATION SUPERVISEUR
  ○ Formation Niveau 3 (distanciel + présentiel)
  ○ Co-supervision guidée
  ○ Commissionnement automatique

MARKETPLACE RESSOURCES (V3+)
  ○ Praticiens peuvent vendre leurs ressources à la communauté
  ○ ORIA perçoit 20% de commission
  ○ Modération par les superviseurs
```

---

## 6. USER STORIES COMPLÈTES

### Epic 1 : Gestion du Compte et Accès

**US-001** — En tant que praticien, je veux m'inscrire avec mon email et recevoir un lien de vérification, afin de créer mon compte de manière sécurisée.

**US-002** — En tant qu'Admin ORIA, je veux recevoir une notification quand un praticien s'inscrit, afin de vérifier sa demande et activer son accès après validation de sa certification.

**US-003** — En tant que participant, je veux recevoir un email d'invitation de mon praticien, afin de créer mon compte et accéder à mon espace ORIA.

**US-004** — En tant que praticien, je veux me connecter avec email/mot de passe et rester connecté pendant 7 jours, afin de ne pas devoir me reconnecter à chaque séance.

**US-005** — En tant qu'utilisateur, je veux pouvoir réinitialiser mon mot de passe par email, afin de ne pas être bloqué si je l'oublie.

**US-006** — En tant que praticien, je veux activer la double authentification, afin de sécuriser l'accès aux données sensibles de mes participants.

---

### Epic 2 : Gestion des Participants

**US-010** — En tant que praticien, je veux inviter un participant en entrant son prénom et son email, afin qu'il reçoive une invitation et puisse créer son compte.

**US-011** — En tant que praticien, je veux voir la liste de tous mes participants actifs avec leur phase en cours et leur dernière activité, afin d'avoir une vue d'ensemble instantanée.

**US-012** — En tant que praticien, je veux filtrer mes participants par phase ou par statut, afin de prioriser mon attention.

**US-013** — En tant que praticien, je veux archiver un participant ayant terminé son parcours, afin de garder ma liste active lisible.

**US-014** — En tant que praticien, je veux ajouter des notes internes sur un participant (jamais vues de lui), afin de me rappeler des éléments contextuels importants.

---

### Epic 3 : Pilotage du Parcours

**US-020** — En tant que praticien, je veux voir le parcours complet d'un participant (phases 0 à 7) avec leur état (validée, active, verrouillée), afin de suivre sa progression d'un coup d'œil.

**US-021** — En tant que praticien, je veux valider une phase après avoir jugé que le participant est prêt, et écrire une note de validation, afin de débloquer la phase suivante pour le participant.

**US-022** — En tant que praticien, je veux activer la Phase 2.5 (Désert Identitaire) indépendamment de la Phase 2, afin d'accompagner un participant qui traverse cette transition.

**US-023** — En tant que participant, je veux être notifié quand mon praticien valide une phase, afin de savoir que mon travail est reconnu et de voir mon Mandala évoluer.

**US-024** — En tant que participant, je veux voir quelles phases sont validées, lesquelles sont actives et lesquelles sont à venir, afin de me situer dans mon parcours.

---

### Epic 4 : Journal Personnel

**US-030** — En tant que participant, je veux écrire librement dans mon journal chaque jour avec un prompt optionnel lié à ma phase, afin de continuer mon travail de réflexion entre les séances.

**US-031** — En tant que participant, je veux choisir si chaque entrée de journal est privée ou partagée avec mon praticien, afin de garder le contrôle sur ce que je partage.

**US-032** — En tant que praticien, je veux lire les entrées journal que mon participant a choisies de partager avec moi, afin de mieux préparer la séance suivante.

**US-033** — En tant que participant, je veux pouvoir rechercher dans mon journal par date ou mot-clé, afin de retrouver une entrée précise.

**US-034** — En tant que participant, je veux exporter mon journal en PDF personnel, afin de garder une copie hors de l'application.

---

### Epic 5 : Exercices

**US-040** — En tant que participant, je veux voir la liste des exercices de ma phase active avec leur état (non commencé, en cours, complété), afin de savoir où j'en suis.

**US-041** — En tant que participant, je veux réaliser un exercice structuré question par question, pouvoir sauvegarder et reprendre plus tard, afin de ne pas perdre mon travail.

**US-042** — En tant que participant, je veux choisir si mes réponses à un exercice sont partagées avec mon praticien, afin de décider ce que je transmets.

**US-043** — En tant que praticien, je veux voir les réponses partagées par mes participants pour chaque exercice, afin de préparer mes séances de manière informée.

**US-044** — En tant que praticien, je veux assigner un exercice spécifique à un participant hors de la séquence standard, afin d'adapter le parcours à ses besoins.

---

### Epic 6 : Notes de Séance

**US-050** — En tant que praticien, je veux créer une note de séance structurée (date, phase, observations, tags ORIA), afin de garder une trace précise de chaque séance.

**US-051** — En tant que praticien, je veux distinguer la partie privée de mes notes (pour moi seul) de la synthèse partageable (pour le participant), afin de contrôler ce que je transmets.

**US-052** — En tant que praticien, je veux partager une synthèse de séance au participant d'un clic, afin qu'il puisse y accéder dans son espace.

**US-053** — En tant que participant, je veux lire les synthèses de séance partagées par mon praticien, afin de me rappeler les points clés et continuer le travail chez moi.

**US-054** — En tant que praticien, je veux taguer mes notes avec des concepts ORIA (Voile identifié, Force émergente, Observation somatique), afin de retrouver facilement des patterns sur la durée.

---

### Epic 7 : Mandala Vivant

**US-060** — En tant que participant, je veux voir mon Mandala Vivant sur ma page d'accueil et dans une vue plein écran, afin d'avoir une représentation visuelle de mon parcours.

**US-061** — En tant que participant, je veux choisir la couleur de l'anneau que mon praticien vient de valider, afin de personnaliser mon mandala et en faire quelque chose d'unique.

**US-062** — En tant que participant, je veux voir mon mandala s'animer quand une phase est validée, afin de vivre ce moment comme une reconnaissance.

**US-063** — En tant que praticien, je veux choisir un symbole pour chaque anneau validé depuis une bibliothèque ORIA, afin d'ancrer visuellement le sens de chaque étape.

**US-064** — En tant que praticien, je veux attribuer les Archétypes d'Âme de mon participant dans l'anneau correspondant du mandala, afin qu'ils apparaissent visuellement dans la représentation.

**US-065** — En tant que participant, je veux exporter mon Mandala en PNG haute résolution, afin de le garder, l'imprimer ou le partager.

**US-066** — En tant que participant, je veux survoler chaque anneau pour voir le nom de la phase et la date de validation, afin de comprendre ce que représente chaque couche.

---

### Epic 8 : Dossier d'Âme

**US-070** — En tant que participant, je veux voir mon Dossier d'Âme en construction au fil des phases, afin de voir la trace de ce que j'ai traversé se constituer.

**US-071** — En tant que praticien, je veux générer le Dossier d'Âme complet en PDF à la fin du parcours, afin de le remettre à mon participant comme livrable de notre travail commun.

**US-072** — En tant que praticien, je veux pouvoir éditer les sections du Dossier d'Âme avant génération, afin de vérifier et ajuster le contenu.

**US-073** — En tant que participant, je veux télécharger mon Dossier d'Âme final en PDF, afin de garder une trace permanente de mon parcours.

**US-074** — En tant que praticien (plan PRO), je veux que mon logo apparaisse sur le Dossier d'Âme de mes participants, afin de professionnaliser mon offre d'accompagnement.

---

### Epic 9 : Certification

**US-080** — En tant que praticien candidat, je veux soumettre une demande de certification ORIA avec mes informations (formation, expérience), afin d'entrer dans le cursus.

**US-081** — En tant que praticien en formation, je veux accéder aux modules vidéo de la formation par étapes, afin de suivre le cursus à mon rythme.

**US-082** — En tant que praticien en formation, je veux passer les quiz et exercices de validation de chaque module, afin de valider ma progression.

**US-083** — En tant que superviseur, je veux valider la progression d'un candidat et lui délivrer la certification, afin d'officialiser son accès praticien ORIA.

**US-084** — En tant que praticien certifié, je veux afficher mon badge de certification sur mon profil public, afin de valoriser ma formation auprès de potentiels participants.

---

### Epic 10 : Abonnement et Paiement

**US-090** — En tant que praticien, je veux m'abonner au plan SOLO ou PRO via Stripe, afin d'accéder à la plateforme.

**US-091** — En tant que praticien, je veux accéder à mon portail client Stripe pour gérer mon abonnement (changement de plan, annulation, factures), afin de gérer mon compte en autonomie.

**US-092** — En tant que praticien, je veux être notifié 7 jours avant l'expiration de mon abonnement, afin de renouveler sans interruption.

**US-093** — En tant que praticien, je veux voir dans mon dashboard combien de participants actifs j'ai et ma limite selon mon plan, afin de savoir si je dois upgrader.

---

## 7. BACKLOG COMPLET — PRIORITÉS

### Priorité P0 (MVP critique — bloquant le lancement)

```
ID    Feature                                    Effort  Impact
────────────────────────────────────────────────────────────────
B001  Auth complète (register/login/refresh)      M       Critique
B002  Invitation participant                       S       Critique
B003  Profil praticien (basique)                  S       Critique
B004  Liste participants                           S       Critique
B005  Fiche participant + parcours phases          M       Critique
B006  Validation de phase                          S       Critique
B007  Phase 2.5 activation                        S       Critique
B008  Journal participant (CRUD + partage)         M       Critique
B009  Exercices phases 0-2 (contenu + réponses)   L       Critique
B010  Exercices phases 3-7 (contenu + réponses)   L       Critique
B011  Notes de séance (CRUD + partage)            M       Critique
B012  Mandala SVG interactif                      L       Critique
B013  Mandala illumination + animation            M       Critique
B014  Choix couleur mandala (participant)         S       Critique
B015  Attribution symboles mandala (praticien)    S       Critique
B016  Attribution archétypes                     M       Critique
B017  Dossier d'Âme — construction progressive   M       Critique
B018  Génération PDF Dossier d'Âme               L       Critique
B019  Export Mandala PNG                         M       Critique
B020  Messagerie praticien-participant            M       Critique
B021  Médiathèque basique (ressources)           M       Haute
B022  Stripe intégration (plan SOLO)             M       Critique
B023  Admin — validation comptes praticiens      S       Critique
B024  Email notifications (invitation, validation) S     Critique
B025  Dashboard praticien                        M       Critique
B026  Dashboard participant                      M       Critique
```

### Priorité P1 (V1 — premier trimestre post-lancement)

```
B030  Plan PRO (Stripe)                          S       Haute
B031  Agenda intégré + Google Calendar          L       Haute
B032  Analytics praticien (stats basiques)      M       Haute
B033  Modèles notes de séance                   S       Haute
B034  Certification Niveau 1 (modules vidéo)    L       Haute
B035  Quiz / exercices certification            M       Haute
B036  Badge certification                       S       Haute
B037  Page profil praticien (publique)          M       Haute
B038  Co-branding Dossier d'Âme (logo)          S       Haute
B039  2FA praticien (TOTP)                      M       Haute
B040  Export données RGPD (participant)         M       Haute
B041  Suppression compte complète               M       Haute
B042  Ressources audio (lecteur intégré)        M       Haute
B043  Notifications push PWA                   M       Haute
B044  Plan annuel Stripe                        S       Haute
B045  Participants supplémentaires (usage)      M       Haute
```

### Priorité P2 (V2 — consolidation)

```
B050  Dashboard superviseur                     L       Moyenne
B051  Vue anonymisée parcours (supervision)     M       Moyenne
B052  Sessions de supervision                   M       Moyenne
B053  Certification Niveau 2                    L       Moyenne
B054  Forum communauté praticiens               L       Moyenne
B055  Annuaire praticiens (public)              M       Moyenne
B056  Système parrainage                        M       Moyenne
B057  Plan ÉCOLE + dashboard multi-praticiens   L       Moyenne
B058  Mandala — mode timeline/évolution         M       Moyenne
B059  Mandala — partage lien public             S       Moyenne
B060  Dossier d'Âme — templates multiples      M       Moyenne
B061  Dossier d'Âme — sections éditables        M       Moyenne
B062  Intégration Zoom (lien séance)            S       Moyenne
B063  Webhook API (plan ÉCOLE)                  L       Moyenne
```

### Priorité P3 (V3 — scale)

```
B070  Internationalisation EN                   L       Basse
B071  Internationalisation ES                   L       Basse
B072  API REST documentée                       L       Basse
B073  Certification Niveau 3 (superviseur)      L       Basse
B074  Marketplace ressources                    L       Basse
B075  Analytics avancés (ML churn)              L       Basse
B076  Mobile native (évaluation)               XL       Basse
B077  Rapport d'impact praticien                M       Basse
```

---

## 8. DÉFINITION OF DONE (DoD)

Pour chaque feature, "terminée" signifie :

```
□ Code review approuvé par au moins 1 autre développeur
□ Tests unitaires écrits et passants (coverage > 70% sur la feature)
□ Tests d'intégration écrits pour les flux critiques
□ Tests manuels réalisés en staging (golden path + edge cases)
□ Accessibilité vérifiée (WCAG 2.1 AA minimum)
□ Performance vérifiée (< 200ms API, < 2s chargement page)
□ Données sensibles chiffrées si applicable
□ Documentation technique mise à jour
□ Feature validée par le Product Manager
□ Déployée en staging + vérifiée
```

---

## 9. KPIs PAR PHASE

### KPIs MVP (M0-M6)

| KPI | Mesure | Cible |
|---|---|---|
| Praticiens inscrits | Comptes praticiens créés | 20 |
| Participants actifs | Participants ayant au moins 1 exercice complété | 50 |
| Dossiers d'Âme générés | PDFs téléchargés | 3 |
| NPS praticien | Score enquête mensuelle | > 50 |
| Bug critique | Incidents en production | 0 |
| Uptime | Disponibilité | > 99% |

### KPIs V1 (M6-M12)

| KPI | Mesure | Cible |
|---|---|---|
| MRR | Revenus récurrents mensuels | > 7 500 € |
| Praticiens actifs payants | Abonnements actifs | > 100 |
| Taux rétention M1 praticien | % praticiens toujours actifs 30j après inscription | > 85% |
| Taux rétention M6 praticien | % praticiens toujours actifs 6 mois après inscription | > 75% |
| Certifications délivrées | Niveau 1 | > 10 |
| Participants actifs | En parcours | > 500 |
| NPS participant | Score enquête | > 65 |
| Churn mensuel | % praticiens qui annulent | < 3% |

### KPIs V2 (M12-M18)

| KPI | Mesure | Cible |
|---|---|---|
| MRR | | > 18 000 € |
| Praticiens PRO | % praticiens en plan PRO | > 40% |
| Superviseurs actifs | | > 3 |
| Plans ÉCOLE actifs | | > 2 |
| Forum activité | Posts/semaine | > 20 |
| Parrainage actif | % praticiens ayant parrainé | > 15% |
| NPS praticien | | > 65 |

### KPIs V3 (M18-M24)

| KPI | Mesure | Cible |
|---|---|---|
| MRR | | > 26 000 € |
| ARR | | > 310 000 € |
| Praticiens totaux | | > 300 |
| Praticiens EN | | > 30 |
| LTV praticien | Valeur vie moyenne | > 3 500 € |
| CAC | Coût acquisition praticien | < 250 € |
| LTV/CAC | | > 10x |
| Churn mensuel | | < 2% |

---

## 10. ÉQUIPE RECOMMANDÉE

### Équipe MVP (M0-M6)

| Rôle | Profil | Engagement |
|---|---|---|
| CTO / Lead Dev | Fullstack senior (Next.js + NestJS) | Full-time |
| Frontend Dev | React + D3.js | Full-time |
| Designer UX/UI | Senior, expérience SaaS B2B | Part-time (50%) |
| Product Manager | Expérience SaaS | Part-time (50%) |
| Fondateur (Jonathan) | Vision + méthode + early adopters | Full-time |

### Équipe V1+ (M6-M18)

| Rôle | Profil | Engagement |
|---|---|---|
| CTO / Lead Dev | Idem | Full-time |
| Frontend Dev | Idem | Full-time |
| Backend Dev | Spécialiste NestJS + PostgreSQL | Full-time |
| Designer UX/UI | Full-time si budget | Full-time |
| Product Manager | Full-time | Full-time |
| Customer Success | Support praticiens, onboarding | Part-time → Full-time |
| Fondateur | Direction + certification | Full-time |

---

*ORIA — Roadmap & Backlog · Version 1.0*
*Document propriétaire. Tous droits réservés.*
