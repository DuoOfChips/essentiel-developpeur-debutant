# Node.js et NPM

## 1. Contexte et définitions des concepts clés

### 1.1 Qu'est-ce que Node.js ?

**Node.js** est un **environnement d'exécution JavaScript** qui permet de faire tourner du code JavaScript en dehors d'un navigateur, directement sur un serveur ou un ordinateur.

**Définitions importantes :**

- **Runtime** : Environnement qui permet d'exécuter du code (Node.js pour JavaScript côté serveur)
- **V8** : Moteur JavaScript open-source développé par Google (aussi utilisé dans Chrome)
- **Event Loop** : Mécanisme qui permet à Node.js de gérer plusieurs opérations en même temps sans bloquer
- **Asynchrone** : Capacité à exécuter plusieurs tâches sans attendre qu'une tâche se termine

### 1.2 Qu'est-ce que NPM ?

**NPM** (Node Package Manager) est :
- Le **gestionnaire de paquets** par défaut pour Node.js
- Un **registre public** contenant des millions de bibliothèques JavaScript
- Un **outil en ligne de commande** pour installer, gérer et publier des packages

**Concepts clés :**

- **Package** : Module réutilisable (bibliothèque de code)
- **Dépendance** : Package dont votre projet a besoin
- **package.json** : Fichier de configuration listant les dépendances
- **node_modules/** : Dossier contenant tous les packages installés

### 1.3 La relation Node.js ↔ NPM

```
Node.js = Moteur d'exécution (comme une voiture)
NPM = Gestionnaire de pièces détachées (garage)
```

Node.js exécute le code, NPM fournit les bibliothèques nécessaires.

## 2. Quand et pourquoi utiliser Node.js et NPM

### 2.1 Cas d'usage de Node.js

| Domaine | Utilisation | Exemple |
|---------|-------------|---------|
| **Backend API** | Créer des serveurs web | Express, NestJS |
| **CLI Tools** | Outils en ligne de commande | Angular CLI, Create React App |
| **Build Tools** | Automatisation de tâches | Webpack, Vite, Rollup |
| **Scripts** | Automatisation simple | Scripts de déploiement, data processing |
| **Real-time** | Applications temps réel | Chat, notifications live |
| **Microservices** | Architecture distribuée | Services découplés |

### 2.2 Pourquoi utiliser Node.js ?

**Avantages :**
- ✅ JavaScript côté serveur ET client (un seul langage)
- ✅ Très performant pour les I/O (entrées/sorties)
- ✅ Écosystème immense via NPM
- ✅ Event-driven et non-bloquant
- ✅ Idéal pour les applications en temps réel

**Limitations :**
- ❌ Moins adapté aux calculs CPU intensifs
- ❌ Gestion de la mémoire à surveiller
- ❌ Callback hell (résolu avec async/await)

### 2.3 Pourquoi utiliser NPM ?

**Avantages :**
- ✅ Accès à plus de 2 millions de packages
- ✅ Gestion automatique des dépendances
- ✅ Versioning sémantique
- ✅ Scripts personnalisés (`npm run build`)
- ✅ Partage facile de code entre projets

**Alternatives :**
- **Yarn** : Alternative à NPM (plus rapide, lock file)
- **pnpm** : Économise de l'espace disque

## 3. Comment cela se passe du point de vue matériel

### 3.1 Architecture de Node.js

```
┌─────────────────────────────────────────┐
│         Application JavaScript          │
└──────────────────┬──────────────────────┘
                   │
┌──────────────────▼──────────────────────┐
│            Node.js API                   │
│  (fs, http, crypto, path, etc.)         │
└──────────────────┬──────────────────────┘
                   │
┌──────────────────▼──────────────────────┐
│           V8 Engine (C++)                │
│  ┌─────────────────────────────────┐   │
│  │      JavaScript Runtime          │   │
│  └─────────────────────────────────┘   │
└──────────────────┬──────────────────────┘
                   │
┌──────────────────▼──────────────────────┐
│           libuv (C Library)              │
│  ┌──────────────┐  ┌─────────────────┐ │
│  │  Event Loop  │  │  Thread Pool    │ │
│  └──────────────┘  └─────────────────┘ │
└──────────────────┬──────────────────────┘
                   │
┌──────────────────▼──────────────────────┐
│       Système d'exploitation             │
│  (fichiers, réseau, processus)          │
└─────────────────────────────────────────┘
```

### 3.2 Fonctionnement de l'Event Loop

**Étape par étape :**

1. **JavaScript exécuté** : Code synchrone exécuté ligne par ligne
2. **Opération async rencontrée** : Déléguée à libuv (lecture fichier, requête HTTP)
3. **Event Loop surveille** : Vérifie en permanence si des opérations sont terminées
4. **Callback placé dans la queue** : Quand l'opération est finie
5. **Callback exécuté** : Quand la pile d'exécution est vide

**Exemple :**
```ts
console.log('1. Début');

setTimeout(() => {
  console.log('2. Timeout');
}, 0);

console.log('3. Fin');

// Résultat :
// 1. Début
// 3. Fin
// 2. Timeout
```

### 3.3 Ce qui se passe lors de l'installation NPM

```
┌──────────────────────────┐
│  npm install express     │
└───────────┬──────────────┘
            │
            ▼
┌──────────────────────────┐
│  1. Lire package.json    │
└───────────┬──────────────┘
            │
            ▼
┌──────────────────────────┐
│  2. Résoudre dépendances │
│     (arbre de dépendances)│
└───────────┬──────────────┘
            │
            ▼
┌──────────────────────────┐
│  3. Télécharger packages │
│     (depuis registry NPM) │
└───────────┬──────────────┘
            │
            ▼
┌──────────────────────────┐
│  4. Extraire dans         │
│     node_modules/         │
└───────────┬──────────────┘
            │
            ▼
┌──────────────────────────┐
│  5. Créer package-lock   │
│     (verrouillage versions)│
└──────────────────────────┘
```

**Sur le disque dur :**
- Les packages sont téléchargés depuis internet
- Stockés dans `node_modules/` (peut peser plusieurs Go)
- `package-lock.json` garantit les versions exactes

## 4. Exemples et cas concrets en TypeScript

### 4.1 Installation et configuration

**Installer Node.js :**
```bash
# Vérifier l'installation
node --version    # v20.11.0
npm --version     # 10.2.4
```

**Initialiser un projet :**
```bash
# Créer package.json
npm init -y

# Installer TypeScript
npm install --save-dev typescript

# Installer types Node.js
npm install --save-dev @types/node

# Créer tsconfig.json
npx tsc --init
```

### 4.2 Exemple : Serveur HTTP simple

**Code TypeScript :**
```ts
// server.ts
import http from 'http';

const PORT = 3000;

const server = http.createServer((req, res) => {
  res.writeHead(200, { 'Content-Type': 'text/plain' });
  res.end('Hello from Node.js!\n');
});

server.listen(PORT, () => {
  console.log(`Server running at http://localhost:${PORT}/`);
});
```

**Exécution :**
```bash
# Compiler
tsc server.ts

# Exécuter
node server.js

# Ou directement avec ts-node
npx ts-node server.ts
```

### 4.3 Exemple : Utilisation de packages NPM

**Installation :**
```bash
npm install express
npm install --save-dev @types/express
```

**Code :**
```ts
// app.ts
import express from 'express';

const app = express();
const PORT = 3000;

app.get('/', (req, res) => {
  res.json({ message: 'Hello World!' });
});

app.get('/users/:id', (req, res) => {
  const userId = req.params.id;
  res.json({ id: userId, name: 'John Doe' });
});

app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```

### 4.4 Exemple : Scripts NPM

**package.json :**
```json
{
  "name": "mon-projet",
  "version": "1.0.0",
  "scripts": {
    "dev": "ts-node src/index.ts",
    "build": "tsc",
    "start": "node dist/index.js",
    "test": "jest",
    "lint": "eslint src/**/*.ts"
  },
  "dependencies": {
    "express": "^4.18.2"
  },
  "devDependencies": {
    "typescript": "^5.3.3",
    "@types/node": "^20.10.6",
    "@types/express": "^4.17.21",
    "ts-node": "^10.9.2"
  }
}
```

**Utilisation :**
```bash
npm run dev      # Lancer en mode dev
npm run build    # Compiler TypeScript
npm start        # Lancer la version compilée
npm test         # Exécuter les tests
```

### 4.5 Exemple : Gestion des variables d'environnement

**Installation :**
```bash
npm install dotenv
```

**Fichier .env :**
```env
PORT=3000
DATABASE_URL=postgresql://localhost:5432/mydb
API_KEY=secret123
```

**Code :**
```ts
// config.ts
import dotenv from 'dotenv';

dotenv.config();

export const config = {
  port: process.env.PORT || 3000,
  databaseUrl: process.env.DATABASE_URL!,
  apiKey: process.env.API_KEY!,
};
```

### 4.6 Exemple : Lecture de fichier asynchrone

```ts
// fileReader.ts
import fs from 'fs/promises';

async function readUserData(filename: string): Promise<string[]> {
  try {
    const content = await fs.readFile(filename, 'utf-8');
    const lines = content.split('\n').filter(line => line.trim());
    return lines;
  } catch (error) {
    console.error('Erreur lecture fichier:', error);
    return [];
  }
}

// Utilisation
readUserData('./users.txt').then(users => {
  console.log('Utilisateurs:', users);
});
```

## 5. Erreurs et pièges à éviter

### 5.1 Erreurs fréquentes

| Erreur | Description | Impact | Solution |
|--------|-------------|--------|----------|
| **node_modules/ committé** | Versionner le dossier node_modules | Repo gigantesque, conflits | Ajouter dans `.gitignore` |
| **Versions incompatibles** | Installer packages avec versions conflictuelles | Crashes, bugs | Utiliser `package-lock.json` |
| **Oublier --save-dev** | Installer un outil de dev comme dépendance normale | Bundle production gonflé | `npm i -D` pour outils dev |
| **Pas de gestion d'erreur async** | Ne pas catch les promesses | Unhandled rejection, crash | Toujours utiliser try/catch |
| **Bloquer l'event loop** | Faire des calculs synchrones lourds | Serveur bloqué | Utiliser worker threads |

### 5.2 Pièges courants

**Piège 1 : Versions non verrouillées**
```json
// ❌ Mauvais
"dependencies": {
  "express": "*"  // Installe n'importe quelle version !
}

// ✅ Bon
"dependencies": {
  "express": "^4.18.2"  // Version compatible 4.x
}
```

**Piège 2 : require vs import**
```ts
// ❌ Mélanger CommonJS et ES Modules
const express = require('express');  // CommonJS
import http from 'http';             // ES Module

// ✅ Cohérent
import express from 'express';
import http from 'http';
```

**Piège 3 : Oublier les types TypeScript**
```bash
# ❌ Installer uniquement le package
npm install lodash

# ✅ Installer le package + ses types
npm install lodash
npm install --save-dev @types/lodash
```

**Piège 4 : Synchroniser les versions entre environnements**
```bash
# ❌ Réinstaller sans lock
rm -rf node_modules
npm install

# ✅ Utiliser le lock file
npm ci  # Clean install depuis package-lock.json
```

### 5.3 Impacts sur les logiciels/webapps

| Problème | Conséquence |
|----------|-------------|
| **Dépendances obsolètes** | Failles de sécurité, bugs non corrigés |
| **node_modules trop gros** | Build lent, déploiement lent |
| **Pas de versioning** | Comportement différent selon environnement |
| **Dépendances inutiles** | Bundle trop lourd, temps de chargement |
| **Event loop bloqué** | Application qui freeze, timeouts |
| **Pas de gestion mémoire** | Memory leaks, crashes en production |

## 6. Résumé de l'essentiel

### Points clés à retenir

1. **Node.js = JavaScript côté serveur**
   - Utilise le moteur V8 de Chrome
   - Event-driven et non-bloquant
   - Idéal pour I/O intensif

2. **NPM = Gestionnaire de packages**
   - Plus de 2 millions de packages disponibles
   - Gestion automatique des dépendances
   - Scripts personnalisables

3. **Architecture asynchrone**
   - Event Loop gère la concurrence
   - Non bloquant mais single-threaded
   - async/await pour la lisibilité

4. **package.json = Configuration projet**
   - Liste des dépendances
   - Scripts de commandes
   - Métadonnées du projet

5. **Gestion des dépendances**
   - `dependencies` : code en production
   - `devDependencies` : outils de développement
   - `package-lock.json` : versions exactes

### Commandes essentielles

```bash
# Initialisation
npm init -y

# Installation
npm install package-name          # Dépendance production
npm install -D package-name       # Dépendance développement
npm install -g package-name       # Installation globale

# Gestion
npm update                         # Mettre à jour packages
npm outdated                       # Voir packages obsolètes
npm audit                          # Vérifier sécurité
npm audit fix                      # Corriger vulnérabilités

# Nettoyage
npm ci                             # Clean install
rm -rf node_modules && npm install # Réinstaller tout

# Scripts
npm run script-name                # Exécuter script custom
npm start                          # Raccourci pour "start"
npm test                           # Raccourci pour "test"
```

### Structure projet type

```
mon-projet/
├── node_modules/          # ❌ Ne jamais commit
├── src/
│   ├── index.ts
│   └── server.ts
├── dist/                  # ❌ Code compilé
├── .env                   # ❌ Variables secrètes
├── .gitignore             # ✅ Essentiel
├── package.json           # ✅ Configuration
├── package-lock.json      # ✅ Lock versions
├── tsconfig.json          # ✅ Config TypeScript
└── README.md              # ✅ Documentation
```

### .gitignore recommandé

```gitignore
# Dépendances
node_modules/

# Build
dist/
build/

# Environnement
.env
.env.local

# Logs
*.log
npm-debug.log*

# Éditeur
.vscode/
.idea/

# OS
.DS_Store
Thumbs.db
```

### Checklist projet Node.js/TypeScript

- [ ] Node.js et NPM installés (versions récentes)
- [ ] `package.json` initialisé
- [ ] TypeScript et @types/node installés
- [ ] `tsconfig.json` configuré
- [ ] `.gitignore` créé (exclure node_modules)
- [ ] Scripts npm définis (dev, build, start)
- [ ] Dépendances séparées (prod vs dev)
- [ ] `.env` pour variables secrètes (+ .env.example)

---

**En une phrase :**

> Node.js est l'environnement qui permet d'exécuter JavaScript côté serveur avec une architecture event-driven non-bloquante, et NPM est son gestionnaire de packages qui facilite l'installation, la gestion et le partage de millions de bibliothèques JavaScript.
