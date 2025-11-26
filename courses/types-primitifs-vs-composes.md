# Types Primitifs vs Types Composés en TypeScript

## 1. Contexte et définitions des concepts clés

### 1.1 Qu'est-ce qu'un type ?

Un **type** définit :
- La **nature** d'une donnée
- L'**espace mémoire** qu'elle occupe
- Les **opérations** autorisées sur cette donnée
- La **structure** de l'information

### 1.2 Types primitifs vs Types composés

**Types primitifs** (ou scalaires) :
- Représentent une **valeur simple unique**
- Stockés directement en mémoire
- Immuables (non modifiables)
- Passage par valeur

**Types composés** (ou complexes) :
- Représentent une **structure de plusieurs valeurs**
- Stockés par référence (pointeur vers la mémoire)
- Mutables (modifiables)
- Passage par référence

### 1.3 Définitions importantes

- **Immutable** : Une fois créé, ne peut pas être modifié
- **Mutable** : Peut être modifié après création
- **Valeur** : Donnée brute stockée directement
- **Référence** : Adresse mémoire pointant vers la donnée
- **Stack** : Zone mémoire pour valeurs simples (rapide)
- **Heap** : Zone mémoire pour objets complexes (plus lent)

## 2. Quand et pourquoi utiliser ces types

### 2.1 Quand utiliser les types primitifs

**Utilisez les primitifs pour :**
- Stocker des valeurs simples (nombre, texte, booléen)
- Garantir l'immutabilité
- Optimiser les performances (accès rapide)
- Passer des données sans risque de modification

**Exemples :**
```ts
let age: number = 30;           // Âge
let nom: string = "Alice";      // Nom
let estActif: boolean = true;   // État
let prix: number = 19.99;       // Prix
```

### 2.2 Quand utiliser les types composés

**Utilisez les composés pour :**
- Regrouper des données liées (objet utilisateur)
- Créer des structures de données (liste, pile, file)
- Modéliser des entités métier complexes
- Partager des références entre fonctions

**Exemples :**
```ts
interface User {
  id: number;
  name: string;
  email: string;
}

const users: User[] = [
  { id: 1, name: "Alice", email: "alice@example.com" },
  { id: 2, name: "Bob", email: "bob@example.com" }
];
```

## 3. Comment cela se passe du point de vue matériel

### 3.1 Stockage en mémoire : Primitifs

**Types primitifs → Stack (pile)**

```
┌─────────────────────┐
│       STACK         │
├─────────────────────┤
│ age = 30            │ ← 8 bytes
│ nom = "Alice"       │ ← pointeur vers string (heap)
│ estActif = true     │ ← 1 byte
│ prix = 19.99        │ ← 8 bytes
└─────────────────────┘
```

**Copie par valeur :**
```ts
let a = 10;
let b = a;  // b reçoit une COPIE de la valeur
a = 20;     // b reste 10
console.log(b); // 10
```

**En mémoire :**
```
┌─────────────────┐
│ a = 20          │ ← Nouvelle valeur
│ b = 10          │ ← Valeur indépendante
└─────────────────┘
```

### 3.2 Stockage en mémoire : Composés

**Types composés → Heap (tas)**

```
┌─────────────────────┐        ┌──────────────────────┐
│       STACK         │        │        HEAP          │
├─────────────────────┤        ├──────────────────────┤
│ user → 0x1A2B      │───────→│ 0x1A2B:              │
│                     │        │   id: 1              │
│                     │        │   name: "Alice"      │
│                     │        │   email: "alice@..." │
└─────────────────────┘        └──────────────────────┘
```

**Copie par référence :**
```ts
const user1 = { name: "Alice", age: 30 };
const user2 = user1;  // user2 pointe vers le MÊME objet
user1.age = 31;       // Modifie l'objet partagé
console.log(user2.age); // 31 ⚠️
```

**En mémoire :**
```
┌─────────────────────┐        ┌──────────────────────┐
│ user1 → 0x1A2B     │───────→│ 0x1A2B:              │
│ user2 → 0x1A2B     │───────→│   name: "Alice"      │
└─────────────────────┘        │   age: 31            │
                               └──────────────────────┘
```

### 3.3 Performance et optimisation

**Primitifs :**
- Accès : **très rapide** (directement dans le stack)
- Copie : **peu coûteuse** (petite taille)
- Allocation : **automatique** (stack)

**Composés :**
- Accès : **plus lent** (indirection via référence)
- Copie : **coûteuse** (copier toute la structure)
- Allocation : **manuelle** (heap, garbage collector)

## 4. Exemples et cas concrets en TypeScript

### 4.1 Les types primitifs en détail

**1. number**
```ts
let entier: number = 42;
let decimal: number = 3.14;
let negatif: number = -10;
let hexadecimal: number = 0xFF;  // 255
let binaire: number = 0b1010;    // 10
let octal: number = 0o744;       // 484
```

**2. string**
```ts
let nom: string = "Alice";
let prenom: string = 'Bob';
let template: string = `Bonjour ${nom}`;
let multiline: string = `
  Ligne 1
  Ligne 2
`;
```

**3. boolean**
```ts
let estConnecte: boolean = true;
let estAdmin: boolean = false;
```

**4. null et undefined**
```ts
let valeurNull: null = null;              // Absence intentionnelle
let valeurUndefined: undefined = undefined; // Non initialisé
let optionnel: string | null = null;       // Union type
```

**5. symbol** (rarement utilisé)
```ts
const id1 = Symbol("id");
const id2 = Symbol("id");
console.log(id1 === id2); // false (toujours unique)
```

**6. bigint** (très grands nombres)
```ts
let grand: bigint = 9007199254740991n;
let tresgrand: bigint = BigInt("999999999999999999999");
```

### 4.2 Les types composés en détail

**1. Objects (Objets)**
```ts
// Type inline
const user: { name: string; age: number } = {
  name: "Alice",
  age: 30
};

// Type alias
type Point = {
  x: number;
  y: number;
};

const point: Point = { x: 10, y: 20 };

// Interface
interface Product {
  id: number;
  name: string;
  price: number;
  inStock?: boolean; // Optionnel
}

const laptop: Product = {
  id: 1,
  name: "MacBook Pro",
  price: 2499.99,
  inStock: true
};
```

**2. Arrays (Tableaux)**
```ts
// Notation crochets
let nombres: number[] = [1, 2, 3, 4, 5];

// Notation générique
let noms: Array<string> = ["Alice", "Bob", "Claire"];

// Tableaux typés complexes
let users: Array<{ id: number; name: string }> = [
  { id: 1, name: "Alice" },
  { id: 2, name: "Bob" }
];
```

**3. Tuples (Tableaux à taille fixe)**
```ts
// Tuple simple
let personne: [string, number] = ["Alice", 30];

// Tuple avec noms
type RGB = [red: number, green: number, blue: number];
const couleur: RGB = [255, 128, 0];

// Tuple readonly
const position: readonly [number, number] = [10, 20];
// position[0] = 5; // ❌ Erreur
```

**4. Functions (Fonctions)**
```ts
// Type fonction
type MathOperation = (a: number, b: number) => number;

const add: MathOperation = (a, b) => a + b;
const multiply: MathOperation = (a, b) => a * b;

// Interface fonction
interface Validator {
  (value: string): boolean;
}

const isEmail: Validator = (value) => value.includes("@");
```

**5. Union Types (Types union)**
```ts
type Status = "pending" | "approved" | "rejected";
type ID = number | string;

function processStatus(status: Status) {
  if (status === "approved") {
    // ...
  }
}

let userId: ID = 123;
userId = "ABC-123"; // ✅ OK
```

**6. Intersection Types**
```ts
type HasName = { name: string };
type HasAge = { age: number };
type Person = HasName & HasAge;

const alice: Person = {
  name: "Alice",
  age: 30
};
```

### 4.3 Cas concrets en application

**Exemple 1 : Panier e-commerce**
```ts
// Types primitifs pour les valeurs simples
type ProductId = number;
type Quantity = number;
type Price = number;

// Types composés pour les structures
interface CartItem {
  productId: ProductId;
  name: string;
  price: Price;
  quantity: Quantity;
}

interface Cart {
  items: CartItem[];
  userId: number;
  createdAt: Date;
}

// Utilisation
const cart: Cart = {
  items: [
    { productId: 1, name: "Laptop", price: 1200, quantity: 1 },
    { productId: 2, name: "Mouse", price: 25, quantity: 2 }
  ],
  userId: 123,
  createdAt: new Date()
};

// Calcul avec primitifs
function calculateTotal(cart: Cart): Price {
  let total: Price = 0;
  for (const item of cart.items) {
    total += item.price * item.quantity;
  }
  return total;
}
```

**Exemple 2 : Gestion d'utilisateurs**
```ts
// Primitifs pour identifiants
type UserId = number;
type Email = string;

// Composés pour données utilisateur
interface User {
  id: UserId;
  email: Email;
  firstName: string;
  lastName: string;
  roles: string[];
  isActive: boolean;
  createdAt: Date;
}

// Type composé pour réponse API
interface UserResponse {
  data: User[];
  total: number;
  page: number;
}

// Fonction manipulant les types
function findActiveUsers(users: User[]): User[] {
  return users.filter(user => user.isActive);
}
```

**Exemple 3 : Configuration d'application**
```ts
// Union types pour options limitées
type Environment = "development" | "staging" | "production";
type LogLevel = "debug" | "info" | "warn" | "error";

// Type composé pour configuration
interface AppConfig {
  env: Environment;
  logLevel: LogLevel;
  port: number;
  database: {
    host: string;
    port: number;
    name: string;
  };
  features: {
    analytics: boolean;
    beta: boolean;
  };
}

const config: AppConfig = {
  env: "production",
  logLevel: "info",
  port: 3000,
  database: {
    host: "localhost",
    port: 5432,
    name: "myapp"
  },
  features: {
    analytics: true,
    beta: false
  }
};
```

## 5. Erreurs et pièges à éviter

### 5.1 Erreurs fréquentes

| Erreur | Description | Impact | Solution |
|--------|-------------|--------|----------|
| **Confusion valeur/référence** | Penser que `obj2 = obj1` fait une copie | Modifications non intentionnelles | Utiliser spread `{...obj}` ou structuredClone |
| **Mutation d'objets** | Modifier directement un objet partagé | Bugs difficiles à tracer | Créer de nouveaux objets (immutabilité) |
| **Type `any`** | Utiliser `any` partout | Perte des avantages TypeScript | Typer précisément |
| **Null/undefined** | Ne pas gérer null/undefined | Crashes runtime | Utiliser `?` et vérifications |
| **Type assertion abusif** | `as` pour forcer un type incorrect | Erreurs runtime | Vérifier réellement le type |

### 5.2 Pièges courants

**Piège 1 : Copie superficielle vs profonde**
```ts
// ❌ Copie superficielle (shallow copy)
const user1 = {
  name: "Alice",
  address: { city: "Paris" }
};

const user2 = { ...user1 };
user2.address.city = "Lyon";  // ⚠️ Modifie aussi user1 !

console.log(user1.address.city); // "Lyon" ❌

// ✅ Copie profonde (deep copy)
const user3 = structuredClone(user1);
user3.address.city = "Marseille";
console.log(user1.address.city); // "Paris" ✅
```

**Piège 2 : Comparaison d'objets**
```ts
// ❌ Comparaison par référence
const obj1 = { value: 10 };
const obj2 = { value: 10 };
console.log(obj1 === obj2); // false ⚠️

// ✅ Comparaison par valeur
function isEqual(obj1: any, obj2: any): boolean {
  return JSON.stringify(obj1) === JSON.stringify(obj2);
}
```

**Piège 3 : Tableau vide vs null/undefined**
```ts
// ❌ Confusion
function getUsers(): User[] | null {
  return null; // Pourquoi null au lieu de [] ?
}

// ✅ Préférer tableau vide
function getUsers(): User[] {
  return []; // Plus simple à gérer
}
```

**Piège 4 : Type narrowing oublié**
```ts
function processValue(value: string | number) {
  // ❌ Erreur : pas de narrowing
  // console.log(value.toUpperCase());

  // ✅ Avec narrowing
  if (typeof value === "string") {
    console.log(value.toUpperCase());
  } else {
    console.log(value.toFixed(2));
  }
}
```

### 5.3 Impacts sur les logiciels/webapps

| Problème | Conséquence |
|----------|-------------|
| **Types mal définis** | Bugs en production, crashes inattendus |
| **Mutations non contrôlées** | État inconsistant, bugs difficiles à reproduire |
| **Type `any` partout** | Perte de la sécurité TypeScript |
| **Pas de validation** | Données corrompues dans la BDD |
| **Copie profonde inutile** | Performances dégradées |
| **Objets trop gros** | Consommation mémoire excessive |

## 6. Résumé de l'essentiel

### Points clés à retenir

1. **Types primitifs = Valeurs simples**
   - number, string, boolean, null, undefined, symbol, bigint
   - Stockés par valeur
   - Immuables
   - Rapides

2. **Types composés = Structures complexes**
   - Objects, Arrays, Tuples, Functions
   - Stockés par référence
   - Mutables
   - Plus lents

3. **Différence fondamentale : Valeur vs Référence**
   ```ts
   // Primitif : copie de valeur
   let a = 10;
   let b = a; // Copie
   a = 20;
   console.log(b); // 10

   // Composé : copie de référence
   const obj1 = { x: 10 };
   const obj2 = obj1; // Référence
   obj1.x = 20;
   console.log(obj2.x); // 20
   ```

4. **Règles d'utilisation**
   - Primitifs pour valeurs simples
   - Composés pour structures de données
   - Immutabilité quand possible
   - Typage strict toujours

### Tableau récapitulatif

| Aspect | Primitifs | Composés |
|--------|-----------|----------|
| **Nature** | Valeur unique | Structure de valeurs |
| **Stockage** | Stack (valeur) | Heap (référence) |
| **Copie** | Par valeur | Par référence |
| **Mutabilité** | Immuable | Mutable |
| **Taille** | Fixe, petite | Variable, grande |
| **Performance** | Rapide | Plus lent |
| **Exemples** | 42, "text", true | {}, [], function |

### Checklist bonnes pratiques

- [ ] Utiliser primitifs pour valeurs simples
- [ ] Typer précisément tous les objets
- [ ] Éviter `any` et `unknown` sans raison
- [ ] Gérer null/undefined explicitement
- [ ] Faire des copies quand nécessaire (spread, structuredClone)
- [ ] Préférer l'immutabilité
- [ ] Utiliser type guards pour narrowing
- [ ] Documenter les structures complexes

---

**En une phrase :**

> Les types primitifs représentent des valeurs simples stockées par valeur et immuables, tandis que les types composés représentent des structures complexes stockées par référence et mutables, cette distinction fondamentale impactant directement la façon dont les données sont copiées, comparées et manipulées en mémoire.
