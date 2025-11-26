# Objets et Immutabilité : Spread, Référence vs Valeur

## 1. Contexte et définitions des concepts clés

### 1.1 Qu'est-ce qu'un objet en TypeScript ?

Un **objet** est une structure de données qui regroupe des propriétés (paires clé-valeur).

**Syntaxe :**
```ts
const person = {
  name: "Alice",
  age: 30,
  email: "alice@example.com"
};
```

### 1.2 Qu'est-ce que l'immutabilité ?

**Immutabilité** : Principe selon lequel une donnée, une fois créée, ne peut plus être modifiée. Pour la "modifier", on crée une nouvelle version.

**Définitions importantes :**

- **Mutable** : Peut être modifié après création
- **Immutable** : Ne peut PAS être modifié après création
- **Référence** : Adresse mémoire pointant vers un objet
- **Valeur** : Donnée brute stockée directement
- **Shallow copy** : Copie superficielle (1er niveau seulement)
- **Deep copy** : Copie profonde (tous les niveaux)
- **Spread operator** : `...` pour copier/fusionner des objets ou tableaux

### 1.3 Pourquoi c'est important ?

**Problèmes avec la mutabilité :**
- Bugs difficiles à tracer
- Effets de bord imprévisibles
- Tests complexes
- Concurrence difficile à gérer

**Avantages de l'immutabilité :**
- Code prévisible
- Facilite le debugging
- Optimisations possibles
- Programmation fonctionnelle

## 2. Quand et pourquoi utiliser ces concepts

### 2.1 Quand utiliser l'immutabilité

**Contextes où l'immutabilité est essentielle :**

1. **État dans les applications (React, Angular, Vue)**
```ts
// ❌ Mutation directe
state.user.name = "Bob"; // Peut casser la détection de changement

// ✅ Immutabilité
setState({ ...state, user: { ...state.user, name: "Bob" }});
```

2. **Fonctions pures**
```ts
// ✅ Fonction pure (immutable)
function addItem<T>(array: T[], item: T): T[] {
  return [...array, item]; // Nouveau tableau
}

// ❌ Fonction impure (mutable)
function addItemBad<T>(array: T[], item: T): void {
  array.push(item); // Modifie l'original
}
```

3. **Historique et undo/redo**
```ts
const history: AppState[] = [];
history.push({ ...currentState }); // Snapshot immutable
```

4. **Données partagées entre modules**
```ts
export const config = Object.freeze({
  apiUrl: "https://api.example.com",
  timeout: 5000
});
```

### 2.2 Quand autoriser la mutabilité

**Cas où la mutabilité est acceptable :**

1. **Performance critique** (gros tableaux, calculs intensifs)
2. **Scope local** (variable temporaire dans une fonction)
3. **Builder pattern** (construction progressive d'un objet)
4. **Algorithmes spécifiques** (tri en place, graphes)

## 3. Comment cela se passe du point de vue matériel

### 3.1 Référence vs Valeur en mémoire

**Valeur (primitifs) :**
```
┌─────────────────────┐
│       STACK         │
├─────────────────────┤
│ x = 10              │ ← Valeur directe
│ y = 10              │ ← Copie indépendante
└─────────────────────┘
```

**Référence (objets) :**
```
┌─────────────────────┐        ┌──────────────────────┐
│       STACK         │        │        HEAP          │
├─────────────────────┤        ├──────────────────────┤
│ obj1 → 0x1A2B      │───────→│ 0x1A2B:              │
│ obj2 → 0x1A2B      │───────→│   name: "Alice"      │
└─────────────────────┘        │   age: 30            │
                               └──────────────────────┘
```

**Problème de mutation :**
```ts
const obj1 = { name: "Alice" };
const obj2 = obj1;  // Même référence !

obj1.name = "Bob";
console.log(obj2.name); // "Bob" ⚠️
```

**En mémoire :**
```
Les deux pointent vers le MÊME emplacement mémoire
→ Modification visible partout
```

### 3.2 Shallow copy (copie superficielle)

**Spread operator ou Object.assign :**
```ts
const obj1 = { name: "Alice", address: { city: "Paris" } };
const obj2 = { ...obj1 }; // Shallow copy

obj2.name = "Bob";           // ✅ OK : propriété de 1er niveau
obj2.address.city = "Lyon";  // ⚠️ Modifie aussi obj1 !
```

**En mémoire :**
```
┌─────────────────────┐        ┌──────────────────────┐
│       STACK         │        │        HEAP          │
├─────────────────────┤        ├──────────────────────┤
│ obj1 → 0x1000      │───────→│ 0x1000:              │
│ obj2 → 0x2000      │──┐     │   name: "Alice"      │
└─────────────────────┘  │     │   address → 0x3000   │──┐
                         │     └──────────────────────┘  │
                         │                                │
                         └────→│ 0x2000:              │  │
                               │   name: "Bob"        │  │
                               │   address → 0x3000   │──┤
                               └──────────────────────┘  │
                                                          │
                               ┌──────────────────────┐  │
                               │ 0x3000: (PARTAGÉ!)   │←─┘
                               │   city: "Lyon"       │
                               └──────────────────────┘
```

### 3.3 Deep copy (copie profonde)

**structuredClone (moderne) :**
```ts
const obj1 = { name: "Alice", address: { city: "Paris" } };
const obj2 = structuredClone(obj1); // Deep copy

obj2.address.city = "Lyon"; // ✅ obj1 reste inchangé
```

**En mémoire :**
```
┌─────────────────────┐        ┌──────────────────────┐
│       STACK         │        │        HEAP          │
├─────────────────────┤        ├──────────────────────┤
│ obj1 → 0x1000      │───────→│ 0x1000:              │
│ obj2 → 0x2000      │──┐     │   name: "Alice"      │
└─────────────────────┘  │     │   address → 0x3000   │
                         │     └──────────────────────┘
                         │     ┌──────────────────────┐
                         │     │ 0x3000:              │
                         │     │   city: "Paris"      │
                         │     └──────────────────────┘
                         │
                         └────→│ 0x2000:              │
                               │   name: "Alice"      │
                               │   address → 0x4000   │
                               └──────────────────────┘
                               ┌──────────────────────┐
                               │ 0x4000: (COPIE)      │
                               │   city: "Paris"      │
                               └──────────────────────┘
```

## 4. Exemples et cas concrets en TypeScript

### 4.1 Manipulation d'objets : Création et copie

**Création d'objet :**
```ts
interface User {
  id: number;
  name: string;
  email: string;
  roles: string[];
}

const user: User = {
  id: 1,
  name: "Alice",
  email: "alice@example.com",
  roles: ["user"]
};
```

**Copie avec spread :**
```ts
// Shallow copy
const userCopy = { ...user };

// Modification indépendante (1er niveau)
userCopy.name = "Bob";
console.log(user.name);      // "Alice" ✅
console.log(userCopy.name);  // "Bob" ✅

// Modification partagée (niveaux profonds)
userCopy.roles.push("admin");
console.log(user.roles);     // ["user", "admin"] ⚠️
```

**Deep copy pour éviter le problème :**
```ts
const userDeepCopy = structuredClone(user);

userDeepCopy.roles.push("admin");
console.log(user.roles);         // ["user"] ✅
console.log(userDeepCopy.roles); // ["user", "admin"] ✅
```

### 4.2 Fusion d'objets avec spread

**Fusion simple :**
```ts
const defaults = {
  theme: "light",
  language: "en"
};

const userPrefs = {
  language: "fr"
};

const config = { ...defaults, ...userPrefs };
// { theme: "light", language: "fr" }
```

**Fusion profonde manuelle :**
```ts
const config1 = {
  api: {
    url: "https://api.example.com",
    timeout: 5000
  },
  features: {
    analytics: true
  }
};

const config2 = {
  api: {
    timeout: 10000
  }
};

// ❌ Spread superficiel écrase tout `api`
const merged1 = { ...config1, ...config2 };
// { api: { timeout: 10000 }, features: { analytics: true } }
// → url perdu !

// ✅ Spread profond manuel
const merged2 = {
  ...config1,
  api: {
    ...config1.api,
    ...config2.api
  }
};
// { api: { url: "...", timeout: 10000 }, features: { analytics: true } }
```

### 4.3 Immutabilité dans les mises à jour d'état

**Mise à jour d'une propriété :**
```ts
interface AppState {
  user: User;
  cart: CartItem[];
  isLoading: boolean;
}

const state: AppState = {
  user: { id: 1, name: "Alice", email: "alice@example.com", roles: [] },
  cart: [],
  isLoading: false
};

// ❌ Mutation directe
state.user.name = "Bob";

// ✅ Immutabilité
const newState = {
  ...state,
  user: {
    ...state.user,
    name: "Bob"
  }
};
```

**Mise à jour dans un tableau :**
```ts
interface CartItem {
  id: number;
  quantity: number;
}

const cart: CartItem[] = [
  { id: 1, quantity: 2 },
  { id: 2, quantity: 1 }
];

// ❌ Mutation
cart[0].quantity = 3;

// ✅ Immutabilité
const newCart = cart.map(item =>
  item.id === 1
    ? { ...item, quantity: 3 }
    : item
);
```

### 4.4 Opérations courantes immutables

**Ajouter une propriété :**
```ts
const user = { name: "Alice", age: 30 };
const userWithEmail = { ...user, email: "alice@example.com" };
```

**Supprimer une propriété :**
```ts
const user = { name: "Alice", age: 30, temp: "delete me" };
const { temp, ...userWithoutTemp } = user;
// userWithoutTemp = { name: "Alice", age: 30 }
```

**Modifier une propriété imbriquée :**
```ts
const state = {
  user: {
    profile: {
      name: "Alice"
    }
  }
};

const newState = {
  ...state,
  user: {
    ...state.user,
    profile: {
      ...state.user.profile,
      name: "Bob"
    }
  }
};
```

**Ajouter à un tableau :**
```ts
const numbers = [1, 2, 3];

// ❌ Mutation
numbers.push(4);

// ✅ Immutabilité
const newNumbers = [...numbers, 4];
```

**Retirer d'un tableau :**
```ts
const numbers = [1, 2, 3, 4];

// ✅ Filtrage
const without3 = numbers.filter(n => n !== 3);
// [1, 2, 4]
```

**Modifier un élément de tableau :**
```ts
const users = [
  { id: 1, name: "Alice" },
  { id: 2, name: "Bob" }
];

// ✅ Map
const updatedUsers = users.map(user =>
  user.id === 1
    ? { ...user, name: "Alicia" }
    : user
);
```

### 4.5 Object.freeze pour l'immutabilité forcée

**Freeze basique :**
```ts
const config = Object.freeze({
  apiUrl: "https://api.example.com",
  timeout: 5000
});

config.timeout = 10000; // ❌ Erreur en mode strict, ignoré sinon
```

**Limitation : freeze superficiel :**
```ts
const state = Object.freeze({
  user: {
    name: "Alice"
  }
});

state.user = {}; // ❌ Erreur
state.user.name = "Bob"; // ✅ Fonctionne ! ⚠️
```

**Deep freeze (fonction personnalisée) :**
```ts
function deepFreeze<T>(obj: T): T {
  Object.freeze(obj);
  
  Object.values(obj).forEach(value => {
    if (typeof value === 'object' && value !== null) {
      deepFreeze(value);
    }
  });
  
  return obj;
}

const state = deepFreeze({
  user: { name: "Alice" }
});

state.user.name = "Bob"; // ❌ Erreur
```

### 4.6 Cas concret : Gestion d'état dans une app

**Redux-like reducer :**
```ts
type Action =
  | { type: "ADD_ITEM"; item: CartItem }
  | { type: "REMOVE_ITEM"; itemId: number }
  | { type: "UPDATE_QUANTITY"; itemId: number; quantity: number };

interface State {
  cart: CartItem[];
  total: number;
}

function cartReducer(state: State, action: Action): State {
  switch (action.type) {
    case "ADD_ITEM":
      return {
        ...state,
        cart: [...state.cart, action.item]
      };
    
    case "REMOVE_ITEM":
      return {
        ...state,
        cart: state.cart.filter(item => item.id !== action.itemId)
      };
    
    case "UPDATE_QUANTITY":
      return {
        ...state,
        cart: state.cart.map(item =>
          item.id === action.itemId
            ? { ...item, quantity: action.quantity }
            : item
        )
      };
    
    default:
      return state;
  }
}
```

**React-like component :**
```ts
interface UserFormProps {
  initialUser: User;
  onSave: (user: User) => void;
}

function UserForm({ initialUser, onSave }: UserFormProps) {
  const [user, setUser] = useState(initialUser);
  
  const handleNameChange = (name: string) => {
    // ❌ Mutation
    // user.name = name;
    // setUser(user);
    
    // ✅ Immutabilité
    setUser({ ...user, name });
  };
  
  return (
    <input
      value={user.name}
      onChange={e => handleNameChange(e.target.value)}
    />
  );
}
```

## 5. Erreurs et pièges à éviter

### 5.1 Erreurs fréquentes

| Erreur | Description | Impact | Solution |
|--------|-------------|--------|----------|
| **Mutation d'objet partagé** | Modifier un objet passé en paramètre | Bugs imprévisibles | Copier avant modification |
| **Shallow copy insuffisant** | Utiliser spread sur objet imbriqué | Modification partagée profonde | structuredClone ou deep copy manuel |
| **Oublier le return** | Modifier sans créer nouveau | Mutation silencieuse | Toujours retourner nouveau |
| **Performance deep copy** | structuredClone sur gros objets | Application lente | Shallow copy ciblé |
| **Object.freeze superficiel** | Croire que freeze est profond | Propriétés imbriquées mutables | deepFreeze manuel |

### 5.2 Pièges courants

**Piège 1 : Spread sur tableau d'objets**
```ts
const users = [
  { id: 1, name: "Alice" },
  { id: 2, name: "Bob" }
];

const usersCopy = [...users]; // Shallow copy

usersCopy[0].name = "Alicia"; // ⚠️ Modifie aussi users !

// ✅ Solution
const usersDeepCopy = users.map(user => ({ ...user }));
```

**Piège 2 : Confusion entre const et immutabilité**
```ts
const user = { name: "Alice" };
user.name = "Bob"; // ✅ Fonctionne ! const ≠ immutable

// const empêche la réassignation, pas la mutation
// user = {}; // ❌ Erreur
```

**Piège 3 : Performance avec immutabilité**
```ts
// ❌ Copie profonde dans une boucle
for (let i = 0; i < 10000; i++) {
  state = structuredClone(state);
  // Très lent !
}

// ✅ Accumuler les changements
let changes = [];
for (let i = 0; i < 10000; i++) {
  changes.push(...);
}
state = { ...state, items: changes };
```

**Piège 4 : JSON.parse/stringify**
```ts
// ⚠️ Alternative pour deep copy (limitations !)
const copy = JSON.parse(JSON.stringify(obj));

// Problèmes :
// - Perd les fonctions
// - Perd undefined
// - Perd Date (devient string)
// - Perd références circulaires
```

### 5.3 Impacts sur les logiciels/webapps

| Problème | Conséquence |
|----------|-------------|
| **Mutation d'état** | Détection de changement cassée (React/Angular) |
| **Objets partagés mutés** | Bugs difficiles à reproduire |
| **Pas de copie** | Effets de bord imprévisibles |
| **Deep copy excessif** | Performance dégradée |
| **Référence vs valeur confusion** | Comportement inattendu |

## 6. Résumé de l'essentiel

### Points clés à retenir

1. **Objets = Références**
   - Pointent vers un emplacement mémoire
   - Assignation = copie de référence, pas de valeur
   - Modification visible partout

2. **Spread operator `...`**
   - Copie superficielle (1er niveau)
   - Propriétés imbriquées restent partagées
   - Syntaxe : `{ ...obj }` ou `[...array]`

3. **Immutabilité = Créer nouveau au lieu de modifier**
   - Avantages : prévisibilité, debugging, optimisations
   - Nécessaire pour : état app, fonctions pures, historique

4. **Deep copy vs Shallow copy**
   - Shallow : `{ ...obj }` ou `Object.assign`
   - Deep : `structuredClone(obj)` ou récursif

### Techniques essentielles

```ts
// Copier objet
const copy = { ...original };

// Modifier propriété
const updated = { ...obj, name: "New" };

// Ajouter propriété
const withNew = { ...obj, newProp: "value" };

// Retirer propriété
const { removed, ...rest } = obj;

// Copier tableau
const arrayCopy = [...original];

// Ajouter à tableau
const withNew = [...array, newItem];

// Modifier dans tableau
const updated = array.map(item =>
  item.id === targetId ? { ...item, field: "new" } : item
);

// Filtrer tableau
const filtered = array.filter(item => item.active);
```

### Checklist bonnes pratiques

- [ ] Toujours utiliser spread pour copier objets/tableaux
- [ ] Ne jamais muter les paramètres de fonction
- [ ] Créer nouveaux objets pour les mises à jour d'état
- [ ] Utiliser structuredClone pour deep copy si nécessaire
- [ ] Préférer map/filter/reduce à push/splice
- [ ] Documenter si mutation nécessaire (rare)
- [ ] Tester les fonctions pures (mêmes entrées = mêmes sorties)
- [ ] Utiliser TypeScript readonly quand possible

### const vs readonly vs freeze

```ts
// const : empêche réassignation
const x = { a: 1 };
// x = {}; // ❌ Erreur
x.a = 2; // ✅ OK

// readonly (TypeScript) : compile-time
interface User {
  readonly id: number;
}
// user.id = 2; // ❌ Erreur TypeScript

// Object.freeze : runtime
const frozen = Object.freeze({ a: 1 });
frozen.a = 2; // ❌ Erreur runtime (strict mode)
```

---

**En une phrase :**

> En TypeScript, les objets sont stockés par référence (pointeur mémoire) et non par valeur, rendant essentielle l'utilisation du spread operator `...` et des techniques immutables pour créer de nouvelles copies au lieu de muter les originaux, afin d'éviter les effets de bord imprévisibles et garantir un comportement prévisible de l'application.
