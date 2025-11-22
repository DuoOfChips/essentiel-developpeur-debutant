# Les Boucles en Programmation

## 1. Qu’est-ce qu’une boucle ? (Définition simple)

Une boucle est un mécanisme qui permet à un programme :
d'exécuter plusieurs fois la même série d’instructions tant qu’une condition est vraie ou pour un nombre défini de répétitions

Sans boucle, beaucoup de programmes modernes seraient impossibles :
traitement de listes, animation d’interfaces, lecture de fichiers, requêtes réseau répétées…

## 2. Comment fonctionne une boucle dans l’ordinateur (Niveau CPU)

Pour comprendre ce qu’il se passe derrière :

- Le CPU exécute ton programme ligne par ligne.
    - Une boucle est une instruction spéciale qui dit : « Après cette ligne, retourne à telle autre ligne, si telle condition est vraie. »

- Les étapes internes d’une boucle :
    - Initialisation : ```let i = 0; → le CPU réserve une case mémoire pour i.```
    - Test de la condition : ```i < 10 → comparaison dans un registre du CPU (opération extrêmement rapide).```
    - Exécution du bloc de code : ```afficher un texte, modifier une variable…```
    - Incrémentation / modification : ```i++ → le CPU ajoute +1 en mémoire.```
    - Saut en arrière : ```Une instruction de type jump ramène l’exécution au début de la boucle.```

Ce cycle se répète des millions de fois par seconde.

## 3. Les types de boucles et quand les utiliser

### 3.1 Boucle for : nombre connu de répétitions

Quand l’utiliser ?
- Tu sais combien de fois tu veux répéter une action.
- Tu parcours un tableau, une liste d’objets.
- Tu veux extraire des données ou effectuer des calculs.

```ts
const scores: number[] = [12, 19, 7, 14];

for (let i = 0; i < scores.length; i++) {
  const score = scores[i];
  console.log(`Score #${i} = ${score}`);
}
```

#### Cas d’usage réel

- afficher une liste de produits dans une page web
- calculer le total d’un panier e-commerce
- analyser un fichier CSV ligne par ligne

### 3.2 Boucle for…of : itération simple sur les collections (liste)

> Plus moderne, plus lisible que for indexé.

```ts
const users: string[] = ["Alice", "Bob", "Claire"];

for (const user of users) {
  console.log(user);
}
```

Quand l’utiliser ?
- Tu ne veux pas gérer les indices (i, j, k)
- Tu parcours un tableau, un Set, une Map (avec for…of)

### 3.3 Boucle while : répéter tant que c’est vrai

```ts
let retries = 0;

while (retries < 3) {
  console.log("Tentative n°", retries);
  retries++;
}
```

Cas d’usage réel :
- continuer une requête réseau tant que la réponse n’est pas valide
- attendre la disponibilité d’une ressource
- lire un fichier ligne par ligne tant qu’il n’est pas terminé

### 3.4 Boucle do…while : exécute au moins une fois

```ts
let input: string;

do {
  input = prompt("Entrez votre nom");
} while (!input);
```

Cas d’usage réel : 
- saisie utilisateur obligatoirement exécutée une fois

### 3.5 Boucles fonctionnelles (map / filter / reduce)

> Ce ne sont pas des boucles visibles, mais ce sont des boucles internes (programmation fonctionnelle, très utilisée en JS/TS).

*On verra plus tard ensemble ce que c'est `filter`, `map`, `find`, `reduce` ....*

```ts
const prices = [10, 20, 30];

const withTax = prices.map(p => p * 1.2);
```

Avantages :
- plus lisible
- immutable (bon pour éviter les bugs)
- plus proche du clean code

## 4. Ce que fait réellement une boucle dans une application

1) Interface graphique / animations

Ex : animation d’un slider ou d’un graphique.

→ La boucle tourne à 60 images/sec via requestAnimationFrame.

2) Traitement de données

Ex : parcourir 30 000 lignes d’un fichier CSV.

3) Réseaux

Ex : retenter l’envoi d’une requête tant que l’API renvoie une erreur temporaire.

4) E-commerce

Ex : recalculer le total du panier à chaque changement de quantité.

5) Sécurité

Ex : vérifier un token, parcourir un set de permissions.

## 5. Les erreurs courantes (débutants)

### 5.1. Boucle infinie

```ts
let i = 0;
while (i < 10) {
  console.log(i);
  // i++ manquant → boucle infinie
}
```

→ Le CPU ne sort jamais du saut arrière.
→ Tu bloques l’application ou le navigateur.

### 5.2. Modifier un tableau pendant qu’on le parcourt

→ Provoque des indices décalés, résultats inattendus.

### 5.3. Utiliser for là où map ou forEach sont plus adaptés

→ Moins lisible
→ Plus sujet aux bugs d’indices

### 5.4. Le "off by one" (erreur très classique)

```ts
for (let i = 0; i <= arr.length; i++) { }
```

→ Boucle une fois de trop
→ Accède à un index inexistant

### 5.5. Logique de condition complexe dans la boucle

→ Violation du principe KISS (Keep It Simple)

## 6. Conseils + bonnes pratiques

- Toujours garder la boucle simple
    - une seule responsabilité → SRP (SOLID)
    - évite les gros blocs dans les boucles → extract method

- Utiliser for…of pour la lisibilité
    - Préféré dans la plupart des cas.

- Préférer les méthodes fonctionnelles quand possible
    - map
    - filter
    - reduce
    - Plus lisibles, moins d’effets de bord.

- Réduire les opérations lourdes dans les boucles
    - requêtes réseau répétées (mettre en batch)
    - accès disque dans la boucle
    - Violent pour les performances.

Pré-calculer tout ce qui peut l’être avant la boucle
- longueur du tableau
- dépendances constantes
- configuration de contexte

## 7. Exemples concrets d’applications réelles

### Total du panier e-commerce

```ts
type Product = { name: string; price: number; quantity: number };

const cart: Product[] = [
  { name: "Laptop", price: 1200, quantity: 1 },
  { name: "Mouse", price: 25, quantity: 2 },
];

let total = 0;

for (const item of cart) {
  total += item.price * item.quantity;
}

console.log("Total :", total);
```

### Nettoyer un tableau (filter)

```ts
const emails = ["test@example.com", "", "invalid", "john@doe.com"];

const validEmails = emails.filter(e => e.includes("@") && e.length > 0);
```

### Algorithme simple (chercher un élément)

```ts
function findUser(users: string[], target: string): string | null {
  for (const user of users) {
    if (user === target) {
      return user;
    }
  }
  return null;
}
```

## Conclusion

Les boucles sont l’un des 4 piliers fondamentaux de la programmation impérative.

Les maîtriser, c’est comprendre :
- comment les programmes répètent des actions
- comment le CPU fonctionne réellement
- comment structurer son code proprement (lisibilité, performance)

Elles sont omniprésentes dans toutes les applications modernes : front, back, mobile, IA, jeux vidéo, bases de données… et les syntaxes différent peu voire pas.