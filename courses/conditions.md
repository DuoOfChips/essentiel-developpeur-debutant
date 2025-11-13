# 🌟 Comprendre les conditions en programmation (if, else, switch, ternaire)

> Un guide complet et progressif pour débutants — théorie, mémoire, exécution, cas d’usage et bonnes pratiques.

---

## 🧠 1. Le rôle des conditions

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

## ⚙️ 2. Syntaxe de base

### 🧩 if / else
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

| Opérateur   | Signification                 |   |            |
| ----------- | ----------------------------- | - | ---------- |
| `==`        | Égalité (avec conversion)     |   |            |
| `===`       | Égalité stricte (même type)   |   |            |
| `!=`        | Différent                     |   |            |
| `>` / `<`   | Supérieur / Inférieur         |   |            |
| `>=` / `<=` | Supérieur / Inférieur ou égal |   |            |
| `&&`        | ET logique                    |   |            |
| `||`        | OU logique                    |   |            |
| `!`         | Négation                      |   |            |
