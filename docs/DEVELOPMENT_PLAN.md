# Plan de développement IDCS – Parcours intégral A → Z
*(Langue : français)*

Ce document définit le plan de réalisation d’IDCS : une application web premium construite avec **Laravel**, **Blade**, **Livewire** et **Tailwind CSS**. Elle couvre le site public, l’espace d’administration, le portail utilisateur et une plateforme de collecte et d’analyse inspirée de KoboToolbox/ODK.

## Socle technique retenu

- **Backend** : Laravel 12 ou version LTS disponible au démarrage, PHP 8.3+, Composer et MySQL 8.
- **Rendu web** : Blade pour les layouts et pages SEO, composants Blade réutilisables, Livewire pour les interfaces interactives sans SPA généralisée.
- **Frontend** : Tailwind CSS, Vite, Alpine.js uniquement pour les interactions locales légères et JavaScript moderne pour les besoins spécialisés.
- **Authentification** : Laravel Fortify ou starter kit Laravel adapté à Livewire, vérification email, réinitialisation du mot de passe et 2FA optionnelle.
- **Données et fichiers** : Eloquent ORM, migrations, factories, seeders, Laravel Filesystem avec stockage local ou S3-compatible.
- **Asynchrone et cache** : Redis, Laravel Queues, Scheduler, Horizon et événements/listeners.
- **API et intégrations** : routes `api.php`, Laravel Sanctum pour les tokens, Resources JSON, webhooks et contrats documentés OpenAPI.
- **Qualité et exploitation** : Pest/PHPUnit, Laravel Pint, Larastan, Laravel Debugbar en développement, Telescope en environnement contrôlé, GitHub Actions et Sentry.

Le principe directeur est de rester **server-driven** : Blade et Livewire portent les parcours métier et l’accessibilité, tandis que JavaScript, Chart.js, FullCalendar ou une carte ne sont ajoutés que lorsqu’ils apportent une valeur réelle.

---

## Phase A – Cadre stratégique & gouvernance
- **Vision** : rappeler la mission IDCS (consultance scientifique, data, orientation humanitaire) et aligner les parties prenantes (PME, ONG, universités, autorités, startups, chercheurs, particuliers).
- **Backlog** : dériver les 13 blocs front/admin/utilisateur listés dans `IDCS.txt` en épics, définir périmètre MVP vs évolutions.
- **Organisation** : RACI, rituels agiles (sprints 2 semaines), outils (Jira/Linear, Figma, Notion, GitHub).
- **Branding** : charte graphique premium, ton éditorial, guidelines UI/UX inspirés de design GitHub-like.

## Phase B – Découverte produit & UX premium
1. **Recherche utilisateur** : ateliers métiers, interviews pour prioriser usages (collecte terrain, consultance, formation, publications, appui recherche).
2. **Information architecture** : cartographie complète : Accueil, Missions/Pourquoi IDCS, Collecte & Analyse, Conseils & Orientation, Publications, Formation & Sensibilisation, Appui recherche/Données, Footer enrichi, Dashboard admin, modules utilisateur.
3. **User flows & maquettes** : prototypes interactifs (hero dynamique, CTA flottant, timeline, carrousel témoignages, FAQ), maquettes responsive (desktop/tablette/mobile) avec animations et transitions prévues.

## Phase C – Architecture Laravel
- **Initialisation** : création du projet Laravel, configuration `.env`, environnements local/staging/production, Composer, Vite, Tailwind CSS et CI GitHub Actions.
- **Organisation** : `app/Models`, `app/Http/Controllers`, `app/Livewire`, `app/Services`, `app/Actions`, `app/Policies`, `app/Jobs`, `app/Notifications`, `app/Events`, `app/Listeners`, `app/Rules` et `app/Support`.
- **Routes et contrôleurs** : `routes/web.php` pour le site et les espaces authentifiés, `routes/api.php` pour les intégrations et collecteurs, contrôleurs fins, Form Requests pour la validation et route model binding.
- **Vues** : layouts Blade publics, layouts admin et utilisateur, composants Blade (`resources/views/components`), composants Livewire par fonctionnalité et partials partagés.
- **Services applicatifs** : actions/services testables pour les imports, exports, analyses, synchronisations, notifications, stockage, anonymisation et génération de documents.
- **Accès aux données** : Eloquent, scopes, relations, casts, API Resources, pagination et repositories uniquement lorsqu’une abstraction est justifiée par le domaine.
- **Packages structurants** : Laravel Sanctum, Laravel Horizon, Spatie Permission pour les rôles/permissions, Spatie Media Library si nécessaire, Laravel Excel pour les exports et Scout/Meilisearch ou Elasticsearch pour la recherche.
- **Observabilité** : logs Laravel/Monolog par canaux, Sentry, Telescope hors production publique, métriques Laravel Pulse ou Prometheus selon l’infrastructure.

## Phase D – Base de données & migrations
1. **Modèle conceptuel** : utilisateurs, rôles, organisations, formulaires, sections/questions, soumissions, médias, rendez-vous/événements, formations, partenaires, publications, notifications, logs/audit, widgets, projets/business plans, tickets/support.
2. **Conception logique** : contraintes FK, index, partitionnement tables volumineuses (soumissions, logs), champs audit (created_at/by, updated_at/by), soft deletes.
3. **Migrations & seeds** : migrations Laravel dans `database/migrations`, factories et seeders reproductibles pour les rôles (admin, analyste, consultant, collecteur, partenaire, utilisateur avancé), les permissions et les contenus de démonstration.

## Phase E – Noyau Laravel & services transverses
- **Domaine** : modèles Eloquent, services/actions métier, événements de domaine et policies pour conserver une logique réutilisable entre Blade, Livewire et API.
- **Middleware** : authentification session et Sanctum, CSRF Laravel, limitation de débit, locale, permissions, vérification email et journalisation des accès.
- **Interfaces Livewire** : recherche, filtres, tableaux paginés, formulaires multi-étapes, upload, modales, notifications et mise à jour temps réel via polling ou broadcasting si nécessaire.
- **API REST** : préfixe `/api/v1` pour formulaires, soumissions, publications, rendez-vous, formations et notifications ; Resources JSON, Form Requests, pagination cursor et versionnement.
- **Sécurité fondamentale** : hachage Argon2id, rotation de session, validation Laravel, échappement Blade, CSP, headers de sécurité, signed URLs et protection des téléchargements.
- **Intégrations** : mailables et notifications Laravel, canaux email/SMS/in-app, stockage Filesystem, analytics privacy-first, webhooks et services d’analyse.

## Phase F – Front public Moderne, premium et dynamique
1. **Accueil** : hero scientifique, slogan, CTA flottant, barre de recherche, timeline,Statistiques, carrousel témoignages, section Actualités dynamique, animations CSS/GSAP.
2. **Nos Missions / Pourquoi IDCS** : cartes objectifs (accessibilité, transparence, innovation, formation, appui recherche) + section “Pour qui ?” (PME, ONG, chercheurs, autorités, startups, personnes physiques).
3. **Collecte & Analyse** : visuels data, graphiques Chart.js, CTA “Demander une analyse” + upload dataset.
4. **Conseils & Orientation** : formulaire prise de rendez-vous, FAQ, témoignages.
5. **Publications & Ressources** : blog/rapports/études, filtres catégories, moteur de recherche.
6. **Formation & Sensibilisation** : calendrier ateliers/webinaires, CTA inscription, espace universités.
7. **Appui à la recherche / Données** : mise en avant datasets, services data, partenariats.
8. **Footer enrichi** : liens rapides, contacts, réseaux sociaux, mini KPI, newsletter, mentions légales.
9. **Techniques UI/UX** : Tailwind CSS avec design tokens IDCS, composants Blade réutilisables, variantes de composants accessibles, SEO schema.org, OG tags, accessibilité AA, tests Lighthouse, navigation progressive, thème clair et sombre, code couleur `#002F6C` et `#FF7F00`, typographie définie dans Tailwind et cohérente avec la marque.
- Les pages publiques sont principalement rendues par Blade pour le SEO ; les filtres, formulaires, tableaux, inscriptions et interactions métier sont réalisés avec Livewire.
- Les styles sont centralisés dans `resources/css/app.css`, la configuration dans `tailwind.config.js` ou les fichiers de configuration de la version installée, et le build dans Vite.


## Phase G – Modules Admin (12 blocs complets)
1. Tableau de bord synthétique : stats temps réel (soumissions, formations, rendez-vous), graphiques, notifications, widgets personnalisables, accès rapide, avec composants Livewire et jobs d’agrégation en arrière-plan.
2. Gestion utilisateurs & rôles : CRUD, import/export, activation/désactivation, 2FA optionnelle, historique connexions, audit.
3. Publications & ressources : workflow `draft` → `review` → `published`, éditeur compatible Blade/Livewire, aperçu, planification de publication et notifications ciblées.
4. Données collectées : mapping formulaires, validation, visualisation tableaux/graphes, export multi-format, logs d’accès.
5. Rendez-vous / événements : calendrier FullCalendar intégré à Livewire, prise de rendez-vous, règles de disponibilité, invitations email/SMS et fichiers ICS.
6. Formations : création sessions, inscriptions, suivi assiduité, certificats.
7. Partenaires / organisations : fiches complètes, documents, KPI, suivi collaborations.
8. Notifications / messages : segmentation audiences, templates, campagnes email/SMS/in-app, historique.
9. Paramètres du centre : branding, politiques sécurité, accès API, quotas.
10. Logs d’activité & audit : recherches multi-filtres, alertes anomalies, export CSV/JSON.
11. Support / FAQ admin : base de connaissances interne, tickets, tutoriels.
12. Recherche globale & widgets : moteur FULLTEXT + filtres, favoris, quick actions drag & drop.

## Phase H – Espace utilisateur avancé (15 blocs)
1. Inscription / connexion / réinitialisation du mot de passe avec starter kit Laravel, validation email et 2FA optionnelle.
2. Onboarding & tutoriels (checklist, NPS).
3. Dashboard personnel : widgets custom, stats projets/soumissions/notifs.
4. Gestion projets : création, collaboration, versioning, export PDF/Docx.
5. Publications & ressources personnalisées.
6. Gestion des données : upload via Livewire avec validation et stockage Filesystem, suivi des analyses par Jobs/Notifications et visualisation interactive.
7. Rendez-vous & calendrier : demandes consultance, sync ICS.
8. Formations : inscriptions, progression, certificats.
9. Partenaires & documents partagés.
10. Notifications / messages : inbox, filtres, ciblage.
11. Paramètres profil & sécurité.
12. Logs activité utilisateur.
13. Support / FAQ contextualisée.
14. Recherche globale (contenus, projets, formations, publications).
15. Widgets personnalisables (drag & drop, favoris).

## Phase I – Collecte & synchronisation type KoboToolbox
Implémentation d’une stack complète de collecte offline-first, synchronisation et analyse, inspirée de KoboToolbox/ODK. Le portail web d’administration et de supervision est réalisé avec Blade/Livewire ; le mode hors ligne du collecteur repose sur une PWA légère avec IndexedDB et JavaScript ciblé.

### **1. Infrastructure de collecte (Collecteurs terrain)**
- **Application mobile-like** : interface optimisée pour appareils mobiles (faible bande passante, écrans petits).
- **Mode offline natif** : formulaires téléchargés, réponses stockées localement en SQLite/IndexedDB, synchronisation au retour connexion.
- **Gestion fichiers médias** : photos, audio, vidéo compressées localement, upload chunked avec retry automatique.
- **Géolocalisation** : capture GPS optionnelle (lat/lon/alt/accuracy), markers sur carte, historique traces.
- **Validation en temps réel** : contraintes type (min/max, regex, skip logic) appliquées côté client.
- **Métadonnées d'audit** : device ID, collecteur ID, timestamp, versioning offline UUID → submission ID.

### **2. Synchronisation & résolution conflits**
- **Manifest pull** : collecteur récupère list formulaires + versions, métadonnées (création date, questions, champs obligatoires).
- **Push différé** : file d'attente locale, retry exponentiel, marquage sync_status (pending → synced).
- **Versioning** : if form_version_updated → notification collecteur, re-download, merge réponses existantes.
- **Résolution conflits** : last-write-wins ou merge intelligent (médias + réponses texte disjoints).
- **Compression** : export JSON compressé gzip, signatures HMAC-SHA256 pour intégrité.

### **3. Flux réception au serveur**
- **API collecteurs** : endpoints `/api/v1/submissions/` protégés par Sanctum, tokens révocables émis par l’administration et Form Requests Laravel.
- **Validation qualité** : vérification schéma, type données, contraintes, images taille, coordonnées valides.
- **Enrichissement** : reverse geocoding GPS, extraction métadonnées fichiers, hachage données sensibles.
- **Stockage sécurisé** : chiffrement champs PII (AES-256), audit trail changements, soft deletes.
- **Acknowledge & feedback** : ACK immédiat, notifications anomalies, quotas (max 100MB/jour/collecteur).

### **4. Gestion formulaires & versions**
- **Versionning formulaires** : tracking modifications questions (add/remove/rename), déploiement progressif.
- **Déploiement** : admin publie version 1.0, collecteurs reçoivent manifest, update optionnel vs forcé.
- **Compatibilité** : server accepte réponses versions N-2, migration données anciennes questions archivées.
- **Rollback** : restauration version antérieure, re-activation pour collecteurs actifs.

### **5. Tableaux de bord temps réel (Analystes/Admin)**
- **Submission tracker** : vue live (auto-refresh 10s) soumissions entrantes, statut sync, anomalies détectées.
- **Géovisualisation** : cluster map, heatmaps soumissions par zone, drill-down détails.
- **Quality monitoring** : graphiques taux complétude, durée remplissage, drop-off questions, outliers détectés.
- **Performance collector** : statistiques par collecteur (count, avg_time, error_rate), leaderboards, incitations.
- **Alerting** : notifications anomalies (image corrompue, GPS hors bounds, réponse impossible), escalade.

### **6. Exports & intégrations**
- **Multi-formats** : CSV (wide + long format), XLS avec styles, JSON (flat + nested), GeoJSON pour GPS.
- **Filtres avancés** : par plage dates, collecteurs, zones géo, statut, complétude, mots-clés.
- **Exports planifiés** : Jobs Laravel sur Redis/Horizon, Scheduler, email automatique, upload vers Google Drive/OneDrive et webhooks.
- **Intégrations** : import vers Power BI/Tableau, export Kobo/ODK Central, API webhooks pour 3e systèmes.

### **7. Analytics & reporting**
- **Rapports dynamiques** : générateur visual (filtres, agrégations, calculs, pivots), exports PDF/PPTX.
- **Analyses prédéfinies** : répartitions réponses (pie/bar charts), cross-tabulations, analyses texte (word clouds).
- **Time series** : courbes soumissions par jour, tendances réponses, comparaisons périodes.
- **Données sensibles** : anonymisation avant export (hash IDs, masquage noms), conformité RGPD, audit traces.

### **8. Sécurité & gouvernance collecte**
- **RBAC collecteurs** : roles (collecteur_basique, senior_collecteur, superviseur), permissions per formulaire/zone.
- **Credentials & tokens** : auth tokens révocables (exp 90j), device tokens, audit logins échoués.
- **Données sensibles** : PII flagging (phone, email, ID), chiffrement stockage/transit, purges programmées.
- **Conformité** : RGPD (right to delete submissions), audit trail complet, backups chiffrés, disaster recovery.

### **9. Scaling & optimisation**
- **Indexation** : full-text search sur réponses texte (MySQL FULLTEXT ou Elasticsearch).
- **Partitionnement** : tables soumissions partitionnées par date (hebdo/mensuel), archivage données anciennes.
- **Caching** : Cache Laravel/Redis des formulaires (5 min) et réponses agrégées (15 min), invalidé par événements métier.
- **Monitoring** : métriques soumissions/sec, latence API, storage usage, alertes seuils dépassés.


## Phase J – Sécurité, conformité & observabilité
- RBAC granulaire avec policies Laravel et permissions par module, organisation, formulaire et zone.
- Chiffrement des données sensibles via casts/services Laravel, stockage privé Filesystem, URLs temporaires et rotation des clés d’application.
- CSRF Laravel, CSP, protections XSS/SQLi, validation Form Requests, scans Composer/npm et tests d’intrusion périodiques.
- RGPD : consentement, droit à l’oubli, registre traitements, anonymisation datasets.
- Backups automatisés (base + médias), PRA/PCA, plan de reprise.
- Audit trail (events user_login, form_submission, data_export, rendez-vous, formation, publication).

## Phase K – Qualité, tests & automatisation
- **Tests unitaires et feature** : Pest ou PHPUnit sur actions, services, modèles, policies et composants Livewire ; parcours clés (collecte, admin, utilisateur) couverts par Laravel HTTP tests et Livewire testing.
- **Tests API** : tests intégrés Laravel, Postman/Newman pour les contrats externes et tests de performance k6.
- **Lint/format** : Laravel Pint, Larastan/PHPStan, ESLint/Prettier si JavaScript spécialisé, contrôle Tailwind et build Vite.
- **CI/CD** : GitHub Actions/GitLab CI (lint, tests, build assets, packaging), sécurité (Dependabot).
- **Environnements** : dev (Docker Compose), staging, préprod, prod avec données anonymisées.

## Phase L – Livraison, déploiement & exploitation
- Pipelines GitHub Actions vers VPS, Laravel Forge ou conteneurs, avec installation Composer/npm, build Vite, migrations `--force` contrôlées et redémarrage des workers Horizon.
- Scheduler Laravel configuré par cron, workers supervisés, health checks applicatifs, monitoring uptime et alerting.
- Observabilité complète : logs Laravel centralisés, dashboards Laravel Pulse/Grafana, Sentry et métriques Prometheus selon le besoin.
- Documentation runbooks (opérations, incidents, déploiements), guides utilisateur/admin.

## Phase M – Formation, adoption & support
- Sessions formation internes (admin, analystes, collecteurs) + contenus e-learning.
- Guides pas-à-pas, vidéos, FAQ intégrée, support omnicanal.
- Boucle feedback (surveys in-app, NPS), roadmap publique, amélioration continue.

## Roadmap complète (24 semaines – exemple A → Z)
1. **Sem 1-2** : Phase A/B – cadrage, recherche utilisateur, architecture d’information, charte graphique.
2. **Sem 3-4** : Phase C/D – installation Laravel, configuration des environnements, design de la base, migrations, factories et seeders initiaux.
3. **Sem 5-6** : Phase E – authentification Laravel, layouts Blade, composants Livewire de base, policies, permissions et services métier.
4. **Sem 7-8** : Phase F – front public Blade/Tailwind (Accueil, Missions, Collecte & Analyse, Footer).
5. **Sem 9-10** : Phase G – builder de formulaires Livewire, import/export XLSForm/Kobo, API Sanctum des collecteurs.
6. **Sem 11-12** : Phase G (suite) – gestion soumissions, automatisation analyses, dashboards data.
7. **Sem 13-16** : Phase G suite – implémentation progressive des 12 modules admin avec Blade, Livewire, policies et jobs Laravel.
8. **Sem 17-20** : Phase H/I – espace utilisateur avancé et collecte offline-first (inscription, onboarding, dashboard, projets, formations, rendez-vous, synchronisation et widgets).
9. **Sem 21-22** : Phase J/K – durcissement sécurité, tests de charge, CI/CD complet, observabilité.
10. **Sem 23** : Phase L – déploiement staging → préprod, tests bout-en-bout, documentation runbooks.
11. **Sem 24** : Phase M – formation, accompagnement lancement, collecte feedback, préparation roadmap post-MVP.

---

En suivant ces phases séquencées, IDCS disposera d’une application Laravel moderne, sécurisée et extensible. Blade assurera le rendu public et les layouts, Livewire les interactions métier riches, Tailwind CSS l’interface cohérente et responsive, et les services Laravel la collecte avancée ainsi que l’exploitation des données comparable aux solutions KoboToolbox/ODK.
