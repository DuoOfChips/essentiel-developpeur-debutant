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
Objectif : full-stack Angular 20+ et NestJS 11 structuré et professionnel.

### 5.1 — Angular 20+ : Fondamentaux

#### Démarrage et bases
- [ ] [Angular : Installation et premier projet](./courses/angular/angular-installation-premier-projet.md)
- [ ] [Composants : Bases et création](./courses/angular/angular-composants-bases.md)
- [ ] [Templates : Syntaxe et data binding](./courses/angular/angular-templates-data-binding.md)
- [ ] [Directives structurelles : *ngIf, *ngFor, *ngSwitch](./courses/angular/angular-directives-structurelles.md)
- [ ] [Directives d'attribut : ngClass, ngStyle](./courses/angular/angular-directives-attribut.md)

#### État et Réactivité
- [ ] [Signals : Nouvelle gestion d'état réactive](./courses/angular/angular-signals.md)
- [ ] [Property Binding et Event Binding](./courses/angular/angular-property-event-binding.md)
- [ ] [Two-way binding avec ngModel](./courses/angular/angular-two-way-binding.md)
- [ ] [Communication parent-enfant : @Input](./courses/angular/angular-input.md)
- [ ] [Communication enfant-parent : @Output et EventEmitter](./courses/angular/angular-output.md)
- [ ] [Lifecycle hooks : ngOnInit, ngOnChanges, ngOnDestroy](./courses/angular/angular-lifecycle-hooks.md)

#### Services et Injection de Dépendances
- [ ] [Services : Création et utilisation](./courses/angular/angular-services.md)
- [ ] [Dependency Injection : Principes et providedIn](./courses/angular/angular-dependency-injection.md)
- [ ] [Injection de dépendances avancée : tokens et factories](./courses/angular/angular-di-avancee.md)

#### Formulaires
- [ ] [Formulaires template-driven](./courses/angular/angular-forms-template-driven.md)
- [ ] [Formulaires réactifs : FormControl et FormGroup](./courses/angular/angular-forms-reactive.md)
- [ ] [Validation de formulaires](./courses/angular/angular-forms-validation.md)
- [ ] [Formulaires dynamiques](./courses/angular/angular-forms-dynamiques.md)

#### Routing
- [ ] [Routing : Configuration de base](./courses/angular/angular-routing-bases.md)
- [ ] [Route parameters et query params](./courses/angular/angular-routing-params.md)
- [ ] [Child routes et routing imbriqué](./courses/angular/angular-routing-child.md)
- [ ] [Lazy loading : Modules différés](./courses/angular/angular-lazy-loading.md)
- [ ] [Guards : CanActivate, CanDeactivate](./courses/angular/angular-guards.md)
- [ ] [Resolvers : Pré-chargement de données](./courses/angular/angular-resolvers.md)

### 5.2 — Angular 20+ : Intermédiaire

#### HTTP et APIs
- [ ] [HttpClient : Requêtes GET et POST](./courses/angular/angular-http-client-bases.md)
- [ ] [Gestion des erreurs HTTP](./courses/angular/angular-http-error-handling.md)
- [ ] [Interceptors : Authentification et logging](./courses/angular/angular-interceptors.md)
- [ ] [Optimistic updates et cache](./courses/angular/angular-http-cache.md)

#### RxJS et Programmation Réactive
- [ ] [RxJS : Introduction aux Observables](./courses/angular/angular-rxjs-observables.md)
- [ ] [Opérateurs de base : map, filter, tap](./courses/angular/angular-rxjs-operators-base.md)
- [ ] [Opérateurs de combinaison : merge, concat, forkJoin](./courses/angular/angular-rxjs-combinaison.md)
- [ ] [Opérateurs de transformation : switchMap, mergeMap, concatMap](./courses/angular/angular-rxjs-transformation.md)
- [ ] [Gestion du temps : debounceTime, throttleTime, delay](./courses/angular/angular-rxjs-temps.md)
- [ ] [Subjects : BehaviorSubject, ReplaySubject](./courses/angular/angular-rxjs-subjects.md)
- [ ] [Gestion de souscriptions et memory leaks](./courses/angular/angular-rxjs-subscriptions.md)

#### Composants avancés
- [ ] [Pipes : Built-in et pipes personnalisés](./courses/angular/angular-pipes.md)
- [ ] [Content projection : ng-content](./courses/angular/angular-content-projection.md)
- [ ] [ViewChild et ViewChildren](./courses/angular/angular-viewchild.md)
- [ ] [Standalone components : Architecture moderne](./courses/angular/angular-standalone-components.md)
- [ ] [Change detection : Stratégies OnPush](./courses/angular/angular-change-detection.md)

### 5.3 — Angular 20+ : Avancé

#### Architecture et Patterns
- [ ] [Architecture en modules : Feature modules](./courses/angular/angular-feature-modules.md)
- [ ] [Shared modules et Core modules](./courses/angular/angular-shared-core-modules.md)
- [ ] [State management : Signals avancés](./courses/angular/angular-state-management-signals.md)
- [ ] [State management : NgRx introduction](./courses/angular/angular-ngrx-intro.md)
- [ ] [Pattern Container/Presentational](./courses/angular/angular-container-presentational.md)

#### Testing
- [ ] [Tests unitaires : Services avec Jasmine](./courses/angular/angular-tests-unitaires-services.md)
- [ ] [Tests de composants : TestBed et fixtures](./courses/angular/angular-tests-composants.md)
- [ ] [Tests d'intégration](./courses/angular/angular-tests-integration.md)
- [ ] [E2E testing avec Cypress](./courses/angular/angular-tests-e2e.md)

#### Performance et Optimisation
- [ ] [Performance : Lazy loading avancé](./courses/angular/angular-perf-lazy-loading.md)
- [ ] [Performance : Tree shaking et bundling](./courses/angular/angular-perf-bundling.md)
- [ ] [Performance : Détection de changements optimisée](./courses/angular/angular-perf-change-detection.md)
- [ ] [PWA : Progressive Web Apps avec Angular](./courses/angular/angular-pwa.md)

### 5.4 — NestJS 11 : Fondamentaux

#### Démarrage et Architecture
- [ ] [NestJS : Installation et premier projet](./courses/nestjs/nestjs-installation-premier-projet.md)
- [ ] [Architecture : Modules, Controllers, Providers](./courses/nestjs/nestjs-architecture-mvc.md)
- [ ] [Modules : Organisation et imports](./courses/nestjs/nestjs-modules.md)
- [ ] [Controllers : Routes et décorateurs](./courses/nestjs/nestjs-controllers.md)
- [ ] [Providers et Services](./courses/nestjs/nestjs-providers-services.md)

#### Injection de Dépendances
- [ ] [Dependency Injection : Principes](./courses/nestjs/nestjs-dependency-injection.md)
- [ ] [Scopes : Default, Request, Transient](./courses/nestjs/nestjs-di-scopes.md)
- [ ] [Custom providers et factories](./courses/nestjs/nestjs-custom-providers.md)

#### Requêtes et Réponses
- [ ] [DTOs : Data Transfer Objects](./courses/nestjs/nestjs-dto.md)
- [ ] [Validation avec class-validator](./courses/nestjs/nestjs-validation.md)
- [ ] [Transformation avec class-transformer](./courses/nestjs/nestjs-transformation.md)
- [ ] [Query parameters, route params, body](./courses/nestjs/nestjs-request-params.md)
- [ ] [Gestion des réponses HTTP](./courses/nestjs/nestjs-response-handling.md)

### 5.5 — NestJS 11 : Intermédiaire

#### Middleware et Intercepteurs
- [ ] [Middleware : Création et utilisation](./courses/nestjs/nestjs-middleware.md)
- [ ] [Pipes : Validation et transformation](./courses/nestjs/nestjs-pipes.md)
- [ ] [Guards : Protection de routes](./courses/nestjs/nestjs-guards.md)
- [ ] [Interceptors : Transformation et logging](./courses/nestjs/nestjs-interceptors.md)
- [ ] [Exception filters : Gestion d'erreurs](./courses/nestjs/nestjs-exception-filters.md)

#### Base de Données
- [ ] [TypeORM : Installation et configuration](./courses/nestjs/nestjs-typeorm-setup.md)
- [ ] [Entities : Définition et décorateurs](./courses/nestjs/nestjs-typeorm-entities.md)
- [ ] [Repository pattern](./courses/nestjs/nestjs-repository-pattern.md)
- [ ] [Relations : OneToMany, ManyToOne, ManyToMany](./courses/nestjs/nestjs-typeorm-relations.md)
- [ ] [Migrations : Création et gestion](./courses/nestjs/nestjs-typeorm-migrations.md)
- [ ] [Query Builder : Requêtes complexes](./courses/nestjs/nestjs-typeorm-query-builder.md)
- [ ] [Transactions](./courses/nestjs/nestjs-typeorm-transactions.md)

#### Authentification et Sécurité
- [ ] [Authentication : Stratégies et Passport](./courses/nestjs/nestjs-auth-bases.md)
- [ ] [JWT : JSON Web Tokens](./courses/nestjs/nestjs-jwt.md)
- [ ] [Hash de mots de passe avec bcrypt](./courses/nestjs/nestjs-password-hashing.md)
- [ ] [Authorization : Rôles et permissions](./courses/nestjs/nestjs-authorization-roles.md)
- [ ] [Guards d'authentification personnalisés](./courses/nestjs/nestjs-auth-guards.md)
- [ ] [Refresh tokens](./courses/nestjs/nestjs-refresh-tokens.md)

### 5.6 — NestJS 11 : Avancé

#### Features Avancées
- [ ] [Upload de fichiers](./courses/nestjs/nestjs-file-upload.md)
- [ ] [Configuration : ConfigModule et variables d'environnement](./courses/nestjs/nestjs-configuration.md)
- [ ] [Logging : Logger personnalisé](./courses/nestjs/nestjs-logging.md)
- [ ] [Validation globale et pipes personnalisés](./courses/nestjs/nestjs-validation-globale.md)
- [ ] [Serialization : Exposer/Exclure des propriétés](./courses/nestjs/nestjs-serialization.md)

#### API Documentation et Communication
- [ ] [Swagger : Documentation API automatique](./courses/nestjs/nestjs-swagger.md)
- [ ] [Versioning d'API](./courses/nestjs/nestjs-api-versioning.md)
- [ ] [WebSockets : Real-time avec Socket.io](./courses/nestjs/nestjs-websockets.md)
- [ ] [GraphQL : Introduction](./courses/nestjs/nestjs-graphql-intro.md)

#### Testing
- [ ] [Tests unitaires : Services et controllers](./courses/nestjs/nestjs-tests-unitaires.md)
- [ ] [Tests d'intégration](./courses/nestjs/nestjs-tests-integration.md)
- [ ] [Tests E2E](./courses/nestjs/nestjs-tests-e2e.md)
- [ ] [Mocking et stubs](./courses/nestjs/nestjs-tests-mocking.md)

#### Performance et Microservices
- [ ] [Caching avec Redis](./courses/nestjs/nestjs-caching-redis.md)
- [ ] [Rate limiting](./courses/nestjs/nestjs-rate-limiting.md)
- [ ] [Compression et optimisation](./courses/nestjs/nestjs-performance-optimization.md)
- [ ] [Microservices : Introduction et patterns](./courses/nestjs/nestjs-microservices-intro.md)
- [ ] [Message brokers : RabbitMQ/Kafka](./courses/nestjs/nestjs-message-brokers.md)

### 5.7 — Projet Full-Stack Intégré
- [ ] [Architecture Full-Stack : Angular + NestJS](./courses/fullstack/fullstack-architecture.md)
- [ ] [Communication Front-Back : Best practices](./courses/fullstack/fullstack-communication.md)
- [ ] [Authentification Full-Stack](./courses/fullstack/fullstack-authentication.md)
- [ ] [Gestion d'état côté client et serveur](./courses/fullstack/fullstack-state-management.md)
- [ ] [Déploiement et CI/CD](./courses/fullstack/fullstack-deployment.md)

### Ressources générales
- [ ] Angular : https://www.youtube.com/watch?v=3qBXWUpoPHo
- [ ] NestJS : https://www.youtube.com/watch?v=F_oOtaxb0L8
- [ ] Documentation officielle Angular : https://angular.dev
- [ ] Documentation officielle NestJS : https://docs.nestjs.com
- [ ] RxJS : https://rxjs.dev
- [ ] TypeORM : https://typeorm.io

---

## Phase 6 — Enterprise Integration Patterns
Objectif : systèmes modulaires et résilients pour applications métier.

### Patterns Essentiels (Haute fréquence dans webapps métier)

#### 1. Messaging Channels
- [ ] [Point-to-Point Channel](./courses/eip/point-to-point-channel.md)
- [ ] [Publish-Subscribe Channel](./courses/eip/publish-subscribe-channel.md)
- [ ] [Dead Letter Channel](./courses/eip/dead-letter-channel.md)
- [ ] [Guaranteed Delivery](./courses/eip/guaranteed-delivery.md)
- [ ] [Message Bus](./courses/eip/message-bus.md)

#### 2. Message Construction
- [ ] [Command Message](./courses/eip/command-message.md)
- [ ] [Event Message](./courses/eip/event-message.md)
- [ ] [Request-Reply](./courses/eip/request-reply.md)
- [ ] [Correlation Identifier](./courses/eip/correlation-identifier.md)
- [ ] [Return Address](./courses/eip/return-address.md)
- [ ] [Message Expiration](./courses/eip/message-expiration.md)

#### 3. Message Routing
- [ ] [Content-Based Router](./courses/eip/content-based-router.md)
- [ ] [Message Filter](./courses/eip/message-filter.md)
- [ ] [Dynamic Router](./courses/eip/dynamic-router.md)
- [ ] [Recipient List](./courses/eip/recipient-list.md)
- [ ] [Splitter](./courses/eip/splitter.md)
- [ ] [Aggregator](./courses/eip/aggregator.md)
- [ ] [Resequencer](./courses/eip/resequencer.md)
- [ ] [Routing Slip](./courses/eip/routing-slip.md)

#### 4. Message Transformation
- [ ] [Message Translator](./courses/eip/message-translator.md)
- [ ] [Envelope Wrapper](./courses/eip/envelope-wrapper.md)
- [ ] [Content Enricher](./courses/eip/content-enricher.md)
- [ ] [Content Filter](./courses/eip/content-filter.md)
- [ ] [Claim Check](./courses/eip/claim-check.md)
- [ ] [Normalizer](./courses/eip/normalizer.md)
- [ ] [Canonical Data Model](./courses/eip/canonical-data-model.md)

#### 5. Messaging Endpoints
- [ ] [Polling Consumer](./courses/eip/polling-consumer.md)
- [ ] [Event-Driven Consumer](./courses/eip/event-driven-consumer.md)
- [ ] [Competing Consumers](./courses/eip/competing-consumers.md)
- [ ] [Message Dispatcher](./courses/eip/message-dispatcher.md)
- [ ] [Idempotent Receiver](./courses/eip/idempotent-receiver.md)
- [ ] [Service Activator](./courses/eip/service-activator.md)

#### 6. System Management & Monitoring
- [ ] [Control Bus](./courses/eip/control-bus.md)
- [ ] [Detour](./courses/eip/detour.md)
- [ ] [Wire Tap](./courses/eip/wire-tap.md)
- [ ] [Message History](./courses/eip/message-history.md)
- [ ] [Message Store](./courses/eip/message-store.md)
- [ ] [Smart Proxy](./courses/eip/smart-proxy.md)

### Patterns Avancés (Moyenne fréquence)

#### 7. Messaging Systems
- [ ] [Channel Adapter](./courses/eip/channel-adapter.md)
- [ ] [Messaging Bridge](./courses/eip/messaging-bridge.md)
- [ ] [Messaging Gateway](./courses/eip/messaging-gateway.md)
- [ ] [Messaging Mapper](./courses/eip/messaging-mapper.md)
- [ ] [Transactional Client](./courses/eip/transactional-client.md)

#### 8. Advanced Routing
- [ ] [Process Manager](./courses/eip/process-manager.md)
- [ ] [Scatter-Gather](./courses/eip/scatter-gather.md)
- [ ] [Composed Message Processor](./courses/eip/composed-message-processor.md)

#### 9. Performance & Reliability
- [ ] [Datatype Channel](./courses/eip/datatype-channel.md)
- [ ] [Invalid Message Channel](./courses/eip/invalid-message-channel.md)
- [ ] [Channel Purger](./courses/eip/channel-purger.md)
- [ ] [Durable Subscriber](./courses/eip/durable-subscriber.md)

### Technologies & Outils
- [ ] RabbitMQ
- [ ] Apache Kafka
- [ ] Redis Pub/Sub
- [ ] NestJS Microservices
- [ ] Azure Service Bus
- [ ] AWS SQS/SNS

### Ressources
- [ ] https://www.youtube.com/watch?v=deG25y_r6OY (Event-Driven Architecture)
- [ ] https://www.enterpriseintegrationpatterns.com/ (Référence officielle)
- [ ] https://docs.nestjs.com/microservices/basics (NestJS Microservices)
- [ ] https://www.rabbitmq.com/getstarted.html (RabbitMQ Tutorial)

---

## Phase 7 - Cloud
- [ ] [Roadmap google cloud](https://roadmap.sh/ai/roadmap/google-cloud-platform-mastery)
