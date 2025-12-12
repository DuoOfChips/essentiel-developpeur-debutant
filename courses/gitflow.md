# GitFlow : Stratégie de Gestion des Branches Git

## 1. Contexte et définitions des concepts clés

### 1.1 Qu'est-ce que GitFlow ?

**GitFlow** est un modèle de branchement Git qui définit :
- Quelles branches créer
- Quand créer des branches
- Comment fusionner les branches
- Workflow structuré pour le développement

**Créé par :** Vincent Driessen en 2010

**Objectif :** Gérer le développement, les releases et les hotfixes de manière organisée.

### 1.2 Principes fondamentaux

**5 types de branches :**
1. **main** (ou master) : Code en production
2. **develop** : Code en développement
3. **feature/** : Nouvelles fonctionnalités
4. **release/** : Préparation d'une release
5. **hotfix/** : Corrections urgentes en production

**Règles d'or :**
- main = Toujours stable et déployable
- Jamais de commit direct sur main
- Toutes les features passent par develop

### 1.3 Définitions importantes

- **main** : Branche principale, reflète la production
- **develop** : Branche de développement, intégration continue
- **feature branch** : Branche pour développer une fonctionnalité
- **release branch** : Préparation d'une version (tests finaux, versioning)
- **hotfix branch** : Correction urgente d'un bug en production
- **merge** : Fusion de branches
- **tag** : Marqueur de version (v1.0.0, v1.1.0)
- **pull request (PR)** : Demande de fusion de code

## 2. Quand et pourquoi utiliser GitFlow

### 2.1 Pourquoi GitFlow est important

**Sans stratégie de branches (chaos) :**
- ❌ Tout le monde commit sur main
- ❌ Code instable en production
- ❌ Impossible de travailler sur plusieurs features en parallèle
- ❌ Hotfix casse les features en cours
- ❌ Difficile de revenir en arrière
- ❌ Pas de traçabilité des versions

**Avec GitFlow :**
- ✅ Production toujours stable
- ✅ Développement organisé
- ✅ Releases maîtrisées
- ✅ Hotfix sans perturber le développement
- ✅ Historique clair
- ✅ Possibilité de rollback rapide

### 2.2 Cas d'usage idéaux

**GitFlow est parfait pour :**
- Applications web/mobile avec releases planifiées
- Équipes de 3+ développeurs
- Projets avec versions multiples en production
- Applications nécessitant stabilité de production
- Environnements multiples (dev, staging, prod)

**Exemples concrets :**
```
✅ Application SaaS avec releases mensuelles
✅ Application mobile sur App Store/Play Store
✅ Site e-commerce avec versions
✅ API avec versioning (v1, v2)
✅ Logiciel on-premise avec releases
```

### 2.3 Quand NE PAS utiliser GitFlow

```
❌ Déploiement continu (plusieurs fois par jour)
   → Trunk-Based Development plus adapté

❌ Très petite équipe (1-2 devs)
   → Overhead trop important

❌ Projet simple sans releases
   → GitHub Flow suffit

❌ Startup en phase très rapide
   → GitFlow trop rigide
```

## 3. Les branches GitFlow en détail

### 3.1 Branche main (production)

**Caractéristiques :**
- Contient uniquement du code en production
- Toujours stable et déployable
- Protégée en écriture directe
- Chaque commit = une version déployée
- Taggée pour chaque version (v1.0.0, v1.1.0)

**Règles strictes :**
```
❌ JAMAIS de commit direct sur main
❌ JAMAIS de code non testé
✅ Merge uniquement depuis release/ ou hotfix/
✅ Chaque merge = nouveau tag
✅ CI/CD déploie automatiquement
```

**Commandes :**
```bash
# Voir l'historique de main
git log main --oneline

# Lister les tags
git tag

# Checkout une version spécifique
git checkout v1.2.0
```

### 3.2 Branche develop (développement)

**Caractéristiques :**
- Point d'intégration pour les features
- Reflète le prochain release
- Tests automatisés en continu
- Déployée sur environnement de développement

**Workflow :**
```
feature/login ──┐
                ├──→ develop ──→ release/1.0 ──→ main
feature/cart ───┘
```

**Commandes :**
```bash
# Créer develop depuis main
git checkout main
git checkout -b develop
git push -u origin develop

# Mettre à jour develop
git checkout develop
git pull origin develop
```

### 3.3 Branches feature/ (fonctionnalités)

**Nomenclature :**
```
feature/nom-de-la-fonctionnalite
feature/user-authentication
feature/shopping-cart
feature/payment-integration
```

**Cycle de vie :**
```
1. Créée depuis develop
2. Développement de la feature
3. Merge vers develop via Pull Request
4. Suppression de la branche
```

**Workflow complet :**
```bash
# 1. Créer la branche feature
git checkout develop
git pull origin develop
git checkout -b feature/user-login

# 2. Développer la feature
git add .
git commit -m "feat: add login form"
git commit -m "feat: add JWT authentication"
git commit -m "test: add login tests"

# 3. Push la branche
git push -u origin feature/user-login

# 4. Créer Pull Request sur GitHub/GitLab
# (via l'interface web)

# 5. Après approbation et merge, nettoyer
git checkout develop
git pull origin develop
git branch -d feature/user-login
git push origin --delete feature/user-login
```

**Bonnes pratiques :**
```
✅ Nom descriptif et clair
✅ Courte durée de vie (< 1-2 semaines)
✅ Commits atomiques et clairs
✅ Tests avant merge
✅ Code review obligatoire (PR)
✅ Rebase régulier depuis develop
```

### 3.4 Branches release/ (préparation release)

**Nomenclature :**
```
release/1.0.0
release/1.1.0
release/2.0.0
```

**Objectif :**
- Geler les features
- Tests finaux
- Corrections de bugs mineurs
- Mise à jour versioning
- Préparation documentation

**Cycle de vie :**
```
1. Créée depuis develop
2. Tests et corrections mineures uniquement
3. Merge vers main (production)
4. Merge vers develop (pour garder les fixes)
5. Tag de la version
6. Suppression de la branche
```

**Workflow complet :**
```bash
# 1. Créer release depuis develop
git checkout develop
git pull origin develop
git checkout -b release/1.0.0

# 2. Mise à jour de version
# Modifier package.json, version.txt, etc.
git commit -m "chore: bump version to 1.0.0"

# 3. Tests finaux et corrections de bugs
git commit -m "fix: typo in welcome message"
git commit -m "fix: button alignment"

# 4. Merge vers main
git checkout main
git merge --no-ff release/1.0.0
git tag -a v1.0.0 -m "Release version 1.0.0"
git push origin main --tags

# 5. Merge vers develop (pour garder les fixes)
git checkout develop
git merge --no-ff release/1.0.0
git push origin develop

# 6. Supprimer la branche release
git branch -d release/1.0.0
git push origin --delete release/1.0.0
```

**⚠️ Règles strictes release :**
```
❌ Pas de nouvelles features
❌ Pas de gros refactoring
✅ Seulement bug fixes mineurs
✅ Ajustements de version
✅ Mise à jour documentation
```

### 3.5 Branches hotfix/ (corrections urgentes)

**Nomenclature :**
```
hotfix/fix-payment-bug
hotfix/security-patch
hotfix/1.0.1
```

**Objectif :**
Corriger un bug critique en production sans attendre le prochain release.

**Cycle de vie :**
```
1. Créée depuis main (production actuelle)
2. Correction du bug
3. Tests de la correction
4. Merge vers main (déploiement immédiat)
5. Merge vers develop (pour garder le fix)
6. Tag de version patch (v1.0.1)
7. Suppression de la branche
```

**Workflow complet :**
```bash
# 1. Créer hotfix depuis main
git checkout main
git pull origin main
git checkout -b hotfix/fix-payment-bug

# 2. Corriger le bug
git commit -m "hotfix: fix payment processing error"
git commit -m "test: add test for payment fix"

# 3. Merge vers main
git checkout main
git merge --no-ff hotfix/fix-payment-bug
git tag -a v1.0.1 -m "Hotfix: Fix payment bug"
git push origin main --tags

# 4. Merge vers develop
git checkout develop
git merge --no-ff hotfix/fix-payment-bug
git push origin develop

# 5. Si release en cours, merger aussi dedans
git checkout release/1.1.0
git merge --no-ff hotfix/fix-payment-bug

# 6. Supprimer la branche hotfix
git branch -d hotfix/fix-payment-bug
git push origin --delete hotfix/fix-payment-bug
```

**Quand créer un hotfix :**
```
✅ Bug critique affectant la production
✅ Faille de sécurité
✅ Perte de données
✅ Feature majeure cassée
✅ Performance dégradée sévèrement

❌ Bug mineur → Attendre prochain release
❌ Amélioration → Feature branch
❌ Refactoring → Feature branch
```

## 4. Workflow GitFlow complet : Exemple

### 4.1 Scénario : Application E-commerce

**État initial :**
```
main: v1.0.0 (en production)
develop: v1.1.0-dev (prochaine version)
```

**Sprint 1 : Développement de 2 features**

```bash
# Feature 1: Wishlist
git checkout develop
git checkout -b feature/wishlist
# ... développement ...
git push origin feature/wishlist
# → PR vers develop → Merge → Delete branch

# Feature 2: Filtres produits
git checkout develop
git checkout -b feature/product-filters
# ... développement ...
git push origin feature/product-filters
# → PR vers develop → Merge → Delete branch
```

**État après Sprint 1 :**
```
develop: contient wishlist + filtres
main: toujours v1.0.0
```

**Préparation Release v1.1.0 :**

```bash
# Créer release branch
git checkout develop
git checkout -b release/1.1.0

# Bump version
# package.json: "version": "1.1.0"
git commit -m "chore: bump version to 1.1.0"

# Tests finaux
# ... découverte de 2 bugs mineurs ...
git commit -m "fix: wishlist icon not showing"
git commit -m "fix: filter clear button"

# Merge vers main (production)
git checkout main
git merge --no-ff release/1.1.0
git tag -a v1.1.0 -m "Release 1.1.0: Wishlist and Filters"
git push origin main --tags

# Merge vers develop (garder les fixes)
git checkout develop
git merge --no-ff release/1.1.0
git push origin develop

# Supprimer release branch
git branch -d release/1.1.0
```

**État après Release :**
```
main: v1.1.0 (déployé en production)
develop: v1.2.0-dev (commence développement suivant)
```

**Bug critique découvert en production :**

```bash
# Bug: Paiement ne fonctionne pas !

# Créer hotfix depuis main
git checkout main
git checkout -b hotfix/fix-payment-gateway

# Corriger le bug
git commit -m "hotfix: fix Stripe API integration"

# Tester la correction

# Merge vers main
git checkout main
git merge --no-ff hotfix/fix-payment-gateway
git tag -a v1.1.1 -m "Hotfix: Payment gateway"
git push origin main --tags
# → Déploiement automatique en production

# Merge vers develop
git checkout develop
git merge --no-ff hotfix/fix-payment-gateway
git push origin develop

# Supprimer hotfix branch
git branch -d hotfix/fix-payment-gateway
```

**État final :**
```
main: v1.1.1 (production stable)
develop: v1.2.0-dev (avec le fix du hotfix)
```

### 4.2 Diagramme complet

```
main      ──●────────────────────●─────────●──→
           v1.0                  v1.1     v1.1.1
            │                     ↑         ↑
            │                     │         │
            ↓                     │         │
develop   ──●──────●──────●──────●─────────●──→
            │      ↑      ↑      │         │
            │      │      │      │         │
feature/  ──●──────●      │      │         │
wishlist              (merge)    │         │
                                 │         │
feature/  ────────●──────────────●         │
filters            ↑         (merge)       │
                   │                       │
release/  ────────────────●────────●       │
1.1.0                  (tests) (merge)     │
                                           │
hotfix/   ───────────────────────────────●─●
fix-payment                             (merge)
```

## 5. Configuration et outils

### 5.1 Configuration Git pour GitFlow

**Protection des branches (GitHub/GitLab) :**
```
main:
  ✅ Require pull request reviews (1-2)
  ✅ Require status checks to pass (CI/CD)
  ✅ Require branches to be up to date
  ✅ Restrict who can push to matching branches
  ✅ Require linear history

develop:
  ✅ Require pull request reviews (1)
  ✅ Require status checks to pass
  ✅ Require branches to be up to date
```

**git-flow extension (optionnel) :**
```bash
# Installation
# Mac
brew install git-flow

# Ubuntu
sudo apt-get install git-flow

# Initialisation
git flow init

# Utilisation
git flow feature start user-login
git flow feature finish user-login

git flow release start 1.0.0
git flow release finish 1.0.0

git flow hotfix start fix-bug
git flow hotfix finish fix-bug
```

### 5.2 Template Pull Request

```markdown
## Description
Brief description of the changes

## Type of change
- [ ] Feature (nouvelle fonctionnalité)
- [ ] Bug fix (correction de bug)
- [ ] Hotfix (correction urgente)
- [ ] Documentation
- [ ] Refactoring

## Checklist
- [ ] Code suit les conventions du projet
- [ ] Tests ajoutés/mis à jour
- [ ] Documentation mise à jour
- [ ] Pas de conflits avec develop
- [ ] CI/CD passe
- [ ] Code review effectuée

## Testing
Comment tester les changements

## Screenshots (si applicable)

## Related Issues
Closes #123
```

### 5.3 CI/CD avec GitFlow

**GitHub Actions example :**
```yaml
name: CI/CD GitFlow

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main, develop ]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Run tests
        run: npm test
      
  deploy-dev:
    if: github.ref == 'refs/heads/develop'
    needs: test
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to Dev
        run: ./deploy-dev.sh
      
  deploy-prod:
    if: github.ref == 'refs/heads/main'
    needs: test
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to Production
        run: ./deploy-prod.sh
```

## 6. Erreurs et pièges à éviter

### 6.1 Erreurs fréquentes

| Erreur | Description | Impact | Solution |
|--------|-------------|--------|----------|
| **Commit direct sur main** | Bypass du workflow | Code non testé en prod | Protection de branche |
| **Feature trop longue** | Feature branch pendant 2 mois | Conflits énormes | Max 1-2 semaines |
| **Oublier merge vers develop** | Après hotfix | Fix perdu dans prochain release | Checklist hotfix |
| **Release avec nouvelles features** | Ajout de code pendant release | Instabilité | Seulement bug fixes |
| **Pas de tag** | Versions non marquées | Impossible de rollback | Tag à chaque release |
| **Pas de suppression branches** | 100 branches feature obsolètes | Confusion | Supprimer après merge |
| **Merge sans review** | Code non relu | Bugs en production | PR obligatoires |

### 6.2 Pièges courants

**Piège 1 : GitFlow pour tout**
```
❌ Utiliser GitFlow pour:
   - Petit projet solo
   - Déploiement continu (10×/jour)
   - Prototype rapide

✅ GitFlow pour:
   - Équipe 3+ devs
   - Releases planifiées
   - Production stable critique
```

**Piège 2 : Features gigantesques**
```
❌ Feature branch avec 50 commits pendant 6 semaines
   → Conflits de merge catastrophiques
   → Review impossible

✅ Feature courte:
   - Max 1-2 semaines
   - 5-15 commits
   - Review facile
   - Si trop gros: découper en sous-features
```

**Piège 3 : Hotfix qui devient feature**
```
❌ "Tant qu'on y est, ajoutons aussi..."
   → Hotfix de 500 lignes
   → Bugs supplémentaires

✅ Hotfix minimal:
   - Correction stricte du bug
   - Tests de la correction
   - Rien d'autre
```

**Piège 4 : Pas de synchronisation develop ↔ main**
```
❌ develop diverge trop de main
   → Merge release impossible

✅ Synchronisation régulière:
   - Hotfix immédiatement mergés dans develop
   - Release branche régulièrement rebased
```

### 6.3 Conseils avancés

**1. Rebase vs Merge :**
```
Feature branch:
✅ Rebase régulier sur develop (historique propre)
git checkout feature/login
git rebase develop

Release/Hotfix → main/develop:
✅ Merge avec --no-ff (préserve historique)
git merge --no-ff release/1.0.0
```

**2. Nommage cohérent :**
```
✅ Bon:
feature/user-authentication
feature/payment-stripe-integration
release/1.2.0
hotfix/fix-memory-leak

❌ Mauvais:
my-branch
test
feature/stuff
fix
```

**3. Commits conventionnels :**
```
feat: add user login
fix: correct payment calculation
chore: bump version to 1.1.0
docs: update API documentation
test: add payment tests
refactor: simplify auth logic
```

## 7. Résumé de l'essentiel

### Points clés à retenir

1. **5 types de branches**
   - main : Production stable
   - develop : Intégration développement
   - feature/ : Nouvelles fonctionnalités
   - release/ : Préparation version
   - hotfix/ : Corrections urgentes

2. **Workflow de base**
   ```
   feature → develop → release → main
                    ↑
                 hotfix
   ```

3. **Règles strictes**
   - Jamais de commit direct sur main
   - Protection de main et develop
   - Pull Request obligatoires
   - Tests avant merge
   - Tag à chaque version

4. **Commandes essentielles**
   ```bash
   git checkout -b feature/nom
   git merge --no-ff branche
   git tag -a v1.0.0 -m "Version 1.0.0"
   ```

### Checklist GitFlow

**Setup initial :**
- [ ] Créer branche develop depuis main
- [ ] Protéger main (no direct push)
- [ ] Protéger develop (require PR)
- [ ] Configurer CI/CD
- [ ] Définir convention nommage branches
- [ ] Template PR créé

**Pour chaque feature :**
- [ ] Créer depuis develop à jour
- [ ] Nom descriptif (feature/xxx)
- [ ] Commits réguliers et clairs
- [ ] Rebase sur develop si nécessaire
- [ ] Tests passent
- [ ] PR avec code review
- [ ] Merge vers develop
- [ ] Supprimer la branche

**Pour chaque release :**
- [ ] Créer depuis develop
- [ ] Bump version
- [ ] Tests finaux uniquement
- [ ] Corrections bugs mineurs seulement
- [ ] Merge vers main avec tag
- [ ] Merge vers develop
- [ ] Supprimer la branche

**Pour chaque hotfix :**
- [ ] Créer depuis main
- [ ] Correction minimale et ciblée
- [ ] Tests de la correction
- [ ] Merge vers main avec tag patch
- [ ] Merge vers develop
- [ ] Merge vers release si existe
- [ ] Supprimer la branche

### Commandes de référence rapide

```bash
# Feature
git checkout develop && git pull
git checkout -b feature/nom
# ... dev ...
git push -u origin feature/nom
# PR → merge → delete

# Release
git checkout develop && git pull
git checkout -b release/1.0.0
# ... bump version & fixes ...
git checkout main && git merge --no-ff release/1.0.0
git tag -a v1.0.0 -m "Release 1.0.0"
git push origin main --tags
git checkout develop && git merge --no-ff release/1.0.0
git branch -d release/1.0.0

# Hotfix
git checkout main && git pull
git checkout -b hotfix/fix-bug
# ... fix ...
git checkout main && git merge --no-ff hotfix/fix-bug
git tag -a v1.0.1 -m "Hotfix"
git push origin main --tags
git checkout develop && git merge --no-ff hotfix/fix-bug
git branch -d hotfix/fix-bug
```

### Versioning sémantique

```
v MAJOR . MINOR . PATCH
  1  .   2   .   3

MAJOR: Changements incompatibles (breaking changes)
MINOR: Nouvelles fonctionnalités (compatibles)
PATCH: Corrections de bugs

Exemples:
v1.0.0 → v1.0.1 (hotfix)
v1.0.1 → v1.1.0 (nouvelle feature)
v1.1.0 → v2.0.0 (breaking change)
```

---

**En une phrase :**

> GitFlow est une stratégie de gestion de branches Git qui organise le développement avec 5 types de branches (main pour production, develop pour intégration, feature/ pour fonctionnalités, release/ pour préparation version, hotfix/ pour corrections urgentes), garantissant une production stable, des releases maîtrisées et une capacité à corriger rapidement les bugs critiques sans perturber le développement en cours.

**Pour être employable :**

**Tu DOIS savoir :**
- ✅ Expliquer les 5 types de branches GitFlow
- ✅ Créer et gérer une feature branch
- ✅ Faire un Pull Request vers develop
- ✅ Comprendre le workflow release
- ✅ Savoir quand créer un hotfix
- ✅ Utiliser git merge avec --no-ff
- ✅ Créer des tags de version

**Vocabulaire à maîtriser absolument :**
GitFlow, main, develop, feature branch, release branch, hotfix, merge, tag, Pull Request, versioning sémantique, protection de branche, CI/CD, --no-ff.
