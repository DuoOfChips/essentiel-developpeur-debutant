# Programme, Compilation, Transpilation et Exécution

## 1. Contexte et définitions des concepts clés

### 1.1 Qu'est-ce qu'un programme ?

Un **programme** est une suite d'instructions écrites dans un langage compréhensible par l'humain (code source), qui sera ensuite traduite en instructions exécutables par une machine.

**Définitions importantes :**

- **Code source** : le texte que tu écris en tant que développeur (ex : TypeScript, JavaScript, Python)
- **Compilation** : transformation du code source en code machine directement exécutable par le processeur
- **Transpilation** : transformation d'un code source d'un langage vers un autre langage de haut niveau (ex : TypeScript → JavaScript)
- **Interprétation** : exécution directe du code source ligne par ligne sans compilation préalable complète
- **Exécution** : le processus par lequel l'ordinateur effectue les instructions du programme

### 1.2 Les différents types de langages

| Type | Exemples | Caractéristique |
|------|----------|-----------------|
| **Compilé** | C, C++, Rust, Go | Code → Binaire exécutable direct |
| **Transpiré** | TypeScript, CoffeeScript | Code → Autre code de haut niveau |
| **Interprété** | JavaScript (dans navigateur), Python | Code → Exécuté directement |
| **Hybride** | Java, C# | Code → Bytecode → Interprété/JIT |

## 2. Quand et pourquoi utiliser ces concepts

### 2.1 Pourquoi compiler/transpiler ?

**Compilation** :
- Pour obtenir des performances maximales (code machine natif)
- Pour détecter les erreurs avant l'exécution
- Pour optimiser le code

**Transpilation** :
- Pour utiliser des fonctionnalités modernes non supportées partout (ex : TypeScript → JavaScript ES5)
- Pour écrire du code plus maintenable avec des types
- Pour garantir la compatibilité entre environnements

**Interprétation** :
- Pour un développement rapide et itératif
- Pour du code portable (fonctionne partout où l'interpréteur existe)
- Pour du scripting et prototypage

### 2.2 Le cas spécifique de TypeScript

TypeScript est **transpiré** vers JavaScript :

```
Code TypeScript (.ts) → Transpileur (tsc) → Code JavaScript (.js) → Exécution (Node.js ou navigateur)
```

**Pourquoi cette étape ?**
- Les navigateurs et Node.js ne comprennent que JavaScript
- TypeScript ajoute du typage statique pour détecter les erreurs en amont
- Le typage disparaît après transpilation (types effacés)

## 3. Comment cela se passe du point de vue matériel

### 3.1 Le parcours d'un programme TypeScript

**Étape 1 : Écriture du code**
```ts
// fichier: app.ts
function greet(name: string): string {
  return `Hello, ${name}!`;
}

console.log(greet("World"));
```

**Étape 2 : Transpilation (tsc)**

Le compilateur TypeScript (`tsc`) :
1. Parse le code source en AST (Abstract Syntax Tree)
2. Vérifie les types (erreurs ?)
3. Génère du JavaScript équivalent

```js
// fichier: app.js (généré)
function greet(name) {
  return `Hello, ${name}!`;
}

console.log(greet("World"));
```

**Étape 3 : Exécution par Node.js ou le navigateur**

1. Le moteur JavaScript (V8 pour Node.js/Chrome) :
   - Parse le JavaScript en AST
   - Compile en bytecode
   - Exécute via le JIT (Just-In-Time compiler)

2. Instructions machine :
   - CPU exécute les instructions natives
   - Mémoire RAM stocke les variables
   - Résultat affiché dans la console

### 3.2 Ce qui se passe dans l'ordinateur

```
┌─────────────────┐
│  Code TS (.ts)  │
└────────┬────────┘
         │ tsc (transpileur)
         ▼
┌─────────────────┐
│  Code JS (.js)  │
└────────┬────────┘
         │ Node.js / Navigateur
         ▼
┌─────────────────┐
│    Bytecode     │
└────────┬────────┘
         │ JIT Compiler
         ▼
┌─────────────────┐
│  Code Machine   │
└────────┬────────┘
         │ CPU
         ▼
┌─────────────────┐
│    Exécution    │
│  (RAM + CPU)    │
└─────────────────┘
```

**Détails matériels :**

1. **Disque dur/SSD** : stocke les fichiers `.ts` et `.js`
2. **RAM** : charge le code et les données en mémoire pendant l'exécution
3. **CPU** : exécute les instructions machine
4. **Cache CPU** : accélère l'accès aux données fréquentes
5. **Registres CPU** : stockent les valeurs temporaires pendant les calculs

## 4. Exemples et cas concrets en TypeScript

### 4.1 Exemple simple : calculatrice

**Code TypeScript :**
```ts
// calculator.ts
type Operation = '+' | '-' | '*' | '/';

function calculate(a: number, b: number, op: Operation): number {
  switch (op) {
    case '+': return a + b;
    case '-': return a - b;
    case '*': return a * b;
    case '/': 
      if (b === 0) throw new Error('Division par zéro');
      return a / b;
  }
}

console.log(calculate(10, 5, '+')); // 15
```

**Transpilation :**
```bash
tsc calculator.ts
```

**Résultat JavaScript :**
```js
// calculator.js
function calculate(a, b, op) {
  switch (op) {
    case '+': return a + b;
    case '-': return a - b;
    case '*': return a * b;
    case '/':
      if (b === 0) throw new Error('Division par zéro');
      return a / b;
  }
}

console.log(calculate(10, 5, '+')); // 15
```

**Exécution :**
```bash
node calculator.js
```

### 4.2 Exemple : Application web

**Workflow complet d'une app Angular/NestJS :**

```
1. Développement
   ├─ Écriture TypeScript (.ts)
   └─ Configuration (tsconfig.json)

2. Build (avant déploiement)
   ├─ Transpilation TS → JS
   ├─ Bundling (webpack/vite)
   ├─ Minification
   └─ Optimisation

3. Déploiement
   ├─ Serveur reçoit le JS compilé
   └─ Pas de TypeScript en production

4. Exécution
   ├─ Navigateur : exécute le JS dans le moteur V8
   └─ Serveur : Node.js exécute le JS backend
```

### 4.3 Cas concret : Détection d'erreur avant exécution

**Code avec erreur de type :**
```ts
function add(a: number, b: number): number {
  return a + b;
}

add("5", 10); // ❌ Erreur de transpilation
```

**Erreur détectée par tsc :**
```
error TS2345: Argument of type 'string' is not assignable to parameter of type 'number'.
```

**Avantage :** L'erreur est détectée **avant** l'exécution, pas au runtime !

### 4.4 Exemple : Configuration de transpilation

**tsconfig.json (fichier de configuration TypeScript) :**
```json
{
  "compilerOptions": {
    "target": "ES2020",           // Version JS cible
    "module": "commonjs",          // Système de modules
    "outDir": "./dist",            // Dossier de sortie
    "strict": true,                // Mode strict (typage fort)
    "esModuleInterop": true,       // Compatibilité imports
    "skipLibCheck": true           // Optimisation
  },
  "include": ["src/**/*"],         // Fichiers à compiler
  "exclude": ["node_modules"]      // Fichiers à ignorer
}
```

## 5. Erreurs et pièges à éviter

### 5.1 Erreurs fréquentes

| Erreur | Description | Impact | Solution |
|--------|-------------|--------|----------|
| **Oublier de transpiler** | Exécuter directement `.ts` avec Node.js | Crash immédiat | Toujours compiler : `tsc` puis `node` |
| **Types incorrects** | Ne pas respecter les types déclarés | Erreurs de compilation | Activer `strict: true` dans tsconfig |
| **Cibles incompatibles** | Transpiler vers ES5 mais utiliser des features ES2020 | Code cassé | Adapter `target` dans tsconfig |
| **Ignorer les erreurs** | Utiliser `@ts-ignore` partout | Bugs en production | Corriger les erreurs réelles |
| **Ne pas configurer tsconfig** | Utiliser les valeurs par défaut | Comportement imprévisible | Créer un tsconfig.json adapté |

### 5.2 Pièges courants

**Piège 1 : Confusion entre développement et production**
```ts
// ❌ Mauvais : importer des types en production
import { MyType } from './types'; // Les types n'existent pas en JS !

// ✅ Bon : séparer logique et types
import type { MyType } from './types';
```

**Piège 2 : Mauvaise configuration du module**
```json
// ❌ Pour Node.js moderne
{ "module": "ES6" }

// ✅ Pour Node.js moderne
{ "module": "commonjs" }
```

**Piège 3 : Ne pas versionner les fichiers générés**
```
# .gitignore
dist/        # ✅ Ne jamais commit les fichiers JS générés
build/       # ✅ Ils seront régénérés au build
*.js.map     # ✅ Source maps générées
```

### 5.3 Impacts sur les logiciels/webapps

| Problème | Conséquence sur l'app |
|----------|----------------------|
| **Transpilation échouée** | Déploiement impossible, application cassée |
| **Types mal définis** | Bugs silencieux en production, crashes inattendus |
| **Target JS trop ancien** | Fonctionnalités manquantes, polyfills nécessaires |
| **Target JS trop récent** | Incompatibilité avec vieux navigateurs/Node.js |
| **Build non optimisé** | Application lente, bundle trop gros |
| **Source maps manquantes** | Debug impossible en production |

## 6. Résumé de l'essentiel

### Points clés à retenir

1. **Programme = Instructions** : Un programme est une suite d'instructions pour l'ordinateur

2. **TypeScript → JavaScript** : TypeScript est transpiré (pas compilé directement en binaire)

3. **Pipeline de production** :
   ```
   Code TS → Transpilation (tsc) → Code JS → Exécution (Node/Browser)
   ```

4. **Avantages de TypeScript** :
   - Détection d'erreurs avant exécution
   - Meilleure maintenabilité
   - Auto-complétion IDE
   - Documentation via types

5. **Configuration essentielle** : `tsconfig.json` contrôle tout le processus

6. **Séparation dev/prod** :
   - Développement : fichiers `.ts` avec types
   - Production : fichiers `.js` sans types

### Commandes essentielles

```bash
# Compiler un fichier
tsc fichier.ts

# Compiler tout le projet
tsc

# Compiler en mode watch (auto-recompilation)
tsc --watch

# Exécuter le résultat
node fichier.js

# Ou directement avec ts-node (dev uniquement)
ts-node fichier.ts
```

### Checklist avant de commencer un projet

- [ ] TypeScript installé (`npm install -g typescript`)
- [ ] `tsconfig.json` configuré correctement
- [ ] `target` adapté à l'environnement d'exécution
- [ ] Mode `strict` activé pour maximum de sécurité
- [ ] `.gitignore` configuré pour exclure les fichiers générés
- [ ] Scripts de build dans `package.json`

---

**En une phrase :**

> Un programme TypeScript est du code que tu écris avec des types, qui est transpiré en JavaScript pur, puis exécuté par un moteur JavaScript (navigateur ou Node.js), en passant par plusieurs étapes de transformation jusqu'au code machine exécuté par le CPU.
