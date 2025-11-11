# Variables et Types Primitifs en TypeScript

## 1. Pourquoi ce cours

Dans tout programme, on manipule des **données** :
- informations utilisateur
- calculs métiers
- états d'une interface
- résultats d'une API ou d'une base de données…

Pour manipuler ces données, on a besoin :
1. **d’un moyen de les stocker** → *variables*
2. **d’un moyen de connaître leur nature** → *types primitifs*

Sans cela :
- impossible de raisonner sur le code
- impossible d’éviter les bugs liés aux mauvais formats
- impossible d’avoir un programme fiable et maintenable.

---

## 2. Qu’est-ce qu’une Variable ?

### 2.1 Définition théorique
Une **variable** est un **nom donné à une zone de mémoire** dans laquelle une valeur est stockée.

Elle permet :
- de **conserver** une information
- de la **modifier**
- de la **transmettre**
- de la **calculer**

> Une variable est un **concept abstrait** permettant à l’humain d’interagir avec la mémoire *sans voir la mémoire directement*.

### 2.2 Fonctionnement en mémoire
Lorsqu’on écrit :

```ts
let age: number = 30;
```

Il se passe 3 choses :
- TypeScript vérifie que 30 est compatible avec number (contrôle du type).
- La machine réserve un espace dans la RAM capable de stocker un number.
- Un nom (age) sert de "référence" pour retrouver cette valeur en mémoire.

**Tu manipules le nom → la machine manipule l'adresse.**

## 3. Déclaration, affectation et mutation

```ts
let age: number; // déclaration (aucune valeur)
age = 30;        // affectation (on place une valeur)
age = 31;        // mutation (on remplace la valeur)
```

| Mot-clé | Signification                            | Recommandation                |
| ------- | ---------------------------------------- | ----------------------------- |
| `let`   | variable dont la valeur peut changer     | ✅ à utiliser par défaut       |
| `const` | valeur **qui ne changera pas**           | ✅ à préférer dès que possible |
| `var`   | ancien mot-clé, comportements inattendus | ❌ à éviter                    |

## 4. Qu’est-ce qu’un Type ?

Un type représente la nature d’une donnée.

Exemple :

23 → c’est un nombre

"Vincent" → c’est une chaîne de caractères

false → c’est un booléen

Le type indique :
- la taille mémoire
- les opérations autorisées
- les valeurs possibles
- le rôle logique dans le programme

## 5. Le cas spécifique de TypeScript

`TypeScript` ajoute du typage statique à `JavaScript`.

### 5.1 Typage statique

Le type est vérifié avant l'exécution, lors de la compilation :

```ts
let prix: number = "100"; // ❌ Erreur avant l'exécution
```

### 5.2 Avantages concrets

| Avantage              | Explication                                               |
| --------------------- | --------------------------------------------------------- |
| Moins de bugs         | La machine détecte les erreurs avant qu'elles n'arrivent. |
| Code lisible          | On sait ce qu’une variable contient sans deviner.         |
| Maintenance facilitée | Un autre développeur comprend ton intention.              |

## 6. Liste des types primitifs (TypeScript)

| Type        | Exemple        | Description                      | Taille mémoire approximative |
| ----------- | -------------- | -------------------------------- | ---------------------------- |
| `number`    | `10`, `3.14`   | Nombre entier ou décimal         | 64 bits                      |
| `string`    | `"Hello"`      | Texte encodé en UTF-16           | variable                     |
| `boolean`   | `true`         | Vrai / Faux                      | 1 bit (logique)              |
| `null`      | `null`         | Absence intentionnelle de valeur | 0                            |
| `undefined` | —              | Déclaré mais non initialisé      | 0                            |
| `bigint`    | `100n`         | Entiers très grands              | variable                     |
| `symbol`    | `Symbol("id")` | Identifiant unique               | variable                     |

```ts
let nom: string = "Martin";
let temperature: number = 18.5;
let estAdmin: boolean = false;
let adresse: string | null = null;
let telephone: string | undefined;
```

## 7. Différence entre null et undefined

| Valeur      | Signification                                  | Exemple         |
| ----------- | ---------------------------------------------- | --------------- |
| `undefined` | Variable déclarée mais **jamais initialisée**  | `let x;`        |
| `null`      | La variable a **volontairement aucune valeur** | `let x = null;` |

