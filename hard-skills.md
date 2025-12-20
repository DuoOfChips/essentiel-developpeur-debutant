# Parcours Hard Skills — Développeur Full Stack & Conception Logicielle

Cours orienté sur les notions de bases, les bonnes pratiques et l'écosystème Web (Angular 20+ et nestJS)

## Phase 0 — Préparer le terrain
- [ ] Comprendre ce qu'est un programme
- [ ] Installer et utiliser correctement l'environnement de développement

### Notions
- [ ] [Programme : code → compilation/transpilation → exécution](./courses/programme-compilation-execution.md)
- [ ] [Node.js + NPM](./courses/nodejs-npm.md)
- [ ] [VSCode : debugging, extensions essentielles](./courses/vscode-debugging-extensions.md)

### Ressources
- [ ] https://www.youtube.com/watch?v=ENrzD9HAZK4 (Node.js expliqué)
- [ ] https://www.youtube.com/watch?v=VqCgcpAypFQ (VSCode)

---

## Phase 1 — Bases de la Programmation
Objectif : logique, structures, fonctions, erreurs.

### Notions (ordre)
- [ ] [Variables et types primitifs](./courses/variables-et-types-primitifs.md)
- [ ] [Opérateurs](./courses/operateurs.md) (arithmétiques, comparaison, logique)
- [ ] [Conditions](./courses/conditions.md) (`if`, `else`, `switch`, ternaire)
- [ ] [Boucles](./courses/boucles.md) (`for`, `while`, `for...of`, `.map/.filter/.reduce`)
- [ ] [Fonctions](./courses/fonctions.md) (paramètres, retour, pure vs impure)
- [ ] [Gestion des erreurs et lecture des messages](./courses/gestion-erreurs.md) (+ méthode d'analyse)
- [ ] [Git & GitHub](./courses/git-github.md)

### Mini-Projets
- [ ] Calculatrice CLI
- [ ] Convertisseur d'unités
- [ ] Générateur de menus aléatoires

### Ressources
- [ ] https://www.youtube.com/watch?v=W6NZfCO5SIk (bases JS)
- [ ] https://www.youtube.com/watch?v=s9wW2PpJsmQ (boucles)
- [ ] https://www.youtube.com/watch?v=PkZNo7MFNFg&t=330s (fonctions)

---

## Phase 1.5 — Web et Réseau (Fondamentaux)
Objectif : Comprendre comment le web fonctionne.

### Notions
- [ ] [HTTP et APIs : Requêtes, Réponses, Status Codes](./courses/http-api-fondamentaux.md)
- [ ] [JSON](./courses/JSON.md) : Format d'échange de données
- [ ] [Client-Serveur : Architecture web](./courses/client-serveur-architecture.md)
- [ ] [CORS : Cross-Origin Resource Sharing](./courses/cors.md)

---

## Phase 2 — TypeScript Propre
Objectif : typage strict + structuration modulaire.

### Notions
- [ ] [Types primitifs vs composés](./courses/types-primitifs-vs-composes.md)
- [ ] [`type` vs `interface` (quand utiliser quoi)](./courses/type-vs-interface.md)
- [ ] [Objets + immutabilité (spread, référence vs valeur)](./courses/objets-immutabilite.md)
- [ ] [Tableaux + méthodes fonctionnelles (`map`, `filter`, `reduce`)](./courses/tableaux-methodes-fonctionnelles.md)
- [ ] [`enum` / `const enum`](./courses/enum-const-enum.md)
- [ ] [Fonctions](./courses/functions.md) → signatures typées
- [ ] [Gestion des erreurs](./courses/gestion-erreurs.md) (`try/catch`, erreurs custom)
- [ ] [Imports/Exports organisés](./courses/imports-exports.md)

### Mini-Projet
- [ ] Todolist typée + refactor fonctionnel (`map/filter/reduce`)

### Ressources
- [ ] https://www.youtube.com/watch?v=30LWjhZzg50 (TS course)
- [ ] https://www.youtube.com/c/mattpocockuk/videos (TS clean practices)

---

## Phase 3 — Clean Code + OOP + SOLID
Objectif : écrire du code **maintenable, lisible, modulaire**.

### Notions Clean Code
- [ ] [Nommage clair, fonctions courtes, DRY, KISS, YAGNI](./courses/clean-code-fondamentaux.md)
- [ ] [Refactoring sans casser : techniques et bonnes pratiques](./courses/refactoring-sans-casser.md)

### Notions OOP
- [ ] [Classes, objets, méthodes : fondamentaux de la POO](./courses/poo-classes-objets.md)
- [ ] [Encapsulation : protéger et contrôler l'accès aux données](./courses/encapsulation.md)
- [ ] [**Composition > Héritage** : pourquoi préférer la composition](./courses/composition-vs-heritage.md)
- [ ] [Interfaces vs classes abstraites : quand utiliser quoi](./courses/interfaces-vs-classes-abstraites.md)

### Architecture logicielle
- [ ] [Architecture Logicelle](https://roadmap.sh/software-design-architecture)

### SOLID - Les 5 principes de conception
- [ ] [SOLID : tous les principes (S.O.L.I.D)](./courses/solid-principes.md)
  - S : Single Responsibility Principle
  - O : Open/Closed Principle
  - L : Liskov Substitution Principle
  - I : Interface Segregation Principle
  - D : Dependency Inversion Principle

### Mini-Projets
- [ ] Panier e-commerce (OOP + responsabilité)
- [ ] Service de notifications UI (Pattern Builder)

### Ressources
- [ ] https://www.youtube.com/watch?v=pTB30aXS77U (SOLID)
- [ ] https://www.youtube.com/watch?v=7EmboKQH8lM (Clean Code concret)

---

## Phase 4 — Design Patterns
Objectif : éviter de réinventer la roue et structurer le code.

### Patterns triés par fréquence d'utilisation (webapp métier)

#### Patterns Créationnels (Creational Patterns)
| Priorité | Pattern | Usage typique webapp métier | Cours |
|----------|---------|----------------------------|-------|
| ⭐⭐⭐ | Singleton | Services Angular, connexions DB, configurations | [Design Pattern : Singleton](./courses/design-pattern-singleton.md) |
| ⭐⭐⭐ | Factory | Créer services API selon environnement, factories de composants | [Design Pattern : Factory](./courses/design-pattern-factory.md) |
| ⭐⭐⭐ | Builder | Configurer snackbars / modals / formulaires / requêtes HTTP | [Design Pattern : Builder](./courses/design-pattern-builder.md) |
| ⭐⭐ | Prototype | Cloner objets complexes, templates de documents | [Design Pattern : Prototype](./courses/design-pattern-prototype.md) |
| ⭐ | Abstract Factory | Créer familles d'objets (ex: thèmes UI) | [Design Pattern : Abstract-Factory](./courses/design-pattern-abstract-factory.md) |

#### Patterns Structurels (Structural Patterns)
| Priorité | Pattern | Usage typique webapp métier | Cours |
|----------|---------|----------------------------|-------|
| ⭐⭐⭐ | Adapter | Uniformiser API externes, adapters pour librairies tierces | [Design Pattern : Adapter](./courses/design-pattern-adapter.md) |
| ⭐⭐⭐ | Decorator | Interceptors Angular/NestJS, logs, auth, caching | [Design Pattern : Decorator](./courses/design-pattern-decorator.md) |
| ⭐⭐⭐ | Facade | Simplifier API complexes, wrappers de services | [Design Pattern : Facade](./courses/design-pattern-facade.md) |
| ⭐⭐⭐ | Proxy | Lazy loading, cache, guards Angular | [Design Pattern : Proxy](./courses/design-pattern-proxy.md) |
| ⭐⭐ | Composite | Arbres de composants UI, menus hiérarchiques | [Design Pattern : Composite](./courses/design-pattern-composite.md) |
| ⭐⭐ | Bridge | Séparer abstraction/implémentation (ex: multi-platforme) | [Design Pattern : Bridge](./courses/design-pattern-bridge.md) |
| ⭐ | Flyweight | Optimiser mémoire pour grandes listes de données | [Design Pattern : Flyweight](./courses/design-pattern-flyweight.md) |

#### Patterns Comportementaux (Behavioral Patterns)
| Priorité | Pattern | Usage typique webapp métier | Cours |
|----------|---------|----------------------------|-------|
| ⭐⭐⭐ | Observer | Streaming d'événements UI / RxJS / WebSockets | [Design Pattern : Observer](./courses/design-pattern-observer.md) |
| ⭐⭐⭐ | Strategy | Remplacer `if/else` multiples (calcul TVA, tri, validation) | [Design Pattern : Strategy](./courses/design-pattern-strategy.md) |
| ⭐⭐⭐ | Command | Actions undo/redo, queues de tâches, event sourcing | [Design Pattern : Command](./courses/design-pattern-command.md) |
| ⭐⭐⭐ | Template Method | Workflows métier, pipelines de traitement | [Design Pattern : Template-Method](./courses/design-pattern-template-method.md) |
| ⭐⭐⭐ | Chain of Responsibility | Middleware, validation en chaîne, error handling | [Design Pattern : Chain-of-Responsibility](./courses/design-pattern-chain-of-responsibility.md) |
| ⭐⭐ | State | Gestion états formulaire, workflow documents | [Design Pattern : State](./courses/design-pattern-state.md) |
| ⭐⭐ | Mediator | Communication entre composants, event bus | [Design Pattern : Mediator](./courses/design-pattern-mediator.md) |
| ⭐⭐ | Iterator | Parcourir collections, pagination | [Design Pattern : Iterator](./courses/design-pattern-iterator.md) |
| ⭐ | Memento | Historique, snapshots d'état, undo/redo | [Design Pattern : Memento](./courses/design-pattern-memento.md) |
| ⭐ | Visitor | Opérations sur structures complexes (AST, DOM) | [Design Pattern : Visitor](./courses/design-pattern-visitor.md) |
| ⭐ | Interpreter | Parsers, DSL, règles métier complexes | [Design Pattern : Interpreter](./courses/design-pattern-interpreter.md) |

#### Pattern Spécifique Web
| Priorité | Pattern | Usage typique webapp métier | Cours |
|----------|---------|----------------------------|-------|
| ⭐⭐⭐ | DTO (Data Transfer Object) | Contrats Front ↔ Back, API REST | [Design Pattern : DTO](./courses/design-pattern-dto.md) |

### Légende priorités
- ⭐⭐⭐ : **Essentiel** - Utilisé quotidiennement dans les webapps métier
- ⭐⭐ : **Important** - Utilisé régulièrement pour des cas spécifiques
- ⭐ : **Occasionnel** - Utilisé pour des besoins avancés ou spécifiques

### Ressources
- [ ] https://refactoring.guru/fr/design-patterns (Catalogue complet en français)
- [ ] https://www.youtube.com/watch?v=tv-_1er1mWI (Design Patterns en TypeScript)
- [ ] https://sbcode.net/typescript/design_patterns/ (TypeScript Design Patterns)
- [ ] https://angular.io/guide/dependency-injection (DI et patterns Angular)

---

## Phase 5 — Architecture Web
Objectif : full-stack Angular + NestJS structuré.

### Angular
- [ ] Modules, Components, Services
- [ ] Inputs/Outputs
- [ ] Dependency Injection
- [ ] RxJS (progressif : map → switchMap → debounceTime)
- [ ] State minimal (signals au début)

### NestJS
- [ ] Modules, Controllers, Providers
- [ ] DTO + Validation
- [ ] Repositories + Database
- [ ] Auth JWT
- [ ] Guards & Interceptors

### Ressources
- [ ] Angular : https://www.youtube.com/watch?v=3qBXWUpoPHo
- [ ] NestJS : https://www.youtube.com/watch?v=F_oOtaxb0L8

---

## Phase 6 — Enterprise Integration Patterns
Objectif : systèmes modulaires et résilients.

### EIP utiles
- [ ] Event-driven architecture
- [ ] Message Queue (RabbitMQ / Kafka)
- [ ] Dead Letter Queue

### Ressource
- [ ] https://www.youtube.com/watch?v=deG25y_r6OY

---

## Phase 7 - Cloud
- [ ] [Roadmap google cloud](https://roadmap.sh/ai/roadmap/google-cloud-platform-mastery)
