# Parcours Hard Skills — Développeur Full Stack & Conception Logicielle

Cours orienté sur les notions de bases, les bonnes pratiques et l'écosystème Web (Angular 20+ et nestJS)

## Phase 0 — Préparer le terrain
- [ ] Comprendre ce qu’est un programme
- [ ] Installer et utiliser correctement l’environnement de développement

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
- [ ] [Gestion des erreurs et lecture des messages](./courses/gestion-erreurs.md) (+ méthode d’analyse)
- [ ] [Git & GitHub](./courses/git-github.md)

### Mini-Projets
- [ ] Calculatrice CLI
- [ ] Convertisseur d’unités
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
- [ ] Une seule responsabilité (SRP)
- [ ] Refactoring sans casser
- [ ] Éviter la duplication

### Notions OOP
- [ ] Classes, objets, méthodes
- [ ] Encapsulation
- [ ] **Composition > Héritage**
- [ ] Interfaces vs classes abstraites

### Architecture logicielle
- [ ] [Architecture Logicelle](https://roadmap.sh/software-design-architecture)

### SOLID
- [ ] S : Single Responsibility
- [ ] O : Open/Closed
- [ ] L : Liskov Substitution
- [ ] I : Interface Segregation
- [ ] D : Dependency Inversion (injection au lieu d’instancier)

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
| Observer | streaming d’événements UI / RxJS |
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
