# Comprendre les conditions en programmation (if, else, switch, ternaire)

## 1. Le rôle des conditions

Une **condition** permet à un programme de **prendre une décision** :
> “Si quelque chose est vrai, fais ceci, sinon fais cela.”

C’est un des piliers fondamentaux de la logique en programmation.  
Sans conditions, un programme ne ferait que répéter mécaniquement des instructions sans s’adapter.

Exemples concrets :
- Si l’utilisateur est connecté → afficher son tableau de bord.  
  Sinon → afficher la page de connexion.
- Si la température est inférieure à 0°C → activer le chauffage.

Les principales structures conditionnelles :
- `if` / `else` : exécuter ou non un bloc de code.
- `switch` : choisir entre plusieurs cas possibles.
- **opérateur ternaire** (`? :`) : forme courte d’un `if/else`.

---

## 2. Syntaxe de base

### if / else
```js
if (isUserLoggedIn) {
  showProfile();
} else {
  showLogin();
}
```

### if / else if / else

```ts
if (score >= 90) {
  grade = "A";
} else if (score >= 80) {
  grade = "B";
} else {
  grade = "C";
}
```

### switch

```ts
switch (orderStatus) {
  case "pending":
    notifyPending();
    break;
  case "shipped":
    notifyShipped();
    break;
  default:
    notifyUnknown();
}
```

### Ternaire

```ts
const greeting = isMorning ? "Bonjour" : "Bonsoir";
```

## 3. Théorie : comment ça marche ?

Une condition repose sur une expression booléenne, c’est-à-dire une opération qui renvoie :

- true (vrai)
ou
- false (faux)

```ts
5 > 3       // true
age >= 18   // true si l’âge est au moins 18
user == null // true si user n’existe pas
```

### Tableau opérateurs de comparaison

| Opérateur   | Signification                 |
|-------------|-------------------------------|
| `==`        | Égalité (avec conversion)     |
| `===`       | Égalité stricte (même type)   |
| `!=`        | Différent                     |
| `>` / `<`   | Supérieur / Inférieur         |
| `>=` / `<=` | Supérieur / Inférieur ou égal |
| `&&`        | ET logique                    |
| &#124;&#124;| OU logique                    |
| `!`         | Négation                      |

## 4. Ce qui se passe dans la mémoire

Voici ce que le processeur et la mémoire font quand tu écris une condition :
1. Évaluation de l’expression
  - Les valeurs nécessaires (x, y, etc.) sont lues depuis la mémoire (RAM → registres CPU).
  - Le processeur compare ces valeurs.
  - Le résultat (true ou false) est stocké dans un registre spécial de la mémoire (flag).
2. Saut conditionnel
  - Le CPU lit l’instruction suivante :
    - Si la condition est true, il continue dans le bloc if.
    - Sinon, il saute ce bloc pour passer au else ou à la suite.
3. Retour au flux normal
  - Après exécution du bloc, le programme reprend son cours normal.

> Cette idée s’appelle le branchement conditionnel.
> Dans le CPU, c’est réalisé avec des instructions de type “jump if true”.

*Note : Optimisation matérielle*
*Les processeurs modernes devinent souvent quelle branche sera prise (branch prediction).*
*Si la prédiction est fausse → perte de quelques cycles CPU (pipeline flush).*

## 5. Détails selon les langages

| Langage          | Particularité                                                                                 |
| ---------------- | --------------------------------------------------------------------------------------------- |
| **JavaScript**   | Les valeurs “falsy” (`0`, `""`, `null`, `undefined`, `false`) sont considérées comme fausses. |
| **C# / Java**    | Comparaison stricte de types : pas de conversion implicite.                                   |
| **TypeScript**   | Aide à éviter les oublis de `else` ou de `switch` incomplets.                                 |
| **C / C++ / JS** | Le `switch` continue sans `break` (fallthrough).                                              |

## 6. Bonnes pratiques (Clean Code)

- KISS — Garder la condition simple et claire.
- DRY — Éviter de répéter les mêmes tests plusieurs fois.
- Clauses de garde : sortir tôt d’une fonction (`return`).

```ts
if (!user) return;
if (!user.isActive) return;
// reste du code
```

- Nommer clairement :

```ts
if (isAdmin)  // ✔️ clair
if (flag)     // ❌ peu parlant
```

- Éviter les ternaires complexes : un ternaire doit rester court.
- Préférer switch / mapping / polymorphisme pour de longues chaînes de if/else. (on verra plus tard ce qu'est le `polymorphisme` et le `mapping`)

## 7. Cas d’usage concrets (métier)

### Exemple 1 — Authentification

```ts
if (!user) {
  redirectToLogin();
  return;
}
if (!user.isEmailVerified) {
  showVerificationNotice();
  return;
}
showDashboard();
```

### Exemple 2 — Calcul de tarif

```ts
let price;
if (quantity >= 100) {
  price = unitPrice * 0.8; // 20% de remise
} else if (quantity >= 50) {
  price = unitPrice * 0.9;
} else {
  price = unitPrice;
}
```

### Exemple 3 — Workflow de commande

```ts
switch (order.status) {
  case "created": prepareOrder(); break;
  case "paid": sendToWarehouse(); break;
  case "shipped": notifyCustomer(); break;
  case "cancelled": refund(); break;
  default: logUnknownStatus(order.status);
}
```

### 8. Erreurs fréquentes

| Erreur                        | Exemple                       | Correction                   |
| ----------------------------- | ----------------------------- | ---------------------------- |
| Oublier `break` dans `switch` | `case 1: doX();`              | Ajouter `break`              |
| Confondre `=` et `==`         | `if (x = 5)`                  | `if (x === 5)`               |
| Trop d’imbrication            | `if (a){ if(b){ if(c){...}}}` | Utiliser guard clauses       |
| Logique dans condition        | `if (increment() > 10)`       | Calculer avant la condition  |
| Valeurs nulles non gérées     | `if (user.isActive)`          | `if (user && user.isActive)` |


## 9. Performance

- En général, les conditions ne posent pas de problème de performance.

Cas où ça peut compter :
  - Boucles très rapides avec conditions lourdes.
  - Branches difficiles à prédire pour le CPU.

- Optimisations :
  - Réorganiser les tests pour que les cas les plus probables arrivent en premier.
  - Utiliser des structures de données (maps, sets) quand cela évite des if multiples.

## 10. Alternatives plus évoluées (niveau avancé)

| Cas                          | Solution                                             |
| ---------------------------- | ---------------------------------------------------- |
| Trop de `switch` selon type  | **Polymorphisme / Strategy Pattern**                 |
| Nombreuses règles métiers    | **Table de règles ou mapping**                       |
| Tests sur plusieurs critères | **Objet de configuration + lookup**                  |
| Code difficile à tester      | **Découper les conditions dans des fonctions pures** |

## 11. Checklist avant de valider le code

- Ma condition est courte et lisible.
- J’utilise `===` au lieu de `==` si applicable.
- J’ai géré les valeurs nulles/undefined.
- J’ai ajouté un else ou default si nécessaire.
- J’ai évité la duplication de logique.
- J’ai testé les deux branches (true et false).

## 12. Exercices pratiques (facultatif)

### Exercice 1 — Frais de livraison

```ts
function getShippingCost(weight: number) {
  // si weight <= 1kg → 5€
  // si weight entre 1 et 5kg → 10€
  // sinon → 25€
}
```

### Exercice 2 — Tester le retour d'une requête HTTP

Créer un switch qui affiche :
- 200 → “Succès”
- 404 → “Page introuvable”
- 500 → “Erreur serveur”
- autre → “Code inconnu”

## 13. En résumé

| Structure     | Usage principal                 | Avantage                         |
| ------------- | ------------------------------- | -------------------------------- |
| `if` / `else` | Décision simple                 | Lisible et intuitif              |
| `switch`      | Choix parmi plusieurs cas       | Plus clair qu’une chaîne de `if` |
| Ternaire      | Choix rapide entre deux valeurs | Compact, expressif               |


➡️ En une phrase :

> Une condition, c’est le cœur de la logique d’un programme : elle permet de réagir à des situations différentes en choisissant le bon chemin d’exécution.