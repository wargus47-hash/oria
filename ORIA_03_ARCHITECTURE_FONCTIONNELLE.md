# ORIA — ARCHITECTURE FONCTIONNELLE
## *Rôles, parcours utilisateurs, features, wireframes · Version 1.0*

---

## 1. RÔLES UTILISATEURS

### Hiérarchie des rôles

```
┌─────────────────────────────────────┐
│            ADMIN ORIA               │
│  (équipe interne — gestion plateforme) │
└─────────────┬───────────────────────┘
              │
┌─────────────▼───────────────────────┐
│          SUPERVISEUR                 │
│  (formateur senior ORIA certifié)   │
└─────────────┬───────────────────────┘
              │ supervise
┌─────────────▼───────────────────────┐
│            PRATICIEN                 │
│  (certifié ORIA, accompagne des     │
│   participants)                     │
└─────────────┬───────────────────────┘
              │ crée et accompagne
┌─────────────▼───────────────────────┐
│           PARTICIPANT                │
│  (accède via invitation du praticien)│
└─────────────────────────────────────┘
```

---

### RÔLE : PARTICIPANT

**Accès :** Sur invitation du praticien uniquement. Aucun accès auto-inscrit.

**Ce que le Participant peut faire :**

| Fonctionnalité | Description |
|---|---|
| Journal personnel | Écriture libre guidée par la phase active. Privé par défaut — partage choisi avec praticien. |
| Exercices de phase | Accès aux exercices correspondant à la phase validée par le praticien. Phase suivante verrouillée. |
| Ressources | Textes, audio, pratiques guidées adaptés à la phase active. |
| Mandala Vivant | Visualisation de son mandala en temps réel. Choix des couleurs pour son anneau actif. |
| Profil & parcours | Vue de son parcours, des phases franchies, du temps passé. |
| Messagerie praticien | Canal de communication avec son praticien (entre séances). |
| Dossier d'Âme | Accès en lecture à son dossier en construction. Téléchargement du PDF final. |
| Synthèses de séance | Lecture des notes de séance partagées par le praticien. |
| Demande de rendez-vous | Intégration calendrier pour planifier les séances. |

**Ce que le Participant ne peut PAS faire :**
- Passer à la phase suivante seul (réservé au praticien)
- Modifier les synthèses du praticien
- Voir les données d'autres participants
- S'inscrire sans invitation praticien

---

### RÔLE : PRATICIEN

**Accès :** Après certification ORIA validée.

**Ce que le Praticien peut faire :**

| Fonctionnalité | Description |
|---|---|
| Gestion participants | Créer, inviter, archiver des participants. Vue liste et vue individuelle. |
| Tableau de bord | Vue globale de tous les participants actifs — phase en cours, dernière activité, alertes. |
| Pilotage du parcours | Valider les étapes et les phases. Débloquer la phase suivante. |
| Notes de séance | Rédiger des notes structurées par séance. Choisir ce qui est partagé avec le participant. |
| Synthèses ORIA | Générer des synthèses structurées à partir des notes (par phase). |
| Attribution des archétypes | Attribuer les Archétypes d'Âme au participant après Phase 4. |
| Mandala Vivant | Déclencher l'évolution du mandala à chaque phase validée. |
| Ressources | Ajouter des ressources personnalisées pour un participant. |
| Dossier d'Âme | Générer le PDF du Dossier d'Âme à la fin du parcours. |
| Agenda | Gérer son calendrier de séances. |
| Facturation | Gestion simple des règlements (optionnel selon plan). |
| Formation continue | Accès aux formations et supervisions dans la plateforme. |

**Ce que le Praticien ne peut PAS faire :**
- Accéder aux journaux privés du participant (sauf si le participant partage)
- Modifier rétroactivement des données du participant
- Certifier d'autres praticiens (réservé au Superviseur)

---

### RÔLE : SUPERVISEUR

**Accès :** Praticien senior désigné par ORIA.

**Ce que le Superviseur peut faire :**

| Fonctionnalité | Description |
|---|---|
| Vue praticiens | Liste des praticiens supervisés — activité, nombre de participants, phases actives. |
| Supervision de parcours | Vue anonymisée des parcours en cours chez les praticiens supervisés. |
| Sessions de supervision | Planifier et animer des sessions de supervision avec les praticiens. |
| Validation des certifications | Valider l'éligibilité à la certification praticien. |
| Statistiques | Données agrégées sur l'usage de la méthode — non nominatives. |
| Ressources pédagogiques | Gérer la bibliothèque de formation des praticiens. |

---

### RÔLE : ADMIN

**Accès :** Équipe interne ORIA uniquement.

**Fonctions :**
- Gestion des comptes, des plans, des paiements
- Gestion de la certification (curriculum, validations)
- Configuration des paramètres de la plateforme
- Monitoring technique
- Support utilisateurs

---

## 2. MATRICE DE PERMISSIONS

```
Fonctionnalité                  | Participant | Praticien | Superviseur | Admin
--------------------------------|-------------|-----------|-------------|------
Journal personnel               | R/W         | -         | -           | -
Journal partagé avec praticien  | R           | R         | -           | -
Exercices                       | R/W         | R/W       | -           | R
Ressources                      | R           | R/W       | R           | R/W
Mandala — visualisation         | R           | R         | -           | R
Mandala — évolution             | -           | W         | -           | -
Mandala — couleurs              | W           | -         | -           | -
Notes de séance (privées)       | -           | R/W       | -           | -
Notes de séance (partagées)     | R           | R/W       | -           | -
Validation de phase             | -           | W         | -           | -
Attribution archétypes          | -           | W         | -           | -
Génération Dossier d'Âme        | R (download)| W         | -           | -
Données praticiens supervisés   | -           | -         | R           | R/W
Certification praticien         | -           | -         | W           | R/W
Statistiques plateforme         | -           | Partielles| Agrégées    | Complètes
Facturation                     | -           | R/W       | -           | R/W
```

---

## 3. PARCOURS PARTICIPANT — COMPLET

### Étape 0 : Invitation et Onboarding

```
[Praticien envoie invitation]
       ↓
[Participant reçoit email d'invitation]
       ↓
[Clic → Page d'accueil ORIA]
       ↓
[Création de compte (nom, email, mot de passe)]
       ↓
[Écran de bienvenue : présentation d'ORIA en 3 slides]
       "ORIA n'est pas une application de développement personnel.
        C'est un espace qui soutient votre parcours avec votre praticien."
       ↓
[Profil minimal : prénom d'usage, couleur préférée (pour le mandala)]
       ↓
[Dashboard Participant — Phase 0 active]
```

---

### Navigation quotidienne du Participant

**Dashboard Participant — Vue principale**

```
┌─────────────────────────────────────────────────────────────┐
│  ORIA                                          [Mon profil] │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Bonjour, [Prénom]                                         │
│  Phase active : Phase 1 — Origine              [Voir phases]│
│                                                             │
│  ┌─────────────────┐  ┌─────────────────┐                  │
│  │   MON MANDALA   │  │  PROCHAINE      │                  │
│  │                 │  │  SÉANCE         │                  │
│  │   [Mandala      │  │  Jeu. 19 juin   │                  │
│  │    miniature]   │  │  14h00          │                  │
│  │                 │  │  [Confirmer]    │                  │
│  └─────────────────┘  └─────────────────┘                  │
│                                                             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  EXERCICE DU JOUR                                   │   │
│  │  La Ligne du Temps Parentale                        │   │
│  │  Phase 1 · Axe 1.1                                  │   │
│  │  [Commencer]                                        │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
│  Journal · Ressources · Messages · Dossier                  │
└─────────────────────────────────────────────────────────────┘
```

---

**Sections du Participant :**

**1. Mon Mandala**
- Visualisation complète du Mandala Vivant
- État actuel : anneaux validés illuminés, anneaux non franchis estompés
- Choix de couleur pour l'anneau actif
- Export image haute résolution

**2. Mon Parcours**
- Vue linéaire des phases (0 → 7)
- Phase active surlignée
- Phases validées avec date et badge
- Phases à venir verrouillées (icône cadenas)

**3. Exercices**
- Liste des exercices de la phase active
- État : non commencé / en cours / complété (partage choix avec praticien)
- Chaque exercice : question guide + espace de réponse + outil spécifique
- Historique des exercices complétés

**4. Journal Personnel**
- Écriture libre quotidienne
- Prompt optionnel du jour (lié à la phase)
- Marquage : "Garder privé" ou "Partager avec mon praticien"
- Historique paginé

**5. Ressources**
- Médiathèque par phase : textes, audios guidés, fiches outils
- Ressources ajoutées par le praticien
- Lecture/écoute directement dans l'app

**6. Mes Séances**
- Historique des séances
- Synthèses partagées par le praticien
- Calendrier et prise de rendez-vous

**7. Messages**
- Messagerie directe avec le praticien (asynchrone)
- Notifications paramétrables

**8. Mon Dossier d'Âme**
- Vue du dossier en construction (phases complètes)
- Téléchargement PDF final (quand généré par le praticien)

---

## 4. PARCOURS PRATICIEN — COMPLET

### Onboarding Praticien

```
[Inscription (après certification ou demande)]
       ↓
[Vérification de certification par Admin/Superviseur]
       ↓
[Accès validé — Email de bienvenue]
       ↓
[Onboarding guidé : 5 étapes]
  1. Compléter profil praticien (bio, spécialités, photo)
  2. Découverte du dashboard praticien (tour guidé)
  3. Invitation du premier participant (tutorial)
  4. Création d'une première note de séance (tutorial)
  5. Exploration du Mandala Vivant
       ↓
[Dashboard Praticien actif]
```

---

### Dashboard Praticien — Vue principale

```
┌─────────────────────────────────────────────────────────────┐
│  ORIA Praticien                        [Nom praticien]      │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Tableau de bord                                            │
│                                                             │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐      │
│  │ 12       │ │ 3        │ │ 2        │ │ 1        │      │
│  │ Participants│ Séances │ │ En attente│ │ Dossier  │      │
│  │ actifs   │ │ ce mois  │ │ de        │ │ à générer│      │
│  │          │ │          │ │ validation│ │          │      │
│  └──────────┘ └──────────┘ └──────────┘ └──────────┘      │
│                                                             │
│  MES PARTICIPANTS                          [+ Inviter]      │
│  ┌─────────────────────────────────────────────────────┐   │
│  │ Marie D.    Phase 2 · Reconnaissance   [Voir]        │   │
│  │ Thomas L.   Phase 4 · Projet d'Âme    [Voir]        │   │
│  │ Sarah M.    Phase 0 · Témoin          [Voir]        │   │
│  │ ...                                                  │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
│  PROCHAINES SÉANCES                                         │
│  ┌─────────────────────────────────────────────────────┐   │
│  │ Lun 16 · 10h · Marie D. (Phase 2)                  │   │
│  │ Mar 17 · 14h · Thomas L. (Phase 4)                  │   │
│  └─────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
```

---

### Vue Participant (côté Praticien)

```
┌─────────────────────────────────────────────────────────────┐
│  ← Retour      Marie Dupont — Parcours ORIA                │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌──────────────────────┐  ┌────────────────────────────┐  │
│  │  Mandala de Marie    │  │  Phase active              │  │
│  │                      │  │  Phase 2 — Reconnaissance   │  │
│  │  [Mandala miniature] │  │  Démarrée le : 12 mai 2026  │  │
│  │                      │  │  Exercices : 4/7 complétés │  │
│  └──────────────────────┘  │  [Valider cette phase]     │  │
│                             └────────────────────────────┘  │
│                                                             │
│  Onglets :                                                  │
│  [Parcours] [Notes séances] [Exercices] [Mandala] [Dossier]│
│                                                             │
│  ──── PARCOURS ─────────────────────────────────────────── │
│  ✓ Phase 0 — Témoin         Validée 28 avr.                │
│  ✓ Phase 1 — Origine        Validée 10 mai                 │
│  → Phase 2 — Reconnaissance  En cours                       │
│  ○ Phase 2.5 — Désert        Verrouillée                   │
│  ○ Phase 3 — Intégration     Verrouillée                   │
│  ...                                                        │
└─────────────────────────────────────────────────────────────┘
```

---

### Flux de validation de phase

```
[Praticien estime que le participant est prêt]
       ↓
[Clic "Valider cette phase" sur la fiche participant]
       ↓
[Modale de confirmation]
  - Phase validée : Phase 2 — Reconnaissance
  - Synthèse à générer : oui/non
  - Partager la validation avec le participant : oui/non
  - Note de validation (optionnelle)
       ↓
[Validation confirmée]
       ↓
[Mandala Vivant mis à jour — anneau suivant s'illumine]
[Participant notifié : "Votre praticien a validé une étape"]
[Phase suivante déverrouillée pour le participant]
```

---

### Création de notes de séance

```
[Praticien clique "Nouvelle note de séance"]
       ↓
[Formulaire structuré]
  - Date et durée
  - Phase de référence
  - Observations de séance (champ libre + tags ORIA)
  - Exercices recommandés
  - Points de vigilance (privé — ne sera pas partagé)
  - Synthèse partageable (pour le participant)
       ↓
[Sauvegarde — brouillon ou publié]
       ↓
[Si publié : participant notifié]
```

---

### Attribution des Archétypes d'Âme

```
[Disponible uniquement après validation Phase 4]
       ↓
[Praticien accède à l'onglet "Archétypes" du participant]
       ↓
[Interface d'attribution]
  - Grille des 12 Archétypes ORIA avec descriptions
  - Sélection : 1 archétype principal + 1 à 2 secondaires
  - Note d'attribution (justification — privée)
  - Texte de présentation (partageable avec le participant)
       ↓
[Confirmation — Attribution visible pour participant]
[Mandala mis à jour — anneau Archétypes]
```

---

## 5. PARCOURS SUPERVISEUR

**Dashboard Superviseur :**

```
┌─────────────────────────────────────────────────────────────┐
│  ORIA Supervision                     [Nom superviseur]     │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  MES PRATICIENS SUPERVISÉS                                  │
│  ┌─────────────────────────────────────────────────────┐   │
│  │ Pierre M.   8 participants actifs   Certification ✓ │   │
│  │ Julie R.    12 participants actifs  Certification ✓ │   │
│  │ Marc T.     3 participants actifs   En formation    │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
│  STATISTIQUES GLOBALES (anonymisées)                        │
│  Participants actifs : 23                                   │
│  Phase la plus fréquente : Phase 2                          │
│  Taux de complétion du parcours : 34%                       │
│                                                             │
│  SESSIONS DE SUPERVISION                      [+ Planifier]│
│  Prochaine : Mar 17 juin · 10h · Groupe (4 praticiens)     │
└─────────────────────────────────────────────────────────────┘
```

---

## 6. FEATURES LISTE COMPLÈTE

### Module Mandala Vivant
- Rendu visuel interactif en canvas/SVG
- 6 anneaux + centre
- Évolution déclenchée par validation praticien
- Palette de couleurs choisies par le participant
- Symboles archétypaux attribués par le praticien
- Animation d'illumination à chaque étape franchie
- Export image (PNG, SVG)

### Module Journal
- Éditeur de texte riche (pas markdown brut — WYSIWYG simple)
- Prompts contextuels selon la phase
- Marquage privé/partagé par entrée
- Recherche plein texte dans ses entrées
- Export journal (PDF personnel)

### Module Exercices
- Exercices structurés par phase et par axe
- Types : texte libre, choix multiple, dessin/upload, grille, carte mentale simplifiée
- Progression visuelle
- Réponses exportables dans le Dossier d'Âme

### Module Ressources
- Médiathèque organisée par phase
- Types : article, audio guidé, pratique somatique, outil téléchargeable
- Upload possible par le praticien (fichiers perso)
- Accès offline pour les audios (mobile)

### Module Messagerie
- Chat asynchrone Participant ↔ Praticien
- Notifications push et email
- Archives des échanges

### Module Agenda
- Calendrier du praticien (intégration Google Calendar / Outlook)
- Prise de rendez-vous par le participant (selon créneaux ouverts)
- Rappels automatiques

### Module Notes de séance
- Formulaire structuré par phase
- Tags ORIA (Voile identifié, Archétype perçu, Observation somatique...)
- Vue chronologique par participant
- Partie privée / partie partageable

### Module Dossier d'Âme
- Construction progressive au fil des phases
- Génération PDF professionnelle (template ORIA)
- Sections éditables par le praticien avant génération
- Envoi par email au participant

### Module Certification
- Curriculum de formation (cours, modules, évaluations)
- Suivi de progression des praticiens en formation
- Validation par le Superviseur
- Badge de certification (affichable sur le profil)

### Module Supervision
- Tableau de bord multi-praticiens
- Données anonymisées des parcours
- Planning de sessions de supervision
- Forum de la communauté praticiens ORIA

---

## 7. WIREFRAMES — ÉCRANS PRINCIPAUX

### Écran 1 : Dashboard Participant

```
┌──────────────────────────────────────────────────────────────┐
│ [≡]  ORIA                                          [👤 Marie]│
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  Bonjour Marie  · Phase 1 — Origine                         │
│                                                              │
│  ┌──────────────────────────────────┐                        │
│  │         MON MANDALA              │                        │
│  │                                  │                        │
│  │       ╭──────────────╮           │                        │
│  │     ╭─╯  ╭────────╮  ╰─╮        │                        │
│  │    ╭╯    │        │    ╰╮       │                        │
│  │    │     │   ●    │     │       │                        │
│  │    ╰╮    │        │    ╭╯       │                        │
│  │     ╰─╮  ╰────────╯  ╭─╯        │                        │
│  │       ╰──────────────╯           │                        │
│  │                                  │                        │
│  │  Anneau actif : Origine [Couleur]│                        │
│  │                [Voir mon mandala]│                        │
│  └──────────────────────────────────┘                        │
│                                                              │
│  EXERCICE DU JOUR                                            │
│  ┌──────────────────────────────────────────────────────┐   │
│  │ 📝 La Ligne du Temps Parentale                       │   │
│  │    Explorez le contexte dans lequel vous êtes né(e). │   │
│  │    Durée estimée : 20 min                            │   │
│  │                            [Commencer] [Plus tard]   │   │
│  └──────────────────────────────────────────────────────┘   │
│                                                              │
│  DERNIÈRE ENTRÉE JOURNAL  · hier                             │
│  "J'ai réalisé que la phrase 'tu as une responsabilité..."  │
│  [Continuer à écrire]                                        │
│                                                              │
├──────────────────────────────────────────────────────────────┤
│ [Mandala]  [Exercices]  [Journal]  [Ressources]  [Messages] │
└──────────────────────────────────────────────────────────────┘
```

---

### Écran 2 : Mandala Vivant (plein écran)

```
┌──────────────────────────────────────────────────────────────┐
│ ← Retour      MON MANDALA VIVANT          [Export] [Couleur]│
├──────────────────────────────────────────────────────────────┤
│                                                              │
│                                                              │
│              ╭─────────────────────────╮                    │
│           ╭──╯  ╭─────────────────╮   ╰──╮                 │
│         ╭─╯     │  ╭───────────╮  │      ╰─╮               │
│        ─╯       │  │  ╭─────╮  │  │        ╰─              │
│        │        │  │  │ ╭─╮ │  │  │         │              │
│        │        │  │  │ │●│ │  │  │         │              │
│        │        │  │  │ ╰─╯ │  │  │         │              │
│        ─╮       │  │  ╰─────╯  │  │        ╭─              │
│         ╰─╮     │  ╰───────────╯  │      ╭─╯               │
│           ╰──╮  ╰─────────────────╯   ╭──╯                 │
│              ╰─────────────────────────╯                    │
│                                                              │
│  Anneaux :                                                   │
│  ● Centre : Projet d'Âme        ○ Non atteint               │
│  ● Anneau 1 : Contribution       ○ Non atteint               │
│  ● Anneau 2 : Archétypes         ○ Non atteint               │
│  ● Anneau 3 : Intégration        ○ Non atteint               │
│  ● Anneau 4 : Libération         ○ Non atteint               │
│  ○ Anneau 5 : Projet-Sens        Validé le 10 mai            │
│  ○ Anneau 6 : Contexte génél.    Validé le 28 avr.           │
│                                                              │
│  [Choisir ma couleur pour cet anneau]                        │
└──────────────────────────────────────────────────────────────┘
```

---

### Écran 3 : Un exercice ORIA

```
┌──────────────────────────────────────────────────────────────┐
│ ← Exercices      La Ligne du Temps Parentale                │
│                  Phase 1 · Axe 1.1 — Le Contexte Génératif  │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  Objectif de cet exercice                                    │
│  Comprendre le champ dans lequel vous avez été conçu(e).    │
│  Durée estimée : 20 à 30 minutes.                            │
│                                                              │
│  ────────────────────────────────────────────────────────── │
│                                                              │
│  POUR VOTRE MÈRE                                             │
│                                                              │
│  Quel était son âge et son état au moment de votre          │
│  conception ? Traversait-elle une période particulière ?     │
│                                                              │
│  ┌────────────────────────────────────────────────────────┐ │
│  │                                                        │ │
│  │  [Écrivez ici... ]                                     │ │
│  │                                                        │ │
│  └────────────────────────────────────────────────────────┘ │
│                                                              │
│  Quel non-dit pensez-vous qu'elle portait ?                  │
│                                                              │
│  ┌────────────────────────────────────────────────────────┐ │
│  │                                                        │ │
│  │  [Écrivez ici... ]                                     │ │
│  │                                                        │ │
│  └────────────────────────────────────────────────────────┘ │
│                                                              │
│  [Page 1/4]  ●●○○  Suivant →                                │
│                                                              │
│  ☑ Partager mes réponses avec mon praticien                  │
│                                                              │
│  [Sauvegarder et terminer plus tard]    [Continuer →]        │
└──────────────────────────────────────────────────────────────┘
```

---

### Écran 4 : Dashboard Praticien — Vue participant

```
┌──────────────────────────────────────────────────────────────┐
│ ← Participants      Marie Dupont              [Actions ▾]   │
│                     Parcours depuis : 28 avr. 2026           │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌────────────────────────┐  ┌────────────────────────────┐ │
│  │  Mandala de Marie      │  │  Phase active              │ │
│  │                        │  │  Phase 2 — Reconnaissance   │ │
│  │  [Mandala interactif]  │  │  Depuis le 12 mai          │ │
│  │                        │  │  Exercices : 4/7 complétés │ │
│  └────────────────────────┘  │                            │ │
│                               │  [Valider Phase 2]        │ │
│                               └────────────────────────────┘ │
│                                                              │
│  [Parcours] [Séances] [Exercices] [Mandala] [Archétypes] [Dossier]│
│                                                              │
│  ──── NOTES DE SÉANCES ──────────────────────────────────── │
│                                                              │
│  ● Séance 5 · 12 mai 2026    [Voir] [Modifier]              │
│    Axe 2.1 — Triangle dramatique · Résistance du système    │
│                                                              │
│  ● Séance 4 · 28 avr. 2026   [Voir] [Modifier]              │
│    Phase 1 validée · Synthèse partagée avec Marie            │
│                                                              │
│  [+ Nouvelle note de séance]                                 │
└──────────────────────────────────────────────────────────────┘
```

---

## 8. NAVIGATION GLOBALE

### Navigation Participant (barre du bas — mobile)
```
[Accueil] [Mandala] [Exercices] [Journal] [Messages]
```

### Navigation Participant (sidebar — desktop)
```
● Accueil
● Mon Mandala
● Mon Parcours
  ├ Phase active
  └ Historique des phases
● Exercices
● Journal
● Ressources
● Mes Séances
● Messages
● Mon Dossier d'Âme
```

### Navigation Praticien (sidebar — desktop)
```
● Tableau de bord
● Mes Participants
  ├ Vue liste
  └ [Participant individuel]
● Agenda
● Ressources
● Formation
● Mon Profil
● Paramètres
```

---

## 9. ÉTATS ET TRANSITIONS CLÉS

### Cycle de vie d'un participant

```
INVITÉ → INSCRIT → PHASE 0 → PHASE 1 → PHASE 2 → [2.5] → PHASE 3
                                                              ↓
                        DOSSIER D'ÂME ← PHASE 7 ← PHASE 6 ← PHASE 4 + 5
```

### États d'un exercice
```
Non démarré → En cours → Complété (partagé) → Complété (privé)
```

### États d'une phase
```
Verrouillée → Active → En validation (praticien) → Validée
```

### États du Mandala
```
Vierge → Anneau 6 actif → Anneau 5 actif → ... → Centre illuminé
```

---

*ORIA — Architecture Fonctionnelle · Version 1.0*
*Document propriétaire. Tous droits réservés.*
