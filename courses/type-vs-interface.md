# Type vs Interface en TypeScript

## 1. Contexte et définitions des concepts clés

### 1.1 Qu'est-ce qu'un `type` ?

Un **type alias** (alias de type) permet de donner un nom à n'importe quel type TypeScript.

**Syntaxe :**
```ts
type NomDuType = définition;
```

**Caractéristiques :**
- Peut représenter n'importe quel type (primitif, union, intersection, tuple, etc.)
- Ne peut pas être étendu après déclaration
- Supporte les types union et intersection
- Plus flexible pour les types complexes

### 1.2 Qu'est-ce qu'une `interface` ?

Une **interface** décrit la structure d'un objet.

**Syntaxe :**
```ts
interface NomInterface {
  propriété: type;
}
```

**Caractéristiques :**
- Spécialisée pour décrire des objets
- Peut être étendue (extends)
- Peut être fusionnée (declaration merging)
- Orientée POO (Programmation Orientée Objet)

### 1.3 Définitions importantes

- **Type alias** : Nom donné à un type
- **Interface** : Contrat décrivant la forme d'un objet
- **Union type** : Type pouvant être A OU B (`A | B`)
- **Intersection type** : Type ayant toutes les propriétés de A ET B (`A & B`)
- **Declaration merging** : Fusion automatique de plusieurs déclarations
- **Extends** : Héritage de propriétés

## 2. Quand et pourquoi utiliser `type` ou `interface`

### 2.1 Quand utiliser `type`

**Utilisez `type` pour :**

1. **Types primitifs et unions**
```ts
type ID = string | number;
type Status = "pending" | "approved" | "rejected";
```

2. **Tuples**
```ts
type Point = [number, number];
type RGB = [red: number, green: number, blue: number];
```

3. **Fonctions**
```ts
type MathOperation = (a: number, b: number) => number;
type Logger = (message: string) => void;
```

4. **Types conditionnels**
```ts
type NonNullable<T> = T extends null | undefined ? never : T;
type ReturnType<T> = T extends (...args: any[]) => infer R ? R : never;
```

5. **Mapped types**
```ts
type Readonly<T> = {
  readonly [P in keyof T]: T[P];
};
```

### 2.2 Quand utiliser `interface`

**Utilisez `interface` pour :**

1. **Objets et classes**
```ts
interface User {
  id: number;
  name: string;
  email: string;
}
```

2. **Extension et héritage**
```ts
interface Admin extends User {
  permissions: string[];
}
```

3. **Implémentation dans les classes**
```ts
class UserService implements UserRepository {
  // ...
}
```

4. **APIs publiques qui pourraient être étendues**
```ts
// Bibliothèque publique
export interface PluginConfig {
  name: string;
  version: string;
}
// Les utilisateurs peuvent l'étendre
```

5. **Declaration merging** (rarement utilisé)
```ts
interface Window {
  myCustomProperty: string;
}
```

### 2.3 Règle générale

**Recommandation :**
- **`interface`** pour les objets et quand vous voulez permettre l'extension
- **`type`** pour tout le reste (unions, tuples, fonctions, types utilitaires)

## 3. Comment cela se passe du point de vue matériel

### 3.1 Compilation TypeScript → JavaScript

**Important :** Les types et interfaces **n'existent pas en JavaScript** !

**Code TypeScript :**
```ts
type UserID = number;

interface User {
  id: UserID;
  name: string;
}

const user: User = {
  id: 1,
  name: "Alice"
};
```

**Code JavaScript généré :**
```js
// Types et interface complètement effacés
const user = {
  id: 1,
  name: "Alice"
};
```

### 3.2 Vérification au moment de la compilation

```
┌───────────────────────┐
│   Code TypeScript     │
│  (types + interface)  │
└───────────┬───────────┘
            │
            ▼
┌───────────────────────┐
│  Compilateur (tsc)    │
│  ┌─────────────────┐  │
│  │ Type Checker    │  │ ← Vérifie les types
│  └─────────────────┘  │
│  ┌─────────────────┐  │
│  │ Type Eraser     │  │ ← Supprime les types
│  └─────────────────┘  │
└───────────┬───────────┘
            │
            ▼
┌───────────────────────┐
│   Code JavaScript     │
│    (pur runtime)      │
└───────────────────────┘
```

**Processus :**
1. **Parse** : Analyse syntaxique du code TS
2. **Type Check** : Vérification des types (erreurs détectées ici)
3. **Transform** : Suppression des types
4. **Emit** : Génération du JavaScript

### 3.3 Impact sur les performances

**Aucun impact runtime :**
- Les types n'existent que pendant le développement
- Pas de surcoût en production
- JavaScript généré identique dans les deux cas
- Vérifications uniquement à la compilation

**Impact développement :**
- Type checker doit analyser le code
- Projet large = compilation plus longue
- Mais détecte les bugs avant exécution !

## 4. Exemples et cas concrets en TypeScript

### 4.1 Exemples avec `type`

**1. Union types**
```ts
type PaymentMethod = "card" | "paypal" | "crypto";
type Result<T> = { success: true; data: T } | { success: false; error: string };

function processPayment(method: PaymentMethod): Result<string> {
  if (method === "crypto") {
    return { success: false, error: "Crypto not supported" };
  }
  return { success: true, data: "Payment processed" };
}
```

**2. Intersection types**
```ts
type HasTimestamps = {
  createdAt: Date;
  updatedAt: Date;
};

type HasId = {
  id: number;
};

type Entity = HasId & HasTimestamps;

const article: Entity = {
  id: 1,
  createdAt: new Date(),
  updatedAt: new Date()
};
```

**3. Fonction types**
```ts
type Validator<T> = (value: T) => boolean;
type Transformer<T, U> = (value: T) => U;

const isPositive: Validator<number> = (n) => n > 0;
const toString: Transformer<number, string> = (n) => n.toString();
```

**4. Mapped types**
```ts
type User = {
  id: number;
  name: string;
  email: string;
};

// Rendre toutes les propriétés optionnelles
type PartialUser = {
  [K in keyof User]?: User[K];
};

// Équivalent à:
// type PartialUser = {
//   id?: number;
//   name?: string;
//   email?: string;
// }
```

**5. Conditional types**
```ts
type IsString<T> = T extends string ? true : false;

type A = IsString<string>;  // true
type B = IsString<number>;  // false

// Exemple pratique
type Flatten<T> = T extends Array<infer U> ? U : T;

type Str = Flatten<string>;      // string
type Num = Flatten<number[]>;    // number
```

### 4.2 Exemples avec `interface`

**1. Objets simples**
```ts
interface Product {
  id: number;
  name: string;
  price: number;
  inStock: boolean;
}

const laptop: Product = {
  id: 1,
  name: "MacBook Pro",
  price: 2499,
  inStock: true
};
```

**2. Extension (extends)**
```ts
interface Animal {
  name: string;
  age: number;
}

interface Dog extends Animal {
  breed: string;
  bark(): void;
}

const myDog: Dog = {
  name: "Rex",
  age: 3,
  breed: "Labrador",
  bark() {
    console.log("Woof!");
  }
};
```

**3. Multiple extends**
```ts
interface Printable {
  print(): void;
}

interface Loggable {
  log(): void;
}

interface Document extends Printable, Loggable {
  title: string;
}

const doc: Document = {
  title: "Report",
  print() { console.log("Printing..."); },
  log() { console.log("Logging..."); }
};
```

**4. Propriétés optionnelles et readonly**
```ts
interface Config {
  readonly apiUrl: string;      // Immuable
  timeout?: number;              // Optionnel
  retries: number;
}

const config: Config = {
  apiUrl: "https://api.example.com",
  retries: 3
};

// config.apiUrl = "autre"; // ❌ Erreur: readonly
```

**5. Index signatures**
```ts
interface StringMap {
  [key: string]: string;
}

const translations: StringMap = {
  hello: "Bonjour",
  goodbye: "Au revoir",
  thanks: "Merci"
};

interface NumberDictionary {
  [index: string]: number;
  length: number;    // OK
  // name: string;   // ❌ Erreur: doit être number
}
```

**6. Callable interfaces**
```ts
interface SearchFunction {
  (source: string, substring: string): boolean;
}

const search: SearchFunction = (source, substring) => {
  return source.includes(substring);
};

console.log(search("hello world", "world")); // true
```

### 4.3 Type vs Interface : Comparaison directe

**Même résultat, syntaxe différente :**
```ts
// Avec type
type UserType = {
  id: number;
  name: string;
};

// Avec interface
interface UserInterface {
  id: number;
  name: string;
}

// Utilisation identique
const user1: UserType = { id: 1, name: "Alice" };
const user2: UserInterface = { id: 2, name: "Bob" };
```

**Extension :**
```ts
// Type : intersection
type Person = {
  name: string;
};

type Employee = Person & {
  employeeId: number;
};

// Interface : extends
interface PersonInterface {
  name: string;
}

interface EmployeeInterface extends PersonInterface {
  employeeId: number;
}
```

**Ce qui n'est possible qu'avec `type` :**
```ts
// Union types
type StringOrNumber = string | number;

// Tuples
type Coordinates = [number, number];

// Primitives aliasées
type ID = number;
type Callback = () => void;
```

**Ce qui est spécifique à `interface` :**
```ts
// Declaration merging
interface User {
  name: string;
}

interface User {
  age: number;
}

// Résultat fusionné automatiquement
const user: User = {
  name: "Alice",
  age: 30
};
```

### 4.4 Cas concrets en application

**Exemple 1 : API REST**
```ts
// Types pour les réponses
type ApiResponse<T> = {
  success: true;
  data: T;
} | {
  success: false;
  error: string;
};

// Interface pour les entités
interface User {
  id: number;
  email: string;
  firstName: string;
  lastName: string;
}

interface Post {
  id: number;
  title: string;
  content: string;
  authorId: number;
}

// Utilisation
async function getUser(id: number): Promise<ApiResponse<User>> {
  try {
    const response = await fetch(`/api/users/${id}`);
    const data = await response.json();
    return { success: true, data };
  } catch (error) {
    return { success: false, error: error.message };
  }
}
```

**Exemple 2 : Configuration et options**
```ts
// Type pour les variantes
type Theme = "light" | "dark" | "auto";
type Language = "fr" | "en" | "es";

// Interface pour la structure
interface AppSettings {
  theme: Theme;
  language: Language;
  notifications: {
    email: boolean;
    push: boolean;
  };
}

const settings: AppSettings = {
  theme: "dark",
  language: "fr",
  notifications: {
    email: true,
    push: false
  }
};
```

**Exemple 3 : Architecture en couches**
```ts
// Interface pour repository (contrat)
interface UserRepository {
  findById(id: number): Promise<User | null>;
  findAll(): Promise<User[]>;
  create(user: Omit<User, 'id'>): Promise<User>;
  update(id: number, data: Partial<User>): Promise<User>;
  delete(id: number): Promise<void>;
}

// Type pour DTO (Data Transfer Object)
type CreateUserDTO = {
  email: string;
  password: string;
  firstName: string;
  lastName: string;
};

type UpdateUserDTO = Partial<CreateUserDTO>;

// Implémentation
class PostgresUserRepository implements UserRepository {
  async findById(id: number): Promise<User | null> {
    // ...
  }
  
  async findAll(): Promise<User[]> {
    // ...
  }
  
  async create(user: Omit<User, 'id'>): Promise<User> {
    // ...
  }
  
  async update(id: number, data: Partial<User>): Promise<User> {
    // ...
  }
  
  async delete(id: number): Promise<void> {
    // ...
  }
}
```

**Exemple 4 : Event system**
```ts
// Types pour les événements
type UserEvents = {
  "user:created": { userId: number; timestamp: Date };
  "user:updated": { userId: number; changes: string[] };
  "user:deleted": { userId: number };
};

type EventName = keyof UserEvents;
type EventHandler<E extends EventName> = (data: UserEvents[E]) => void;

// Interface pour l'event emitter
interface EventEmitter {
  on<E extends EventName>(event: E, handler: EventHandler<E>): void;
  emit<E extends EventName>(event: E, data: UserEvents[E]): void;
}
```

## 5. Erreurs et pièges à éviter

### 5.1 Erreurs fréquentes

| Erreur | Description | Impact | Solution |
|--------|-------------|--------|----------|
| **Type vs Interface confusion** | Utiliser l'un quand l'autre est mieux adapté | Code moins lisible | Suivre les conventions |
| **Declaration merging accidentel** | Déclarer la même interface 2 fois | Comportement inattendu | Attention aux noms |
| **Type circulaire** | Types qui se référencent mutuellement | Erreur compilation | Utiliser interface |
| **Type `any` au lieu de générique** | Perdre le typage | Bugs runtime | Utiliser `<T>` |
| **Extends vs Intersection confusion** | Mélanger les syntaxes | Incohérence | Rester cohérent |

### 5.2 Pièges courants

**Piège 1 : Declaration merging non intentionnel**
```ts
// Fichier 1
interface Config {
  apiUrl: string;
}

// Fichier 2 (même interface!)
interface Config {
  timeout: number;
}

// Résultat fusionné (peut-être non voulu)
const config: Config = {
  apiUrl: "...",
  timeout: 5000
};

// ✅ Solution : Utiliser type si pas voulu
type Config = { apiUrl: string };
```

**Piège 2 : Extends vs Intersection**
```ts
// Interface extends
interface A {
  x: number;
}

interface B extends A {
  x: string; // ❌ Erreur : incompatible
}

// Type intersection
type A2 = {
  x: number;
};

type B2 = A2 & {
  x: string; // Pas d'erreur mais type impossible (never)
};
```

**Piège 3 : Propriétés optionnelles**
```ts
interface User {
  name?: string;
}

const user: User = {};
console.log(user.name.toUpperCase()); // ❌ Crash si undefined

// ✅ Solution
if (user.name) {
  console.log(user.name.toUpperCase());
}
```

**Piège 4 : Interface vide**
```ts
interface Empty {}

const x: Empty = "n'importe quoi"; // ✅ OK ⚠️
const y: Empty = 123;              // ✅ OK ⚠️

// Interface vide accepte tout !
```

### 5.3 Impacts sur les logiciels/webapps

| Problème | Conséquence |
|----------|-------------|
| **Mauvais choix type/interface** | Code moins maintenable |
| **Pas de typage** | Bugs en production |
| **Types trop permissifs** | Erreurs non détectées |
| **Duplication de types** | Incohérence entre modules |
| **Pas de validation runtime** | Données corrompues acceptées |

## 6. Résumé de l'essentiel

### Points clés à retenir

1. **Type = Alias universel**
   - Pour unions, tuples, primitives, fonctions
   - Plus flexible
   - Ne peut pas être étendu après déclaration

2. **Interface = Contrat pour objets**
   - Pour objets et classes
   - Extensible (extends)
   - Declaration merging possible
   - Orientée POO

3. **Choisir selon le contexte**
   ```ts
   // ✅ type pour unions
   type Status = "active" | "inactive";

   // ✅ interface pour objets
   interface User {
     id: number;
     name: string;
   }
   ```

4. **Les deux disparaissent en JavaScript**
   - Vérification uniquement à la compilation
   - Aucun impact runtime
   - Code généré identique

### Tableau de décision

| Cas d'usage | Recommandation |
|-------------|----------------|
| Union types | `type` |
| Tuples | `type` |
| Fonctions | `type` |
| Objets simples | `interface` OU `type` |
| Classes | `interface` |
| Extension/héritage | `interface` |
| API publique | `interface` |
| Types utilitaires | `type` |
| Mapped types | `type` |

### Checklist bonnes pratiques

- [ ] Utiliser `interface` pour les objets métier
- [ ] Utiliser `type` pour les unions et tuples
- [ ] Préférer `interface` pour l'extensibilité
- [ ] Éviter declaration merging sauf si intentionnel
- [ ] Typer tous les paramètres et retours
- [ ] Utiliser `readonly` pour l'immutabilité
- [ ] Gérer les propriétés optionnelles avec `?`
- [ ] Documenter les types complexes
- [ ] Exporter les types publics

### Convention de nommage

```ts
// PascalCase pour types et interfaces
type UserId = number;
interface UserProfile {}

// Suffixes courants
type UserDTO = {};      // Data Transfer Object
type UserProps = {};    // Props React/Angular
interface IUser {}      // I pour Interface (moins courant)
```

---

**En une phrase :**

> `type` est un alias flexible pour n'importe quel type TypeScript (unions, tuples, primitives, objets), tandis que `interface` est spécialisée pour décrire des objets extensibles et implémenter des contrats dans une approche orientée objet, les deux étant effacés à la compilation et n'existant que pour la vérification statique des types.
