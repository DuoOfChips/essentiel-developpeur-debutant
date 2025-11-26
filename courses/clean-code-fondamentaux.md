# Clean Code : Nommage, Fonctions et Principes Fondamentaux

## 1. Contexte et définitions des concepts clés

### 1.1 Qu'est-ce que le Clean Code ?

Le **Clean Code** (code propre) est du code qui est :
- **Lisible** : Facile à comprendre
- **Maintenable** : Facile à modifier
- **Testable** : Facile à tester
- **Simple** : Pas de complexité inutile

> "Any fool can write code that a computer can understand. Good programmers write code that humans can understand." — Martin Fowler

### 1.2 Pourquoi le Clean Code est crucial

**Code sale (legacy code) :**
- 🔴 Bugs difficiles à trouver
- 🔴 Modifications risquées
- 🔴 Nouveaux développeurs perdus
- 🔴 Dette technique qui s'accumule

**Clean Code :**
- ✅ Maintenance facile
- ✅ Bugs rares et faciles à corriger
- ✅ Collaboration fluide
- ✅ Évolution rapide du produit

### 1.3 Définitions importantes

- **Nommage** : Choix des noms de variables, fonctions, classes
- **Fonction** : Bloc de code avec une responsabilité unique
- **DRY** : Don't Repeat Yourself (pas de duplication)
- **KISS** : Keep It Simple, Stupid (rester simple)
- **YAGNI** : You Aren't Gonna Need It (pas de sur-conception)
- **SRP** : Single Responsibility Principle (une seule responsabilité)
- **Refactoring** : Améliorer le code sans changer son comportement
- **Code smell** : Indice qu'il faut refactorer

## 2. Quand et pourquoi appliquer Clean Code

### 2.1 Quand écrire du Clean Code

**Réponse courte : TOUJOURS !**

Le Clean Code n'est pas une option mais une nécessité :
- Dès le premier jour du projet
- À chaque nouvelle fonctionnalité
- Lors de chaque modification
- Pendant le refactoring

### 2.2 Coût vs Bénéfices

**"Je n'ai pas le temps d'écrire du code propre"**

```
Sans Clean Code:
Jour 1-7   : Développement rapide ✅
Jour 8-30  : Ralentissement progressif ⚠️
Jour 31+   : Cauchemar de maintenance 🔴

Avec Clean Code:
Jour 1-7   : Développement un peu plus lent
Jour 8-30  : Vitesse constante ✅
Jour 31+   : Toujours rapide et prévisible ✅
```

**Règle du scout :**
> "Laissez le code plus propre que vous ne l'avez trouvé"

## 3. Les principes fondamentaux

### 3.1 DRY : Don't Repeat Yourself

**Principe :** Chaque connaissance doit avoir une représentation unique dans le système.

**❌ Mauvais exemple (duplication) :**
```ts
function calculatePriceWithTaxForLaptop(price: number): number {
  const tax = price * 0.2;
  return price + tax;
}

function calculatePriceWithTaxForMouse(price: number): number {
  const tax = price * 0.2;
  return price + tax;
}

function calculatePriceWithTaxForKeyboard(price: number): number {
  const tax = price * 0.2;
  return price + tax;
}
```

**✅ Bon exemple (DRY) :**
```ts
function calculatePriceWithTax(price: number, taxRate: number = 0.2): number {
  const tax = price * taxRate;
  return price + tax;
}

// Utilisation
const laptopPrice = calculatePriceWithTax(1000);
const mousePrice = calculatePriceWithTax(25);
const keyboardPrice = calculatePriceWithTax(75);
```

**Impact de la violation :**
- Changement du taux de TVA → modifier 3+ endroits
- Risque d'oubli et d'incohérence
- Tests multipliés

### 3.2 KISS : Keep It Simple, Stupid

**Principe :** La solution la plus simple est souvent la meilleure.

**❌ Complexité inutile :**
```ts
function isUserEligibleForDiscount(user: User): boolean {
  const now = new Date();
  const userCreationDate = new Date(user.createdAt);
  const daysSinceCreation = Math.floor(
    (now.getTime() - userCreationDate.getTime()) / (1000 * 60 * 60 * 24)
  );
  
  if (user.isPremium === true) {
    if (daysSinceCreation > 30) {
      return true;
    } else {
      return false;
    }
  } else {
    if (user.totalOrders >= 5 && daysSinceCreation > 90) {
      return true;
    } else {
      return false;
    }
  }
}
```

**✅ Simple et clair :**
```ts
function isUserEligibleForDiscount(user: User): boolean {
  const daysSinceCreation = getDaysSince(user.createdAt);
  
  if (user.isPremium) {
    return daysSinceCreation > 30;
  }
  
  return user.totalOrders >= 5 && daysSinceCreation > 90;
}

function getDaysSince(date: string): number {
  const now = new Date();
  const past = new Date(date);
  return Math.floor((now.getTime() - past.getTime()) / (1000 * 60 * 60 * 24));
}
```

### 3.3 YAGNI : You Aren't Gonna Need It

**Principe :** N'implémentez que ce dont vous avez besoin maintenant.

**❌ Sur-conception :**
```ts
// Créer une abstraction "au cas où"
interface PaymentProcessor {
  processPayment(amount: number): Promise<void>;
  processRefund(amount: number): Promise<void>;
  processPartialRefund(amount: number): Promise<void>;
  processRecurringPayment(amount: number, interval: string): Promise<void>;
  processSplitPayment(amounts: number[]): Promise<void>;
  // 10 autres méthodes jamais utilisées...
}

class CreditCardProcessor implements PaymentProcessor {
  // Implémenter 15 méthodes dont 13 ne servent jamais
}
```

**✅ Seulement ce qui est nécessaire :**
```ts
interface PaymentProcessor {
  processPayment(amount: number): Promise<void>;
}

class CreditCardProcessor implements PaymentProcessor {
  async processPayment(amount: number): Promise<void> {
    // Implémentation simple
  }
}

// Ajouter processRefund() quand vraiment nécessaire
```

## 4. Nommage : L'art de choisir de bons noms

### 4.1 Principes du nommage

**Règles d'or :**
1. **Révéler l'intention** : Le nom dit ce que fait la variable/fonction
2. **Éviter la désinformation** : Pas de faux indices
3. **Faire des distinctions significatives** : Pas de `data1`, `data2`
4. **Prononçable** : Éviter `genymdhms` (generation year month day hour minute second)
5. **Recherchable** : `MAX_RETRY_COUNT` plutôt que `7`

### 4.2 Variables et constantes

**❌ Mauvais noms :**
```ts
const d = 86400000;  // Qu'est-ce que c'est ?
let x = user.getName();
const temp = calculateSomething();
const data = fetchData();
let flag = true;
const list = getList();
```

**✅ Bons noms :**
```ts
const MILLISECONDS_PER_DAY = 86400000;
const userName = user.getName();
const totalPrice = calculateCartTotal();
const activeUsers = fetchActiveUsers();
const isAuthenticated = true;
const productList = getProducts();
```

### 4.3 Fonctions

**Convention :** Verbe + Complément

**❌ Mauvais noms :**
```ts
function data() { }
function process() { }
function handle() { }
function doIt() { }
function manager() { }
```

**✅ Bons noms :**
```ts
function getUserById(id: number): User { }
function calculateTotalPrice(items: CartItem[]): number { }
function validateEmail(email: string): boolean { }
function sendNotification(user: User, message: string): void { }
function formatDate(date: Date): string { }
```

### 4.4 Classes et interfaces

**Convention :** Nom (substantif)

**❌ Mauvais noms :**
```ts
class Data { }
class Manager { }
class Processor { }
class Utils { }
```

**✅ Bons noms :**
```ts
class User { }
class ProductRepository { }
class EmailService { }
class OrderValidator { }
interface PaymentGateway { }
```

### 4.5 Booléens

**Convention :** Question (is, has, can, should)

**❌ Mauvais noms :**
```ts
const active = true;
const premium = user.premium;
const visible = element.visible;
```

**✅ Bons noms :**
```ts
const isActive = true;
const isPremium = user.isPremium;
const isVisible = element.isVisible;
const hasPermission = user.hasPermission();
const canDelete = checkDeletePermission();
const shouldNotify = user.preferences.notifications;
```

## 5. Fonctions : Courtes et une seule responsabilité

### 5.1 Taille des fonctions

**Règle :** Une fonction doit faire UNE chose et la faire bien.

**Idéal :** 5-15 lignes
**Maximum acceptable :** 20-30 lignes
**Au-delà :** Refactoring nécessaire

**❌ Fonction trop longue :**
```ts
function processOrder(order: Order): void {
  // Valider commande (10 lignes)
  if (!order.items || order.items.length === 0) {
    throw new Error("Order is empty");
  }
  for (const item of order.items) {
    if (item.quantity <= 0) {
      throw new Error("Invalid quantity");
    }
  }
  
  // Calculer total (15 lignes)
  let total = 0;
  for (const item of order.items) {
    total += item.price * item.quantity;
  }
  const tax = total * 0.2;
  total += tax;
  
  // Appliquer remises (20 lignes)
  if (order.user.isPremium) {
    total *= 0.9;
  }
  if (total > 100) {
    total -= 10;
  }
  
  // Créer facture (15 lignes)
  const invoice = {
    orderId: order.id,
    total: total,
    date: new Date()
  };
  
  // Envoyer emails (10 lignes)
  sendEmail(order.user.email, "Order confirmed");
  sendEmail("admin@shop.com", "New order");
  
  // Mettre à jour stock (15 lignes)
  for (const item of order.items) {
    updateStock(item.productId, -item.quantity);
  }
  
  // 85 lignes au total ❌
}
```

**✅ Fonctions courtes et ciblées :**
```ts
function processOrder(order: Order): void {
  validateOrder(order);
  const total = calculateOrderTotal(order);
  const discountedTotal = applyDiscounts(total, order.user);
  const invoice = createInvoice(order, discountedTotal);
  sendOrderNotifications(order);
  updateInventory(order.items);
}

function validateOrder(order: Order): void {
  if (!order.items || order.items.length === 0) {
    throw new Error("Order is empty");
  }
  
  for (const item of order.items) {
    if (item.quantity <= 0) {
      throw new Error(`Invalid quantity for ${item.name}`);
    }
  }
}

function calculateOrderTotal(order: Order): number {
  const subtotal = order.items.reduce(
    (sum, item) => sum + item.price * item.quantity,
    0
  );
  const tax = subtotal * 0.2;
  return subtotal + tax;
}

function applyDiscounts(total: number, user: User): number {
  let discounted = total;
  
  if (user.isPremium) {
    discounted *= 0.9; // 10% reduction
  }
  
  if (total > 100) {
    discounted -= 10; // 10€ off
  }
  
  return discounted;
}
```

### 5.2 Paramètres de fonction

**Règle :** Maximum 3 paramètres. Au-delà, utiliser un objet.

**❌ Trop de paramètres :**
```ts
function createUser(
  firstName: string,
  lastName: string,
  email: string,
  password: string,
  age: number,
  country: string,
  city: string,
  isPremium: boolean
): User {
  // ...
}

// Appel illisible
createUser("Alice", "Dupont", "alice@example.com", "pass123", 25, "France", "Paris", false);
```

**✅ Objet de paramètres :**
```ts
interface CreateUserParams {
  firstName: string;
  lastName: string;
  email: string;
  password: string;
  age: number;
  country: string;
  city: string;
  isPremium: boolean;
}

function createUser(params: CreateUserParams): User {
  // ...
}

// Appel lisible
createUser({
  firstName: "Alice",
  lastName: "Dupont",
  email: "alice@example.com",
  password: "pass123",
  age: 25,
  country: "France",
  city: "Paris",
  isPremium: false
});
```

### 5.3 Pas d'effets de bord

**Principe :** Une fonction ne doit pas modifier d'état en dehors de son scope.

**❌ Effet de bord :**
```ts
let totalOrders = 0; // État global

function processOrder(order: Order): void {
  // ...
  totalOrders++; // ⚠️ Modifie état global
}
```

**✅ Fonction pure ou effet explicite :**
```ts
// Version pure
function calculateNewTotal(currentTotal: number): number {
  return currentTotal + 1;
}

// Ou version avec effet explicite dans le nom
function processOrderAndIncrementCounter(order: Order, counter: OrderCounter): void {
  processOrder(order);
  counter.increment();
}
```

## 6. Commentaires : Quand et pourquoi

### 6.1 Le meilleur commentaire est... pas de commentaire

**Préférez du code auto-explicatif :**

**❌ Commentaire inutile :**
```ts
// Incrémente i
i++;

// Récupère l'utilisateur
const user = getUser();

// Boucle sur les produits
for (const product of products) {
  // ...
}
```

**✅ Code auto-explicatif :**
```ts
const activeUsers = users.filter(user => user.isActive);
const totalPrice = items.reduce((sum, item) => sum + item.price, 0);
```

### 6.2 Quand commenter

**Commentaires utiles :**

1. **Intention complexe :**
```ts
// Utilise binary search car la liste est triée et peut contenir
// des millions d'entrées (performance critique)
const index = binarySearch(sortedList, target);
```

2. **Avertissements :**
```ts
// ATTENTION: Cette fonction modifie le tableau en place
function sortInPlace(arr: number[]): void {
  arr.sort((a, b) => a - b);
}
```

3. **Explication d'un algorithme :**
```ts
// Algorithme de Luhn pour valider les numéros de carte bancaire
function validateCreditCard(number: string): boolean {
  // ...
}
```

4. **TODOs :**
```ts
// TODO: Migrer vers la nouvelle API v2 (ticket #1234)
// FIXME: Race condition possible ici (voir issue #456)
```

**❌ Commentaires à éviter :**
```ts
// Mauvais : redondant
const daysSinceCreation = 30; // 30 jours

// Mauvais : commentaire périmé
const TAX_RATE = 0.2; // Taux de TVA 19.6% (obsolète!)

// Mauvais : code commenté
// function oldImplementation() {
//   return something;
// }
```

## 7. Erreurs et pièges à éviter

### 7.1 Code smells courants

| Code Smell | Description | Solution |
|------------|-------------|----------|
| **Long Method** | Fonction > 50 lignes | Extraire méthodes |
| **Large Class** | Classe > 500 lignes | Séparer responsabilités |
| **Duplication** | Code copié-collé | Extraire fonction commune |
| **Magic Numbers** | `if (status === 3)` | Constantes nommées |
| **Dead Code** | Code jamais exécuté | Supprimer |
| **God Object** | Classe qui fait tout | Diviser |

### 7.2 Pièges courants

**Piège 1 : Optimisation prématurée**
```ts
// ❌ Complexité inutile pour "optimiser"
const cache = new Map();
function getUser(id) {
  if (!cache.has(id)) {
    cache.set(id, fetchUser(id));
  }
  return cache.get(id);
}

// ✅ Commencer simple
function getUser(id: number): User {
  return fetchUser(id);
}
// Optimiser seulement si performance est un problème mesuré
```

**Piège 2 : Abstractions prématurées**
```ts
// ❌ Interface "au cas où"
interface Animal {
  eat(): void;
  sleep(): void;
  fly(): void;  // Tous les animaux ne volent pas!
}

// ✅ Commencer concret
class Dog {
  eat(): void { }
  sleep(): void { }
  bark(): void { }
}
```

### 7.3 Impacts sur les logiciels/webapps

| Problème | Conséquence |
|----------|-------------|
| **Code sale** | Bugs multiples, développement ralenti |
| **Fonctions longues** | Impossible à tester, bugs cachés |
| **Mauvais nommage** | Temps perdu à comprendre le code |
| **Duplication** | Bugs incohérents, maintenance difficile |
| **Pas de refactoring** | Dette technique, projet ingérable |

## 8. Résumé de l'essentiel

### Points clés à retenir

1. **Clean Code = Code pour humains**
   - Lisible avant tout
   - Simple plutôt que clever
   - Intention claire

2. **Principes fondamentaux**
   - **DRY** : Pas de duplication
   - **KISS** : Rester simple
   - **YAGNI** : Pas de sur-conception
   - **SRP** : Une responsabilité

3. **Nommage révélateur**
   - Variables : noms descriptifs
   - Fonctions : verbe + complément
   - Booléens : is/has/can

4. **Fonctions courtes**
   - Une seule responsabilité
   - 5-20 lignes idéal
   - Max 3 paramètres

### Checklist Clean Code

**Avant de committer :**
- [ ] Noms révèlent l'intention ?
- [ ] Fonctions < 20 lignes ?
- [ ] Pas de duplication ?
- [ ] Code simple (pas clever) ?
- [ ] Commentaires nécessaires seulement ?
- [ ] Pas de magic numbers ?
- [ ] Testable facilement ?

### Refactoring progressif

**Règle du scout :**
> Laissez le code plus propre que vous l'avez trouvé

**Comment :**
1. Identifier un code smell
2. Écrire tests si nécessaire
3. Refactorer (petit pas)
4. Vérifier tests passent
5. Commit
6. Répéter

**Ne pas :**
- ❌ Tout refactorer d'un coup
- ❌ Refactorer sans tests
- ❌ Changer comportement

---

**En une phrase :**

> Le Clean Code consiste à écrire du code lisible et maintenable en suivant des principes simples (DRY, KISS, YAGNI), en choisissant des noms révélateurs, en gardant les fonctions courtes avec une seule responsabilité, et en refactorant continuellement pour éviter l'accumulation de dette technique.
