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
- [ ] Client-Serveur : Architecture web
- [ ] CORS : Cross-Origin Resource Sharing

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

### Patterns prioritaires (avec usages concrets web)
| Pattern | Usage typique |
|---|---|
| Builder | configurer snackbars / modals / formulaires |
| Factory | créer services API selon environnement |
| Strategy | remplacer `if/else` multiples (ex: calcul TVA) |
| Adapter | uniformiser API externes |
| Observer | streaming d'événements UI / RxJS |
| DTO | contrats Front ↔ Back |

### Ressource
- [ ] https://refactoring.guru/fr/design-patterns

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
