# Agile Scrum : Méthode de Gestion de Projet Itérative

## 1. Contexte et définitions des concepts clés

### 1.1 Qu'est-ce que Scrum ?

**Scrum** est une méthode agile de gestion de projet qui permet de :
- Livrer des fonctionnalités rapidement et régulièrement
- S'adapter aux changements facilement
- Travailler en équipe de manière collaborative
- Obtenir du feedback utilisateur fréquemment

### 1.2 Principes fondamentaux

**Les 3 piliers de Scrum :**
1. **Transparence** : Tout le monde voit l'avancement du projet
2. **Inspection** : On vérifie régulièrement ce qui fonctionne
3. **Adaptation** : On ajuste en fonction des retours

### 1.3 Définitions importantes

- **Sprint** : Période de travail de 1 à 4 semaines (généralement 2 semaines)
- **Product Backlog** : Liste priorisée de toutes les fonctionnalités à développer
- **Sprint Backlog** : Liste des tâches à faire pendant le sprint en cours
- **User Story** : Description d'une fonctionnalité du point de vue utilisateur
- **Story Points** : Estimation de la complexité d'une tâche
- **Definition of Done (DoD)** : Critères pour considérer une tâche terminée
- **Velocity** : Nombre de story points complétés par sprint
- **Burndown Chart** : Graphique montrant le travail restant dans le sprint
- **Daily Standup** : Réunion quotidienne de 15 minutes debout
- **Sprint Planning** : Réunion pour planifier le sprint
- **Sprint Review** : Démonstration des fonctionnalités terminées
- **Sprint Retrospective** : Réunion pour améliorer les processus

## 2. Quand et pourquoi utiliser Scrum

### 2.1 Pourquoi Scrum est indispensable

**Sans Scrum (méthode classique) :**
- ❌ Découverte des problèmes trop tard
- ❌ Difficile de changer de direction
- ❌ Utilisateurs voient le produit seulement à la fin
- ❌ Risque de livrer quelque chose qui ne correspond pas aux besoins
- ❌ Équipe démotivée par manque de feedback

**Avec Scrum :**
- ✅ Livraisons fréquentes (toutes les 2 semaines)
- ✅ Feedback rapide des utilisateurs
- ✅ Adaptation facile aux changements
- ✅ Équipe autonome et motivée
- ✅ Réduction des risques
- ✅ Transparence totale sur l'avancement

### 2.2 Cas d'usage idéaux

**Scrum est parfait pour :**
- Développement de logiciels et applications web/mobile
- Projets avec des besoins qui évoluent
- Équipes de 3 à 9 personnes
- Projets complexes où on ne peut pas tout planifier à l'avance
- Startups qui doivent pivoter rapidement

**Exemples concrets :**
```
✅ Développement d'une application mobile
✅ Création d'un site e-commerce
✅ Développement d'une API
✅ Projet de refactoring d'une application
✅ Création d'un MVP (Minimum Viable Product)
```

### 2.3 Quand NE PAS utiliser Scrum

```
❌ Projet avec scope 100% défini et immuable
❌ Équipe de 1-2 personnes (overhead trop important)
❌ Projet très court (< 1 mois)
❌ Équipe distribuée sans bons outils de communication
❌ Organisation très hiérarchique qui ne laisse pas d'autonomie
```

## 3. Les rôles dans Scrum

### 3.1 Product Owner (PO)

**Responsabilités :**
- Définit et priorise le Product Backlog
- Représente les utilisateurs et le business
- Accepte ou rejette le travail terminé
- Prend les décisions sur ce qui doit être développé

**Compétences requises :**
- Connaissance du domaine métier
- Capacité à prioriser
- Communication claire avec l'équipe

**Ce qu'il fait au quotidien :**
```
- Rédige les User Stories
- Répond aux questions de l'équipe
- Participe au Sprint Planning
- Valide les fonctionnalités terminées
- Parle avec les utilisateurs et stakeholders
```

### 3.2 Scrum Master

**Responsabilités :**
- Facilite les cérémonies Scrum
- Supprime les obstacles de l'équipe
- Protège l'équipe des interruptions externes
- Coach l'équipe sur les pratiques agiles

**Compétences requises :**
- Leadership serviteur
- Facilitation de réunions
- Résolution de conflits

**Ce qu'il fait au quotidien :**
```
- Anime le Daily Standup
- Aide l'équipe à résoudre les blocages
- Organise les cérémonies Scrum
- Améliore les processus de l'équipe
```

**⚠️ Ce que le Scrum Master N'EST PAS :**
- ❌ Un chef de projet traditionnel
- ❌ Un manager qui donne des ordres
- ❌ Responsable de la livraison

### 3.3 Development Team (Équipe de développement)

**Responsabilités :**
- Développe les fonctionnalités
- S'auto-organise pour atteindre les objectifs
- Estime le travail
- Livre un incrément fonctionnel chaque sprint

**Caractéristiques :**
- 3 à 9 personnes
- Cross-fonctionnelle (développeurs, testeurs, designers)
- Auto-organisée (pas de hiérarchie interne)

## 4. Le cycle Scrum : Sprints et cérémonies

### 4.1 Anatomie d'un Sprint

```
Sprint (2 semaines)
│
├─ Jour 1: Sprint Planning (2-4h)
│   ↓
├─ Jours 1-10: Développement
│   │  ├─ Daily Standup (15 min/jour)
│   │  └─ Travail sur les User Stories
│   ↓
├─ Jour 10: Sprint Review (1-2h)
│   ↓
└─ Jour 10: Sprint Retrospective (1h)
```

### 4.2 Sprint Planning (Planification)

**Durée :** 2-4 heures pour un sprint de 2 semaines

**Objectif :** Décider ce qui sera fait pendant le sprint

**Déroulement :**
1. **Partie 1 : QUOI ?** (1-2h)
   - Product Owner présente les User Stories prioritaires
   - Équipe pose des questions
   - Équipe sélectionne les stories pour le sprint

2. **Partie 2 : COMMENT ?** (1-2h)
   - Équipe décompose les stories en tâches techniques
   - Estimation des tâches
   - Création du Sprint Backlog

**Résultat :**
- Sprint Goal (objectif du sprint)
- Sprint Backlog (liste des tâches)

**Exemple de Sprint Goal :**
```
"Permettre aux utilisateurs de s'inscrire et de se connecter à l'application"
```

### 4.3 Daily Standup (Mêlée quotidienne)

**Durée :** 15 minutes MAXIMUM

**Format :** Debout (pour rester court)

**Heure fixe :** Même heure chaque jour (ex: 9h30)

**Chaque membre répond à 3 questions :**
1. Qu'ai-je fait hier ?
2. Que vais-je faire aujourd'hui ?
3. Ai-je des blocages ?

**Exemple :**
```
Alice:
"Hier : J'ai terminé l'intégration de l'API de paiement
Aujourd'hui : Je vais commencer les tests
Blocages : Aucun"

Bob:
"Hier : J'ai commencé le design de la page profil
Aujourd'hui : Je vais finir le design et commencer l'intégration
Blocages : J'ai besoin des maquettes finales du client"
```

**⚠️ Ce que le Daily Standup N'EST PAS :**
- ❌ Un rapport au manager
- ❌ Une résolution de problèmes détaillée
- ❌ Une planification de sprint
- ❌ Plus de 15 minutes

### 4.4 Sprint Review (Revue de Sprint)

**Durée :** 1-2 heures

**Participants :** Équipe Scrum + Stakeholders + Utilisateurs

**Objectif :** Démontrer ce qui a été fait

**Déroulement :**
1. Product Owner rappelle le Sprint Goal
2. Équipe démontre les fonctionnalités terminées (DEMO)
3. Stakeholders donnent du feedback
4. Discussions sur les prochaines priorités
5. Mise à jour du Product Backlog

**Format de démo :**
```
"Avant, on ne pouvait pas...
Maintenant, regardez : [démonstration live]
Cela permet à l'utilisateur de..."
```

**⚠️ Important :**
- ❌ Pas de PowerPoint, que du code fonctionnel
- ✅ Environnement de démo qui fonctionne
- ✅ Données réalistes pour la démo

### 4.5 Sprint Retrospective (Rétrospective)

**Durée :** 1 heure

**Participants :** Équipe Scrum uniquement (pas de stakeholders)

**Objectif :** Améliorer le processus de l'équipe

**Format classique (Start-Stop-Continue) :**
1. **Start** : Qu'est-ce qu'on devrait commencer à faire ?
2. **Stop** : Qu'est-ce qu'on devrait arrêter de faire ?
3. **Continue** : Qu'est-ce qui fonctionne bien ?

**Exemple :**
```
START:
- Pair programming sur le code complexe
- Revue de code systématique

STOP:
- Réunions qui débordent
- Interruptions pendant le Daily Standup

CONTINUE:
- Documentation des décisions techniques
- Tests automatisés
```

**Actions concrètes :**
```
Action 1: Alice et Bob feront du pair programming sur la nouvelle architecture
Responsable: Bob
Échéance: Prochain sprint

Action 2: Scrum Master s'assurera que les réunions finissent à l'heure
Responsable: Scrum Master
Échéance: Dès aujourd'hui
```

## 5. Artifacts Scrum : Backlog et Burndown

### 5.1 Product Backlog

**Définition :** Liste priorisée de toutes les fonctionnalités du produit

**Caractéristiques :**
- Toujours priorisé (en haut = plus important)
- Vivant (évolue constamment)
- Géré par le Product Owner
- Visible par toute l'équipe

**Format User Story :**
```
En tant que [type d'utilisateur]
Je veux [action]
Afin de [bénéfice]

Exemple:
En tant qu'utilisateur inscrit
Je veux pouvoir réinitialiser mon mot de passe
Afin de récupérer l'accès à mon compte si je l'oublie

Critères d'acceptation:
- Lien "Mot de passe oublié" sur la page de connexion
- Email de réinitialisation envoyé en moins de 5 minutes
- Lien valide pendant 24h seulement
```

**Estimation en Story Points :**
```
1 point  = Très simple (< 2h)
2 points = Simple (2-4h)
3 points = Moyen (1 jour)
5 points = Complexe (2-3 jours)
8 points = Très complexe (> 3 jours)
13+ points = À découper en plus petites stories
```

### 5.2 Sprint Backlog

**Définition :** Liste des User Stories et tâches du sprint en cours

**Contenu :**
- User Stories sélectionnées pour le sprint
- Tâches techniques pour chaque story
- Estimation en heures pour chaque tâche

**Exemple :**
```
User Story: Login utilisateur (5 points)
  ├─ Créer la page de login (4h)
  ├─ Implémenter l'API de login (6h)
  ├─ Ajouter validation des champs (2h)
  ├─ Gestion des erreurs (3h)
  └─ Tests unitaires (3h)
  Total: 18h
```

### 5.3 Burndown Chart

**Définition :** Graphique montrant le travail restant dans le sprint

```
Heures
restantes
  │
80│ ╲
  │   ╲
60│     ╲  ← Idéal
  │       ╲
40│    ╱─╲  ╲  ← Réel
  │  ╱     ╲  ╲
20│╱         ╲  ╲
  │            ╲  ╲
 0└─────────────────╲─→ Jours
  1  3  5  7  9  11  13
```

**Comment lire le Burndown :**
- **Ligne idéale** : Descente linéaire
- **Ligne réelle** : Travail réel effectué
- **Au-dessus de l'idéal** : On est en retard
- **En-dessous de l'idéal** : On est en avance

**⚠️ Attention :**
```
❌ Burndown qui ne descend pas → Équipe bloquée ou sous-estimé
❌ Burndown en plateau → Pas de mise à jour ou vraiment bloqué
✅ Burndown qui descend régulièrement → Bon rythme
```

### 5.4 Velocity (Vélocité)

**Définition :** Nombre de story points complétés par sprint

**Calcul :**
```
Sprint 1: 23 points complétés
Sprint 2: 27 points complétés
Sprint 3: 25 points complétés

Velocity moyenne = (23 + 27 + 25) / 3 = 25 points/sprint
```

**Utilisation :**
- Prédire combien de sprints pour finir le backlog
- Planifier les releases
- Ajuster les engagements de sprint

**Exemple :**
```
Product Backlog: 250 story points
Velocity: 25 points/sprint

Estimation: 250 / 25 = 10 sprints
Si sprint = 2 semaines → 20 semaines (~5 mois)
```

## 6. Exemples concrets et outils

### 6.1 Outils Scrum populaires

**Outils gratuits/freemium :**
- **Trello** : Simple, visuel, parfait pour débuter
- **Jira** : Standard de l'industrie, puissant mais complexe
- **Monday.com** : Visuel et flexible
- **Azure DevOps** : Intégré avec Microsoft, excellent pour .NET
- **GitHub Projects** : Intégré avec GitHub, simple

**Outils physiques :**
- Tableau blanc avec post-its
- Cartes physiques pour les stories

### 6.2 Exemple de workflow sur Jira

**Statuts typiques :**
```
TODO → IN PROGRESS → CODE REVIEW → TESTING → DONE
```

**Colonnes Kanban :**
```
┌─────────┬────────────┬─────────┬─────────┬──────┐
│  TO DO  │IN PROGRESS │ REVIEW  │ TESTING │ DONE │
├─────────┼────────────┼─────────┼─────────┼──────┤
│ Story A │  Story B   │Story C  │Story D  │Story E│
│ Story F │            │         │         │Story G│
│ Story H │            │         │         │      │
└─────────┴────────────┴─────────┴─────────┴──────┘
```

### 6.3 Template de Daily Standup sur Slack

```markdown
🌅 Daily Standup - [Date]

👤 **Alice** (@alice)
✅ Hier: Terminé l'intégration Stripe
📋 Aujourd'hui: Tests de l'API de paiement
🚧 Blocages: Aucun

👤 **Bob** (@bob)
✅ Hier: Design page profil
📋 Aujourd'hui: Intégration front-end
🚧 Blocages: Besoin de la maquette finale du client

👤 **Charlie** (@charlie)
✅ Hier: Fix bugs sur le formulaire
📋 Aujourd'hui: Code review + nouvelle feature
🚧 Blocages: Aucun
```

### 6.4 Template de Sprint Planning

```markdown
# Sprint Planning - Sprint #12

## Sprint Goal
Permettre aux utilisateurs de gérer leur panier d'achat

## Stories sélectionnées

### 1. Ajouter produit au panier (5 points)
En tant qu'utilisateur
Je veux ajouter un produit au panier
Afin de préparer ma commande

**Critères d'acceptation:**
- [ ] Bouton "Ajouter au panier" visible sur chaque produit
- [ ] Produit ajouté avec quantité = 1 par défaut
- [ ] Message de confirmation affiché
- [ ] Compteur du panier mis à jour

**Tâches:**
- [ ] Créer API POST /cart/items (6h) - Alice
- [ ] Intégrer bouton front-end (4h) - Bob
- [ ] Tests unitaires API (3h) - Alice
- [ ] Tests E2E (3h) - Charlie

### 2. Afficher le panier (3 points)
...

## Capacity
- Alice: 32h disponibles
- Bob: 28h disponibles (congé 1 jour)
- Charlie: 32h disponibles
**Total: 92h**

## Engagement
Total story points: 23 points
Total heures estimées: 85h
✅ Réaliste par rapport à la capacité
```

## 7. Erreurs et pièges à éviter

### 7.1 Erreurs fréquentes

| Erreur | Description | Impact | Solution |
|--------|-------------|--------|----------|
| **Sprint trop long** | Sprint de 4+ semaines | Feedback trop tardif | Sprints de 2 semaines max |
| **Pas de DoD** | Aucun critère de "terminé" | Stories jamais vraiment finies | Définir DoD clair dès le début |
| **PO absent** | PO pas disponible pour l'équipe | Décisions bloquées | PO dédicacé et disponible |
| **Pas de démo fonctionnelle** | PowerPoint au lieu de code | Pas de validation réelle | Toujours démo en live |
| **Changer le Sprint Backlog** | Ajouter des stories en cours de sprint | Sprint instable | Backlog fixe pendant sprint |
| **Estimation en heures** | Estimer en temps exact | Stress et mauvaise planification | Utiliser story points |
| **Pas de retrospective** | Pas d'amélioration continue | Répéter les mêmes erreurs | Rétro obligatoire chaque sprint |

### 7.2 Pièges courants

**Piège 1 : Scrum à l'eau**
```
❌ On fait les réunions mais pas l'esprit
❌ Daily de 45 minutes
❌ Product Owner qui ne priorise pas
❌ Pas d'amélioration continue

✅ Solution: Respecter les timeboxes
✅ Scrum Master fait respecter les règles
✅ Formation de l'équipe
```

**Piège 2 : Micro-management déguisé**
```
❌ Manager demande des rapports quotidiens détaillés
❌ Scrum Master qui assigne les tâches
❌ Chaque décision doit être validée

✅ Solution: Équipe auto-organisée
✅ Scrum Master = facilitateur, pas manager
✅ Confiance en l'équipe
```

**Piège 3 : Pas de définition claire du "Done"**
```
❌ "C'est fait" mais pas testé
❌ "C'est fait" mais pas déployé
❌ Chaque personne a sa définition

✅ Solution: Definition of Done explicite
Exemple DoD:
- [ ] Code écrit
- [ ] Tests unitaires passent
- [ ] Code review approuvée
- [ ] Tests d'intégration passent
- [ ] Documentation mise à jour
- [ ] Déployé en staging
- [ ] Validé par le PO
```

**Piège 4 : Velocity comme pression**
```
❌ "Pourquoi on a fait que 15 points ce sprint ?"
❌ Utiliser velocity pour comparer les équipes
❌ Pression pour augmenter artificiellement

✅ Solution: Velocity = outil de planification
✅ Pas de comparaison entre équipes
✅ Variations normales acceptées
```

### 7.3 Anti-patterns Scrum

**1. Zombie Scrum**
- On fait les rituels mais sans résultats
- Pas de valeur livrée aux utilisateurs
- Équipe démotivée

**2. Scrum-but**
- "On fait Scrum MAIS..."
- "On fait Scrum mais pas de Daily"
- "On fait Scrum mais le chef décide tout"

**3. Dark Scrum**
- Scrum utilisé comme outil de contrôle
- Micro-management via les outils
- Pression constante sur la velocity

## 8. Résumé de l'essentiel

### Points clés à retenir

1. **Scrum = Itératif et incrémental**
   - Sprints courts (2 semaines)
   - Livraisons régulières
   - Adaptation constante

2. **3 rôles essentiels**
   - Product Owner : Définit le QUOI
   - Scrum Master : Facilite le COMMENT
   - Dev Team : Fait le TRAVAIL

3. **4 cérémonies obligatoires**
   - Sprint Planning : Planifier
   - Daily Standup : Synchroniser
   - Sprint Review : Démontrer
   - Sprint Retrospective : Améliorer

4. **3 artifacts principaux**
   - Product Backlog : Toutes les features
   - Sprint Backlog : Features du sprint
   - Burndown Chart : Suivi du sprint

### Workflow Scrum en une image

```
┌─────────────────────────────────────┐
│         Product Backlog             │
│  (Priorisé par Product Owner)       │
└────────────┬────────────────────────┘
             │
             ▼
┌────────────────────────────────────┐
│      Sprint Planning               │
│  → Sélection des User Stories      │
└────────────┬───────────────────────┘
             │
             ▼
      ┌─────────────┐
      │   Sprint    │ ← Daily Standup (tous les jours)
      │  (2 sem.)   │
      └──────┬──────┘
             │
             ▼
┌────────────────────────────────────┐
│      Sprint Review                 │
│  → Démo aux stakeholders           │
└────────────┬───────────────────────┘
             │
             ▼
┌────────────────────────────────────┐
│   Sprint Retrospective             │
│  → Amélioration processus          │
└────────────┬───────────────────────┘
             │
             ▼ (Nouveau sprint)
```

### Checklist pour démarrer avec Scrum

**Avant le premier sprint :**
- [ ] Équipe Scrum formée (PO + SM + Dev Team)
- [ ] Rôles et responsabilités clairs
- [ ] Product Backlog initial créé
- [ ] Definition of Done définie
- [ ] Durée de sprint choisie (2 semaines recommandé)
- [ ] Outil choisi (Jira, Trello, etc.)
- [ ] Horaires des cérémonies fixés

**Pendant le sprint :**
- [ ] Daily Standup tous les jours (15 min)
- [ ] Burndown mis à jour quotidiennement
- [ ] Blocages résolus rapidement
- [ ] Sprint Backlog visible par tous

**Fin de sprint :**
- [ ] Sprint Review avec démo
- [ ] Sprint Retrospective avec actions
- [ ] Nouveau Sprint Planning

### Les 5 valeurs Scrum

1. **Courage** : Oser dire quand ça ne va pas
2. **Focus** : Se concentrer sur le Sprint Goal
3. **Commitment** : S'engager sur les objectifs
4. **Respect** : Respecter l'équipe et les rôles
5. **Openness** : Transparence sur tout

### Métriques à suivre

**Métriques essentielles :**
- Velocity (story points par sprint)
- Sprint Burndown (quotidien)
- Release Burnup (progression globale)
- Nombre de bugs en production

**Métriques avancées :**
- Lead Time (temps idée → production)
- Cycle Time (temps développement → production)
- Satisfaction client (NPS)
- Bonheur de l'équipe

---

**En une phrase :**

> Scrum est une méthode agile qui permet de livrer de la valeur rapidement via des sprints de 2 semaines, avec des rôles clairs (Product Owner, Scrum Master, Dev Team), 4 cérémonies essentielles (Planning, Daily, Review, Retro), et un focus constant sur l'adaptation, la transparence et l'amélioration continue.

**Pour être employable :**

**Tu DOIS savoir :**
- ✅ Expliquer les 3 rôles Scrum
- ✅ Participer activement au Daily Standup
- ✅ Estimer des User Stories en story points
- ✅ Comprendre et utiliser un Burndown Chart
- ✅ Participer à une Sprint Review et Retrospective
- ✅ Utiliser Jira ou Trello pour suivre le travail

**Vocabulaire à maîtriser absolument :**
Sprint, User Story, Story Points, Velocity, Burndown, Product Owner, Scrum Master, Daily Standup, Sprint Planning, Sprint Review, Retrospective, Product Backlog, Sprint Backlog, Definition of Done.
