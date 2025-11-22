# Comprendre les Fonctions en Programmation

## Qu’est-ce qu’une fonction ?

Une fonction est :
- un bloc de code réutilisable,
- qui effectue une action précise,
- et qui peut recevoir des données (paramètres),
- puis retourner un résultat

> Une fonction sert à éviter de répéter du code, rendre un programme plus lisible, testable, maintenable.

### Exemple

```ts
function add(a: number, b: number): number {
  return a + b;
}

const result = add(2, 3); // 5
```

## Comment ça fonctionne dans l’ordinateur ?

Pour vulgariser :

Quand tu définis une fonction :
1. Le compilateur/interpréteur enregistre l’emplacement de la fonction en mémoire (dans la zone code).
2. Il note aussi :
    - -ses paramètres
    - son type de retour
    - son corps

Quand tu appelles une fonction :
1. L’ordinateur crée une “pile d’exécution” (stack frame) :
    - copie les paramètres
    - réserve l’espace pour les variables internes

2. Il saute à l’adresse de la fonction en mémoire.
3. Exécute les instructions ligne par ligne.
4. Place la valeur retournée en mémoire.
5. Supprime le stack frame et revient à la ligne d’appel.

> Cette gestion de pile s’appelle le call stack.

### Illustration

Appel :

```ts
add(2, 3);
```

La pile va contenir :

| a = 2 |
| b = 3 |
| return address: ligne suivante |


Puis elle disparaît quand la fonction finit.

## Pourquoi utilise-t-on des fonctions ?

- Pour réutiliser du code
- Pour isoler une logique
- Pour éviter les erreurs
- Pour tester plus facilement
- Pour organiser le programme

> Dans des apps réelles : chaque fonctionnalité (login, achat, email, calcul TVA…) repose sur des fonctions.

## Les différents types de fonctions

### Fonctions déclarées

```ts
function multiply(a: number, b: number): number {
  return a * b;
}
```

- Lisibles
- Hoisting (utilisables avant leur définition)
* *Parfois verbeuses*

### Fonctions anonymes

- Pas de nom.
- Souvent utilisées comme callbacks.

```ts
const log = function(message: string): void {
  console.log(message);
};
```

- Pratique à passer en paramètre
* *Moins lisible si mal utilisée*

### Fonctions fléchées (arrow functions)

```ts
const add = (a: number, b: number): number => a + b;
```

- Syntaxe courte
- Pratique pour callbacks
- this lexique (très utile en JS/TS)
* *Moins adaptées comme méthodes de classes dans certains cas*

### Fonctions pures

- Toujours même entrée → même sortie
- Sans effet de bord

```ts
function double(n: number): number {
  return n * 2;
}
```

- Prévisibles
- Testables
- Clean Code
* *Ne peuvent pas manipuler l’état global de l'application*

### Fonctions impures

- Manipulent l’extérieur :
    - fichiers
    - BDD
    - API
    - état global
    - DOM

```ts
let counter = 0;

function increment(): number {
  counter++;
  return counter; // counter est déclaré dans l'application
}
```

- Nécessaires pour interagir avec le monde réel
* *Moins testables*

### Fonctions asynchrones

- Utilisent async/await.

```ts
async function fetchUser(id: number): Promise<User> {
  const response = await fetch(`/api/users/${id}`);
  return response.json();
}
```


- Indispensables en web moderne
- Non bloquantes
* *Plus complexes à raisonner*

## Cas d’usage concrets dans une vraie application

### Login utilisateur

> Avec un formulaire de connexion

```ts
async function login(email: string, password: string): Promise<boolean> {
  const response = await fetch('/api/login', {
    method: 'POST',
    body: JSON.stringify({ email, password })
  });
  return response.ok;
}
```

### Calcul de TVA

```ts
function computeVat(amount: number, rate: number = 0.2): number {
  return amount * rate;
}
```

### Formatage de date (front)

```ts
function formatDate(date: Date): string {
  return date.toLocaleDateString('fr-FR');
}
```

### Récupération de produits depuis serveur

```ts
async function getProducts(): Promise<Product[]> {
  const res = await fetch('/api/products');
  return res.json();
}
```

### Une fonction de validation (formulaire)

```ts
function validateEmail(email: string): boolean {
  return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
}
```

## Erreurs courantes (débutants)

### Fonction trop longue

Problème : Difficulté de maintenance
Solution : Découper

### Trop de responsabilités

Problème : Violation du principe Single Responsibility (SOLID)
Solution : Une fonction = une seule action

### Mélanger logique pure et logique DOM/API

Problème : Difficulté de maintenance
Solution : Séparer fonctions “métier” et “techniques”

### Utiliser trop de paramètres

Problème : Difficulté de maintenance
Solution Préférer un paramètre objet

```ts
function createUser({ name, age }: { name: string; age: number }) {}
```

### Ne pas typer suffisamment

Problème : Erreurs silencieuses très compliquées à détécter et à corriger
Solution : Utiliser les *fucking* types. Ils sont là pour ca.

> les *fucking* types n'existe pas. C'est de l'humour. Un langage où les types ne sont pas obligatoires ne devrait pas exister.

### Retourner des types incohérents

Problème : Comportement imprévisible et compliquer à tester
Solution : Toujours un type clair en sortie

### Ne pas gérer les erreurs (async)

Problème : Crash de l'application incontrolable, indetectable, risque majeur de corruption de la données.
Solution : Utiliser des `try/catch`

```ts
async function safeFetch<T>(url: string): Promise<T | null> {
  try {
    const res = await fetch(url);
    return res.json();
  } catch {
    return null;
  }
}
```

## Bonnes pratiques (Clean Code + SOLID + KISS + YAGNI)

> On verra plus tard ensemble les principes `Clean Code` et les acronymes `SOLID`, `KISS`, `YAGNI`

- Une fonction = une seule responsabilité
- La rendre courte, lisible
- La nommer clairement : verbe + complément
- Préférer les fonctions pures quand possible
- Toujours typer les entrées/sorties
- Ne jamais ajouter de compléxité inutile (YAGNI)
- Garder la simplicité (KISS)
- Écrire des fonctions testables
- Ne pas dépendre de variables globales
- Clarifier les erreurs (exceptions ou types unions)

> On verra plus tard ensemble les `exceptions` et les types `unions`

### Avantages et défauts des types de fonctions

| Type     | Avantages               | Défauts                     | Cas d’usage               |
| -------- | ----------------------- | --------------------------- | ------------------------- |
| Déclarée | Lisible, hoisting       | Verbose                     | APIs internes, services   |
| Anonyme  | Pratique comme callback | Peut perdre en lisibilité   | setTimeout, eventListener |
| Arrow    | Courte, this lexical    | Pas adaptée en classe       | callbacks, map/filter     |
| Pure     | Testable, simple        | Pas d’interactions externes | règles métier             |
| Impure   | Permet I/O              | Test plus complexe          | API, BDD, DOM             |
| Async    | Non bloquante           | Erreurs plus subtiles       | réseau, E/S               |
