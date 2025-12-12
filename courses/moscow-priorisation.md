# MoSCoW : Méthode de Priorisation des Fonctionnalités

## 1. Contexte et définitions des concepts clés

### 1.1 Qu'est-ce que MoSCoW ?

**MoSCoW** est une technique de priorisation qui classe les fonctionnalités en 4 catégories :
- **M**ust have (Doit avoir)
- **S**hould have (Devrait avoir)
- **C**ould have (Pourrait avoir)
- **W**on't have (N'aura pas - pour l'instant)

**Objectif :** Aider à décider ce qui est vraiment essentiel dans un projet.

**Analogie simple :**
> Si tu déménages et ne peux prendre qu'une voiture de choses, tu priorises : Must = Papiers d'identité, Should = Vêtements, Could = Livres, Won't = Vieux magazines.

### 1.2 Principes fondamentaux

**Règle du 60/20/20 :**
- **60%** des fonctionnalités = Must have
- **20%** des fonctionnalités = Should have
- **20%** des fonctionnalités = Could have
- **Won't have** = Le reste (pas de limite)

**⚠️ Important :**
Si tout est "Must have", rien n'est vraiment prioritaire !

### 1.3 Définitions importantes

- **Must have** : Sans cela, le projet échoue
- **Should have** : Important mais pas bloquant
- **Could have** : Bon à avoir si temps/budget permet
- **Won't have** : Pas pour cette version, peut-être plus tard
- **MVP** : Minimum Viable Product (composé des Must have)
- **Scope creep** : Ajout incontrôlé de fonctionnalités
- **Value vs Effort** : Valeur business vs effort de développement

## 2. Quand et pourquoi utiliser MoSCoW

### 2.1 Pourquoi MoSCoW est puissant

**Sans priorisation claire :**
- ❌ Tout semble urgent et important
- ❌ On développe des features inutiles
- ❌ Pas de focus sur l'essentiel
- ❌ Retards constants
- ❌ Budget dépassé
- ❌ Frustration de l'équipe

**Avec MoSCoW :**
- ✅ Focus sur l'essentiel (Must have)
- ✅ MVP livré rapidement
- ✅ Décisions claires en cas de contrainte
- ✅ Réduction du scope creep
- ✅ Satisfaction client (livraison rapide de valeur)
- ✅ Équipe concentrée

### 2.2 Cas d'usage idéaux

**MoSCoW est parfait pour :**
- Définition du MVP
- Priorisation du Product Backlog
- Négociation de scope avec le client
- Gestion de contraintes (temps, budget)
- Sprint Planning (Scrum)
- Phase de découverte produit

**Exemples concrets :**
```
✅ Startup qui doit lancer rapidement un MVP
✅ Projet avec budget/deadline fixes
✅ Product Owner qui doit prioriser le backlog
✅ Équipe qui doit réduire le scope
✅ Client qui veut "tout" dans la v1
```

### 2.3 Quand utiliser MoSCoW

**Moments clés :**
```
1. Début du projet : Définir le MVP
2. Sprint Planning : Prioriser le sprint
3. Changement de scope : Décider quoi garder/enlever
4. Contrainte découverte : Adapter rapidement
5. Release Planning : Décider quoi livrer quand
```

## 3. Les 4 catégories MoSCoW en détail

### 3.1 Must have (M) - Doit avoir

**Définition :**
Fonctionnalités absolument essentielles sans lesquelles le projet est un échec.

**Questions à se poser :**
```
❓ Le système fonctionne-t-il sans cette fonctionnalité ?
❓ Y a-t-il une obligation légale/contractuelle ?
❓ Le business peut-il opérer sans ?
❓ Quel est l'impact de ne pas l'avoir ?
```

**Si la réponse = NON, c'est un Must have**

**Critères :**
- ✅ Obligation légale ou contractuelle
- ✅ Sans cela, le produit est inutilisable
- ✅ ROI immédiat et critique
- ✅ Fondation pour autres fonctionnalités

**Exemples pour une application E-commerce :**
```
Must have:
✅ Inscription/Connexion utilisateur
✅ Affichage des produits
✅ Panier d'achat
✅ Paiement sécurisé
✅ Confirmation de commande
✅ RGPD compliance (obligation légale)
```

**⚠️ Attention :**
- Trop de Must have = Mauvaise priorisation
- Must have ≠ Ce que le client veut en premier
- Must have = Ce dont on ne peut pas se passer

### 3.2 Should have (S) - Devrait avoir

**Définition :**
Fonctionnalités importantes mais pas vitales. Le projet peut réussir sans, mais avec dégradation de l'expérience.

**Questions à se poser :**
```
❓ Peut-on contourner/différer cette fonctionnalité ?
❓ Y a-t-il un workaround acceptable ?
❓ Quel impact sur l'expérience utilisateur ?
❓ Peut-on livrer en v1.1 plutôt que v1.0 ?
```

**Critères :**
- Important pour l'expérience utilisateur
- Valeur business significative
- Pas de contournement idéal mais possible
- Livrable dans la version suivante

**Exemples pour une application E-commerce :**
```
Should have:
✅ Filtres avancés (prix, catégorie, marque)
✅ Liste de souhaits (wishlist)
✅ Recommandations de produits
✅ Avis clients
✅ Historique des commandes
✅ Notifications par email
```

**Workarounds possibles :**
```
Feature manquante: Filtres avancés
→ Workaround: Recherche textuelle basique
→ Impact: Utilisateur met plus de temps à trouver

Feature manquante: Liste de souhaits
→ Workaround: Mettre dans panier pour plus tard
→ Impact: Expérience moins bonne mais possible
```

### 3.3 Could have (C) - Pourrait avoir

**Définition :**
Fonctionnalités "nice to have" qui améliorent l'expérience mais ne sont pas nécessaires.

**Questions à se poser :**
```
❓ Quel est le coût/bénéfice ?
❓ Cette feature est-elle vraiment utilisée ?
❓ Est-ce une demande de 1 utilisateur ou de beaucoup ?
❓ Peut-on s'en passer indéfiniment ?
```

**Critères :**
- Peu d'impact si absent
- Effort souvent élevé pour peu de valeur
- Utilisateurs ne le demandent pas activement
- Peut-être jamais implémenté

**Exemples pour une application E-commerce :**
```
Could have:
✅ Partage sur réseaux sociaux
✅ Comparateur de produits
✅ Mode sombre
✅ Animations sophistiquées
✅ Chatbot IA
✅ Intégration avec 10 moyens de paiement exotiques
```

**Règle d'or :**
> Si en cas de contrainte tu dois couper quelque chose, commence par les "Could have"

### 3.4 Won't have (W) - N'aura pas (cette fois)

**Définition :**
Fonctionnalités explicitement exclues de cette version. Peut-être dans le futur, mais pas maintenant.

**Pourquoi c'est important :**
- Gérer les attentes
- Éviter les discussions récurrentes
- Focus de l'équipe
- Documentation des décisions

**Raisons d'un Won't have :**
```
❌ Trop coûteux pour la valeur
❌ Technologie pas encore mature
❌ Hors du scope du projet
❌ Dépendance externe non prête
❌ Besoin utilisateur non validé
❌ Complexité trop élevée
```

**Exemples pour une application E-commerce :**
```
Won't have (v1.0):
❌ Application mobile native
❌ Marketplace multi-vendeurs
❌ Réalité augmentée pour essayer les produits
❌ Intégration ERP complexe
❌ Support de 50 devises
❌ Programme de fidélité avancé
```

**Communication :**
```
"Cette fonctionnalité est classée Won't have pour la v1.0.
Nous la réévaluerons pour la v2.0 après avoir validé
le MVP et reçu du feedback utilisateur."
```

## 4. Comment prioriser avec MoSCoW

### 4.1 Processus en 5 étapes

**Étape 1 : Lister toutes les fonctionnalités**
```
Brainstorming complet:
- Inscription/Connexion
- Catalogue produits
- Panier
- Paiement
- Profil utilisateur
- Filtres
- Wishlist
- Recommandations
- Chat support
- Blog
- ... (30 features au total)
```

**Étape 2 : Classification initiale**
```
Chaque feature → M, S, C ou W
Règle: En cas de doute, descendre d'une catégorie
```

**Étape 3 : Validation avec critères**
```
Pour chaque Must have, demander:
"Peut-on lancer sans ça ?"

Si réponse = "Oui, mais..." → Descendre en Should have
```

**Étape 4 : Vérifier les proportions**
```
Calculer:
Must have: 8 features → 27% ❌ Trop peu !
Should have: 15 features → 50% ❌ Trop !
Could have: 7 features → 23% ✅ OK

Ajuster pour atteindre 60/20/20
```

**Étape 5 : Consensus et documentation**
```
- Présenter à l'équipe
- Débattre les cas limites
- Obtenir validation Product Owner
- Documenter dans Product Backlog
```

### 4.2 Techniques de facilitation

**Technique 1 : Dot Voting**
```
1. Lister 20 features sur un mur
2. Chaque personne a 5 stickers
3. Coller stickers sur features prioritaires
4. Features avec plus de stickers = Must/Should
```

**Technique 2 : Matrice Valeur/Effort**
```
        Valeur Haute
             │
   Should    │    Must
   have      │    have
─────────────┼─────────────
   Won't     │   Could
   have      │    have
             │
        Valeur Basse
```

**Technique 3 : Buy a Feature**
```
1. Chaque feature a un "prix"
2. Chaque stakeholder a un "budget"
3. Ils "achètent" les features qu'ils veulent
4. Features plus chères = Plus de valeur
```

### 4.3 Exemple complet : Application de Réservation Restaurant

**Liste initiale (30 features) :**

**Must have (60% = 18 features) :**
```
M1.  Recherche restaurants par ville
M2.  Affichage des disponibilités
M3.  Réservation de table
M4.  Confirmation par email
M5.  Annulation de réservation
M6.  Inscription utilisateur
M7.  Connexion utilisateur
M8.  Profil restaurant (horaires, adresse)
M9.  Photos du restaurant
M10. Gestion des réservations (restaurant)
M11. Calendrier des réservations
M12. RGPD - Consentement données
M13. Système de paiement caution
M14. Notifications SMS confirmation
M15. Recherche par date/heure
M16. Nombre de personnes dans résa
M17. Validation email inscription
M18. Récupération mot de passe
```

**Should have (20% = 6 features) :**
```
S1. Filtres recherche (cuisine, prix, note)
S2. Avis clients
S3. Note moyenne restaurant
S4. Modification de réservation
S5. Historique des réservations
S6. Programme de fidélité basique
```

**Could have (20% = 6 features) :**
```
C1. Recommandations personnalisées
C2. Partage sur réseaux sociaux
C3. Liste de restaurants favoris
C4. Intégration Google Maps avancée
C5. Photos uploadées par utilisateurs
C6. Chatbot pour aide
```

**Won't have (v1.0) :**
```
W1. Application mobile native
W2. Réservation de groupes (>12 personnes)
W3. Commande de plats à l'avance
W4. Système de queue virtuelle
W5. Réalité augmentée du restaurant
W6. Intégration avec 20 plateformes
```

### 4.4 Gestion des désaccords

**Scénario commun :**
```
Product Owner: "Le chat en direct est un Must have!"
Dev Team: "C'est très complexe, plutôt Should have"
```

**Approche de résolution :**

**1. Questions de clarification :**
```
❓ Pourquoi est-ce Must have ?
   → "Les utilisateurs ont besoin d'aide immédiate"

❓ Que se passe-t-il sans ?
   → "Ils nous appellent ou nous envoient un email"

❓ Y a-t-il un workaround ?
   → "Email de support avec engagement réponse < 2h"
```

**2. Critères objectifs :**
```
Obligation légale ? Non
Bloque lancement ? Non
Alternative viable ? Oui (email + téléphone)

→ Conclusion: Should have, pas Must have
```

**3. Compromis :**
```
MVP (Must): Email et téléphone support
v1.1 (Should): Chat en direct basique
v2.0 (Could): Chatbot IA avancé
```

## 5. Outils et templates

### 5.1 Template Excel/Google Sheets

**Colonnes recommandées :**
```
| ID | Feature | Description | Priorité MoSCoW | Valeur (1-5) | Effort (1-5) | Notes |
```

**Exemple :**
```
| F01 | Paiement | Intégration Stripe | Must | 5 | 3 | Critique pour lancement |
| F02 | Wishlist | Liste de souhaits | Should | 3 | 2 | Demandé par 60% users |
| F03 | Mode sombre | Dark mode UI | Could | 2 | 4 | Effort élevé, peu de valeur |
```

### 5.2 Template Product Backlog Jira/Trello

**Labels Jira :**
```
🔴 Must have (Priority: Highest)
🟠 Should have (Priority: High)
🟡 Could have (Priority: Medium)
⚪ Won't have (Priority: Lowest ou Icebox)
```

**Description de User Story :**
```
Titre: En tant qu'utilisateur, je veux payer par carte

Priorité MoSCoW: Must have

Justification:
Sans paiement, impossible de monétiser l'application.
Obligation légale de sécuriser les paiements.

Critères d'acceptation:
- Intégration Stripe
- Paiement sécurisé (HTTPS)
- Confirmation après paiement
- Gestion des erreurs

Valeur business: 5/5
Effort estimé: 13 story points
```

### 5.3 Board Miro/Mural pour workshop

**Template visuel :**
```
┌─────────────────────────────────────────┐
│  MoSCoW Priorisation Workshop           │
├─────────────────────────────────────────┤
│                                         │
│  ┌──────────┐  ┌──────────┐            │
│  │          │  │          │            │
│  │  MUST    │  │  SHOULD  │            │
│  │  HAVE    │  │   HAVE   │            │
│  │ (60%)    │  │  (20%)   │            │
│  │          │  │          │            │
│  │ [Features│  │ [Features│            │
│  │   here]  │  │   here]  │            │
│  │          │  │          │            │
│  └──────────┘  └──────────┘            │
│                                         │
│  ┌──────────┐  ┌──────────┐            │
│  │          │  │          │            │
│  │  COULD   │  │  WON'T   │            │
│  │  HAVE    │  │   HAVE   │            │
│  │ (20%)    │  │  (now)   │            │
│  │          │  │          │            │
│  │ [Features│  │ [Features│            │
│  │   here]  │  │   here]  │            │
│  │          │  │          │            │
│  └──────────┘  └──────────┘            │
│                                         │
└─────────────────────────────────────────┘
```

## 6. Erreurs et pièges à éviter

### 6.1 Erreurs fréquentes

| Erreur | Description | Impact | Solution |
|--------|-------------|--------|----------|
| **Tout est Must have** | 90% des features en Must | Pas de priorisation réelle | Règle 60/20/20 stricte |
| **Confusion besoin/solution** | "Must: Chat IA" au lieu de "Must: Support client" | Mauvaise priorisation | Raisonner en besoin d'abord |
| **Pression du client** | Client veut tout en Must | Scope irréaliste | Éducation + critères objectifs |
| **Ignorer l'effort** | Must have avec 6 mois d'effort | Impossible à livrer | Considérer Valeur ET Effort |
| **Pas de revue** | MoSCoW figé au début | Inadapté aux changements | Revue chaque sprint |
| **Won't have non communiqué** | Équipe développe quand même | Gaspillage | Won't visible et expliqué |
| **Pas de critères** | Décisions émotionnelles | Débats sans fin | Critères objectifs définis |

### 6.2 Pièges courants

**Piège 1 : Feature déguisée en Must**
```
❌ "Le chat en direct est Must have car le CEO le veut"

Questions de validation:
❓ Le produit est-il inutilisable sans ?
   → Non, email fonctionne
❓ Obligation légale ?
   → Non
❓ ROI critique ?
   → Non, nice to have

✅ Conclusion: Should have ou Could have
```

**Piège 2 : Must have = Première version**
```
❌ Confusion: "Must have = dans la v1.0"

✅ Clarification:
Must have = Minimum pour que le produit ait de la valeur
≠ Tout ce qu'on veut dans la v1

Si contrainte, on livre que les Must have
```

**Piège 3 : Pas de Won't have**
```
❌ Toutes les features sont M, S ou C
   → Scope creep
   → Discussions récurrentes

✅ Won't have explicite:
   → Attentes gérées
   → Focus préservé
   → Décisions documentées
```

**Piège 4 : MoSCoW = Waterfall**
```
❌ "On fait tous les Must, puis tous les Should..."

✅ Approche agile:
Sprint 1: Must have critiques
Sprint 2: Must have + Should have prioritaires
Sprint 3: Reste Must + Should
Sprint 4: Could have si temps
```

### 6.3 Antipatterns

**1. Moscow Inflation**
```
Début du projet:
- Must: 10 features
- Should: 5 features

2 mois après:
- Must: 25 features ❌
- Should: 15 features ❌

Cause: Pas de discipline, pression client

Solution: Règle stricte - Ajouter un Must = Retirer un autre
```

**2. Silent Won't have**
```
❌ Features marquées Won't sans communication
   → Équipe les développe quand même
   → Client s'attend à les avoir

✅ Won't have documenté et communiqué
   → Meeting pour expliquer
   → Visible dans backlog
```

**3. Priorisation politique**
```
❌ "C'est Must car c'est l'idée du VP Marketing"

✅ Critères objectifs uniquement:
   - Valeur utilisateur
   - Impact business
   - Obligations légales
   - Effort technique
```

## 7. MoSCoW et autres méthodes

### 7.1 MoSCoW + Value vs Effort

**Combinaison puissante :**
```
         Value
           ↑
    S      │      M
   High    │    High
   Effort  │   Low Effort
───────────┼───────────→ Effort
    W      │      C
   Low     │    Low
   Value   │    Value
```

**Règle de correspondance :**
- Haute valeur + Faible effort = Must have
- Haute valeur + Effort élevé = Should have
- Faible valeur + Faible effort = Could have
- Faible valeur + Effort élevé = Won't have

### 7.2 MoSCoW + User Story Mapping

**Intégration :**
```
Backbone (activités principales)
├─ Activité 1
│  ├─ Must: Story A
│  ├─ Must: Story B
│  └─ Should: Story C
│
├─ Activité 2
│  ├─ Must: Story D
│  ├─ Should: Story E
│  └─ Could: Story F
│
Release 1 = Toutes les Must have
Release 2 = Must + Should
```

### 7.3 MoSCoW vs autres méthodes

| Méthode | Usage | Avantages MoSCoW |
|---------|-------|------------------|
| **RICE** | Scoring (Reach, Impact, Confidence, Effort) | Plus simple, moins analytique |
| **Kano** | Satisfaction client | Plus direct, catégories claires |
| **ICE** | Impact, Confidence, Ease | Mieux pour débutants |
| **Weighted Shortest Job First** | Agile SAFe | Plus accessible, moins de calculs |

## 8. Résumé de l'essentiel

### Points clés à retenir

1. **MoSCoW = 4 catégories simples**
   - Must have : Essentiel à la réussite
   - Should have : Important mais contournable
   - Could have : Bonus si temps permet
   - Won't have : Pas maintenant, peut-être plus tard

2. **Règle du 60/20/20**
   - 60% Must have
   - 20% Should have
   - 20% Could have
   - Won't have sans limite

3. **Questions de validation**
   - Le produit fonctionne sans ? → Should/Could
   - Obligation légale ? → Must
   - Alternative viable ? → Should
   - Peu de valeur ? → Could ou Won't

4. **Avantages clés**
   - Clarté des priorités
   - Gestion des attentes
   - Focus sur l'essentiel
   - Décisions rapides en cas de contrainte

### Processus décisionnel rapide

```
Pour chaque feature, demande-toi:

1. Est-ce obligatoire légalement ?
   OUI → Must have
   NON → Question 2

2. Le produit est-il inutilisable sans ?
   OUI → Must have
   NON → Question 3

3. Y a-t-il une alternative viable ?
   NON → Must have
   OUI → Question 4

4. Impact significatif sur l'expérience ?
   OUI → Should have
   NON → Question 5

5. Valeur > Effort ?
   OUI → Could have
   NON → Won't have
```

### Checklist priorisation

**Avant de finaliser :**
- [ ] Toutes les features classées M, S, C ou W
- [ ] Proportions respectées (60/20/20)
- [ ] Chaque Must validé avec critères objectifs
- [ ] Won't have communiqués et documentés
- [ ] Consensus équipe obtenu
- [ ] Product Owner a validé
- [ ] Backlog mis à jour

### Template décision

```
Feature: [Nom]

Catégorie MoSCoW proposée: [M/S/C/W]

Critères de validation:
□ Obligation légale: [Oui/Non]
□ Produit inutilisable sans: [Oui/Non]
□ Alternative viable: [Oui/Non]
□ Valeur business (1-5): [X]
□ Effort technique (1-5): [X]

Justification:
[Explication de la catégorisation]

Décision finale: [M/S/C/W]
Validé par: [Product Owner]
Date: [JJ/MM/AAAA]
```

### Communication avec les stakeholders

**Script pour gérer les attentes :**
```
"Nous avons classé cette fonctionnalité en [Catégorie]
parce que [Justification basée sur critères].

Cela signifie que:
- Must: Dans le MVP, livraison prioritaire
- Should: Version 1.1, après le MVP
- Could: Si temps le permet
- Won't: Pas prévu pour l'instant, réévaluation future

Cette décision peut être révisée si [critères changent]."
```

---

**En une phrase :**

> MoSCoW est une technique de priorisation simple et efficace qui classe les fonctionnalités en Must have (60%, essentiel), Should have (20%, important), Could have (20%, bonus), et Won't have (exclus), permettant de livrer un MVP rapidement en se concentrant sur ce qui apporte vraiment de la valeur tout en gérant les attentes des parties prenantes.

**Pour être employable :**

**Tu DOIS savoir :**
- ✅ Expliquer les 4 catégories MoSCoW
- ✅ Appliquer la règle 60/20/20
- ✅ Classifier correctement une fonctionnalité
- ✅ Argumenter pourquoi quelque chose est Must vs Should
- ✅ Gérer les désaccords de priorisation
- ✅ Utiliser MoSCoW dans le Product Backlog

**Vocabulaire à maîtriser absolument :**
MoSCoW, Must have, Should have, Could have, Won't have, MVP (Minimum Viable Product), Scope creep, Priorisation, Règle 60/20/20, Valeur vs Effort.
