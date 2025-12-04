# Les Opérateurs en Programmation (TypeScript)

## 1. Introduction

Les **opérateurs** sont des symboles ou mots-clés permettant de **manipuler des valeurs** au sein d’expressions.  
Ils permettent de :

- faire des **calculs**
- **comparer** des valeurs
- effectuer de la **logique**
- prendre des **décisions** dans un programme

Ils sont essentiels pour que le programme réagisse à des données et ne soit pas un simple texte statique.

---

## 2. Rappel : Variables et Types

Une **variable** contient une **valeur**, et cette valeur possède un **type**.  
Les opérateurs **agissent sur ces valeurs**.

Exemple simple :
```ts
let a: number = 10;
let b: number = 5;
let resultat = a + b;
```

Ici :
- `+` est un opérateur
- `a` et `b` sont des opérandes

## 3. Les opérateurs arithmétiques (calculs)

Ces opérateurs s’appliquent aux nombres (`number`, `bigint`).

| Opérateur | Nom            | Exemple  | Résultat |
| --------- | -------------- | -------- | -------- |
| `+`       | Addition       | `10 + 5` | `15`     |
| `-`       | Soustraction   | `10 - 3` | `7`      |
| `*`       | Multiplication | `4 * 2`  | `8`      |
| `/`       | Division       | `10 / 2` | `5`      |
| `%`       | Modulo (reste) | `10 % 3` | `1`      |
| `**`      | Exponentiation | `2 ** 3` | `8`      |

Exemple: 

```ts
let prixHT = 120;
let tva = 0.2; // ici 20 %
let prixTTC = prixHT * (1 + tva);
console.log(prixTTC); // 144
```

### Cas métier — Calcul d’un pourcentage de remise

```ts
let prixInitial = 200;
let remise = 15; // en %
let prixFinal = prixInitial * (1 - remise / 100);
console.log(prixFinal); // 170 €
```

## 4. L’opérateur + avec les chaînes (string)

Lorsque l’*un* des opérandes est une string, l’opérateur devient une concaténation, pas une addition.

```ts
console.log("Age : " + 30); // "Age : 30"
```

## 5. Les opérateurs d'affectation

| Opérateur | Signification          | équivalent  |
| --------- | ---------------------- | ----------- |
| `=`       | Affectation            | —           |
| `+=`      | Ajoute puis affecte    | `x = x + y` |
| `-=`      | Soustrait puis affecte | `x = x - y` |
| `*=`      | Multiplie puis affecte | `x = x * y` |
| `/=`      | Divise puis affecte    | `x = x / y` |

```ts
let stock = 10;
stock += 5; // stock = 15
stock -= 3; // stock = 12
```

## 6. Les opérateurs de comparaison

Ils retournent toujours un boolean (true ou false).

| Opérateur | Signifie                        | Exemple     | Résultat |
| --------- | ------------------------------- | ----------- | -------- |
| `==`      | Égal en **valeur**              | `5 == "5"`  | `true`   |
| `===`     | Égal en **valeur et type**      | `5 === "5"` | `false`  |
| `!=`      | Différent en valeur             | `5 != "5"`  | `false`  |
| `!==`     | Différent en valeur **ou** type | `5 !== "5"` | `true`   |
| `>`       | Strictement supérieur           | `7 > 3`     | `true`   |
| `<`       | Strictement inférieur           | `2 < 1`     | `false`  |
| `>=`      | Supérieur ou égal               | `10 >= 10`  | `true`   |
| `<=`      | Inférieur ou égal               | `8 <= 9`    | `true`   |

> IMPORTANT : En TypeScript et JavaScript, on utilise toujours === et !==

Pour éviter les conversions automatiques ambigües.

## 7. Cas métier — Validation utilisateur

```ts
let age = 17;
let estAutorisé = age >= 18;
console.log(estAutorisé); // false
```

```ts
let panierTotal = 0;
let estPanierVide = panierTotal === 0;
```

## 8. Les opérateurs logiques (raisonnement)

| Opérateur | Nom         | Signification                    | Exemple           | Résultat |
| --------- | ----------- | -------------------------------- | ----------------- | -------- |
| `&&`      | ET logique  | Vrai si **tous** sont vrais      | `true && false`   | `false`  |
| `\|\|`    | OU logique  | Vrai si **au moins un** est vrai | `true \|\| false` | `true`   |
| `!`       | NON logique | Inverse la valeur                | `!true`           | `false`  |

### Cas métier - exemple authentification

```ts
let estConnecté = true;
let estAdmin = false;

if (estConnecté && estAdmin) {
  console.log("Accès admin autorisé");
} else {
  console.log("Accès refusé");
}
```

### Cas métier - gestion des données optionnelles

```ts
let email: string | null = null;
let emailFinal = email || "adresse-inconnue@example.com";

// Attention
let isAutheticated: boolean | string = false;
let isAutheticated = isAutheticated || "adresse-inconnue@example.com";
console.log(isAutheticated) // "adresse-inconnue@example.com"

let isAutheticated: boolean | string = false;
let isAutheticated = isAutheticated ?? "adresse-inconnue@example.com";
console.log(isAutheticated) // false

let nom: string | null = null;
let nomFinal = nom ?? "itsme";
```

> `||` permet de définir une valeur par défaut (avec une valeur `false` on prendre la valeur par défaut avec cet opérateur)
> `??` est la version moderne de `||` et permet de définir une valeur par defaut si la première est undefined ou null

## 9. Opérateurs d’incrémentation

| Opérateur | Effet                    |
| --------- | ------------------------ |
| `x++`     | Retourne x puis ajoute 1 |
| `++x`     | Ajoute 1 puis retourne x |

```ts
let compteur = 0;
console.log(compteur++); // 0
console.log(compteur);   // 1
console.log(++compteur); // 2
```

## 10. Erreurs fréquentes à éviter

| Mauvaise pratique              | Explication                                   |                                 |                                     |
| ------------------------------ | --------------------------------------------- | ------------------------------- | ----------------------------------- |
| Utiliser `==` au lieu de `===` | Risque de conversions implicites indésirables |                                 |                                     |
| Mélanger nombres et strings    | `"10" + 2 = "102"`                            |                                 |                                     |
| Oublier que `                  |                                               | ` fournit une valeur par défaut | Peut masquer des `0`, `false`, etc. |
