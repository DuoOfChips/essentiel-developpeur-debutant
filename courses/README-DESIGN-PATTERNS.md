# Design Patterns - Guide Complet

Ce dossier contient un catalogue complet des design patterns pour le développement web moderne, spécialisé dans les applications web métier avec TypeScript, Angular 20+ et NestJS.

## 📋 Vue d'ensemble

Les design patterns sont des solutions éprouvées à des problèmes récurrents en conception logicielle. Ce curriculum couvre **24 patterns** organisés par fréquence d'utilisation dans les webapp métier.

## 🎯 Patterns Essentiels (⭐⭐⭐) - Utilisation Quotidienne

Ces 13 patterns sont **indispensables** pour tout développeur web professionnel. Ils sont utilisés quotidiennement dans les projets Angular/NestJS.

### Patterns Créationnels (3)

| Pattern | Usage Principal | Cours |
|---------|----------------|-------|
| **Singleton** | Services Angular/NestJS, connexions DB, configuration globale | [→ Cours](./design-pattern-singleton.md) |
| **Factory** | Création de services selon l'environnement (dev/prod), factories de composants | [→ Cours](./design-pattern-factory.md) |
| **Builder** | Configuration d'objets complexes (modals, formulaires, requêtes HTTP) | [→ Cours](./design-pattern-builder.md) |

### Patterns Structurels (4)

| Pattern | Usage Principal | Cours |
|---------|----------------|-------|
| **Adapter** | Uniformisation d'API externes, wrappers de librairies tierces | [→ Cours](./design-pattern-adapter.md) |
| **Decorator** | Interceptors Angular/NestJS, Guards, Pipes, middleware | [→ Cours](./design-pattern-decorator.md) |
| **Facade** | Simplification de systèmes complexes, agrégation de services | [→ Cours](./design-pattern-facade.md) |
| **Proxy** | Lazy loading, caching, Guards Angular, contrôle d'accès | [→ Cours](./design-pattern-proxy.md) |

### Patterns Comportementaux (5)

| Pattern | Usage Principal | Cours |
|---------|----------------|-------|
| **Observer** | RxJS Observables, streaming d'événements, WebSockets | [→ Cours](./design-pattern-observer.md) |
| **Strategy** | Remplacement de if/else multiples (calcul TVA, tri, validation) | [→ Cours](./design-pattern-strategy.md) |
| **Command** | Undo/redo, queues de tâches, event sourcing | [→ Cours](./design-pattern-command.md) |
| **Template Method** | Workflows métier, pipelines de traitement | [→ Cours](./design-pattern-template-method.md) |
| **Chain of Responsibility** | Middleware, validation en chaîne, gestion d'erreurs | [→ Cours](./design-pattern-chain-of-responsibility.md) |

### Pattern Spécifique Web (1)

| Pattern | Usage Principal | Cours |
|---------|----------------|-------|
| **DTO** | Contrats de données Front ↔ Back, APIs REST | [→ Cours](./design-pattern-dto.md) |

---

## 📚 Structure des Cours

Chaque cours suit une structure pédagogique éprouvée :

1. **Introduction**
   - Objectifs d'apprentissage clairs
   - Scope et applications concrètes

2. **Définitions et Concepts**
   - Explication détaillée du pattern
   - Analogies de la vie quotidienne
   - Diagrammes Mermaid

3. **Problèmes et Solutions**
   - Exemples de code SANS le pattern (❌)
   - Solutions AVEC le pattern (✅)
   - Avantages et inconvénients

4. **Implémentations**
   - TypeScript pur
   - Angular (composants, services, guards)
   - NestJS (controllers, services, interceptors)
   - Exemples concrets webapp métier

5. **Erreurs Courantes**
   - Pièges à éviter
   - Anti-patterns
   - Corrections

6. **Exercices Pratiques**
   - Facile : mise en pratique basique
   - Intermédiaire : cas métier réel

7. **Recommandations Senior**
   - Quand utiliser / ne pas utiliser
   - Best practices
   - Astuces de développeurs expérimentés

8. **Résumé et Ressources**
   - Points clés à retenir
   - Ressources externes (français et anglais)

---

## 🚀 Parcours d'Apprentissage Recommandé

### Pour Débutants

**Commencez par ces 5 patterns fondamentaux :**

1. **Singleton** - Comprendre les services Angular
2. **Observer** - Maîtriser RxJS et les Observables
3. **Factory** - Créer des objets intelligemment
4. **Strategy** - Éviter les if/else complexes
5. **DTO** - Structurer les données API

### Pour Développeurs Intermédiaires

**Ajoutez ces patterns pour la production :**

6. **Decorator** - Interceptors et middleware
7. **Facade** - Simplifier la complexité
8. **Proxy** - Optimiser les performances
9. **Builder** - Construire des objets complexes
10. **Adapter** - Intégrer des API tierces

### Pour Développeurs Avancés

**Complétez avec les patterns avancés :**

11. **Command** - Undo/redo et event sourcing
12. **Template Method** - Workflows réutilisables
13. **Chain of Responsibility** - Pipelines de traitement

---

## 💡 Cas d'Usage par Domaine

### E-commerce / Boutique en Ligne

- **Factory** : Créer différents types de produits
- **Strategy** : Calcul de prix avec promotions, TVA
- **Observer** : Mise à jour du panier en temps réel
- **Command** : Historique des commandes, undo
- **Chain of Responsibility** : Validation de commande

### Application de Gestion (CRUD)

- **Singleton** : Services de configuration
- **DTO** : Contrats API pour CRUD
- **Proxy** : Cache des requêtes
- **Decorator** : Logging, auth, validation
- **Facade** : Simplification des opérations CRUD

### Dashboard / Analytics

- **Observer** : Mises à jour temps réel
- **Adapter** : Unifier plusieurs sources de données
- **Facade** : Agrégation de métriques
- **Strategy** : Différents types de graphiques
- **Builder** : Configuration de widgets

### Workflow / Approbation

- **Template Method** : Processus d'approbation
- **Chain of Responsibility** : Chaîne de validation
- **Command** : Actions annulables
- **State** : États de workflow (À venir)

---

## 📖 Ressources Complémentaires

### Générales sur les Design Patterns

- 📚 [Refactoring Guru - Design Patterns](https://refactoring.guru/fr/design-patterns) (français)
- 🎥 [Design Patterns en TypeScript](https://www.youtube.com/watch?v=tv-_1er1mWI) (anglais)
- 📖 [TypeScript Design Patterns](https://sbcode.net/typescript/design_patterns/) (anglais)

### Spécifiques aux Frameworks

- 📚 [Angular - Architecture](https://angular.io/guide/architecture)
- 📚 [NestJS - Fundamentals](https://docs.nestjs.com/fundamentals/custom-providers)
- 📚 [RxJS - Documentation officielle](https://rxjs.dev/)

### Livres Recommandés

- 📖 *Design Patterns: Elements of Reusable Object-Oriented Software* - Gang of Four
- 📖 *Head First Design Patterns* - Eric Freeman, Elisabeth Robson
- 📖 *Patterns of Enterprise Application Architecture* - Martin Fowler

---

## 🎓 Validation des Connaissances

Pour valider votre maîtrise des design patterns :

1. ✅ Compléter tous les exercices des cours ⭐⭐⭐
2. ✅ Implémenter au moins 3 patterns dans un projet réel
3. ✅ Être capable d'expliquer quand utiliser/ne pas utiliser chaque pattern
4. ✅ Reconnaître les patterns dans du code existant
5. ✅ Refactorer du code legacy vers des patterns appropriés

---

## 🔄 Prochaines Étapes

### Patterns à venir (⭐⭐ - Utilisation Régulière)

- **Prototype** - Clonage d'objets complexes
- **Composite** - Structures arborescentes
- **Bridge** - Séparation abstraction/implémentation
- **State** - Gestion d'états de workflows
- **Mediator** - Communication entre composants
- **Iterator** - Parcours de collections

### Patterns avancés (⭐ - Cas Spécifiques)

- **Abstract Factory** - Familles d'objets
- **Flyweight** - Optimisation mémoire
- **Memento** - Snapshots d'état
- **Visitor** - Opérations sur structures
- **Interpreter** - Parsers, DSL

---

## 📞 Support et Contribution

Pour questions ou suggestions :
- Ouvrir une issue sur GitHub
- Contribuer via Pull Request
- Partager vos exemples d'utilisation

---

## 📊 Statistiques

- **Patterns documentés** : 13/24 (54%)
- **Patterns essentiels** : 13/13 (100%) ✅
- **Lignes de cours** : ~12,000+
- **Exemples de code** : 100+
- **Diagrammes** : 13
- **Exercices** : 26+

---

**Bon apprentissage ! 🚀**

> Les design patterns ne sont pas une fin en soi, mais des outils pour résoudre des problèmes récurrents. L'objectif est de savoir quand les utiliser, et surtout quand ne PAS les utiliser (YAGNI - You Aren't Gonna Need It).
