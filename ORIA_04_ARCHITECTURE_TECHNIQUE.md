# ORIA — ARCHITECTURE TECHNIQUE
## *Stack, base de données, sécurité, RGPD · Version 1.0*

---

## 1. STACK TECHNOLOGIQUE

### Principes de choix

- **Rapidité de mise sur marché** : Technologies matures, documentation abondante, hiring facile
- **Scalabilité** : Architecture qui tient de 100 à 100 000 utilisateurs sans refonte
- **Sécurité** : Standards RGPD et hébergement européen dès le premier jour
- **Coût** : Éviter les dépendances coûteuses au démarrage
- **Maintenabilité** : Stack lisible pour une équipe de 3-6 développeurs

---

### Stack Recommandée

#### Frontend
```
Framework      : Next.js 14+ (React)
                 → SSR pour SEO, App Router, Server Components
Styling        : Tailwind CSS + Shadcn/UI
                 → Composants accessibles, personnalisables, rapides à livrer
Mandala        : D3.js + Canvas API
                 → Rendu SVG/Canvas interactif haute performance
State          : Zustand (client) + React Query (server state)
Animations     : Framer Motion
                 → Animations fluides pour le mandala et les transitions
PDF Viewer     : react-pdf
Forms          : React Hook Form + Zod
Rich Text      : Tiptap (editeur journal et notes)
Mobile         : Progressive Web App (PWA) — pas d'app native en MVP
```

#### Backend
```
Runtime        : Node.js (TypeScript)
Framework      : NestJS
                 → Architecture modulaire, DI, Guards, Interceptors
ORM            : Prisma
                 → Typage fort, migrations, DX excellente
API            : REST + WebSocket (Socket.io pour messagerie temps réel)
Auth           : JWT + Refresh Token + NextAuth.js côté front
                 → Sessions sécurisées, OAuth optionnel (Google)
File Storage   : AWS S3 compatible (ou Scaleway Object Storage — EU)
PDF Generation : Puppeteer (headless Chrome) pour le Dossier d'Âme
Email          : Resend (ou Brevo)
                 → Invitations, notifications, rapports
Queue          : BullMQ + Redis
                 → Génération PDF async, emails, tâches lourdes
```

#### Base de données
```
Principale     : PostgreSQL 16
                 → Relationnel robuste, JSON natif pour données flexibles
Cache          : Redis
                 → Sessions, rate limiting, queues
Search         : PostgreSQL Full-Text Search (suffisant en MVP)
                 → Passage à Meilisearch si besoin
```

#### Infrastructure
```
Hébergement    : Railway.app (MVP) → migration vers AWS/Scaleway EU (V1+)
                 → Simplicité de déploiement, PostgreSQL managé inclus
CDN            : Cloudflare
                 → Cache, protection DDoS, WAF
Monitoring     : Sentry (erreurs) + Posthog (analytics) + Uptime Robot
Logs           : Logtail ou Better Stack
CI/CD          : GitHub Actions
Containers     : Docker + Docker Compose (dev) → Kubernetes si scaling
DNS            : Cloudflare
```

#### Services tiers
```
Paiement       : Stripe
                 → Abonnements SaaS, portail client, webhooks
Calendrier     : Google Calendar API + CalDAV pour Outlook
Stockage EU    : Scaleway (Paris) pour conformité RGPD
Email          : Brevo (anciennement Sendinblue — hébergement EU)
```

---

### Architecture applicative

```
┌─────────────────────────────────────────────────────────────────┐
│                         INTERNET                                │
└───────────────────────────┬─────────────────────────────────────┘
                            │ HTTPS
                ┌───────────▼───────────┐
                │      Cloudflare       │
                │   (CDN + WAF + DDoS)  │
                └───────────┬───────────┘
                            │
                ┌───────────▼───────────┐
                │    Next.js (Vercel     │
                │    ou Railway)         │
                │    Frontend App        │
                └───────────┬───────────┘
                            │ API calls (HTTPS)
                ┌───────────▼───────────┐
                │    NestJS API          │
                │    (Railway/Docker)    │
                └─┬─────────────────┬───┘
                  │                 │
         ┌────────▼────┐    ┌───────▼───────┐
         │ PostgreSQL  │    │     Redis      │
         │ (Managé EU) │    │  (Cache/Queue) │
         └─────────────┘    └───────────────┘
                  │
         ┌────────▼────┐
         │  Scaleway   │
         │ Object Store│
         │  (fichiers) │
         └─────────────┘
```

---

## 2. SCHÉMA DE BASE DE DONNÉES

### Tables principales

---

#### TABLE : users
```sql
CREATE TABLE users (
  id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  email           VARCHAR(255) UNIQUE NOT NULL,
  email_verified  BOOLEAN DEFAULT false,
  password_hash   VARCHAR(255),
  role            ENUM('participant', 'practitioner', 'supervisor', 'admin') NOT NULL,
  first_name      VARCHAR(100),
  last_name       VARCHAR(100),
  display_name    VARCHAR(100),
  avatar_url      VARCHAR(500),
  locale          VARCHAR(10) DEFAULT 'fr',
  timezone        VARCHAR(50) DEFAULT 'Europe/Paris',
  is_active       BOOLEAN DEFAULT true,
  last_login_at   TIMESTAMPTZ,
  created_at      TIMESTAMPTZ DEFAULT NOW(),
  updated_at      TIMESTAMPTZ DEFAULT NOW(),
  deleted_at      TIMESTAMPTZ   -- soft delete
);
```

---

#### TABLE : practitioners
```sql
CREATE TABLE practitioners (
  id                  UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id             UUID REFERENCES users(id) ON DELETE CASCADE,
  bio                 TEXT,
  specialties         TEXT[],              -- ex: ['psychogénéalogie', 'somatique']
  certification_status ENUM('pending', 'in_training', 'certified', 'suspended'),
  certification_date  DATE,
  certified_by        UUID REFERENCES users(id),  -- superviseur
  max_participants    INTEGER DEFAULT 30,
  stripe_customer_id  VARCHAR(100),
  plan                ENUM('solo', 'pro', 'school') DEFAULT 'solo',
  created_at          TIMESTAMPTZ DEFAULT NOW(),
  updated_at          TIMESTAMPTZ DEFAULT NOW()
);
```

---

#### TABLE : participants
```sql
CREATE TABLE participants (
  id                UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id           UUID REFERENCES users(id) ON DELETE CASCADE,
  practitioner_id   UUID REFERENCES practitioners(id),
  intake_date       DATE,
  status            ENUM('invited', 'active', 'paused', 'completed', 'archived'),
  current_phase     INTEGER DEFAULT 0,  -- 0 à 7 (2.5 = phase 2, étape désert)
  desert_active     BOOLEAN DEFAULT false,  -- Phase 2.5 active
  mandala_theme     JSONB DEFAULT '{}',  -- couleurs choisies par le participant
  soul_archetypes   JSONB,             -- attribués par le praticien
  completion_date   DATE,
  notes_internal    TEXT,              -- notes praticien (jamais vues du participant)
  created_at        TIMESTAMPTZ DEFAULT NOW(),
  updated_at        TIMESTAMPTZ DEFAULT NOW()
);
```

---

#### TABLE : phases
```sql
CREATE TABLE phases (
  id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  participant_id  UUID REFERENCES participants(id) ON DELETE CASCADE,
  phase_number    DECIMAL(3,1) NOT NULL,   -- 0, 1, 2, 2.5, 3, 4, 5, 6, 7
  status          ENUM('locked', 'active', 'pending_validation', 'validated'),
  started_at      TIMESTAMPTZ,
  validated_at    TIMESTAMPTZ,
  validated_by    UUID REFERENCES users(id),
  validation_note TEXT,
  mandala_layer   JSONB,    -- données visuelles de l'anneau (couleur, symbole)
  created_at      TIMESTAMPTZ DEFAULT NOW(),
  updated_at      TIMESTAMPTZ DEFAULT NOW(),
  UNIQUE(participant_id, phase_number)
);
```

---

#### TABLE : sessions (séances)
```sql
CREATE TABLE sessions (
  id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  participant_id  UUID REFERENCES participants(id) ON DELETE CASCADE,
  practitioner_id UUID REFERENCES practitioners(id),
  session_number  INTEGER,
  phase_ref       DECIMAL(3,1),
  session_date    TIMESTAMPTZ NOT NULL,
  duration_min    INTEGER,
  status          ENUM('scheduled', 'completed', 'cancelled', 'no_show'),
  notes_private   TEXT,              -- uniquement visible praticien
  notes_shared    TEXT,              -- visible participant si partagé
  is_notes_shared BOOLEAN DEFAULT false,
  tags            TEXT[],            -- tags ORIA (Voile, Archétype, observation...)
  created_at      TIMESTAMPTZ DEFAULT NOW(),
  updated_at      TIMESTAMPTZ DEFAULT NOW()
);
```

---

#### TABLE : journal_entries
```sql
CREATE TABLE journal_entries (
  id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  participant_id  UUID REFERENCES participants(id) ON DELETE CASCADE,
  phase_ref       DECIMAL(3,1),
  title           VARCHAR(255),
  content         TEXT NOT NULL,     -- contenu chiffré at-rest
  is_shared       BOOLEAN DEFAULT false,    -- partagé avec praticien
  entry_date      DATE DEFAULT CURRENT_DATE,
  word_count      INTEGER,
  prompt_used     VARCHAR(255),      -- prompt du jour utilisé
  created_at      TIMESTAMPTZ DEFAULT NOW(),
  updated_at      TIMESTAMPTZ DEFAULT NOW()
);
```

---

#### TABLE : exercises
```sql
CREATE TABLE exercises (
  id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  phase_number    DECIMAL(3,1) NOT NULL,
  axis_ref        VARCHAR(20),               -- ex: '1.1', '2.3'
  title           VARCHAR(255) NOT NULL,
  description     TEXT,
  instructions    JSONB NOT NULL,            -- structure de l'exercice (champs, questions)
  estimated_min   INTEGER,
  is_required     BOOLEAN DEFAULT false,
  order_index     INTEGER,
  created_at      TIMESTAMPTZ DEFAULT NOW()
);
```

---

#### TABLE : exercise_responses
```sql
CREATE TABLE exercise_responses (
  id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  participant_id  UUID REFERENCES participants(id) ON DELETE CASCADE,
  exercise_id     UUID REFERENCES exercises(id),
  responses       JSONB NOT NULL,            -- réponses chiffrées
  is_shared       BOOLEAN DEFAULT false,
  status          ENUM('started', 'completed'),
  completed_at    TIMESTAMPTZ,
  created_at      TIMESTAMPTZ DEFAULT NOW(),
  updated_at      TIMESTAMPTZ DEFAULT NOW(),
  UNIQUE(participant_id, exercise_id)
);
```

---

#### TABLE : mandala_state
```sql
CREATE TABLE mandala_state (
  id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  participant_id  UUID REFERENCES participants(id) ON DELETE CASCADE UNIQUE,
  center_data     JSONB,    -- Projet d'Âme (centre)
  ring_1          JSONB,    -- Contribution
  ring_2          JSONB,    -- Archétypes
  ring_3          JSONB,    -- Intégration
  ring_4          JSONB,    -- Libération systémique
  ring_5          JSONB,    -- Projet-Sens
  ring_6          JSONB,    -- Contexte généalogique
  last_updated_at TIMESTAMPTZ DEFAULT NOW(),
  exported_at     TIMESTAMPTZ
);

-- Structure JSONB d'un anneau :
-- {
--   "status": "active|validated|locked",
--   "color": "#C8A882",
--   "symbol": "tisseur",    -- attribué par praticien
--   "opacity": 0.9,
--   "validated_at": "2026-05-10",
--   "custom_label": null    -- label optionnel praticien
-- }
```

---

#### TABLE : archetypes
```sql
CREATE TABLE archetypes (
  id          UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  code        VARCHAR(50) UNIQUE NOT NULL,   -- ex: 'tisseur', 'alchimiste'
  name        VARCHAR(100) NOT NULL,
  emoji       VARCHAR(10),
  description TEXT NOT NULL,
  shadow_text TEXT,                          -- risques/ombre de l'archétype
  force_tags  TEXT[],                        -- Forces ORIA associées
  order_index INTEGER
);

CREATE TABLE participant_archetypes (
  id                UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  participant_id    UUID REFERENCES participants(id) ON DELETE CASCADE,
  archetype_id      UUID REFERENCES archetypes(id),
  is_primary        BOOLEAN DEFAULT false,
  attribution_note  TEXT,          -- justification privée praticien
  display_text      TEXT,          -- texte partagé avec participant
  attributed_by     UUID REFERENCES users(id),
  attributed_at     TIMESTAMPTZ DEFAULT NOW()
);
```

---

#### TABLE : soul_files (Dossier d'Âme)
```sql
CREATE TABLE soul_files (
  id                UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  participant_id    UUID REFERENCES participants(id) ON DELETE CASCADE,
  status            ENUM('draft', 'generated', 'shared'),
  content           JSONB NOT NULL,    -- sections du dossier
  pdf_url           VARCHAR(500),      -- URL S3 du PDF
  generated_at      TIMESTAMPTZ,
  shared_at         TIMESTAMPTZ,
  generated_by      UUID REFERENCES users(id),
  version           INTEGER DEFAULT 1,
  created_at        TIMESTAMPTZ DEFAULT NOW(),
  updated_at        TIMESTAMPTZ DEFAULT NOW()
);

-- Structure content JSONB :
-- {
--   "project_sens": {...},
--   "systemic_map": {...},
--   "desert_passage": {...},
--   "somatic_map": {...},
--   "soul_portrait": {...},
--   "archetypes": {...},
--   "personal_myth": {...},
--   "alignment_plan": {...},
--   "mandala": {...}
-- }
```

---

#### TABLE : messages
```sql
CREATE TABLE messages (
  id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  conversation_id UUID NOT NULL,
  sender_id       UUID REFERENCES users(id),
  recipient_id    UUID REFERENCES users(id),
  content         TEXT NOT NULL,      -- chiffré at-rest
  is_read         BOOLEAN DEFAULT false,
  read_at         TIMESTAMPTZ,
  created_at      TIMESTAMPTZ DEFAULT NOW()
);

CREATE TABLE conversations (
  id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  participant_id  UUID REFERENCES participants(id),
  practitioner_id UUID REFERENCES practitioners(id),
  created_at      TIMESTAMPTZ DEFAULT NOW(),
  UNIQUE(participant_id, practitioner_id)
);
```

---

#### TABLE : resources
```sql
CREATE TABLE resources (
  id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  title           VARCHAR(255) NOT NULL,
  description     TEXT,
  type            ENUM('article', 'audio', 'practice', 'tool', 'video'),
  phase_numbers   DECIMAL(3,1)[],    -- phases où la ressource est disponible
  content_url     VARCHAR(500),
  duration_min    INTEGER,           -- pour audio/vidéo
  is_global       BOOLEAN DEFAULT true,   -- disponible pour tous ou spécifique praticien
  created_by      UUID REFERENCES users(id),
  created_at      TIMESTAMPTZ DEFAULT NOW()
);

CREATE TABLE participant_resources (
  participant_id  UUID REFERENCES participants(id),
  resource_id     UUID REFERENCES resources(id),
  assigned_by     UUID REFERENCES users(id),
  assigned_at     TIMESTAMPTZ DEFAULT NOW(),
  PRIMARY KEY (participant_id, resource_id)
);
```

---

#### TABLE : certifications
```sql
CREATE TABLE certifications (
  id                UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id           UUID REFERENCES users(id),
  supervisor_id     UUID REFERENCES users(id),
  curriculum_path   VARCHAR(100),           -- niveau de formation
  status            ENUM('applied', 'in_training', 'under_review', 'certified', 'rejected'),
  modules_completed JSONB DEFAULT '[]',
  exam_score        DECIMAL(5,2),
  issued_at         DATE,
  expires_at        DATE,
  notes             TEXT,
  created_at        TIMESTAMPTZ DEFAULT NOW(),
  updated_at        TIMESTAMPTZ DEFAULT NOW()
);
```

---

#### TABLE : audit_logs
```sql
CREATE TABLE audit_logs (
  id          UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id     UUID REFERENCES users(id),
  action      VARCHAR(100) NOT NULL,
  entity_type VARCHAR(100),
  entity_id   UUID,
  details     JSONB,
  ip_address  INET,
  user_agent  TEXT,
  created_at  TIMESTAMPTZ DEFAULT NOW()
);
-- Indexé sur (user_id, created_at) et (entity_type, entity_id)
```

---

### Index principaux

```sql
-- Performance requêtes fréquentes
CREATE INDEX idx_participants_practitioner ON participants(practitioner_id);
CREATE INDEX idx_phases_participant ON phases(participant_id);
CREATE INDEX idx_sessions_participant ON sessions(participant_id, session_date DESC);
CREATE INDEX idx_journal_participant ON journal_entries(participant_id, entry_date DESC);
CREATE INDEX idx_exercise_responses_participant ON exercise_responses(participant_id);
CREATE INDEX idx_messages_conversation ON messages(conversation_id, created_at DESC);
CREATE INDEX idx_audit_user_date ON audit_logs(user_id, created_at DESC);

-- Full-text search journal
CREATE INDEX idx_journal_fulltext ON journal_entries 
  USING gin(to_tsvector('french', content));
```

---

## 3. API — ENDPOINTS PRINCIPAUX

### Authentification
```
POST   /auth/register          Inscription praticien (en attente de validation)
POST   /auth/login             Connexion
POST   /auth/refresh           Rafraîchir le token
POST   /auth/logout            Déconnexion
POST   /auth/forgot-password   Réinitialisation mot de passe
POST   /auth/verify-email      Vérification email
```

### Participants (Praticien)
```
GET    /practitioners/:id/participants          Liste des participants
POST   /practitioners/:id/participants/invite   Inviter un participant
GET    /participants/:id                        Détail participant
PATCH  /participants/:id                        Mise à jour
DELETE /participants/:id                        Archiver

GET    /participants/:id/phases                 Parcours phases
PATCH  /participants/:id/phases/:phase/validate Valider une phase
```

### Mandala
```
GET    /participants/:id/mandala                État du mandala
PATCH  /participants/:id/mandala/ring/:n        Mettre à jour un anneau
POST   /participants/:id/mandala/export         Générer export image
```

### Journal
```
GET    /participants/:id/journal                Liste entrées (paginée)
POST   /participants/:id/journal                Nouvelle entrée
GET    /participants/:id/journal/:entryId       Détail entrée
PATCH  /participants/:id/journal/:entryId       Modifier entrée
DELETE /participants/:id/journal/:entryId       Supprimer entrée
```

### Exercices
```
GET    /exercises?phase=1                       Exercices d'une phase
GET    /participants/:id/exercises              Exercices du participant + état
POST   /participants/:id/exercises/:exId/start  Démarrer
PATCH  /participants/:id/exercises/:exId        Sauvegarder réponses
POST   /participants/:id/exercises/:exId/submit Soumettre
```

### Séances
```
GET    /practitioners/:id/sessions              Agenda praticien
POST   /practitioners/:id/sessions             Créer séance
GET    /sessions/:id                            Détail
PATCH  /sessions/:id                            Modifier
PATCH  /sessions/:id/notes                      Mettre à jour notes
PATCH  /sessions/:id/share                      Partager notes au participant
```

### Archétypes
```
GET    /archetypes                              Liste des 12 archétypes
POST   /participants/:id/archetypes             Attribuer un archétype
GET    /participants/:id/archetypes             Archétypes attribués
```

### Dossier d'Âme
```
GET    /participants/:id/soul-file              Statut et contenu
POST   /participants/:id/soul-file/generate     Générer le PDF
GET    /participants/:id/soul-file/download     Télécharger le PDF
POST   /participants/:id/soul-file/share        Partager avec participant
```

### Messagerie
```
GET    /conversations/:id/messages              Messages (paginé)
POST   /conversations/:id/messages              Envoyer un message
PATCH  /conversations/:id/read                  Marquer comme lu
```

---

## 4. SÉCURITÉ

### Authentification et sessions

```
- JWT (Access Token) : durée 15 minutes
- Refresh Token : durée 7 jours, rotation à chaque usage
- Stockage Refresh Token : httpOnly cookie (pas localStorage)
- Invalidation : table de refresh tokens révoqués en Redis
- 2FA optionnel (TOTP) pour les praticiens
```

### Autorisation

```
Modèle RBAC (Role-Based Access Control) + ABAC (Attribute-Based)

Guards NestJS sur chaque route :
- AuthGuard : token valide requis
- RolesGuard : rôle requis
- OwnershipGuard : "ce participant appartient-il à ce praticien ?"

Exemple :
  PATCH /participants/:id/phases/:phase/validate
  → Requiert : role=practitioner + participant.practitioner_id = req.user.practitionerId
```

### Chiffrement des données sensibles

```
At-rest :
  - Journal du participant : chiffré en base (AES-256-GCM)
  - Messages : chiffrés en base
  - Notes privées praticien : chiffrées en base
  - Clé de chiffrement : gérée via KMS (AWS KMS ou Scaleway Secret Manager)

In-transit :
  - TLS 1.3 obligatoire sur toutes les connexions
  - HSTS activé

PDF Dossier d'Âme :
  - Stocké en S3 avec accès présigné (URL temporaire 24h)
  - Pas de stockage direct côté client
```

### Sécurité applicative

```
- Rate limiting : 100 req/min par IP, 20 tentatives de login puis lockout
- Validation des inputs : Zod (frontend) + class-validator (backend)
- Protection CSRF : headers SameSite + CSRF token pour mutations
- Injection SQL : Prisma ORM paramétré — pas de requêtes dynamiques
- XSS : sanitisation Tiptap, Content Security Policy strict
- Headers sécurité : Helmet.js (NestJS)
- WAF : Cloudflare Rules
- Audit log : toutes les actions sensibles tracées
- Secrets : variables d'environnement via Railway/Vault — jamais en code
```

---

## 5. CONFORMITÉ RGPD

### Classification des données

| Catégorie | Type | Niveau de sensibilité |
|---|---|---|
| Identité | Prénom, email | Standard |
| Parcours thérapeutique | Journal, exercices, notes | Très sensible (Art. 9 RGPD) |
| Données de santé | Informations somatiques | Sensible |
| Communications | Messages praticien-participant | Sensible |
| Données de navigation | Logs, analytics | Standard |

**Note critique :** Les données de parcours ORIA peuvent être considérées comme des données relatives à la santé mentale au sens de l'article 9 RGPD. Un avis juridique sur la classification est fortement recommandé avant le lancement.

---

### Droits des utilisateurs

| Droit | Implémentation |
|---|---|
| Droit d'accès | Export de toutes ses données (JSON + PDF) depuis son profil |
| Droit de rectification | Modification profil + corrections journal |
| Droit à l'effacement | Suppression de compte avec suppression en cascade (soft delete 30j, hard delete ensuite) |
| Droit à la portabilité | Export données structuré (JSON) |
| Droit d'opposition | Désactivation notifications, analytics opt-out |
| Droit à la limitation | Suspension du compte sans suppression |

---

### Mesures organisationnelles

```
- DPA (Data Processing Agreement) avec tous les sous-traitants
- Registre des traitements (Article 30) tenu à jour
- Politique de confidentialité accessible avant toute inscription
- Consentement explicite au moment de l'inscription
- CGU spécifiques praticiens (incluant engagement confidentialité participants)
- Processus de réponse aux demandes de droits : < 30 jours
- Formation RGPD de l'équipe
- DPO désigné (ou recours à un DPO externalisé)
- Procédure de notification de violation : < 72h à la CNIL
```

---

### Hébergement et sous-traitants

| Service | Prestataire | Localisation |
|---|---|---|
| Base de données | Railway (PostgreSQL) → Scaleway (V1) | France/EU |
| Fichiers | Scaleway Object Storage | Paris, France |
| CDN | Cloudflare | EU nodes prioritaires |
| Email | Brevo | France |
| Paiement | Stripe | EU (DPA disponible) |
| Monitoring | Sentry | EU (option) |
| Analytics | Posthog (self-hosted ou EU cloud) | EU |

---

### Durées de conservation

```
Données actives participant         → Durée du parcours + 2 ans
Données archivées (parcours terminé)→ 5 ans (à partir de la fin du parcours)
Logs d'audit                        → 1 an
Données marketing / analytics       → 13 mois
Données de facturation              → 10 ans (obligation légale)
```

---

## 6. PERFORMANCE ET SCALABILITÉ

### Objectifs de performance (SLA MVP)

```
Temps de réponse API          : < 200ms (p95)
Chargement page principale    : < 2s (LCP)
Rendu Mandala Vivant          : < 500ms
Génération PDF Dossier d'Âme  : < 30s (async, notification)
Disponibilité                 : 99.5% (MVP) → 99.9% (V2)
```

### Stratégies de cache

```
Redis :
  - Sessions utilisateurs : 7 jours
  - État mandala : 5 minutes (invalidé à chaque update)
  - Liste des exercices par phase : 1 heure
  - Ressources médiathèque : 24 heures

PostgreSQL :
  - Query result caching via Prisma
  - Index stratégiques (voir section DB)

Cloudflare :
  - Assets statiques (images, fonts) : 1 an
  - Pages publiques (landing, certification) : 1 heure
  - API : pas de cache (données privées)
```

### Plan de scalabilité

```
MVP (0-500 utilisateurs) :
  - 1 serveur Railway (2 vCPU, 4 GB RAM)
  - PostgreSQL managé 20 GB
  - Monolithe modulaire NestJS

V1 (500-5 000 utilisateurs) :
  - Migration vers Scaleway EU
  - PostgreSQL dédié + Read Replica
  - Redis cluster
  - Load balancer
  - Séparation frontend/backend

V2+ (5 000+ utilisateurs) :
  - Architecture microservices si justifiée
  - Service PDF dédié (workers)
  - Service messagerie dédié
  - CDN pour assets participants
  - Backup multi-région
```

---

*ORIA — Architecture Technique · Version 1.0*
*Document propriétaire. Tous droits réservés.*
