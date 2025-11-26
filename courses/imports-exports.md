# Imports et Exports Organisés en TypeScript

## 1. Contexte et définitions des concepts clés

### 1.1 Qu'est-ce qu'un module ?

Un **module** est un fichier contenant du code (fonctions, classes, variables, types) qui peut être réutilisé dans d'autres fichiers.

**Avant les modules :**
- Tout dans un seul fichier
- Variables globales (pollution du scope)
- Ordre de chargement des scripts important
- Conflits de noms

**Avec les modules :**
- Code organisé en fichiers séparés
- Isolation du scope
- Dépendances explicites
- Réutilisabilité

### 1.2 Import vs Export

**Export** : Rendre disponible du code pour d'autres modules
```ts
export function add(a: number, b: number): number {
  return a + b;
}
```

**Import** : Utiliser du code d'un autre module
```ts
import { add } from './math';
```

### 1.3 Définitions importantes

- **Module** : Fichier avec code exportable/importable
- **Named export** : Export avec nom spécifique
- **Default export** : Export par défaut (un seul par fichier)
- **Namespace import** : Importer tout sous un nom
- **Re-export** : Exporter depuis un point central
- **Barrel file** : Fichier `index.ts` centralisant les exports
- **Module resolution** : Algorithme de recherche des modules
- **Side effects** : Code exécuté lors de l'import

## 2. Quand et pourquoi utiliser les imports/exports

### 2.1 Pourquoi organiser avec des modules ?

**Avantages :**
- ✅ **Séparation des responsabilités** : Chaque fichier a un rôle clair
- ✅ **Réutilisabilité** : Utiliser le même code partout
- ✅ **Maintenabilité** : Plus facile de trouver et modifier
- ✅ **Testabilité** : Tester chaque module isolément
- ✅ **Collaboration** : Plusieurs développeurs sans conflits
- ✅ **Lazy loading** : Charger code à la demande
- ✅ **Tree-shaking** : Bundler supprime code non utilisé

### 2.2 Quand utiliser named exports

**Utilisez named exports pour :**
- Plusieurs exports dans un fichier
- Fonctions utilitaires
- Types et interfaces
- Constantes et configurations

```ts
// utils.ts
export function formatDate(date: Date): string { }
export function formatCurrency(amount: number): string { }
export const API_URL = "https://api.example.com";
```

### 2.3 Quand utiliser default export

**Utilisez default export pour :**
- Un seul export principal par fichier
- Classes principales
- Composants React/Angular
- Modules avec un concept principal

```ts
// User.ts
export default class User {
  constructor(public name: string) {}
}
```

**⚠️ Attention :** Default exports compliquent le refactoring et l'auto-import. Beaucoup de guides modernes recommandent de les éviter.

### 2.4 Organisation recommandée

**Structure de dossiers :**
```
src/
├── models/
│   ├── user.ts
│   ├── product.ts
│   └── index.ts       (barrel)
├── services/
│   ├── user.service.ts
│   ├── auth.service.ts
│   └── index.ts
├── utils/
│   ├── date.ts
│   ├── string.ts
│   └── index.ts
└── index.ts           (point d'entrée)
```

## 3. Comment cela se passe du point de vue matériel

### 3.1 Module resolution

**Étapes de résolution d'un import :**

```ts
import { User } from './models/user';
```

1. **TypeScript compiler** cherche le fichier
2. **Extensions tentées** : `.ts`, `.tsx`, `.d.ts`
3. **Résolution relative** : `./` ou `../`
4. **Vérification** : Fichier existe ?
5. **Parse et type-check** : Analyse du module
6. **Génération JS** : Conversion en require/import JS

**Algorithme de recherche :**
```
'./models/user'
→ ./models/user.ts
→ ./models/user.tsx
→ ./models/user.d.ts
→ ./models/user/index.ts
→ Erreur si rien trouvé
```

### 3.2 Compilation ES Modules vs CommonJS

**ES Modules (moderne) :**
```ts
// TypeScript
import { add } from './math';

// JavaScript généré (ES6)
import { add } from './math.js';
```

**CommonJS (Node.js traditionnel) :**
```ts
// TypeScript
import { add } from './math';

// JavaScript généré (CommonJS)
const { add } = require('./math');
```

**Configuration dans tsconfig.json :**
```json
{
  "compilerOptions": {
    "module": "ES2020",      // ou "CommonJS"
    "moduleResolution": "node"
  }
}
```

### 3.3 Tree-shaking

**Avec named exports (tree-shakable) :**
```ts
// utils.ts
export function used() { }
export function unused() { }

// app.ts
import { used } from './utils';
```

**Bundler (webpack, vite) :**
- Analyse les imports
- Détecte `unused` n'est jamais importé
- Supprime `unused` du bundle final
- Bundle plus petit ✅

**Avec default export (plus difficile) :**
```ts
// utils.ts
export default {
  used: () => {},
  unused: () => {}
};

// Tout l'objet est importé, pas de tree-shaking
```

### 3.4 Circular dependencies

**Problème des imports circulaires :**
```
A.ts imports B.ts
B.ts imports A.ts
→ Erreur ou comportement undefined
```

**Détection :**
```
┌──────┐
│  A   │──→ imports B
└──┬───┘
   │    ┌──────┐
   └────│  B   │──→ imports A
        └──────┘
```

**Solution :**
- Extraire code commun dans C
- Utiliser injection de dépendances
- Réorganiser l'architecture

## 4. Exemples et cas concrets en TypeScript

### 4.1 Named exports et imports

**Exporter plusieurs éléments :**
```ts
// math.ts
export function add(a: number, b: number): number {
  return a + b;
}

export function subtract(a: number, b: number): number {
  return a - b;
}

export const PI = 3.14159;

export type MathOperation = (a: number, b: number) => number;
```

**Importer sélectivement :**
```ts
// app.ts
import { add, PI } from './math';

console.log(add(5, 3));  // 8
console.log(PI);         // 3.14159
```

**Importer tout sous un namespace :**
```ts
import * as Math from './math';

console.log(Math.add(5, 3));
console.log(Math.PI);
```

**Renommer lors de l'import :**
```ts
import { add as sum } from './math';

console.log(sum(5, 3));  // 8
```

### 4.2 Default exports et imports

**Export default :**
```ts
// User.ts
export default class User {
  constructor(public name: string, public email: string) {}
  
  greet(): string {
    return `Hello, ${this.name}!`;
  }
}
```

**Import default :**
```ts
// app.ts
import User from './User';

const user = new User("Alice", "alice@example.com");
console.log(user.greet());
```

**Mélanger default et named :**
```ts
// config.ts
export const API_URL = "https://api.example.com";
export const TIMEOUT = 5000;

export default {
  apiUrl: API_URL,
  timeout: TIMEOUT,
  retries: 3
};

// app.ts
import config, { API_URL } from './config';
```

### 4.3 Barrel files (index.ts)

**Organisation avec barrels :**

```ts
// models/user.ts
export interface User {
  id: number;
  name: string;
}

// models/product.ts
export interface Product {
  id: number;
  name: string;
  price: number;
}

// models/index.ts (barrel)
export * from './user';
export * from './product';

// ou avec renommage
export { User } from './user';
export { Product } from './product';
```

**Utilisation simplifiée :**
```ts
// ❌ Sans barrel
import { User } from './models/user';
import { Product } from './models/product';

// ✅ Avec barrel
import { User, Product } from './models';
```

### 4.4 Re-exports organisés

**Point d'entrée centralisé :**
```ts
// src/index.ts
export { User, Admin } from './models/user';
export { Product, Category } from './models/product';
export { AuthService } from './services/auth.service';
export { UserService } from './services/user.service';
export * from './utils';

// Types uniquement
export type { LoginCredentials, UserProfile } from './types';
```

**Consommation externe :**
```ts
// Application utilisant votre bibliothèque
import { User, AuthService, formatDate } from 'my-library';
```

### 4.5 Import types vs valeurs

**Import types seulement (optimisation) :**
```ts
// types.ts
export interface User {
  id: number;
  name: string;
}

// app.ts
// ✅ Import type seulement (supprimé à la compilation)
import type { User } from './types';

const user: User = { id: 1, name: "Alice" };
```

**Import valeurs et types :**
```ts
// user.ts
export interface User {
  id: number;
  name: string;
}

export const DEFAULT_USER: User = {
  id: 0,
  name: "Guest"
};

// app.ts
import { User, DEFAULT_USER } from './user';
// ou
import type { User } from './user';
import { DEFAULT_USER } from './user';
```

### 4.6 Dynamic imports (lazy loading)

**Import dynamique :**
```ts
// Chargement à la demande
async function loadUser() {
  const { User } = await import('./models/user');
  return new User("Alice");
}

// Utilisation
const user = await loadUser();
```

**Cas d'usage :**
```ts
// Route splitting (Angular/React)
const routes = [
  {
    path: '/admin',
    loadChildren: () => import('./admin/admin.module')
  }
];

// Feature conditionnelle
if (needsHeavyLibrary) {
  const { processData } = await import('./heavy-lib');
  processData();
}
```

### 4.7 Path mapping (alias)

**Configuration tsconfig.json :**
```json
{
  "compilerOptions": {
    "baseUrl": "./src",
    "paths": {
      "@models/*": ["models/*"],
      "@services/*": ["services/*"],
      "@utils/*": ["utils/*"],
      "@/*": ["*"]
    }
  }
}
```

**Utilisation :**
```ts
// ❌ Sans alias (relatif)
import { User } from '../../../models/user';

// ✅ Avec alias (absolu)
import { User } from '@models/user';
import { formatDate } from '@utils/date';
```

### 4.8 Cas concret : Architecture en couches

**Structure complète :**
```
src/
├── models/
│   ├── user.model.ts
│   ├── product.model.ts
│   └── index.ts
├── repositories/
│   ├── user.repository.ts
│   ├── product.repository.ts
│   └── index.ts
├── services/
│   ├── user.service.ts
│   ├── auth.service.ts
│   └── index.ts
├── controllers/
│   ├── user.controller.ts
│   └── index.ts
└── index.ts
```

**models/user.model.ts :**
```ts
export interface User {
  id: number;
  email: string;
  name: string;
}

export interface CreateUserDTO {
  email: string;
  password: string;
  name: string;
}
```

**models/index.ts :**
```ts
export * from './user.model';
export * from './product.model';
```

**repositories/user.repository.ts :**
```ts
import type { User, CreateUserDTO } from '@models';

export class UserRepository {
  async findById(id: number): Promise<User | null> {
    // ...
  }
  
  async create(dto: CreateUserDTO): Promise<User> {
    // ...
  }
}
```

**services/user.service.ts :**
```ts
import { UserRepository } from '@repositories';
import type { User, CreateUserDTO } from '@models';

export class UserService {
  constructor(private userRepo: UserRepository) {}
  
  async createUser(dto: CreateUserDTO): Promise<User> {
    return this.userRepo.create(dto);
  }
}
```

**Point d'entrée src/index.ts :**
```ts
// Re-exports publics
export { User, CreateUserDTO } from './models';
export { UserService } from './services';

// Types seulement
export type { UserRepository } from './repositories';
```

## 5. Erreurs et pièges à éviter

### 5.1 Erreurs fréquentes

| Erreur | Description | Impact | Solution |
|--------|-------------|--------|----------|
| **Import circulaire** | A import B qui import A | undefined, erreurs | Refactorer architecture |
| **Default export inconsistant** | Mix default et named | Confusion | Choisir une convention |
| **Chemins relatifs profonds** | `../../../models` | Illisible, fragile | Utiliser path aliases |
| **Import side-effects oublié** | Ordre import important | Bugs subtils | Documenter dependencies |
| **Extension .ts dans import** | `import './file.ts'` | Erreur bundler | Ne jamais mettre extension |

### 5.2 Pièges courants

**Piège 1 : Oublier l'extension en JS généré**
```ts
// ❌ TypeScript
import { User } from './user.ts';

// ✅ TypeScript (sans extension)
import { User } from './user';

// JavaScript généré dépend de la config
// ES Modules : import { User } from './user.js';
// CommonJS : const { User } = require('./user');
```

**Piège 2 : Import circulaire non détecté**
```ts
// user.ts
import { Product } from './product';
export class User {
  products: Product[];
}

// product.ts
import { User } from './user';
export class Product {
  owner: User;
}

// ⚠️ Circular dependency !

// ✅ Solution : types dans fichier séparé
// types.ts
export interface User {
  products: Product[];
}
export interface Product {
  owner: User;
}
```

**Piège 3 : Barrel file avec side-effects**
```ts
// config.ts
console.log("Config loaded"); // Side-effect
export const API_URL = "...";

// index.ts
export * from './config';

// app.ts
import { API_URL } from './';
// → "Config loaded" s'affiche même si on n'utilise que API_URL
```

**Piège 4 : Default export et refactoring**
```ts
// user.ts
export default class User {}

// app.ts
import User from './user';

// ⚠️ Renommer la classe ne renomme pas l'import !
// ⚠️ Auto-import peut donner n'importe quel nom

// ✅ Named export
export class User {}
import { User } from './user'; // Refactoring safe
```

**Piège 5 : Import tout avec side-effects**
```ts
// utils.ts
console.log("Utils loaded");
export function add() {}
export function multiply() {}

// ❌ Import tout
import * as Utils from './utils';
// → "Utils loaded" s'affiche
// → Tree-shaking impossible

// ✅ Import sélectif
import { add } from './utils';
```

### 5.3 Impacts sur les logiciels/webapps

| Problème | Conséquence |
|----------|-------------|
| **Imports circulaires** | undefined values, crashes |
| **Pas d'organisation** | Code difficile à maintenir, bugs |
| **Chemins relatifs profonds** | Refactoring difficile, erreurs |
| **Pas de tree-shaking** | Bundle JavaScript trop gros |
| **Side-effects non contrôlés** | Comportement imprévisible |
| **Mix default/named** | Confusion, incohérence |

## 6. Résumé de l'essentiel

### Points clés à retenir

1. **Modules = Fichiers isolés**
   - Scope privé par défaut
   - Export pour rendre public
   - Import pour utiliser

2. **Named exports (recommandé)**
   - Plusieurs exports par fichier
   - Refactoring safe
   - Tree-shakable

3. **Default exports (éviter)**
   - Un seul par fichier
   - Refactoring difficile
   - Moins tree-shakable

4. **Organisation**
   - Barrel files pour simplifier imports
   - Path aliases pour éviter `../../../`
   - Architecture en couches

### Syntaxe essentielle

```ts
// Named exports
export const API_URL = "...";
export function add(a, b) { }
export class User { }
export type UserDTO = { };

// Named imports
import { add, User } from './module';
import { add as sum } from './module';
import * as Module from './module';

// Default export
export default class User { }

// Default import
import User from './module';

// Type import
import type { User } from './module';

// Dynamic import
const { User } = await import('./module');

// Barrel
export * from './user';
export { User } from './user';
```

### Checklist bonnes pratiques

- [ ] Préférer named exports à default exports
- [ ] Utiliser barrel files (index.ts) pour simplifier
- [ ] Configurer path aliases (@models, @services)
- [ ] Un fichier = une responsabilité claire
- [ ] Éviter imports circulaires
- [ ] Import types avec `import type` quand possible
- [ ] Ne jamais mettre extension dans import
- [ ] Organiser en couches (models, services, controllers)
- [ ] Exporter types et interfaces séparément
- [ ] Documenter side-effects si nécessaires

### Structure recommandée

```
src/
├── models/          (Types, interfaces)
│   └── index.ts
├── services/        (Logique métier)
│   └── index.ts
├── repositories/    (Accès données)
│   └── index.ts
├── controllers/     (API endpoints)
│   └── index.ts
├── utils/           (Fonctions utilitaires)
│   └── index.ts
├── types/           (Types globaux)
│   └── index.ts
└── index.ts         (Point d'entrée)
```

### tsconfig.json recommandé

```json
{
  "compilerOptions": {
    "module": "ES2020",
    "moduleResolution": "node",
    "baseUrl": "./src",
    "paths": {
      "@models/*": ["models/*"],
      "@services/*": ["services/*"],
      "@utils/*": ["utils/*"]
    },
    "esModuleInterop": true,
    "resolveJsonModule": true,
    "isolatedModules": true
  }
}
```

---

**En une phrase :**

> Les imports et exports TypeScript permettent d'organiser le code en modules isolés et réutilisables, avec named exports recommandés pour leur robustesse au refactoring et leur compatibilité avec le tree-shaking, le tout organisé via barrel files et path aliases pour une architecture claire et maintenable.
