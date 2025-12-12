# Méthode Kanban : Flux Continu et Visualisation du Travail

## 1. Contexte et définitions des concepts clés

### 1.1 Qu'est-ce que Kanban ?

**Kanban** est une méthode de gestion visuelle du travail qui permet de :
- Visualiser le flux de travail
- Limiter le travail en cours (WIP)
- Gérer le flux plutôt que les itérations
- Améliorer continuellement le processus
- Livrer en continu

**Origine :** Inventé par Toyota dans les années 1940 pour la production automobile, adapté au développement logiciel dans les années 2000.

### 1.2 Principes fondamentaux

**Les 4 principes de base :**
1. **Visualiser le travail** : Rendre visible ce qui est invisible
2. **Limiter le WIP** : Ne pas tout faire en même temps
3. **Gérer le flux** : Optimiser la fluidité du travail
4. **Améliorer continuellement** : Kaizen permanent

### 1.3 Définitions importantes

- **Kanban Board** : Tableau visuel avec colonnes représentant les étapes
- **Card (Carte)** : Représentation d'une tâche ou user story
- **WIP Limit** : Nombre maximum de cartes dans une colonne
- **Lead Time** : Temps total de la demande à la livraison
- **Cycle Time** : Temps de développement effectif (de "En cours" à "Terminé")
- **Throughput** : Nombre de cartes complétées par période
- **Bottleneck** : Goulot d'étranglement qui ralentit le flux
- **Swimlane** : Ligne horizontale pour catégoriser les cartes
- **Cumulative Flow Diagram (CFD)** : Graphique montrant le flux cumulé
- **Blockers** : Obstacles qui empêchent l'avancement

## 2. Quand et pourquoi utiliser Kanban

### 2.1 Pourquoi Kanban est puissant

**Sans Kanban (travail non visualisé) :**
- ❌ On ne sait pas qui fait quoi
- ❌ Travail en cours partout, rien de terminé
- ❌ Goulots d'étranglement invisibles
- ❌ Impossible de prédire les délais
- ❌ Stress et surcharge cognitive

**Avec Kanban :**
- ✅ Travail 100% visible
- ✅ Focus sur la finition (moins de multitasking)
- ✅ Identification rapide des blocages
- ✅ Prédictibilité des délais
- ✅ Flux continu et régulier
- ✅ Amélioration continue basée sur les données

### 2.2 Cas d'usage idéaux

**Kanban est parfait pour :**
- Support technique et maintenance
- Projets avec flux continu de demandes
- Équipes DevOps et SRE
- Corrections de bugs
- Équipes qui ne peuvent pas planifier par sprint
- Travail avec beaucoup d'urgences

**Exemples concrets :**
```
✅ Équipe support avec tickets clients
✅ Maintenance d'applications existantes
✅ Équipe DevOps avec demandes diverses
✅ Corrections de bugs de production
✅ Équipe qui reçoit des demandes imprévisibles
✅ Amélioration continue d'un produit mature
```

### 2.3 Kanban vs Scrum

| Aspect | Kanban | Scrum |
|--------|--------|-------|
| **Itérations** | Flux continu | Sprints fixes |
| **Rôles** | Pas de rôles obligatoires | PO, SM, Dev Team |
| **Changements** | Ajout à tout moment | Backlog figé pendant sprint |
| **Estimation** | Optionnelle | Story points obligatoires |
| **Cérémonies** | Standup optionnel | 4 cérémonies obligatoires |
| **Meilleur pour** | Flux imprévisible | Développement produit |

**⚠️ Attention :**
- Kanban n'est PAS plus simple que Scrum
- Kanban exige une discipline de l'équipe
- On peut combiner les deux (Scrumban)

## 3. Le tableau Kanban : Colonne et cartes

### 3.1 Structure de base

**Colonnes minimales :**
```
┌─────────┬────────────┬──────┐
│  TO DO  │IN PROGRESS │ DONE │
└─────────┴────────────┴──────┘
```

**Colonnes typiques pour le développement :**
```
┌─────────┬─────────┬──────────┬────────┬────────┬──────┐
│BACKLOG  │SELECTED │   DEV    │ REVIEW │  TEST  │ DONE │
└─────────┴─────────┴──────────┴────────┴────────┴──────┘
```

**Colonnes détaillées (avancé) :**
```
┌─────────┬──────────┬────────┬──────────┬────────┬────────┬──────────┬──────┐
│BACKLOG  │ READY    │  DEV   │CODE      │  TEST  │STAGING │DEPLOYED  │DONE  │
│         │          │        │REVIEW    │        │        │          │      │
└─────────┴──────────┴────────┴──────────┴────────┴────────┴──────────┴──────┘
```

### 3.2 Anatomie d'une carte Kanban

**Informations essentielles sur une carte :**
```
┌─────────────────────────────────┐
│ 🔴 URGENTE          #TASK-1234  │
│                                 │
│ Corriger bug de connexion       │
│                                 │
│ 👤 Alice                        │
│ ⏱️  2 jours estimés             │
│ 🏷️  Bug, Critique              │
└─────────────────────────────────┘
```

**Composants d'une carte :**
- **ID unique** : Pour référencer (#TASK-1234)
- **Titre clair** : Description courte
- **Assignée à** : Qui travaille dessus
- **Labels/Tags** : Type (Bug, Feature, etc.)
- **Estimation** : Temps ou complexité (optionnel)
- **Priorité** : Urgence (couleur ou étiquette)
- **Blockers** : Indicateur si bloqué

### 3.3 WIP Limits (Limites de travail en cours)

**Concept clé :** Limiter le nombre de cartes dans chaque colonne

**Exemple avec WIP limits :**
```
┌─────────┬────────────┬────────────┬──────┐
│  TO DO  │IN PROGRESS │   REVIEW   │ DONE │
│    ∞    │   WIP: 3   │   WIP: 2   │  ∞   │
├─────────┼────────────┼────────────┼──────┤
│ Card A  │  Card B    │  Card E    │Card F│
│ Card B  │  Card C    │  Card G    │Card H│
│ Card I  │  Card D    │            │      │
│ Card J  │            │            │      │
└─────────┴────────────┴────────────┴──────┘
         ↑ 3/3 (limite atteinte!)
```

**Pourquoi limiter le WIP ?**
```
❌ Sans WIP limit:
- 10 tâches commencées, 0 terminée
- Multitasking permanent
- Rien n'avance vraiment
- Stress élevé

✅ Avec WIP limit:
- 3 tâches en cours maximum
- Focus sur la finition
- Livraison régulière
- Flux fluide
```

**Règle d'or :**
> Si une colonne est pleine, on ne tire pas de nouvelle carte. On aide plutôt à débloquer le travail en cours.

**Comment calculer le WIP limit ?**
```
WIP limit par colonne = Nombre de personnes × 1.5

Exemple:
- Équipe de 4 personnes
- WIP limit = 4 × 1.5 = 6 cartes maximum en "In Progress"
```

### 3.4 Swimlanes (Couloirs)

**Définition :** Lignes horizontales pour catégoriser les tâches

**Exemple par priorité :**
```
┌──────────────┬─────────┬────────────┬──────┐
│              │ TO DO   │IN PROGRESS │ DONE │
├──────────────┼─────────┼────────────┼──────┤
│ 🔴 URGENT    │ Card A  │  Card B    │Card C│
├──────────────┼─────────┼────────────┼──────┤
│ 🟠 HAUTE     │ Card D  │  Card E    │Card F│
│              │ Card G  │            │Card H│
├──────────────┼─────────┼────────────┼──────┤
│ 🟢 NORMALE   │ Card I  │            │Card J│
│              │ Card K  │            │      │
└──────────────┴─────────┴────────────┴──────┘
```

**Exemple par type :**
```
┌──────────────┬─────────┬────────────┬──────┐
│              │ BACKLOG │IN PROGRESS │ DONE │
├──────────────┼─────────┼────────────┼──────┤
│ 🐛 BUG       │         │  Card B    │Card C│
├──────────────┼─────────┼────────────┼──────┤
│ ✨ FEATURE   │ Card D  │  Card E    │Card F│
├──────────────┼─────────┼────────────┼──────┤
│ 🔧 TECH      │ Card G  │            │Card H│
└──────────────┴─────────┴────────────┴──────┘
```

## 4. Métriques Kanban : Lead Time et Cycle Time

### 4.1 Lead Time vs Cycle Time

**Lead Time :**
Temps total de la demande du client à la livraison

```
┌────────────────────────────────────────────┐
│           LEAD TIME (10 jours)             │
├────────────┬──────────────────┬────────────┤
│  Demande   │   Développement  │ Livraison  │
│  (2 jours) │    (6 jours)     │ (2 jours)  │
└────────────┴──────────────────┴────────────┘
```

**Cycle Time :**
Temps de travail effectif (de "En cours" à "Terminé")

```
              ┌──────────────────┐
              │  CYCLE TIME      │
              │   (6 jours)      │
┌────────────┬──────────────────┬────────────┐
│  Backlog   │   En cours       │   Done     │
│            │  (travail actif) │            │
└────────────┴──────────────────┴────────────┘
```

**Formules :**
```
Lead Time = Temps depuis la création de la demande
Cycle Time = Temps depuis le début du travail
```

### 4.2 Comment utiliser ces métriques

**Réduire le Lead Time :**
- Limiter le backlog
- Prioriser efficacement
- Réduire les attentes

**Réduire le Cycle Time :**
- Limiter le WIP
- Supprimer les blocages
- Améliorer le processus

**Exemple concret :**
```
Ticket #1234 - Bug de connexion

1. Créé: Lundi 8h → Backlog
2. Sélectionné: Lundi 14h
3. Développement: Mardi 9h ← Début Cycle Time
4. Code Review: Mercredi 10h
5. Test: Mercredi 14h
6. Déployé: Jeudi 9h ← Fin Cycle Time
7. Fermé: Jeudi 9h ← Fin Lead Time

Lead Time = 3 jours (Lundi 8h → Jeudi 9h)
Cycle Time = 2 jours (Mardi 9h → Jeudi 9h)
```

### 4.3 Throughput (Débit)

**Définition :** Nombre de cartes complétées par période

**Exemple :**
```
Semaine 1: 12 cartes terminées
Semaine 2: 15 cartes terminées
Semaine 3: 13 cartes terminées
Semaine 4: 14 cartes terminées

Throughput moyen = (12 + 15 + 13 + 14) / 4 = 13.5 cartes/semaine
```

**Utilisation :**
- Prédire les délais
- Planifier les releases
- Identifier les variations

**Prédiction :**
```
Backlog: 50 cartes
Throughput: 13.5 cartes/semaine

Estimation: 50 / 13.5 = 3.7 semaines (~1 mois)
```

### 4.4 Cumulative Flow Diagram (CFD)

**Définition :** Graphique montrant l'évolution du travail dans chaque colonne

```
Cartes
  │
80│              ┌─────────── Done
  │            ┌─┘
60│          ┌─┘  ┌────────── Testing
  │        ┌─┘  ┌─┘
40│      ┌─┘  ┌─┘  ┌──────── In Progress
  │    ┌─┘  ┌─┘  ┌─┘
20│  ┌─┘  ┌─┘  ┌─┘  ┌─────── To Do
  │┌─┘  ┌─┘  ┌─┘  ┌─┘
 0└───────────────────────→ Temps
  Sem1 Sem2 Sem3 Sem4 Sem5
```

**Comment lire le CFD :**
- **Hauteur d'une bande** = Nombre de cartes dans cette colonne
- **Bande stable** = Flux régulier
- **Bande qui gonfle** = Goulot d'étranglement
- **Bande qui rétrécit** = Colonne qui se vide

**Problèmes visibles :**
```
❌ Bande "In Progress" qui gonfle → Trop de WIP
❌ Bande "Testing" qui augmente → Goulot en test
✅ Bandes parallèles → Flux stable et sain
```

## 5. Pratiques Kanban au quotidien

### 5.1 Daily Standup (optionnel mais recommandé)

**Format Kanban Standup :**
On parcourt le tableau de droite à gauche (priorité à finir)

```
1. "Que peut-on finir aujourd'hui ?"
   → Focus sur DONE

2. "Qu'est-ce qui est bloqué ?"
   → Identifier les obstacles

3. "Peut-on tirer de nouvelles cartes ?"
   → Vérifier WIP limits
```

**Exemple :**
```
Scrum Master:
"Ok, regardons le tableau.

DONE: Rien depuis hier, on va voir pourquoi.

TESTING (WIP 2/3):
- Card E bloquée, besoin d'un environnement de test
  → Bob va créer l'environnement

IN PROGRESS (WIP 3/4):
- Card B sera terminée ce matin
- Card C et D avancent bien

TO DO:
- On pourra tirer Card F une fois Card B terminée"
```

### 5.2 Pull system (Système tiré)

**Principe :** On ne pousse pas le travail, on le tire

**❌ Push (pousser) :**
```
Manager: "Alice, je t'assigne ces 5 nouvelles tâches"
→ Alice surchargée
→ Rien ne finit
```

**✅ Pull (tirer) :**
```
Alice: "J'ai fini ma tâche, je tire la prochaine carte"
→ Flux naturel
→ WIP limit respecté
```

**Règles du Pull :**
1. Finir avant de commencer du nouveau
2. Respecter les WIP limits
3. Tirer de la colonne précédente
4. Priorité à débloquer plutôt que commencer du nouveau

### 5.3 Classes de Service

**Définition :** Catégories de travail avec SLA différents

**4 classes typiques :**

**1. Expedite (Urgence) :**
- Contourne toutes les files
- WIP limit = 1
- Exemples : Bug critique en production, sécurité

**2. Fixed Date (Date fixe) :**
- Deadline non négociable
- Exemples : Conformité légale, événement

**3. Standard :**
- Travail normal
- Exemples : Nouvelles features, améliorations

**4. Intangible :**
- Pas visible par le client
- Exemples : Dette technique, refactoring

**Visualisation :**
```
┌──────────────┬─────────┬────────────┬──────┐
│ Type         │ BACKLOG │IN PROGRESS │ DONE │
├──────────────┼─────────┼────────────┼──────┤
│🚨 EXPEDITE   │         │  Card A    │      │ ← Max 1 carte
│   (WIP: 1)   │         │            │      │
├──────────────┼─────────┼────────────┼──────┤
│📅 FIXED DATE │ Card B  │  Card C    │Card D│
├──────────────┼─────────┼────────────┼──────┤
│⭐ STANDARD   │ Card E  │  Card F    │Card G│
│              │ Card H  │  Card I    │      │
├──────────────┼─────────┼────────────┼──────┤
│🔧 INTANGIBLE │ Card J  │            │Card K│
└──────────────┴─────────┴────────────┴──────┘
```

### 5.4 Gestion des blockers

**Visualisation d'un blocker :**
```
┌─────────────────────────────────┐
│ 🚫 BLOQUÉ           #TASK-1234  │
│                                 │
│ Intégrer API paiement           │
│                                 │
│ 👤 Alice                        │
│ ⚠️  En attente: accès API       │
│ 🏷️  Feature, Bloqué            │
└─────────────────────────────────┘
```

**Process de déblocage :**
1. **Identifier** : Marquer la carte comme bloquée
2. **Escalader** : Informer immédiatement l'équipe
3. **Résoudre** : Priorité #1 pour débloquer
4. **Documenter** : Noter la cause pour éviter répétition

**Causes fréquentes de blocage :**
- Attente d'information externe
- Dépendance d'une autre équipe
- Environnement technique défaillant
- Besoin de validation
- Ressource manquante

## 6. Outils et exemples concrets

### 6.1 Outils Kanban populaires

**Outils numériques :**
- **Trello** : Simple, visuel, gratuit
- **Jira** : Puissant, métriques avancées
- **Azure Boards** : Intégré avec Microsoft
- **GitHub Projects** : Intégré avec GitHub
- **Notion** : Flexible et moderne

**Outils physiques :**
- Tableau blanc + post-its colorés
- Magnets sur tableau métallique
- Cartes sur mur

**⚠️ Conseil :**
- Débuter avec un tableau physique pour bien comprendre
- Passer au numérique ensuite pour les métriques

### 6.2 Template Trello/Jira

**Colonnes :**
```
1. 📋 Backlog
2. 🎯 Ready (WIP: 5)
3. 💻 In Progress (WIP: 4)
4. 👀 Code Review (WIP: 3)
5. 🧪 Testing (WIP: 2)
6. ✅ Done
```

**Labels :**
- 🐛 Bug
- ✨ Feature
- 🔧 Tech Debt
- 📚 Documentation
- 🔴 Urgent
- 🟠 High Priority
- 🟢 Normal

### 6.3 Exemple de flux complet

**Ticket : Ajouter paiement par carte**

```
Jour 1 (Lundi):
📋 Backlog
  └─ #PAY-123: Ajouter paiement carte
     Créé par: Product Owner
     Priorité: Haute

Jour 2 (Mardi):
🎯 Ready
  └─ #PAY-123
     Détails ajoutés, spécifications claires

Jour 3 (Mercredi):
💻 In Progress
  └─ #PAY-123
     Assigné à: Alice
     Travail commencé

Jour 5 (Vendredi):
👀 Code Review
  └─ #PAY-123
     PR créée: #456
     Reviewer: Bob

Jour 6 (Lundi):
👀 Code Review
  └─ #PAY-123
     ⚠️ BLOQUÉ: Commentaires de review à adresser

Jour 7 (Mardi):
👀 Code Review
  └─ #PAY-123
     Review approuvée

Jour 7 (Mardi après-midi):
🧪 Testing
  └─ #PAY-123
     Tests en cours

Jour 8 (Mercredi):
✅ Done
  └─ #PAY-123
     Déployé en production

Lead Time: 8 jours
Cycle Time: 6 jours (Mercredi → Mercredi)
```

### 6.4 Réunion de replenishment

**Fréquence :** Hebdomadaire ou quand backlog faible

**Objectif :** Remplir et prioriser le backlog

**Déroulement :**
1. Revoir les nouvelles demandes
2. Prioriser selon valeur et urgence
3. Ajouter détails nécessaires
4. Estimer si nécessaire
5. Mettre dans "Ready"

**Participants :**
- Product Owner
- Tech Lead
- Stakeholders clés

## 7. Erreurs et pièges à éviter

### 7.1 Erreurs fréquentes

| Erreur | Description | Impact | Solution |
|--------|-------------|--------|----------|
| **Pas de WIP limit** | Tout le monde fait tout | Rien ne se termine | Définir WIP limits stricts |
| **WIP trop élevé** | Limite à 20 pour 3 personnes | Équivaut à pas de limite | WIP = Nb personnes × 1.5 |
| **Ignorer les blockers** | Carte bloquée pendant des jours | Flux cassé | Process d'escalade immédiat |
| **Colonnes trop larges** | "En cours" = 5 étapes | Masque les problèmes | Découper en colonnes précises |
| **Pas de métriques** | Aucun suivi de performance | Pas d'amélioration | Lead Time, Cycle Time, CFD |
| **Tableau non à jour** | Cartes déplacées une fois par semaine | Perte de visibilité | Mise à jour en temps réel |
| **Trop de colonnes** | 15 colonnes différentes | Complexité inutile | Maximum 7-8 colonnes |

### 7.2 Pièges courants

**Piège 1 : Kanban = Simple tableau**
```
❌ "On a mis un tableau, on fait du Kanban"
❌ Pas de WIP limits
❌ Pas de métriques
❌ Pas d'amélioration continue

✅ Kanban complet:
✅ WIP limits définis et respectés
✅ Métriques suivies
✅ Amélioration continue du processus
✅ Classes de service
```

**Piège 2 : WIP limit non respecté**
```
❌ "C'est urgent, on dépasse le WIP juste cette fois"
   → Répété tous les jours
   → WIP limit devient inutile

✅ Discipline stricte:
✅ WIP limit = règle inviolable
✅ Si urgent → Expedite lane avec WIP=1
✅ Débloquer avant de commencer nouveau
```

**Piège 3 : Pas de prioritisation**
```
❌ Tout est dans le backlog sans ordre
❌ Chacun prend ce qu'il veut
❌ Tâches importantes jamais faites

✅ Priorisation claire:
✅ Backlog ordonné de haut en bas
✅ On tire toujours la carte la plus haute
✅ Product Owner priorise régulièrement
```

**Piège 4 : Oublier l'amélioration continue**
```
❌ Tableau mis en place et jamais changé
❌ Process figé
❌ Problèmes qui persistent

✅ Kaizen permanent:
✅ Rétrospectives régulières
✅ Ajustement des WIP limits
✅ Ajout/suppression de colonnes selon besoins
✅ Expérimentation de nouvelles pratiques
```

### 7.3 Antipatterns Kanban

**1. Mini-Waterfall déguisé**
```
❌ Colonnes: Analysis → Design → Dev → Test → Deploy
❌ Travail passe séquentiellement
❌ Équipes spécialisées par colonne

✅ Solution: Équipe cross-fonctionnelle
✅ Une carte traverse rapidement
```

**2. Tableau Kanban seulement**
```
❌ On a un joli tableau coloré
❌ Mais pas de discipline du flux
❌ Pas de mesures, pas d'amélioration

✅ Solution: Kanban = méthode complète
✅ Pas juste un outil visuel
```

**3. Backlog infini**
```
❌ 500 cartes dans le backlog
❌ Impossible de prioriser
❌ Démotivant pour l'équipe

✅ Solution: Limiter le backlog
✅ Maximum 2-3 mois de travail
✅ Archiver le reste
```

## 8. Résumé de l'essentiel

### Points clés à retenir

1. **Kanban = Visualiser + Limiter + Gérer le flux**
   - Tableau visuel obligatoire
   - WIP limits sur chaque colonne
   - Focus sur le flux continu

2. **3 métriques essentielles**
   - Lead Time : Temps total
   - Cycle Time : Temps de travail
   - Throughput : Débit de livraison

3. **Règles d'or**
   - Finir avant de commencer
   - Respecter les WIP limits
   - Débloquer avant de tirer nouveau
   - Améliorer continuellement

4. **4 pratiques clés**
   - Visualiser le travail
   - Limiter le WIP
   - Gérer le flux
   - Rendre les politiques explicites

### Différences Kanban vs Scrum

```
Kanban:
✅ Flux continu
✅ Pas d'itérations fixes
✅ Changements à tout moment
✅ Métriques: Lead/Cycle Time
✅ Pas de rôles obligatoires

Scrum:
✅ Sprints de 2 semaines
✅ Itérations time-boxées
✅ Backlog figé pendant sprint
✅ Métriques: Velocity, Burndown
✅ 3 rôles définis (PO, SM, Dev)
```

### Checklist pour démarrer avec Kanban

**Setup initial :**
- [ ] Définir les colonnes (3-7 colonnes)
- [ ] Calculer et afficher les WIP limits
- [ ] Créer les cartes pour le travail actuel
- [ ] Définir les classes de service
- [ ] Choisir l'outil (physique ou numérique)

**Pratiques quotidiennes :**
- [ ] Mettre à jour le tableau en temps réel
- [ ] Respecter les WIP limits
- [ ] Marquer et escalader les blockers
- [ ] Daily standup (optionnel mais recommandé)

**Métriques et amélioration :**
- [ ] Tracker Lead Time et Cycle Time
- [ ] Créer un Cumulative Flow Diagram
- [ ] Calculer le Throughput
- [ ] Rétrospectives régulières
- [ ] Ajuster le process selon les données

### Les 6 pratiques Kanban de base

1. **Visualiser le workflow** : Rendre le travail visible
2. **Limiter le WIP** : Ne pas tout faire en même temps
3. **Gérer le flux** : Optimiser la fluidité
4. **Rendre les politiques explicites** : Définition claire de "Done"
5. **Boucles de feedback** : Amélioration continue
6. **Améliorer collaborativement** : Kaizen avec toute l'équipe

### Formules à retenir

```
WIP Limit (par colonne) = Nombre de personnes × 1.5

Lead Time = Date de livraison - Date de demande

Cycle Time = Date de livraison - Date de début du travail

Throughput = Nombre de cartes terminées / Période

Estimation de livraison = Taille backlog / Throughput moyen
```

### Indicateurs d'un Kanban sain

```
✅ WIP limits respectés
✅ Lead Time prévisible
✅ Peu ou pas de blockers
✅ CFD avec bandes parallèles
✅ Throughput stable
✅ Équipe confiante sur les délais
✅ Amélioration continue visible
```

---

**En une phrase :**

> Kanban est une méthode de gestion visuelle du flux de travail qui utilise un tableau avec colonnes et WIP limits pour optimiser la livraison continue, réduire le multitasking, identifier les goulots d'étranglement, et améliorer la prévisibilité grâce aux métriques de Lead Time, Cycle Time et Throughput.

**Pour être employable :**

**Tu DOIS savoir :**
- ✅ Expliquer ce qu'est un WIP limit et pourquoi c'est crucial
- ✅ Différencier Lead Time et Cycle Time
- ✅ Lire un Cumulative Flow Diagram
- ✅ Identifier un goulot d'étranglement sur un tableau
- ✅ Utiliser un tableau Kanban (Trello, Jira, etc.)
- ✅ Expliquer la différence entre Kanban et Scrum

**Vocabulaire à maîtriser absolument :**
Kanban Board, WIP Limit, Lead Time, Cycle Time, Throughput, Blocker, Swimlane, Pull System, CFD (Cumulative Flow Diagram), Bottleneck, Classes de Service, Expedite.
