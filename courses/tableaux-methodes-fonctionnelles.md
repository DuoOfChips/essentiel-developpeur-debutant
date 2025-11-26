# Tableaux et Méthodes Fonctionnelles : map, filter, reduce

## 1. Contexte et définitions des concepts clés

### 1.1 Qu'est-ce qu'un tableau ?

Un **tableau** (array) est une structure de données ordonnée qui contient une collection d'éléments du même type ou de types variés.

**Syntaxe TypeScript :**
```ts
let nombres: number[] = [1, 2, 3, 4, 5];
let noms: Array<string> = ["Alice", "Bob", "Claire"];
```

### 1.2 Programmation fonctionnelle vs impérative

**Impérative** (avec boucles) :
```ts
const nombres = [1, 2, 3, 4, 5];
const doubles: number[] = [];

for (let i = 0; i < nombres.length; i++) {
  doubles.push(nombres[i] * 2);
}
```

**Fonctionnelle** (avec méthodes) :
```ts
const nombres = [1, 2, 3, 4, 5];
const doubles = nombres.map(n => n * 2);
```

### 1.3 Définitions importantes

- **Higher-order function** : Fonction qui prend ou retourne une fonction
- **Callback** : Fonction passée en paramètre
- **Immutabilité** : Ne modifie pas le tableau original
- **Chaînage** : Enchaîner plusieurs méthodes (`array.map().filter()`)
- **Prédicat** : Fonction retournant un booléen
- **Accumulator** : Variable qui accumule des valeurs (dans reduce)

### 1.4 Les trois méthodes essentielles

| Méthode | Utilité | Retour |
|---------|---------|--------|
| **map** | Transformer chaque élément | Nouveau tableau (même longueur) |
| **filter** | Filtrer selon condition | Nouveau tableau (≤ longueur) |
| **reduce** | Réduire à une seule valeur | Valeur unique |

## 2. Quand et pourquoi utiliser ces méthodes

### 2.1 Quand utiliser `map`

**Cas d'usage :**
- Transformer chaque élément d'un tableau
- Extraire des propriétés d'objets
- Convertir des types
- Formatter des données

**Pourquoi :**
- ✅ Plus lisible qu'une boucle
- ✅ Immutable (ne modifie pas l'original)
- ✅ Chainable avec d'autres méthodes
- ✅ Déclaratif ("quoi" plutôt que "comment")

### 2.2 Quand utiliser `filter`

**Cas d'usage :**
- Garder seulement les éléments qui respectent une condition
- Retirer des éléments indésirables
- Chercher plusieurs éléments
- Nettoyer des données

**Pourquoi :**
- ✅ Expressif et clair
- ✅ Immutable
- ✅ Combinable avec map/reduce

### 2.3 Quand utiliser `reduce`

**Cas d'usage :**
- Calculer une somme, moyenne, min, max
- Transformer un tableau en objet
- Grouper des données
- Aplatir des tableaux imbriqués
- Compter des occurrences

**Pourquoi :**
- ✅ Puissant et flexible
- ✅ Remplace beaucoup de boucles
- ✅ Fonctionnel et immutable

### 2.4 Autres méthodes utiles

| Méthode | Utilité | Exemple |
|---------|---------|---------|
| **find** | Trouver 1er élément | `users.find(u => u.id === 1)` |
| **findIndex** | Trouver index | `users.findIndex(u => u.id === 1)` |
| **some** | Au moins 1 élément respecte | `numbers.some(n => n > 10)` |
| **every** | Tous les éléments respectent | `numbers.every(n => n > 0)` |
| **forEach** | Itérer (sans retour) | `users.forEach(u => console.log(u))` |
| **sort** | Trier (⚠️ mute l'original) | `[...arr].sort((a,b) => a-b)` |
| **slice** | Extraire portion | `arr.slice(0, 3)` |
| **concat** | Fusionner tableaux | `arr1.concat(arr2)` |
| **flat** | Aplatir | `[[1,2],[3,4]].flat()` |
| **flatMap** | map + flat | `arr.flatMap(x => [x, x*2])` |

## 3. Comment cela se passe du point de vue matériel

### 3.1 Stockage d'un tableau en mémoire

```
┌─────────────────────┐        ┌──────────────────────┐
│       STACK         │        │        HEAP          │
├─────────────────────┤        ├──────────────────────┤
│ arr → 0x1000       │───────→│ 0x1000: [référence]  │
│                     │        │   length: 3          │
└─────────────────────┘        │   [0]: 10            │
                               │   [1]: 20            │
                               │   [2]: 30            │
                               └──────────────────────┘
```

**Caractéristiques :**
- Taille dynamique (redimensionnable)
- Accès par index en O(1)
- Stockés de manière contiguë (ou presque)

### 3.2 Fonctionnement de `map`

**Étapes internes :**
```ts
[1, 2, 3].map(x => x * 2)
```

1. **Création nouveau tableau** : Alloue mémoire pour résultat
2. **Itération** : Boucle sur chaque élément
3. **Callback appliqué** : `x => x * 2` appelé pour chaque
4. **Stockage résultat** : Valeur retournée ajoutée au nouveau tableau
5. **Retour** : Nouveau tableau retourné

**Pseudo-code simplifié :**
```ts
function map<T, U>(array: T[], callback: (item: T, index: number) => U): U[] {
  const result: U[] = [];
  for (let i = 0; i < array.length; i++) {
    result.push(callback(array[i], i));
  }
  return result;
}
```

### 3.3 Fonctionnement de `filter`

**Étapes internes :**
```ts
[1, 2, 3, 4].filter(x => x > 2)
```

1. **Création tableau vide** : Résultat initialisé
2. **Itération** : Boucle sur éléments
3. **Test condition** : Callback retourne true/false
4. **Ajout conditionnel** : Si true, élément ajouté
5. **Retour** : Nouveau tableau (potentiellement plus petit)

**Pseudo-code :**
```ts
function filter<T>(array: T[], predicate: (item: T) => boolean): T[] {
  const result: T[] = [];
  for (let i = 0; i < array.length; i++) {
    if (predicate(array[i])) {
      result.push(array[i]);
    }
  }
  return result;
}
```

### 3.4 Fonctionnement de `reduce`

**Étapes internes :**
```ts
[1, 2, 3].reduce((acc, x) => acc + x, 0)
```

1. **Initialisation accumulateur** : `acc = 0`
2. **Première itération** : `acc = 0 + 1 = 1`
3. **Deuxième itération** : `acc = 1 + 2 = 3`
4. **Troisième itération** : `acc = 3 + 3 = 6`
5. **Retour** : Valeur finale `6`

**Pseudo-code :**
```ts
function reduce<T, U>(
  array: T[],
  callback: (acc: U, item: T, index: number) => U,
  initial: U
): U {
  let accumulator = initial;
  for (let i = 0; i < array.length; i++) {
    accumulator = callback(accumulator, array[i], i);
  }
  return accumulator;
}
```

## 4. Exemples et cas concrets en TypeScript

### 4.1 map : Transformation de données

**Exemple 1 : Doubler les nombres**
```ts
const nombres = [1, 2, 3, 4, 5];
const doubles = nombres.map(n => n * 2);
// [2, 4, 6, 8, 10]
```

**Exemple 2 : Extraire propriétés**
```ts
interface User {
  id: number;
  firstName: string;
  lastName: string;
}

const users: User[] = [
  { id: 1, firstName: "Alice", lastName: "Dupont" },
  { id: 2, firstName: "Bob", lastName: "Martin" }
];

const fullNames = users.map(u => `${u.firstName} ${u.lastName}`);
// ["Alice Dupont", "Bob Martin"]
```

**Exemple 3 : Transformer objets**
```ts
interface Product {
  id: number;
  name: string;
  price: number;
}

const products: Product[] = [
  { id: 1, name: "Laptop", price: 1000 },
  { id: 2, name: "Mouse", price: 25 }
];

const withTax = products.map(p => ({
  ...p,
  priceWithTax: p.price * 1.2
}));
// [
//   { id: 1, name: "Laptop", price: 1000, priceWithTax: 1200 },
//   { id: 2, name: "Mouse", price: 25, priceWithTax: 30 }
// ]
```

**Exemple 4 : Conversion de types**
```ts
const strings = ["1", "2", "3"];
const numbers = strings.map(s => parseInt(s, 10));
// [1, 2, 3]

const dates = ["2024-01-01", "2024-06-15"];
const dateObjects = dates.map(d => new Date(d));
```

### 4.2 filter : Filtrage de données

**Exemple 1 : Nombres pairs**
```ts
const nombres = [1, 2, 3, 4, 5, 6];
const pairs = nombres.filter(n => n % 2 === 0);
// [2, 4, 6]
```

**Exemple 2 : Filtrer utilisateurs**
```ts
interface User {
  id: number;
  name: string;
  isActive: boolean;
  age: number;
}

const users: User[] = [
  { id: 1, name: "Alice", isActive: true, age: 25 },
  { id: 2, name: "Bob", isActive: false, age: 30 },
  { id: 3, name: "Claire", isActive: true, age: 22 }
];

const activeUsers = users.filter(u => u.isActive);
const adults = users.filter(u => u.age >= 18);
const activeAdults = users.filter(u => u.isActive && u.age >= 18);
```

**Exemple 3 : Nettoyer données**
```ts
const emails = ["alice@example.com", "", "invalid", "bob@test.com", null];

const validEmails = emails.filter(
  email => email && email.includes("@")
);
// ["alice@example.com", "bob@test.com"]
```

**Exemple 4 : Filtrer par recherche**
```ts
const products = [
  { name: "Laptop Dell", category: "electronics" },
  { name: "Mouse Logitech", category: "electronics" },
  { name: "Desk Chair", category: "furniture" }
];

function searchProducts(query: string) {
  const lowerQuery = query.toLowerCase();
  return products.filter(p =>
    p.name.toLowerCase().includes(lowerQuery)
  );
}

searchProducts("laptop"); // [{ name: "Laptop Dell", ... }]
```

### 4.3 reduce : Réduction et agrégation

**Exemple 1 : Somme**
```ts
const numbers = [1, 2, 3, 4, 5];
const sum = numbers.reduce((acc, n) => acc + n, 0);
// 15
```

**Exemple 2 : Moyenne**
```ts
const numbers = [10, 20, 30, 40];
const average = numbers.reduce((acc, n, i, arr) => {
  acc += n;
  if (i === arr.length - 1) {
    return acc / arr.length;
  }
  return acc;
}, 0);
// 25
```

**Exemple 3 : Trouver min/max**
```ts
const numbers = [5, 2, 8, 1, 9];

const max = numbers.reduce((max, n) => n > max ? n : max, -Infinity);
// 9

const min = numbers.reduce((min, n) => n < min ? n : min, Infinity);
// 1
```

**Exemple 4 : Grouper par catégorie**
```ts
interface Product {
  name: string;
  category: string;
  price: number;
}

const products: Product[] = [
  { name: "Laptop", category: "electronics", price: 1000 },
  { name: "Mouse", category: "electronics", price: 25 },
  { name: "Desk", category: "furniture", price: 300 }
];

type GroupedProducts = Record<string, Product[]>;

const grouped = products.reduce<GroupedProducts>((acc, product) => {
  const category = product.category;
  if (!acc[category]) {
    acc[category] = [];
  }
  acc[category].push(product);
  return acc;
}, {});

// {
//   electronics: [{ name: "Laptop", ...}, { name: "Mouse", ... }],
//   furniture: [{ name: "Desk", ... }]
// }
```

**Exemple 5 : Compter occurrences**
```ts
const fruits = ["apple", "banana", "apple", "orange", "banana", "apple"];

const counts = fruits.reduce<Record<string, number>>((acc, fruit) => {
  acc[fruit] = (acc[fruit] || 0) + 1;
  return acc;
}, {});

// { apple: 3, banana: 2, orange: 1 }
```

**Exemple 6 : Tableau vers objet**
```ts
interface User {
  id: number;
  name: string;
}

const users: User[] = [
  { id: 1, name: "Alice" },
  { id: 2, name: "Bob" }
];

const usersById = users.reduce<Record<number, User>>((acc, user) => {
  acc[user.id] = user;
  return acc;
}, {});

// { 1: { id: 1, name: "Alice" }, 2: { id: 2, name: "Bob" } }
```

**Exemple 7 : Aplatir tableau**
```ts
const nested = [[1, 2], [3, 4], [5]];

const flat = nested.reduce<number[]>((acc, arr) => {
  return acc.concat(arr);
}, []);

// [1, 2, 3, 4, 5]

// Ou plus simple avec .flat()
const flat2 = nested.flat();
```

### 4.4 Chaînage de méthodes

**Exemple 1 : Pipeline de transformation**
```ts
const users = [
  { id: 1, name: "Alice", age: 25, isActive: true },
  { id: 2, name: "Bob", age: 17, isActive: false },
  { id: 3, name: "Claire", age: 30, isActive: true }
];

const activeAdultNames = users
  .filter(u => u.isActive)
  .filter(u => u.age >= 18)
  .map(u => u.name);

// ["Alice", "Claire"]
```

**Exemple 2 : Calcul de panier**
```ts
interface CartItem {
  name: string;
  price: number;
  quantity: number;
}

const cart: CartItem[] = [
  { name: "Laptop", price: 1000, quantity: 1 },
  { name: "Mouse", price: 25, quantity: 2 },
  { name: "Keyboard", price: 75, quantity: 1 }
];

const total = cart
  .map(item => item.price * item.quantity)
  .reduce((sum, price) => sum + price, 0);

// 1125
```

**Exemple 3 : Transformation complexe**
```ts
const transactions = [
  { type: "income", amount: 1000 },
  { type: "expense", amount: 200 },
  { type: "income", amount: 500 },
  { type: "expense", amount: 150 }
];

const balance = transactions
  .map(t => t.type === "income" ? t.amount : -t.amount)
  .reduce((sum, amount) => sum + amount, 0);

// 1150
```

### 4.5 Autres méthodes utiles

**find et findIndex :**
```ts
const users = [
  { id: 1, name: "Alice" },
  { id: 2, name: "Bob" }
];

const user = users.find(u => u.id === 2);
// { id: 2, name: "Bob" }

const index = users.findIndex(u => u.id === 2);
// 1
```

**some et every :**
```ts
const numbers = [1, 2, 3, 4, 5];

const hasEven = numbers.some(n => n % 2 === 0);
// true

const allPositive = numbers.every(n => n > 0);
// true
```

**sort (⚠️ mutable) :**
```ts
const numbers = [3, 1, 4, 1, 5, 9];

// ❌ Mutation
numbers.sort((a, b) => a - b);

// ✅ Immutable
const sorted = [...numbers].sort((a, b) => a - b);
// [1, 1, 3, 4, 5, 9]
```

**flatMap :**
```ts
const users = [
  { name: "Alice", hobbies: ["reading", "running"] },
  { name: "Bob", hobbies: ["gaming"] }
];

const allHobbies = users.flatMap(u => u.hobbies);
// ["reading", "running", "gaming"]
```

## 5. Erreurs et pièges à éviter

### 5.1 Erreurs fréquentes

| Erreur | Description | Impact | Solution |
|--------|-------------|--------|----------|
| **Oublier le return** | `map(x => { x * 2 })` sans return | undefined dans tableau | `map(x => x * 2)` ou `=> { return x * 2 }` |
| **Muter dans map** | `map(x => { x.value = 2; return x })` | Mutation de l'original | Copier avec spread |
| **Chaîner sans besoin** | `.map().map().map()` | Performance | Combiner en une passe |
| **reduce sans initial** | `reduce((a, b) => a + b)` | Bugs sur tableau vide | Toujours fournir valeur initiale |
| **forEach au lieu de map** | Utiliser forEach pour transformer | Pas de retour | Utiliser map |

### 5.2 Pièges courants

**Piège 1 : Return implicite avec `{}`**
```ts
// ❌ Retourne undefined
const doubled = numbers.map(n => {
  n * 2
});

// ✅ Return explicite
const doubled = numbers.map(n => {
  return n * 2;
});

// ✅ Return implicite (sans {})
const doubled = numbers.map(n => n * 2);
```

**Piège 2 : Mutation dans map**
```ts
const users = [{ name: "Alice", age: 25 }];

// ❌ Mute l'original
const updated = users.map(u => {
  u.age = 26;
  return u;
});

// ✅ Immutable
const updated = users.map(u => ({ ...u, age: 26 }));
```

**Piège 3 : filter avec index**
```ts
// ❌ Indices décalés après filter
const numbers = [1, 2, 3, 4];
const evens = numbers.filter((n, i) => i % 2 === 0);
// [1, 3] ⚠️ filtre sur index, pas valeur

// ✅ Filtrer sur valeur
const evens = numbers.filter(n => n % 2 === 0);
// [2, 4]
```

**Piège 4 : reduce sans valeur initiale**
```ts
const numbers = [1, 2, 3];

// ⚠️ Fonctionne mais risqué
const sum = numbers.reduce((a, b) => a + b);

// ✅ Toujours fournir initial
const sum = numbers.reduce((a, b) => a + b, 0);

// ❌ Crash sur tableau vide
[].reduce((a, b) => a + b); // TypeError

// ✅ Safe
[].reduce((a, b) => a + b, 0); // 0
```

**Piège 5 : Performance avec chaînage**
```ts
// ❌ Multiple passes
const result = arr
  .map(x => x * 2)
  .filter(x => x > 10)
  .map(x => x + 1);

// ✅ Une seule passe avec reduce
const result = arr.reduce<number[]>((acc, x) => {
  const doubled = x * 2;
  if (doubled > 10) {
    acc.push(doubled + 1);
  }
  return acc;
}, []);
```

### 5.3 Impacts sur les logiciels/webapps

| Problème | Conséquence |
|----------|-------------|
| **Mutation avec map** | Bugs imprévisibles, état corrompu |
| **Chaînage excessif** | Performance dégradée (gros tableaux) |
| **Pas de valeur initiale reduce** | Crash sur tableaux vides |
| **forEach au lieu de map** | Code moins fonctionnel, moins chainable |
| **Boucles au lieu de méthodes** | Code moins lisible, plus de bugs |

## 6. Résumé de l'essentiel

### Points clés à retenir

1. **map : Transformer**
   - Transforme chaque élément
   - Retourne nouveau tableau (même longueur)
   - `array.map(x => transformation)`

2. **filter : Filtrer**
   - Garde éléments respectant condition
   - Retourne nouveau tableau (≤ longueur)
   - `array.filter(x => condition)`

3. **reduce : Réduire**
   - Réduit à une seule valeur
   - Très flexible et puissant
   - `array.reduce((acc, x) => ..., initial)`

4. **Avantages programmation fonctionnelle**
   - Plus lisible et déclaratif
   - Immutable par défaut
   - Chainable
   - Moins de bugs

### Cheat Sheet

```ts
// Transformer
arr.map(x => x * 2)

// Filtrer
arr.filter(x => x > 10)

// Somme
arr.reduce((sum, x) => sum + x, 0)

// Trouver
arr.find(x => x.id === 1)

// Tester
arr.some(x => x > 10)    // Au moins 1
arr.every(x => x > 0)    // Tous

// Chaîner
arr
  .filter(x => x.active)
  .map(x => x.name)
  .sort()
```

### Checklist bonnes pratiques

- [ ] Préférer map/filter/reduce aux boucles for
- [ ] Toujours retourner dans map
- [ ] Ne pas muter dans map/filter
- [ ] Fournir valeur initiale à reduce
- [ ] Chaîner quand cela améliore la lisibilité
- [ ] Utiliser find au lieu de filter[0]
- [ ] Utiliser some/every pour tests booléens
- [ ] Copier avant sort ([...arr].sort())

### Quand utiliser quoi ?

| Besoin | Méthode |
|--------|---------|
| Transformer chaque élément | `map` |
| Garder certains éléments | `filter` |
| Calculer somme/moyenne | `reduce` |
| Trouver 1 élément | `find` |
| Vérifier si au moins 1 | `some` |
| Vérifier si tous | `every` |
| Grouper données | `reduce` |
| Aplatir tableau | `flat` / `flatMap` |
| Trier | `sort` (avec copie) |

---

**En une phrase :**

> Les méthodes fonctionnelles `map` (transformer), `filter` (filtrer) et `reduce` (réduire) permettent de manipuler des tableaux de manière déclarative, immutable et chainable, remplaçant avantageusement les boucles traditionnelles pour produire un code plus lisible, maintenable et moins sujet aux bugs.
