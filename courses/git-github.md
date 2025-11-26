# Git et GitHub : Contrôle de Version et Collaboration

## 1. Contexte et définitions des concepts clés

### 1.1 Qu'est-ce que Git ?

**Git** est un système de contrôle de version distribué qui permet de :
- Suivre l'historique des modifications du code
- Travailler à plusieurs sur le même projet
- Revenir en arrière en cas d'erreur
- Créer des branches pour développer des fonctionnalités isolément

### 1.2 Qu'est-ce que GitHub ?

**GitHub** est une plateforme en ligne qui héberge des dépôts Git et offre :
- Stockage cloud des projets
- Collaboration via Pull Requests
- Gestion de projets (issues, projects)
- CI/CD (GitHub Actions)
- Documentation (README, Wiki)

### 1.3 Définitions importantes

- **Repository (repo)** : Dossier contenant votre projet et son historique Git
- **Commit** : Snapshot (instantané) de vos modifications à un moment donné
- **Branch** : Ligne de développement parallèle
- **Merge** : Fusion de deux branches
- **Pull Request (PR)** : Demande de fusion de code avec revue
- **Clone** : Copie locale d'un repository distant
- **Push** : Envoyer vos commits vers le serveur
- **Pull** : Récupérer les dernières modifications du serveur
- **Staging area** : Zone intermédiaire avant le commit
- **Remote** : Serveur distant (GitHub, GitLab, etc.)

## 2. Quand et pourquoi utiliser Git et GitHub

### 2.1 Pourquoi Git est indispensable

**Sans Git :**
- ❌ Perte de code en cas d'erreur
- ❌ Impossible de travailler à plusieurs
- ❌ Pas d'historique des modifications
- ❌ Difficile de tester des idées sans risque
- ❌ Versions multiples : `projet_final.js`, `projet_final_v2.js`, `projet_final_VRAIMENT_final.js`

**Avec Git :**
- ✅ Historique complet des modifications
- ✅ Collaboration fluide
- ✅ Retour arrière facile
- ✅ Branches pour expérimenter
- ✅ Sauvegarde distribuée

### 2.2 Cas d'usage quotidiens

**Développement solo :**
```bash
# Sauvegarder votre progression
git add .
git commit -m "Add user authentication"
git push

# Tester une idée sans risque
git checkout -b experiment
# ... modifications ...
git checkout main  # Revenir en arrière
```

**Travail en équipe :**
```bash
# Récupérer le travail des autres
git pull

# Créer une fonctionnalité
git checkout -b feature/new-dashboard

# Proposer vos changements
git push origin feature/new-dashboard
# Créer PR sur GitHub
```

### 2.3 Workflow Git standard

```
1. Clone repository
   ↓
2. Créer branch feature
   ↓
3. Faire modifications
   ↓
4. Commit changes
   ↓
5. Push branch
   ↓
6. Créer Pull Request
   ↓
7. Code Review
   ↓
8. Merge vers main
```

## 3. Comment cela se passe du point de vue matériel

### 3.1 Structure d'un repository Git

```
mon-projet/
├── .git/                    ← Base de données Git
│   ├── objects/             ← Stockage des commits
│   ├── refs/                ← Références (branches, tags)
│   ├── HEAD                 ← Pointe vers la branche actuelle
│   └── config               ← Configuration locale
├── src/
├── package.json
└── README.md
```

**Dossier .git :**
- Contient tout l'historique
- Compressé et optimisé
- Si supprimé = perte de l'historique !

### 3.2 Anatomie d'un commit

```
Commit: a1b2c3d4e5f6...
Author: Alice <alice@example.com>
Date: 2024-11-26 10:00:00

    Add user authentication

    - Implement login/logout
    - Add JWT tokens
    - Update tests

Changed files:
  auth.service.ts  | 45 +++++++++++++
  user.model.ts    | 12 ++++
```

**Ce qui est stocké :**
- Snapshot complet des fichiers modifiés
- Métadonnées (auteur, date, message)
- Référence au commit parent
- Hash SHA-1 unique

### 3.3 Branches en mémoire

```
       main
         ↓
    C1 ← C2 ← C3
           ↖
            C4 ← C5
                 ↑
              feature
```

**En réalité :**
- Branches = pointeurs vers commits
- Très légers (quelques bytes)
- Création instantanée
- Pas de duplication de fichiers

### 3.4 Remote et synchronisation

```
┌─────────────────────┐
│   GitHub (remote)   │
│   origin/main       │
└──────────┬──────────┘
           │
     git push/pull
           │
┌──────────▼──────────┐
│   Local Repository  │
│   .git/             │
│   main              │
└──────────┬──────────┘
           │
      checkout
           │
┌──────────▼──────────┐
│  Working Directory  │
│  src/, files...     │
└─────────────────────┘
```

## 4. Exemples et cas concrets en TypeScript

### 4.1 Installation et configuration

**Installer Git :**
```bash
# Windows
winget install Git.Git

# Mac
brew install git

# Linux
sudo apt-get install git

# Vérifier
git --version
```

**Configuration initiale :**
```bash
# Identité (obligatoire)
git config --global user.name "Alice Dupont"
git config --global user.email "alice@example.com"

# Éditeur par défaut
git config --global core.editor "code --wait"

# Couleurs
git config --global color.ui auto

# Vérifier la config
git config --list
```

### 4.2 Créer un nouveau projet

**Méthode 1 : Commencer localement**
```bash
# Créer dossier projet
mkdir mon-projet
cd mon-projet

# Initialiser Git
git init

# Créer fichiers
echo "# Mon Projet" > README.md
echo "node_modules/" > .gitignore

# Premier commit
git add .
git commit -m "Initial commit"

# Lier à GitHub (créer repo sur GitHub d'abord)
git remote add origin https://github.com/username/mon-projet.git
git branch -M main
git push -u origin main
```

**Méthode 2 : Cloner un projet existant**
```bash
# Cloner
git clone https://github.com/username/projet.git
cd projet

# Vérifier
git status
git log
```

### 4.3 Workflow quotidien

**Cycle de travail standard :**
```bash
# 1. Vérifier l'état
git status

# 2. Voir les modifications
git diff

# 3. Ajouter fichiers au staging
git add src/user.service.ts
# ou tout ajouter
git add .

# 4. Commit
git commit -m "Add user authentication service"

# 5. Push vers GitHub
git push
```

**Avec messages de commit détaillés :**
```bash
git commit -m "Add user authentication" -m "
- Implement login with JWT
- Add password hashing with bcrypt
- Create auth middleware
- Add unit tests for auth service
"
```

### 4.4 Travailler avec des branches

**Créer et utiliser une branche :**
```bash
# Créer branche + switch
git checkout -b feature/user-profile

# Ou en deux étapes
git branch feature/user-profile
git checkout feature/user-profile

# Lister branches
git branch

# Voir toutes les branches (locales + remote)
git branch -a

# Supprimer une branche locale
git branch -d feature/old-feature

# Supprimer branche remote
git push origin --delete feature/old-feature
```

**Workflow de feature branch :**
```bash
# 1. Créer branche depuis main
git checkout main
git pull
git checkout -b feature/new-dashboard

# 2. Développer
# ... modifications ...
git add .
git commit -m "Add dashboard layout"

# 3. Push branche
git push -u origin feature/new-dashboard

# 4. Sur GitHub : créer Pull Request

# 5. Après merge, nettoyer
git checkout main
git pull
git branch -d feature/new-dashboard
```

### 4.5 Résoudre les conflits

**Quand survient un conflit :**
```bash
git pull origin main
# Auto-merging src/user.ts
# CONFLICT (content): Merge conflict in src/user.ts
# Automatic merge failed; fix conflicts and then commit.
```

**Fichier avec conflit :**
```ts
// src/user.ts
export class User {
  constructor(
    public name: string,
<<<<<<< HEAD
    public email: string,
    public age: number
=======
    public username: string,
    public email: string
>>>>>>> feature/add-username
  ) {}
}
```

**Résolution :**
```ts
// Décider de la version finale
export class User {
  constructor(
    public name: string,
    public username: string,
    public email: string,
    public age: number
  ) {}
}
```

**Finaliser :**
```bash
# Marquer comme résolu
git add src/user.ts

# Commit du merge
git commit -m "Merge feature/add-username - resolve conflicts"

# Push
git push
```

### 4.6 Commandes utiles au quotidien

**Voir l'historique :**
```bash
# Log complet
git log

# Log compact
git log --oneline

# Log graphique
git log --graph --oneline --all

# Derniers 5 commits
git log -5

# Par auteur
git log --author="Alice"

# Commits d'un fichier
git log -- src/user.ts
```

**Annuler des modifications :**
```bash
# Annuler modifications non staged
git checkout -- fichier.ts

# Retirer du staging
git reset HEAD fichier.ts

# Annuler dernier commit (garder modifs)
git reset --soft HEAD~1

# Annuler dernier commit (supprimer modifs)
git reset --hard HEAD~1

# Créer commit inverse
git revert <commit-hash>
```

**Stash (mettre de côté) :**
```bash
# Sauvegarder temporairement
git stash

# Lister stash
git stash list

# Récupérer
git stash pop

# Appliquer sans supprimer
git stash apply

# Supprimer stash
git stash drop
```

### 4.7 .gitignore essentiel

```gitignore
# Node.js
node_modules/
npm-debug.log*
yarn-error.log*

# Build
dist/
build/
*.js.map

# Environment
.env
.env.local
.env.*.local

# IDE
.vscode/
.idea/
*.swp
*.swo

# OS
.DS_Store
Thumbs.db

# TypeScript
*.tsbuildinfo

# Tests
coverage/

# Logs
logs/
*.log
```

### 4.8 Pull Request sur GitHub

**Créer une PR :**
1. Push votre branche
2. GitHub → onglet "Pull requests"
3. "New pull request"
4. Sélectionner branches : `base: main` ← `compare: feature/...`
5. Titre et description
6. "Create pull request"

**Template PR :**
```markdown
## Description
Ajout d'un système d'authentification utilisateur avec JWT

## Changements
- [ ] Service d'authentification
- [ ] Middleware de vérification
- [ ] Tests unitaires
- [ ] Documentation API

## Type de changement
- [x] Nouvelle fonctionnalité
- [ ] Bug fix
- [ ] Refactoring
- [ ] Documentation

## Tests
- [x] Tests unitaires passent
- [x] Tests d'intégration passent
- [x] Testé manuellement

## Screenshots
[Si applicable]
```

## 5. Erreurs et pièges à éviter

### 5.1 Erreurs fréquentes

| Erreur | Description | Impact | Solution |
|--------|-------------|--------|----------|
| **Commit node_modules/** | Versionner dépendances | Repo gigantesque | Ajouter dans .gitignore |
| **Commit .env** | Secrets dans le code | Faille sécurité | .gitignore + .env.example |
| **Pas de .gitignore** | Fichiers inutiles versionnés | Repo pollué | Créer .gitignore dès le début |
| **Messages vagues** | "fix", "update" | Historique illisible | Messages descriptifs |
| **Commit trop gros** | 50 fichiers en un commit | Revue impossible | Commits atomiques |
| **Force push sur main** | Écrase l'historique | Perte de code | Ne JAMAIS force push main |
| **Pas de pull avant push** | Conflits systématiques | Blocages | Toujours pull avant push |

### 5.2 Pièges courants

**Piège 1 : Oublier de pull avant push**
```bash
# ❌ Erreur classique
git push
# rejected! Updates were rejected...

# ✅ Solution
git pull
# Résoudre conflits si nécessaire
git push
```

**Piège 2 : Modifier l'historique public**
```bash
# ❌ Dangereux sur branche partagée
git rebase -i HEAD~5
git push --force

# ✅ Safe : créer nouveau commit
git revert <commit-hash>
git push
```

**Piège 3 : Commit de secrets**
```bash
# ❌ Secret committé
git add .env
git commit -m "Add config"

# ⚠️ Le supprimer ne suffit pas (reste dans l'historique)

# ✅ Prévenir
echo ".env" >> .gitignore
git rm --cached .env
git commit -m "Remove .env from repo"
# + Changer les secrets compromis
```

**Piège 4 : Branches obsolètes**
```bash
# Après merge de PR, nettoyer
git checkout main
git pull
git branch -d feature/old-branch
git remote prune origin
```

### 5.3 Impacts sur les logiciels/webapps

| Problème | Conséquence |
|----------|-------------|
| **Pas de Git** | Perte de code, impossibilité de collaboration |
| **Mauvais messages** | Impossible de comprendre l'historique |
| **Commits énormes** | Revue de code impossible, bugs difficiles à tracer |
| **Secrets committés** | Failles de sécurité graves |
| **Pas de branches** | Développement chaotique, risque de casser main |
| **Force push main** | Perte de travail d'équipe |

## 6. Résumé de l'essentiel

### Points clés à retenir

1. **Git = Historique du code**
   - Chaque commit = snapshot
   - Branches = développement parallèle
   - Retour arrière toujours possible

2. **GitHub = Collaboration**
   - Hébergement cloud
   - Pull Requests pour revue
   - Issues pour gestion

3. **Workflow standard**
   ```bash
   git pull → modify → git add → git commit → git push
   ```

4. **Toujours utiliser .gitignore**
   - node_modules/
   - .env
   - dist/
   - Fichiers IDE

### Commandes essentielles quotidiennes

```bash
# Démarrer
git clone <url>
git init

# Workflow
git status           # État actuel
git add .           # Ajouter au staging
git commit -m "..."  # Commit
git push            # Envoyer
git pull            # Récupérer

# Branches
git checkout -b feature/name  # Créer branche
git checkout main            # Changer branche
git merge feature/name       # Fusionner

# Historique
git log --oneline
git diff

# Annuler
git reset HEAD fichier.ts
git checkout -- fichier.ts
```

### Messages de commit

**Format recommandé :**
```
<type>: <sujet court>

<description détaillée si nécessaire>
```

**Types courants :**
- `feat`: Nouvelle fonctionnalité
- `fix`: Correction de bug
- `docs`: Documentation
- `style`: Formatage, pas de changement de code
- `refactor`: Refactoring sans nouveau comportement
- `test`: Ajout de tests
- `chore`: Tâches (build, config)

**Exemples :**
```bash
git commit -m "feat: add user authentication with JWT"
git commit -m "fix: resolve memory leak in data service"
git commit -m "docs: update API documentation"
git commit -m "refactor: extract validation logic to separate service"
```

### Checklist projet Git

- [ ] Repository initialisé (git init)
- [ ] .gitignore créé
- [ ] Premier commit effectué
- [ ] Remote configuré (GitHub)
- [ ] Branches de travail créées
- [ ] README.md présent
- [ ] Commits réguliers et atomiques
- [ ] Messages de commit clairs
- [ ] Pull requests pour review

### Bonnes pratiques

1. **Commit souvent** : Petits commits atomiques
2. **Messages clairs** : Décrire le "pourquoi"
3. **Branches features** : Une fonctionnalité = une branche
4. **Pull avant push** : Éviter conflits
5. **.gitignore dès le début** : Éviter pollution
6. **Review code** : Pull Requests systématiques
7. **Ne pas committer secrets** : .env dans .gitignore
8. **Nettoyer branches** : Supprimer après merge

---

**En une phrase :**

> Git est l'outil indispensable de contrôle de version qui permet de suivre l'historique complet des modifications, travailler en équipe via des branches et Pull Requests sur GitHub, et garantir qu'aucun code n'est jamais perdu grâce à un système de snapshots distribués.
