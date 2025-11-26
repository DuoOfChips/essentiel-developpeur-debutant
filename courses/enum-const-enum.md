# Enum et Const Enum en TypeScript

## 1. Contexte et définitions des concepts clés

### 1.1 Qu'est-ce qu'un `enum` ?

Un **enum** (énumération) est un type spécial TypeScript qui permet de définir un ensemble de constantes nommées.

**Syntaxe de base :**
```ts
enum Direction {
  Up,
  Down,
  Left,
  Right
}
```

### 1.2 Types d'enums

**1. Numeric enum (par défaut)**
```ts
enum Status {
  Pending,    // 0
  Approved,   // 1
  Rejected    // 2
}
```

**2. String enum**
```ts
enum Direction {
  Up = "UP",
  Down = "DOWN",
  Left = "LEFT",
  Right = "RIGHT"
}
```

**3. Const enum**
```ts
const enum LogLevel {
  Debug,
  Info,
  Warning,
  Error
}
```

### 1.3 Définitions importantes

- **Enum** : Ensemble de constantes nommées
- **Numeric enum** : Valeurs numériques (0, 1, 2...)
- **String enum** : Valeurs textuelles explicites
- **Const enum** : Enum supprimé à la compilation (inline)
- **Reverse mapping** : Accès par valeur (numeric enums seulement)
- **Heterogeneous enum** : Mix numeric/string (déconseillé)

## 2. Quand et pourquoi utiliser les enums

### 2.1 Quand utiliser un `enum`

**Cas d'usage :**

1. **Ensemble fini de valeurs connues**
```ts
enum UserRole {
  Guest,
  User,
  Admin,
  SuperAdmin
}
```

2. **États d'une machine à états**
```ts
enum OrderStatus {
  Created = "CREATED",
  Paid = "PAID",
  Shipped = "SHIPPED",
  Delivered = "DELIVERED",
  Cancelled = "CANCELLED"
}
```

3. **Configuration et options**
```ts
enum Theme {
  Light = "light",
  Dark = "dark",
  Auto = "auto"
}
```

4. **Codes d'erreur**
```ts
enum ErrorCode {
  NotFound = 404,
  Unauthorized = 401,
  ServerError = 500
}
```

### 2.2 Pourquoi utiliser `enum` ?

**Avantages :**
- ✅ Auto-complétion dans l'IDE
- ✅ Type-safe (erreurs détectées à la compilation)
- ✅ Lisibilité (noms expressifs au lieu de valeurs brutes)
- ✅ Maintenabilité (changement centralisé)
- ✅ Refactoring facile
- ✅ Documentation auto-générée

**Sans enum :**
```ts
// ❌ Magic strings/numbers
function setStatus(status: string) {
  if (status === "approved") { // Typo possible
    // ...
  }
}

setStatus("aprooved"); // ❌ Erreur silencieuse
```

**Avec enum :**
```ts
// ✅ Type-safe
enum Status {
  Pending = "pending",
  Approved = "approved",
  Rejected = "rejected"
}

function setStatus(status: Status) {
  if (status === Status.Approved) {
    // ...
  }
}

setStatus(Status.Approved); // ✅ OK
// setStatus("aprooved");   // ❌ Erreur TypeScript
```

### 2.3 Quand utiliser `const enum` ?

**Utilisez `const enum` pour :**
- Optimiser la taille du bundle JavaScript
- Éliminer le code enum en production
- Applications où la performance est critique

**Attention :**
- Ne peut pas être utilisé pour reverse mapping
- Peut causer des problèmes avec certains bundlers
- Incompatible avec isolatedModules

### 2.4 Alternatives aux enums

**Union types (souvent préféré en TypeScript moderne) :**
```ts
// Alternative à enum
type Status = "pending" | "approved" | "rejected";

const status: Status = "approved"; // ✅ Type-safe
```

**Comparaison enum vs union :**

| Aspect | Enum | Union Type |
|--------|------|------------|
| Type-safe | ✅ | ✅ |
| Auto-complétion | ✅ | ✅ |
| Code JavaScript généré | Oui | Non |
| Reverse mapping | ✅ (numeric) | ❌ |
| Bundle size | Plus gros | Plus petit |
| Valeur à runtime | Objet | Primitif |

## 3. Comment cela se passe du point de vue matériel

### 3.1 Compilation d'un numeric enum

**Code TypeScript :**
```ts
enum Status {
  Pending,
  Approved,
  Rejected
}

const current = Status.Approved;
```

**Code JavaScript généré :**
```js
var Status;
(function (Status) {
  Status[Status["Pending"] = 0] = "Pending";
  Status[Status["Approved"] = 1] = "Approved";
  Status[Status["Rejected"] = 2] = "Rejected";
})(Status || (Status = {}));

const current = Status.Approved; // 1
```

**Objet résultant en mémoire :**
```js
Status = {
  0: "Pending",
  1: "Approved",
  2: "Rejected",
  Pending: 0,
  Approved: 1,
  Rejected: 2
}
```

**Reverse mapping :**
```ts
Status.Approved    // 1
Status[1]          // "Approved"
```

### 3.2 Compilation d'un string enum

**Code TypeScript :**
```ts
enum Direction {
  Up = "UP",
  Down = "DOWN",
  Left = "LEFT",
  Right = "RIGHT"
}

const move = Direction.Up;
```

**Code JavaScript généré :**
```js
var Direction;
(function (Direction) {
  Direction["Up"] = "UP";
  Direction["Down"] = "DOWN";
  Direction["Left"] = "LEFT";
  Direction["Right"] = "RIGHT";
})(Direction || (Direction = {}));

const move = Direction.Up; // "UP"
```

**Objet résultant :**
```js
Direction = {
  Up: "UP",
  Down: "DOWN",
  Left: "LEFT",
  Right: "RIGHT"
}
// Pas de reverse mapping pour string enums
```

### 3.3 Compilation d'un const enum

**Code TypeScript :**
```ts
const enum LogLevel {
  Debug = 0,
  Info = 1,
  Warning = 2,
  Error = 3
}

console.log(LogLevel.Error);
```

**Code JavaScript généré :**
```js
// Enum complètement supprimé !
console.log(3 /* Error */);
```

**Avantages :**
- Aucun code généré pour l'enum
- Valeurs inlinées directement
- Bundle JavaScript plus petit
- Mêmes garanties TypeScript

**Limitations :**
- Pas d'objet runtime
- Pas de reverse mapping
- Impossible de boucler sur les valeurs
- Problèmes avec isolatedModules

## 4. Exemples et cas concrets en TypeScript

### 4.1 Numeric enum

**Exemple 1 : Priorités**
```ts
enum Priority {
  Low,      // 0
  Medium,   // 1
  High,     // 2
  Critical  // 3
}

interface Task {
  id: number;
  title: string;
  priority: Priority;
}

const task: Task = {
  id: 1,
  title: "Fix bug",
  priority: Priority.High
};

// Comparaison
if (task.priority >= Priority.High) {
  console.log("Urgent task!");
}
```

**Exemple 2 : Valeurs personnalisées**
```ts
enum HttpStatus {
  OK = 200,
  Created = 201,
  BadRequest = 400,
  Unauthorized = 401,
  NotFound = 404,
  ServerError = 500
}

function handleResponse(status: HttpStatus) {
  switch (status) {
    case HttpStatus.OK:
      return "Success";
    case HttpStatus.NotFound:
      return "Resource not found";
    case HttpStatus.ServerError:
      return "Server error";
    default:
      return "Unknown status";
  }
}
```

**Exemple 3 : Auto-incrémentation**
```ts
enum FileAccess {
  None,         // 0
  Read,         // 1
  Write,        // 2
  ReadWrite = Read | Write,  // 3
  Execute = 4,  // 4
  All = ReadWrite | Execute  // 7
}
```

### 4.2 String enum

**Exemple 1 : États de commande**
```ts
enum OrderStatus {
  Pending = "PENDING",
  Processing = "PROCESSING",
  Shipped = "SHIPPED",
  Delivered = "DELIVERED",
  Cancelled = "CANCELLED"
}

interface Order {
  id: number;
  status: OrderStatus;
  items: OrderItem[];
}

function updateOrderStatus(order: Order, status: OrderStatus) {
  order.status = status;
  
  // Sauvegarde en BDD avec valeur string lisible
  database.save({ ...order, status: status }); // "SHIPPED"
}

const order: Order = {
  id: 123,
  status: OrderStatus.Pending,
  items: []
};

updateOrderStatus(order, OrderStatus.Shipped);
```

**Exemple 2 : Types d'événements**
```ts
enum EventType {
  UserCreated = "user:created",
  UserUpdated = "user:updated",
  UserDeleted = "user:deleted",
  OrderPlaced = "order:placed",
  PaymentReceived = "payment:received"
}

interface Event {
  type: EventType;
  timestamp: Date;
  payload: any;
}

function emitEvent(type: EventType, payload: any) {
  const event: Event = {
    type,
    timestamp: new Date(),
    payload
  };
  
  console.log(`Event: ${type}`, payload);
}

emitEvent(EventType.UserCreated, { userId: 123 });
```

**Exemple 3 : Configuration**
```ts
enum Environment {
  Development = "development",
  Staging = "staging",
  Production = "production"
}

enum LogLevel {
  Debug = "debug",
  Info = "info",
  Warning = "warning",
  Error = "error"
}

interface AppConfig {
  env: Environment;
  logLevel: LogLevel;
  apiUrl: string;
}

const config: AppConfig = {
  env: Environment.Production,
  logLevel: LogLevel.Error,
  apiUrl: "https://api.example.com"
};

if (config.env === Environment.Production) {
  // Désactiver debug en production
  if (config.logLevel === LogLevel.Debug) {
    config.logLevel = LogLevel.Info;
  }
}
```

### 4.3 Const enum pour performance

**Exemple 1 : Directions**
```ts
const enum Direction {
  Up,
  Down,
  Left,
  Right
}

function move(direction: Direction) {
  switch (direction) {
    case Direction.Up:
      console.log("Moving up");
      break;
    case Direction.Down:
      console.log("Moving down");
      break;
    case Direction.Left:
      console.log("Moving left");
      break;
    case Direction.Right:
      console.log("Moving right");
      break;
  }
}

move(Direction.Up);

// JavaScript généré (inline) :
// move(0 /* Up */);
```

**Exemple 2 : Flags de compilation**
```ts
const enum CompilerFlags {
  None = 0,
  Strict = 1 << 0,       // 1
  NoImplicitAny = 1 << 1, // 2
  SourceMap = 1 << 2      // 4
}

const flags = CompilerFlags.Strict | CompilerFlags.SourceMap;
// Compilé en : const flags = 1 | 4;
```

### 4.4 Itérer sur un enum

**Numeric enum :**
```ts
enum Role {
  Guest,
  User,
  Admin
}

// Obtenir toutes les clés
const roleKeys = Object.keys(Role).filter(k => isNaN(Number(k)));
// ["Guest", "User", "Admin"]

// Obtenir toutes les valeurs
const roleValues = Object.values(Role).filter(v => typeof v === 'number');
// [0, 1, 2]

// Itérer
for (const key in Role) {
  if (isNaN(Number(key))) {
    console.log(key, Role[key as keyof typeof Role]);
  }
}
```

**String enum :**
```ts
enum Status {
  Pending = "pending",
  Approved = "approved"
}

// Plus simple avec string enum
const statuses = Object.values(Status);
// ["pending", "approved"]

const statusKeys = Object.keys(Status);
// ["Pending", "Approved"]
```

### 4.5 Enum dans les fonctions

**Type guard avec enum :**
```ts
enum Animal {
  Dog = "dog",
  Cat = "cat",
  Bird = "bird"
}

function isAnimal(value: string): value is Animal {
  return Object.values(Animal).includes(value as Animal);
}

const input = "dog";
if (isAnimal(input)) {
  const animal: Animal = input; // Type-safe
}
```

**Valeurs par défaut :**
```ts
enum Theme {
  Light = "light",
  Dark = "dark",
  Auto = "auto"
}

function getTheme(preference?: Theme): Theme {
  return preference ?? Theme.Auto;
}
```

## 5. Erreurs et pièges à éviter

### 5.1 Erreurs fréquentes

| Erreur | Description | Impact | Solution |
|--------|-------------|--------|----------|
| **Heterogeneous enum** | Mix numeric et string | Confusion, bugs | Rester cohérent |
| **Computed values** | Valeurs calculées dans enum | Ordre important | Éviter ou documenter |
| **Const enum export** | Exporter const enum | Problèmes cross-modules | Utiliser enum normal |
| **Reverse mapping oublié** | Compter sur reverse mapping string | undefined | Seulement pour numeric |
| **Type assertion incorrecte** | `as unknown as Enum` | Contourne type-safety | Valider correctement |

### 5.2 Pièges courants

**Piège 1 : Heterogeneous enum**
```ts
// ❌ À éviter (mix numeric/string)
enum Mixed {
  No = 0,
  Yes = "YES"
}

// ✅ Cohérent
enum Status {
  Pending = 0,
  Approved = 1
}

// Ou
enum StatusStr {
  Pending = "pending",
  Approved = "approved"
}
```

**Piège 2 : Computed enum values**
```ts
// ⚠️ Ordre important !
function getValue() {
  return 100;
}

enum Numbers {
  A = getValue(), // Doit être en premier
  B,              // Erreur : besoin initializer
  C
}

// ✅ Solution
enum Numbers {
  A = getValue(),
  B = 1,
  C = 2
}
```

**Piège 3 : Const enum avec isolatedModules**
```ts
// ❌ Erreur avec isolatedModules: true
const enum Status {
  Active,
  Inactive
}

export { Status }; // Erreur

// ✅ Solution : enum normal
enum Status {
  Active,
  Inactive
}
```

**Piège 4 : Validation runtime**
```ts
enum Status {
  Active = "active",
  Inactive = "inactive"
}

// ❌ Pas de validation automatique
function setStatus(status: string) {
  // TypeScript accepte n'importe quelle string
  const s: Status = status as Status; // Dangereux !
}

// ✅ Validation
function setStatus(status: string) {
  if (!Object.values(Status).includes(status as Status)) {
    throw new Error("Invalid status");
  }
  const s: Status = status as Status;
}
```

**Piège 5 : Enum vs Union type**
```ts
// Enum génère du code
enum Theme {
  Light = "light",
  Dark = "dark"
}
// → Objet en JavaScript

// Union type ne génère rien
type Theme = "light" | "dark";
// → Disparaît à la compilation

// ✅ Pour minimize bundle, préférer union type
```

### 5.3 Impacts sur les logiciels/webapps

| Problème | Conséquence |
|----------|-------------|
| **Magic strings au lieu d'enum** | Typos non détectées, refactoring difficile |
| **Const enum mal utilisé** | Problèmes de build, incompatibilités |
| **Pas de validation runtime** | Valeurs invalides acceptées depuis API |
| **Enums trop gros** | Bundle JavaScript gonflé |
| **Heterogeneous enum** | Confusion, comportement imprévisible |

## 6. Résumé de l'essentiel

### Points clés à retenir

1. **Enum = Ensemble de constantes nommées**
   - Numeric enum : valeurs numériques auto-incrémentées
   - String enum : valeurs string explicites
   - Const enum : inline à la compilation

2. **Avantages**
   - Type-safe
   - Auto-complétion
   - Lisibilité
   - Refactoring facile

3. **Enum vs Union type**
   - Enum : génère code JS, reverse mapping
   - Union : plus léger, moderne, recommandé

4. **Const enum pour performance**
   - Valeurs inlinées
   - Pas de code généré
   - Bundle plus petit

### Syntaxe de base

```ts
// Numeric enum
enum Status {
  Pending,    // 0
  Approved,   // 1
  Rejected    // 2
}

// String enum
enum Direction {
  Up = "UP",
  Down = "DOWN"
}

// Const enum
const enum LogLevel {
  Debug,
  Info
}

// Utilisation
const current: Status = Status.Approved;

if (current === Status.Approved) {
  // ...
}
```

### Checklist bonnes pratiques

- [ ] Utiliser enum pour ensembles finis de valeurs
- [ ] Préférer string enum pour lisibilité en BDD/API
- [ ] Utiliser const enum pour optimiser bundle
- [ ] Éviter heterogeneous enum
- [ ] Valider les valeurs venant de l'extérieur
- [ ] Considérer union type comme alternative moderne
- [ ] Documenter les valeurs custom
- [ ] Ne pas compter sur reverse mapping pour string enum

### Enum vs Union type : Quand utiliser quoi ?

**Utiliser enum si :**
- Besoin de reverse mapping
- Préférence pour objets runtime
- Compatibilité avec code existant
- Besoin d'itérer sur les valeurs

**Utiliser union type si :**
- Bundle size est critique
- Approche moderne préférée
- Pas besoin de reverse mapping
- Intégration avec types existants

```ts
// Enum
enum Status {
  Active = "active",
  Inactive = "inactive"
}

// Équivalent union type
type Status = "active" | "inactive";

// Helper pour validation
const STATUSES = ["active", "inactive"] as const;
type Status = typeof STATUSES[number];
```

---

**En une phrase :**

> Les enums TypeScript permettent de définir des ensembles de constantes nommées avec type-safety et auto-complétion, existant en trois variantes (numeric, string, const), mais peuvent souvent être avantageusement remplacés par des union types pour réduire la taille du bundle JavaScript généré.
