# VSCode : Debugging et Extensions Essentielles

## 1. Contexte et définitions des concepts clés

### 1.1 Qu'est-ce que VSCode ?

**Visual Studio Code** (VSCode) est un éditeur de code gratuit, open-source, développé par Microsoft. C'est l'un des IDE (Integrated Development Environment) les plus populaires pour le développement web.

**Définitions importantes :**

- **IDE** : Environnement de développement intégré (éditeur + outils de développement)
- **Extension** : Plugin qui ajoute des fonctionnalités à VSCode
- **Debugger** : Outil permettant d'inspecter et corriger le code en cours d'exécution
- **IntelliSense** : Auto-complétion intelligente du code
- **Breakpoint** : Point d'arrêt dans le code pour inspecter l'état du programme
- **Workspace** : Configuration spécifique à un projet

### 1.2 Pourquoi VSCode ?

**Avantages :**
- ✅ Gratuit et open-source
- ✅ Léger et rapide
- ✅ Écosystème d'extensions immense
- ✅ Git intégré
- ✅ Debugging puissant
- ✅ Support TypeScript natif
- ✅ Multi-plateforme (Windows, Mac, Linux)
- ✅ Mises à jour fréquentes

## 2. Quand et pourquoi utiliser ces fonctionnalités

### 2.1 Le Debugging : Pourquoi c'est essentiel

**Sans debugger :**
```ts
function calculateTotal(items: number[]): number {
  let total = 0;
  for (const item of items) {
    total += item;
  }
  console.log('Total:', total); // ❌ Debug à l'ancienne
  return total;
}
```

**Problèmes :**
- Pollue le code avec des `console.log`
- Pas d'inspection de l'état complet
- Difficile de suivre l'exécution ligne par ligne
- Oubli de nettoyer les logs

**Avec debugger :**
- ✅ Pause l'exécution où tu veux
- ✅ Inspecte toutes les variables
- ✅ Parcourt le code pas à pas
- ✅ Pas de pollution du code

### 2.2 Les Extensions : Pourquoi c'est important

**Productivité :**
- Évite les erreurs de typage
- Formate le code automatiquement
- Détecte les bugs avant exécution
- Accélère l'écriture du code

**Qualité :**
- Applique les bonnes pratiques
- Maintient la cohérence du code
- Facilite le refactoring

## 3. Comment cela se passe du point de vue matériel

### 3.1 Architecture du Debugger

```
┌─────────────────────────────┐
│      VSCode Interface       │
│  (Breakpoints, Variables)   │
└──────────────┬──────────────┘
               │
┌──────────────▼──────────────┐
│     Debug Adapter Protocol  │
│         (DAP)               │
└──────────────┬──────────────┘
               │
┌──────────────▼──────────────┐
│        Debug Engine          │
│   (V8 Inspector / Node)     │
└──────────────┬──────────────┘
               │
┌──────────────▼──────────────┐
│    Process en cours          │
│    d'exécution (Node.js)    │
└─────────────────────────────┘
```

**Ce qui se passe en mémoire :**

1. **Breakpoint placé** : VSCode marque l'adresse mémoire de la ligne
2. **Exécution lancée** : Node.js démarre en mode debug
3. **Breakpoint atteint** : CPU s'arrête, état gelé
4. **Inspection** : VSCode lit la RAM pour afficher les variables
5. **Step-by-step** : VSCode demande au CPU d'avancer d'une instruction
6. **Reprise** : Exécution normale reprend

### 3.2 Extensions : Comment ça fonctionne

**Extension = Code JavaScript qui s'exécute dans VSCode**

```
┌──────────────────────────────┐
│    VSCode Core (Electron)    │
│  ┌────────────────────────┐  │
│  │  Extension Host        │  │
│  │  ┌──────────────────┐  │  │
│  │  │  Extension 1     │  │  │
│  │  │  Extension 2     │  │  │
│  │  │  Extension 3     │  │  │
│  │  └──────────────────┘  │  │
│  └────────────────────────┘  │
└──────────────────────────────┘
```

**Performance :**
- Extensions s'exécutent dans un processus séparé
- N'impactent pas l'éditeur principal
- Communication via API VSCode

## 4. Exemples et cas concrets en TypeScript

### 4.1 Configuration du Debugging

**Créer launch.json :**

1. Ouvrir le panneau Debug (Ctrl+Shift+D)
2. Cliquer "create a launch.json file"
3. Choisir "Node.js"

**.vscode/launch.json :**
```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "node",
      "request": "launch",
      "name": "Debug TypeScript",
      "skipFiles": ["<node_internals>/**"],
      "program": "${workspaceFolder}/src/index.ts",
      "preLaunchTask": "tsc: build - tsconfig.json",
      "outFiles": ["${workspaceFolder}/dist/**/*.js"],
      "sourceMaps": true
    },
    {
      "type": "node",
      "request": "launch",
      "name": "Debug Current File",
      "skipFiles": ["<node_internals>/**"],
      "program": "${file}",
      "runtimeArgs": ["-r", "ts-node/register"],
      "console": "integratedTerminal"
    },
    {
      "type": "node",
      "request": "attach",
      "name": "Attach to Process",
      "port": 9229,
      "skipFiles": ["<node_internals>/**"]
    }
  ]
}
```

### 4.2 Exemple : Debugging en action

**Code avec bug :**
```ts
// calculator.ts
type Operation = 'add' | 'subtract' | 'multiply' | 'divide';

interface CalculatorInput {
  a: number;
  b: number;
  operation: Operation;
}

function calculate(input: CalculatorInput): number {
  switch (input.operation) {
    case 'add':
      return input.a + input.b;
    case 'subtract':
      return input.a - input.b;
    case 'multiply':
      return input.a * input.b;
    case 'divide':
      // 🐛 Bug : pas de gestion division par zéro
      return input.a / input.b;
    default:
      throw new Error('Operation unknown');
  }
}

// Test
const result = calculate({ a: 10, b: 0, operation: 'divide' });
console.log('Result:', result); // Infinity ❌
```

**Debugging :**

1. **Placer un breakpoint** : Clic sur la marge gauche ligne 18
2. **Lancer le debug** : F5 ou bouton Play vert
3. **Inspecter les variables** :
   - `input.a` = 10
   - `input.b` = 0
   - `input.operation` = 'divide'
4. **Identifier le problème** : Division par zéro non gérée
5. **Corriger** :

```ts
case 'divide':
  if (input.b === 0) {
    throw new Error('Division by zero');
  }
  return input.a / input.b;
```

### 4.3 Techniques de Debugging avancées

**1. Conditional Breakpoints :**
```ts
// S'arrêter seulement si userId === 123
function processUser(userId: number) {
  // Breakpoint conditionnel : userId === 123
  const user = getUserById(userId);
  // ...
}
```

**2. Logpoints (console.log sans modifier le code) :**
```ts
// Au lieu de :
console.log('Value:', value);

// Utiliser un Logpoint : Click droit > Add Logpoint
// Message: Value: {value}
```

**3. Watch Expressions :**
```
// Panneau WATCH
user.isActive
cart.items.length
total > 100
```

**4. Call Stack :**
```
// Voir la pile d'appels
main()
  └─ processOrder()
      └─ calculateTotal()  ← Breakpoint ici
          └─ applyDiscount()
```

### 4.4 Extensions Essentielles

**Installation :**
```
Ctrl+Shift+X → Rechercher → Installer
```

#### TypeScript & JavaScript

**1. ESLint**
```json
// .eslintrc.json
{
  "extends": ["eslint:recommended", "plugin:@typescript-eslint/recommended"],
  "parser": "@typescript-eslint/parser",
  "plugins": ["@typescript-eslint"],
  "rules": {
    "no-console": "warn",
    "@typescript-eslint/no-unused-vars": "error"
  }
}
```

**2. Prettier - Code formatter**
```json
// .prettierrc
{
  "semi": true,
  "trailingComma": "es5",
  "singleQuote": true,
  "printWidth": 80,
  "tabWidth": 2
}
```

**3. Error Lens**
- Affiche les erreurs inline directement dans l'éditeur
- Plus besoin d'ouvrir le panneau "Problems"

**4. GitLens**
- Voir qui a modifié chaque ligne
- Historique Git complet
- Comparaison de branches

**5. Path Intellisense**
```ts
// Auto-complétion des chemins
import { User } from './models/User'; // ✅ Suggère automatiquement
```

**6. Auto Rename Tag**
```html
<!-- Renomme automatiquement la balise fermante -->
<div>...</div>
<!-- Devient -->
<section>...</section>
```

**7. Bracket Pair Colorizer 2**
```ts
// Colore les paires de parenthèses
function nested() {
  if (condition) {
    for (let i = 0; i < 10; i++) {
      // Chaque niveau a une couleur
    }
  }
}
```

**8. Import Cost**
```ts
// Affiche la taille des imports
import lodash from 'lodash'; // 📦 72.4kB ⚠️
import debounce from 'lodash/debounce'; // 📦 2.1kB ✅
```

**9. Code Spell Checker**
```ts
// Détecte les fautes d'orthographe
const userName = 'John'; // ✅
const usrNam = 'Jane';   // ⚠️ Erreur orthographe ?
```

**10. REST Client**
```http
### Test API
GET http://localhost:3000/api/users
Content-Type: application/json

###
POST http://localhost:3000/api/users
Content-Type: application/json

{
  "name": "John Doe",
  "email": "john@example.com"
}
```

#### Framework Spécifiques

**11. Angular Language Service** (pour Angular)
**12. Vetur / Volar** (pour Vue.js)
**13. Tailwind CSS IntelliSense** (pour Tailwind)

### 4.5 Raccourcis Clavier Essentiels

| Raccourci | Action |
|-----------|--------|
| **F5** | Lancer le debugging |
| **F9** | Ajouter/retirer breakpoint |
| **F10** | Step Over (passer à la ligne suivante) |
| **F11** | Step Into (entrer dans la fonction) |
| **Shift+F11** | Step Out (sortir de la fonction) |
| **Ctrl+Shift+F5** | Redémarrer debugging |
| **Ctrl+P** | Ouvrir fichier rapidement |
| **Ctrl+Shift+P** | Palette de commandes |
| **Alt+↑/↓** | Déplacer ligne |
| **Ctrl+D** | Sélectionner mot suivant |
| **Ctrl+/** | Commenter/décommenter |
| **F2** | Renommer symbole |
| **Ctrl+Space** | Déclencher IntelliSense |
| **Ctrl+Shift+O** | Aller à un symbole |

### 4.6 Configuration Workspace

**.vscode/settings.json :**
```json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "typescript.tsdk": "node_modules/typescript/lib",
  "typescript.enablePromptUseWorkspaceTsdk": true,
  "files.exclude": {
    "**/.git": true,
    "**/node_modules": true,
    "**/dist": true
  },
  "search.exclude": {
    "**/node_modules": true,
    "**/dist": true
  }
}
```

**.vscode/extensions.json :**
```json
{
  "recommendations": [
    "dbaeumer.vscode-eslint",
    "esbenp.prettier-vscode",
    "usernamehw.errorlens",
    "eamodio.gitlens",
    "christian-kohler.path-intellisense",
    "ms-vscode.vscode-typescript-next"
  ]
}
```

## 5. Erreurs et pièges à éviter

### 5.1 Erreurs fréquentes avec le Debugger

| Erreur | Impact | Solution |
|--------|--------|----------|
| **Source maps manquantes** | Debugging sur JS compilé au lieu de TS | Activer `"sourceMap": true` dans tsconfig.json |
| **Breakpoints ignorés** | Code s'exécute sans s'arrêter | Vérifier que le code est bien compilé/exécuté |
| **Attach au mauvais process** | Debugging ne fonctionne pas | Vérifier le port dans launch.json |
| **Skip files incorrect** | Rentre dans node_modules | Configurer `skipFiles` correctement |

### 5.2 Pièges avec les Extensions

**Piège 1 : Trop d'extensions**
```
❌ Problème : VSCode devient lent
✅ Solution : Désactiver extensions inutiles par workspace
```

**Piège 2 : Conflits d'extensions**
```
❌ Prettier + Beautify en même temps
✅ Choisir un seul formatteur
```

**Piège 3 : Extensions obsolètes**
```
❌ Extensions non maintenues
✅ Vérifier les mises à jour régulièrement
```

### 5.3 Impacts sur la productivité

| Problème | Conséquence |
|----------|-------------|
| **Pas de debugger** | 10x plus de temps pour trouver bugs |
| **Code non formaté** | Revues de code difficiles, conflits Git |
| **Pas d'ESLint** | Bugs évitables en production |
| **Extensions mal configurées** | Éditeur lent, frustration |

## 6. Résumé de l'essentiel

### Points clés à retenir

1. **Debugging = Indispensable**
   - Remplace console.log
   - Inspecte l'état complet
   - Suit l'exécution pas à pas

2. **Extensions = Superpouvoir**
   - ESLint : qualité du code
   - Prettier : formatage automatique
   - GitLens : historique Git
   - Error Lens : erreurs visibles

3. **Configuration projet**
   - `.vscode/launch.json` : config debug
   - `.vscode/settings.json` : config éditeur
   - `.vscode/extensions.json` : extensions recommandées

4. **Raccourcis clavier**
   - Apprendre les principaux
   - Gain de temps énorme
   - Workflow fluide

### Extensions Minimales (Top 5)

1. **ESLint** : Qualité du code
2. **Prettier** : Formatage automatique
3. **Error Lens** : Erreurs visibles
4. **GitLens** : Historique Git
5. **Path Intellisense** : Auto-complétion chemins

### Workflow de Debugging Recommandé

```
1. ❌ Problème détecté
   └─ Reproduire le bug

2. 🔍 Placer breakpoint
   └─ Avant la ligne suspecte

3. ▶️ Lancer debugging (F5)
   └─ Attendre breakpoint

4. 👀 Inspecter variables
   └─ Panneau Variables/Watch

5. ➡️ Step by step (F10/F11)
   └─ Suivre l'exécution

6. 💡 Identifier la cause
   └─ Analyser les valeurs

7. ✅ Corriger le code
   └─ Relancer les tests

8. 🚀 Retirer breakpoints
   └─ Code propre
```

### Checklist Configuration VSCode

- [ ] VSCode installé (version récente)
- [ ] Extensions essentielles installées
- [ ] `.vscode/launch.json` créé
- [ ] `.vscode/settings.json` configuré
- [ ] ESLint et Prettier configurés
- [ ] Source maps activées (tsconfig.json)
- [ ] Raccourcis clavier appris
- [ ] Git intégré configuré

---

**En une phrase :**

> VSCode avec ses extensions et son debugger puissant transforme le développement en permettant de détecter les erreurs immédiatement, de corriger les bugs efficacement en inspectant l'état du programme en temps réel, et d'automatiser le formatage et la qualité du code pour une productivité maximale.
