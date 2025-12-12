# WBS : Work Breakdown Structure (Structure de Découpage du Projet)

## 1. Contexte et définitions des concepts clés

### 1.1 Qu'est-ce que le WBS ?

**WBS (Work Breakdown Structure)** est une méthode de découpage hiérarchique du projet en éléments plus petits et gérables qui permet de :
- Décomposer un projet complexe en tâches simples
- Ne rien oublier dans la planification
- Estimer le temps et les coûts précisément
- Répartir le travail efficacement
- Suivre l'avancement du projet

**Analogie simple :**
> Construire une maison est complexe, mais si tu la découpes en : fondations → murs → toit → électricité → plomberie → finitions, chaque partie devient gérable.

### 1.2 Principes fondamentaux

**La règle des 100% :**
- Le WBS doit représenter 100% du travail du projet
- Ni plus (pas de travail hors scope)
- Ni moins (tout doit être inclus)

**Décomposition hiérarchique :**
```
Niveau 0: Projet complet
    ↓
Niveau 1: Livrables principaux
    ↓
Niveau 2: Sous-livrables
    ↓
Niveau 3: Packages de travail
    ↓
Niveau 4: Tâches individuelles
```

### 1.3 Définitions importantes

- **Livrable** : Résultat tangible ou produit du projet
- **Package de travail (Work Package)** : Plus petite unité du WBS qu'on peut assigner
- **Niveau** : Profondeur de décomposition (généralement 3-5 niveaux)
- **Tâche** : Unité de travail élémentaire
- **Milestone** : Point de contrôle ou événement important
- **Dépendance** : Relation entre tâches (A doit finir avant B)
- **Dictionnaire WBS** : Document décrivant chaque élément du WBS

## 2. Quand et pourquoi utiliser le WBS

### 2.1 Pourquoi le WBS est indispensable

**Sans WBS (approche chaotique) :**
- ❌ On oublie des tâches importantes
- ❌ Impossible d'estimer le projet correctement
- ❌ On ne sait pas par où commencer
- ❌ Travail mal réparti dans l'équipe
- ❌ Surprises et retards constants
- ❌ Dépassement de budget

**Avec WBS :**
- ✅ Vue d'ensemble complète du projet
- ✅ Estimation précise du temps et coûts
- ✅ Répartition claire du travail
- ✅ Suivi d'avancement facile
- ✅ Détection précoce des problèmes
- ✅ Communication claire avec les parties prenantes

### 2.2 Cas d'usage idéaux

**Le WBS est parfait pour :**
- Projets complexes avec beaucoup de composants
- Planification initiale d'un projet
- Estimation de budget et ressources
- Projets avec équipes multiples
- Projets waterfall ou hybrides
- Appels d'offres et soumissions

**Exemples concrets :**
```
✅ Développement d'une application web complète
✅ Migration d'un système legacy
✅ Refonte d'un site web
✅ Mise en place d'une infrastructure cloud
✅ Projet ERP pour une entreprise
```

### 2.3 Quand ne pas utiliser le WBS seul

```
⚠️ Projet très agile avec scope changeant
   → Utiliser avec Product Backlog

⚠️ Maintenance quotidienne
   → Kanban plus adapté

⚠️ Très petit projet (< 1 semaine)
   → Overhead trop important
```

## 3. Comment créer un WBS

### 3.1 Les 2 approches de décomposition

**Approche Top-Down (Du haut vers le bas) :**
```
1. Commencer par le projet complet
2. Identifier les livrables majeurs
3. Décomposer chaque livrable en sous-livrables
4. Continuer jusqu'aux tâches élémentaires
```

**Exemple :**
```
Site E-commerce
  ├─ Front-end
  │   ├─ Page d'accueil
  │   ├─ Catalogue produits
  │   └─ Panier & Checkout
  ├─ Back-end
  │   ├─ API REST
  │   └─ Base de données
  └─ Déploiement
      ├─ Configuration serveur
      └─ CI/CD
```

**Approche Bottom-Up (Du bas vers le haut) :**
```
1. Brainstorming de toutes les tâches possibles
2. Regrouper les tâches similaires
3. Créer une hiérarchie
4. Valider la complétude
```

### 3.2 Les 3 formats de WBS

**1. Format hiérarchique (Organigramme) :**
```
                    ┌──────────────────┐
                    │ Site E-commerce  │
                    │  (Niveau 0)      │
                    └────────┬─────────┘
                             │
         ┌───────────────────┼───────────────────┐
         │                   │                   │
    ┌────▼─────┐      ┌─────▼──────┐     ┌─────▼──────┐
    │Front-end │      │  Back-end  │     │Déploiement │
    │(Niveau 1)│      │ (Niveau 1) │     │(Niveau 1)  │
    └────┬─────┘      └─────┬──────┘     └─────┬──────┘
         │                  │                   │
    ┌────┼────┐        ┌───┼────┐         ┌────┼─────┐
    │    │    │        │   │    │         │    │     │
  Page Cata. Panier  API  BDD  Infra   Config CI/CD
  Accueil                                 Serveur
```

**2. Format liste indentée :**
```
1.0 Site E-commerce
  1.1 Front-end
    1.1.1 Page d'accueil
      1.1.1.1 Design header
      1.1.1.2 Slider produits vedettes
      1.1.1.3 Footer
    1.1.2 Catalogue produits
      1.1.2.1 Liste des produits
      1.1.2.2 Filtres
      1.1.2.3 Pagination
    1.1.3 Panier & Checkout
  1.2 Back-end
    1.2.1 API REST
    1.2.2 Base de données
  1.3 Déploiement
    1.3.1 Configuration serveur
    1.3.2 CI/CD
```

**3. Format mind map (carte mentale) :**
```
            Front-end ─┬─ Page accueil
                       ├─ Catalogue
                       └─ Panier
                 ┌─────┘
Site E-commerce ─┼─ Back-end ─┬─ API
                 │             └─ Database
                 └─ Déploiement ─┬─ Serveur
                                 └─ CI/CD
```

### 3.3 Règles de décomposition

**Règle du 8/80 :**
- Aucune tâche < 8 heures (trop granulaire)
- Aucune tâche > 80 heures (trop gros, à découper)

**Règle de profondeur :**
- Minimum 3 niveaux
- Maximum 5-6 niveaux
- Au-delà = trop complexe

**Critères d'arrêt de la décomposition :**
```
Arrêter quand:
✅ La tâche peut être assignée à une personne
✅ On peut estimer le temps (1-10 jours)
✅ On peut mesurer l'avancement (0%, 50%, 100%)
✅ La tâche a un livrable clair
```

**Exemple de découpage correct :**
```
❌ Trop gros:
"Développer le site web" (500h)

✅ Bien découpé:
1.1.1.1 "Créer composant Header React" (8h)
1.1.1.2 "Intégrer Slider avec Swiper.js" (6h)
1.1.1.3 "Développer Footer responsive" (4h)
```

## 4. Exemple complet : Application E-commerce

### 4.1 WBS complet (Format liste)

```
1.0 Application E-commerce (Total: 800h)
│
├─ 1.1 Planification & Design (80h)
│   ├─ 1.1.1 Analyse des besoins (16h)
│   ├─ 1.1.2 Architecture système (16h)
│   ├─ 1.1.3 Design UI/UX (32h)
│   │   ├─ 1.1.3.1 Wireframes (12h)
│   │   ├─ 1.1.3.2 Maquettes haute fidélité (12h)
│   │   └─ 1.1.3.3 Prototypes interactifs (8h)
│   └─ 1.1.4 Spécifications techniques (16h)
│
├─ 1.2 Développement Front-end (300h)
│   ├─ 1.2.1 Configuration projet (16h)
│   │   ├─ 1.2.1.1 Setup React + TypeScript (4h)
│   │   ├─ 1.2.1.2 Configuration ESLint/Prettier (3h)
│   │   ├─ 1.2.1.3 Setup Tailwind CSS (3h)
│   │   ├─ 1.2.1.4 Configuration routing (3h)
│   │   └─ 1.2.1.5 State management (Redux) (3h)
│   │
│   ├─ 1.2.2 Authentification (40h)
│   │   ├─ 1.2.2.1 Page de login (8h)
│   │   ├─ 1.2.2.2 Page d'inscription (8h)
│   │   ├─ 1.2.2.3 Récupération mot de passe (8h)
│   │   ├─ 1.2.2.4 Gestion JWT tokens (8h)
│   │   └─ 1.2.2.5 Page profil utilisateur (8h)
│   │
│   ├─ 1.2.3 Catalogue produits (80h)
│   │   ├─ 1.2.3.1 Liste produits avec pagination (16h)
│   │   ├─ 1.2.3.2 Filtres et recherche (16h)
│   │   ├─ 1.2.3.3 Tri des résultats (8h)
│   │   ├─ 1.2.3.4 Page détail produit (16h)
│   │   ├─ 1.2.3.5 Carrousel images produit (8h)
│   │   ├─ 1.2.3.6 Avis clients (8h)
│   │   └─ 1.2.3.7 Produits similaires (8h)
│   │
│   ├─ 1.2.4 Panier & Commande (80h)
│   │   ├─ 1.2.4.1 Ajout au panier (8h)
│   │   ├─ 1.2.4.2 Page panier (16h)
│   │   ├─ 1.2.4.3 Calcul totaux et livraison (12h)
│   │   ├─ 1.2.4.4 Formulaire adresse (12h)
│   │   ├─ 1.2.4.5 Choix mode livraison (8h)
│   │   ├─ 1.2.4.6 Page paiement (16h)
│   │   └─ 1.2.4.7 Confirmation commande (8h)
│   │
│   ├─ 1.2.5 Dashboard Admin (64h)
│   │   ├─ 1.2.5.1 Vue d'ensemble (16h)
│   │   ├─ 1.2.5.2 Gestion produits (24h)
│   │   ├─ 1.2.5.3 Gestion commandes (16h)
│   │   └─ 1.2.5.4 Statistiques (8h)
│   │
│   └─ 1.2.6 Tests Front-end (20h)
│       ├─ 1.2.6.1 Tests unitaires composants (12h)
│       └─ 1.2.6.2 Tests E2E avec Cypress (8h)
│
├─ 1.3 Développement Back-end (280h)
│   ├─ 1.3.1 Configuration projet (16h)
│   │   ├─ 1.3.1.1 Setup Node.js + Express (4h)
│   │   ├─ 1.3.1.2 Configuration TypeScript (3h)
│   │   ├─ 1.3.1.3 Setup base de données (5h)
│   │   └─ 1.3.1.4 Configuration environnements (4h)
│   │
│   ├─ 1.3.2 API Authentification (40h)
│   │   ├─ 1.3.2.1 Endpoint register (8h)
│   │   ├─ 1.3.2.2 Endpoint login (8h)
│   │   ├─ 1.3.2.3 JWT génération/validation (8h)
│   │   ├─ 1.3.2.4 Reset password (8h)
│   │   └─ 1.3.2.5 Middleware auth (8h)
│   │
│   ├─ 1.3.3 API Produits (56h)
│   │   ├─ 1.3.3.1 CRUD produits (24h)
│   │   ├─ 1.3.3.2 Recherche et filtres (16h)
│   │   ├─ 1.3.3.3 Gestion images (8h)
│   │   └─ 1.3.3.4 Avis produits (8h)
│   │
│   ├─ 1.3.4 API Commandes (56h)
│   │   ├─ 1.3.4.1 Création commande (16h)
│   │   ├─ 1.3.4.2 Gestion panier (12h)
│   │   ├─ 1.3.4.3 Calcul frais livraison (8h)
│   │   ├─ 1.3.4.4 Gestion statuts (12h)
│   │   └─ 1.3.4.5 Historique commandes (8h)
│   │
│   ├─ 1.3.5 Intégration Paiement (40h)
│   │   ├─ 1.3.5.1 Intégration Stripe (24h)
│   │   ├─ 1.3.5.2 Webhooks paiement (8h)
│   │   └─ 1.3.5.3 Gestion remboursements (8h)
│   │
│   ├─ 1.3.6 Base de données (40h)
│   │   ├─ 1.3.6.1 Schéma utilisateurs (8h)
│   │   ├─ 1.3.6.2 Schéma produits (8h)
│   │   ├─ 1.3.6.3 Schéma commandes (8h)
│   │   ├─ 1.3.6.4 Indexes et optimisation (8h)
│   │   └─ 1.3.6.5 Migrations (8h)
│   │
│   └─ 1.3.7 Tests Back-end (32h)
│       ├─ 1.3.7.1 Tests unitaires (16h)
│       ├─ 1.3.7.2 Tests d'intégration (12h)
│       └─ 1.3.7.3 Tests de charge (4h)
│
├─ 1.4 Infrastructure & DevOps (80h)
│   ├─ 1.4.1 Configuration CI/CD (24h)
│   │   ├─ 1.4.1.1 Pipeline GitHub Actions (8h)
│   │   ├─ 1.4.1.2 Tests automatisés (8h)
│   │   └─ 1.4.1.3 Déploiement auto (8h)
│   │
│   ├─ 1.4.2 Configuration serveurs (32h)
│   │   ├─ 1.4.2.1 Setup AWS/Azure (12h)
│   │   ├─ 1.4.2.2 Configuration Docker (8h)
│   │   ├─ 1.4.2.3 Base de données production (8h)
│   │   └─ 1.4.2.4 CDN pour assets (4h)
│   │
│   ├─ 1.4.3 Monitoring (16h)
│   │   ├─ 1.4.3.1 Logs centralisés (8h)
│   │   └─ 1.4.3.2 Alertes (8h)
│   │
│   └─ 1.4.4 Sécurité (8h)
│       ├─ 1.4.4.1 HTTPS/SSL (4h)
│       └─ 1.4.4.2 Firewall & Backup (4h)
│
└─ 1.5 Documentation & Formation (60h)
    ├─ 1.5.1 Documentation technique (24h)
    │   ├─ 1.5.1.1 Documentation API (8h)
    │   ├─ 1.5.1.2 Guide développeur (8h)
    │   └─ 1.5.1.3 Architecture (8h)
    │
    ├─ 1.5.2 Documentation utilisateur (24h)
    │   ├─ 1.5.2.1 Guide utilisateur (12h)
    │   ├─ 1.5.2.2 FAQ (8h)
    │   └─ 1.5.2.3 Vidéos tutoriels (4h)
    │
    └─ 1.5.3 Formation équipe (12h)
        ├─ 1.5.3.1 Formation admin (6h)
        └─ 1.5.3.2 Formation support (6h)
```

### 4.2 Dictionnaire WBS (Extraits)

**1.2.3 - Catalogue produits**
```
ID: 1.2.3
Nom: Catalogue produits
Description: Interface permettant aux utilisateurs de parcourir 
             et rechercher les produits
Livrables: 
  - Pages React fonctionnelles
  - Composants réutilisables
  - Tests unitaires
Responsable: Équipe Front-end
Durée estimée: 80h
Dépendances: 1.2.1 (Configuration projet)
Critères d'acceptation:
  - Liste affiche tous les produits
  - Pagination fonctionne (20 produits/page)
  - Filtres opérationnels (prix, catégorie, marque)
  - Recherche retourne résultats pertinents
  - Responsive mobile/desktop
```

## 5. Du WBS à la planification

### 5.1 Estimer les durées

**Techniques d'estimation :**

**1. Estimation par analogie :**
```
"La page login précédente a pris 8h
La page inscription est similaire
→ Estimation: 8h"
```

**2. Estimation 3 points (PERT) :**
```
Optimiste (O): 6h
Probable (P): 8h
Pessimiste (P): 14h

Estimation PERT = (O + 4×P + P) / 6
                = (6 + 4×8 + 14) / 6
                = 52 / 6
                = 8.7h ≈ 9h
```

**3. Bottom-up (du bas vers le haut) :**
```
Page Login:
  - HTML/JSX: 2h
  - Validation formulaire: 2h
  - Intégration API: 2h
  - Tests: 2h
  Total: 8h
```

### 5.2 Identifier les dépendances

**Types de dépendances :**

**Finish-to-Start (FS) - La plus courante :**
```
A doit finir avant que B commence
Exemple: Design UI → Développement UI
```

**Start-to-Start (SS) :**
```
B peut commencer quand A commence
Exemple: Développement Front + Back en parallèle
```

**Finish-to-Finish (FF) :**
```
B finit quand A finit
Exemple: Tests manuels + Documentation
```

**Start-to-Finish (SF) - Rare :**
```
B finit quand A commence
```

**Exemple de dépendances :**
```
1.1.3 Design UI/UX → 1.2.2 Développement Front-end (FS)
1.2.2 Dev Front → 1.3.2 Dev Back (SS - parallèle)
1.2.6 Tests Front → 1.3.7 Tests Back (FF - finissent ensemble)
```

### 5.3 Assigner les ressources

**Matrice de responsabilités :**
```
Tâche               | Alice | Bob | Charlie | Total
--------------------|-------|-----|---------|------
1.2.2 Auth Frontend |  40h  |  -  |    -    |  40h
1.2.3 Catalogue     |  40h  | 40h |    -    |  80h
1.2.4 Panier        |   -   | 40h |   40h   |  80h
1.3.2 API Auth      |   -   |  -  |   40h   |  40h
```

## 6. Outils pour créer un WBS

### 6.1 Outils gratuits

**Microsoft Excel / Google Sheets :**
```
Avantages:
✅ Accessible partout
✅ Facile pour débutants
✅ Export facile

Format de colonnes:
| ID | Tâche | Durée | Ressource | Début | Fin |
```

**Outil en ligne gratuit :**
- **WBS Tool** (wbstool.com)
- **MindMup** (mindmup.com)
- **Coggle** (coggle.it) - Mind maps

**PowerPoint / Google Slides :**
```
Avantages:
✅ Visuel pour présentation
✅ Organigramme facile

Inconvénients:
❌ Pas de calculs automatiques
❌ Mise à jour manuelle
```

### 6.2 Outils professionnels

**Microsoft Project :**
```
Avantages:
✅ Standard de l'industrie
✅ Intégration complète (WBS + Gantt + Ressources)
✅ Puissant

Inconvénients:
❌ Payant (cher)
❌ Courbe d'apprentissage
```

**Alternatives :**
- **Monday.com** : Visuel et moderne
- **Asana** : Simple et collaboratif
- **Jira** : Intégration avec développement agile
- **Smartsheet** : Comme Excel mais collaboratif

### 6.3 Template Excel à utiliser

**Colonnes recommandées :**
```
| WBS ID | Tâche | Description | Durée | Unité | Début | Fin | Ressource | Coût | Statut |
```

**Exemple :**
```
| 1.2.3.1 | Liste produits | Composant React affichant tous les produits | 16 | heures | 01/12 | 02/12 | Alice | 800€ | En cours |
```

## 7. Erreurs et pièges à éviter

### 7.1 Erreurs fréquentes

| Erreur | Description | Impact | Solution |
|--------|-------------|--------|----------|
| **Trop de détails** | WBS de 500 tâches de 1h | Ingérable et overhead | Max 100-150 tâches |
| **Pas assez détaillé** | 5 grosses tâches vagues | Impossible à estimer | Respecter règle 8/80 |
| **Oublier des tâches** | Tests, doc oubliés | Retards et dépassements | Checklist complète |
| **Orienté activités** | "Coder", "Tester" | Pas de livrables clairs | Orienté livrables |
| **Pas de numérotation** | Tâches sans ID | Impossible de référencer | Numérotation systématique |
| **Durées irréalistes** | Estimation au doigt mouillé | Planning faux | Techniques d'estimation |
| **Ignorer dépendances** | Tout en parallèle | Blocages imprévus | Cartographie dépendances |

### 7.2 Pièges courants

**Piège 1 : WBS = Liste de tâches**
```
❌ Confusion commune
"To-do list" ≠ WBS

✅ Différence:
WBS = Structure hiérarchique complète du projet
To-do = Liste plate de tâches à faire
```

**Piège 2 : Orienté activités au lieu de livrables**
```
❌ Mauvais (activités):
- Coder
- Tester
- Déployer

✅ Bon (livrables):
- Page de login fonctionnelle
- Suite de tests automatisés
- Application déployée en production
```

**Piège 3 : Planifier trop tôt**
```
❌ WBS créé sans connaissance du projet
   → Estimations fausses
   → Oublis majeurs

✅ Créer WBS après:
   - Analyse des besoins
   - Design architectural
   - Compréhension du scope
```

**Piège 4 : WBS figé et jamais mis à jour**
```
❌ WBS créé au début et oublié
   → Divergence avec la réalité

✅ WBS vivant:
   - Révision régulière
   - Ajustements selon l'avancement
   - Version control (v1.0, v1.1, etc.)
```

### 7.3 Conseils pour un bon WBS

**1. Règle des noms :**
```
✅ Bon: "Page de login React avec validation"
❌ Mauvais: "Login"

✅ Bon: "API REST pour authentification JWT"
❌ Mauvais: "Back-end"
```

**2. Tests de validité :**
```
Pour chaque élément, demande-toi:
❓ Peut-on l'assigner à quelqu'un ?
❓ Peut-on estimer la durée (1-10 jours) ?
❓ Y a-t-il un livrable clair ?
❓ Peut-on mesurer l'avancement ?

Si 4× OUI → Bon niveau de détail
```

**3. Percentage rule (règle du pourcentage) :**
```
Niveau parent = Somme des enfants

1.2 Front-end (300h)
  1.2.1 Config (16h)
  1.2.2 Auth (40h)
  1.2.3 Catalogue (80h)
  1.2.4 Panier (80h)
  1.2.5 Admin (64h)
  1.2.6 Tests (20h)
  Total enfants: 300h ✅
```

## 8. Résumé de l'essentiel

### Points clés à retenir

1. **WBS = Décomposition hiérarchique**
   - Projet → Livrables → Sous-livrables → Tâches
   - 3-5 niveaux de profondeur
   - 100% du travail représenté

2. **3 formats possibles**
   - Organigramme (visuel, présentation)
   - Liste indentée (détaillé, Excel)
   - Mind map (brainstorming, créativité)

3. **Règles de décomposition**
   - Règle 8/80 : 8h minimum, 80h maximum par tâche
   - Règle 100% : Tout le travail inclus
   - Orienté livrables, pas activités

4. **WBS → Planification**
   - Estimation des durées
   - Identification des dépendances
   - Assignation des ressources
   - Calcul du chemin critique

### Template WBS simplifié

```
1.0 Nom du Projet
│
├─ 1.1 Planification
│   ├─ 1.1.1 Analyse besoins
│   ├─ 1.1.2 Architecture
│   └─ 1.1.3 Design
│
├─ 1.2 Développement
│   ├─ 1.2.1 Configuration
│   ├─ 1.2.2 Feature A
│   ├─ 1.2.3 Feature B
│   └─ 1.2.4 Tests
│
├─ 1.3 Déploiement
│   ├─ 1.3.1 Infrastructure
│   ├─ 1.3.2 CI/CD
│   └─ 1.3.3 Monitoring
│
└─ 1.4 Documentation
    ├─ 1.4.1 Doc technique
    └─ 1.4.2 Doc utilisateur
```

### Checklist création WBS

**Préparation :**
- [ ] Scope du projet défini
- [ ] Livrables principaux identifiés
- [ ] Équipe projet constituée

**Création :**
- [ ] Niveau 1 : Livrables majeurs listés
- [ ] Niveau 2-3 : Décomposition complète
- [ ] Numérotation unique pour chaque élément
- [ ] Vérification règle 100%
- [ ] Application règle 8/80

**Validation :**
- [ ] Chaque tâche assignable
- [ ] Durées estimées
- [ ] Dépendances identifiées
- [ ] Livrable clair pour chaque élément
- [ ] Revue par l'équipe

### Processus de création en 7 étapes

```
1. Identifier le projet et les objectifs
   ↓
2. Lister les livrables majeurs (Niveau 1)
   ↓
3. Décomposer chaque livrable (Niveau 2-3)
   ↓
4. Continuer jusqu'aux work packages
   ↓
5. Numéroter tous les éléments
   ↓
6. Estimer les durées
   ↓
7. Valider avec l'équipe et stakeholders
```

### Formules utiles

```
Estimation PERT:
E = (Optimiste + 4×Probable + Pessimiste) / 6

Nombre de tâches optimal:
N = √(Durée totale du projet en heures / 40)

Profondeur WBS:
3 niveaux minimum
5 niveaux maximum
```

---

**En une phrase :**

> Le WBS (Work Breakdown Structure) est un outil de décomposition hiérarchique qui transforme un projet complexe en packages de travail gérables de 8 à 80 heures, permettant une estimation précise, une répartition claire du travail, et un suivi d'avancement efficace grâce à une structure orientée livrables avec numérotation unique.

**Pour être employable :**

**Tu DOIS savoir :**
- ✅ Expliquer ce qu'est un WBS et à quoi ça sert
- ✅ Créer un WBS simple pour un petit projet
- ✅ Décomposer une tâche complexe en sous-tâches
- ✅ Identifier les dépendances entre tâches
- ✅ Estimer la durée d'une tâche
- ✅ Lire et comprendre un WBS existant

**Vocabulaire à maîtriser absolument :**
WBS, Work Package, Livrable, Décomposition hiérarchique, Dépendances, Règle 100%, Règle 8/80, Dictionnaire WBS, Milestone.
