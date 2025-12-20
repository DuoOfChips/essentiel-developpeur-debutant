# Refactoring sans Casser : Techniques et Bonnes Pratiques

## Introduction

### Objectifs du cours
Après avoir lu ce cours, vous serez capable de :
- Comprendre ce qu'est le refactoring et pourquoi c'est essentiel
- Identifier les "code smells" (mauvaises odeurs de code)
- Appliquer des techniques de refactoring sécurisées
- Refactorer du code sans introduire de bugs
- Utiliser les tests pour sécuriser vos refactorings
- Améliorer progressivement la qualité du code

### Scope de la notion
Le refactoring permet de :
- **Améliorer** la qualité du code sans changer son comportement
- **Réduire** la dette technique
- **Faciliter** l'ajout de nouvelles fonctionnalités
- **Préparer** le code pour des évolutions futures
- **Maintenir** un code sain et compréhensible

Dans les webapps métier (Angular/NestJS) :
- Refactorer pour améliorer la maintenabilité
- Préparer le code pour de nouvelles features
- Réduire la complexité des composants et services
- Faciliter les tests

---

## 1. Rappels

### Code propre
```ts
// Code lisible et bien structuré
class UserService {
  constructor(private repository: UserRepository) {}

  async createUser(data: CreateUserDto): Promise<User> {
    return this.repository.save(data);
  }
}
```

### Tests
```ts
describe('UserService', () => {
  it('should create user', async () => {
    // Test qui valide le comportement
  });
});
```

---

## 2. Définitions et Concepts Clés

### 2.1 Qu'est-ce que le Refactoring ?

Le **refactoring** est le processus d'**amélioration de la structure interne du code sans changer son comportement externe**.

**Analogie de la vie quotidienne :**
**Ranger une maison** 🏠 :
- Vous déplacez les objets pour mieux les organiser
- Vous jetez ce qui est inutile
- Vous regroupez ce qui va ensemble
- La maison a toujours les mêmes fonctions, mais elle est plus agréable et pratique

**Caractéristiques du refactoring :**
- ✅ Améliore la structure interne
- ❌ Ne change PAS le comportement
- ✅ Facilite la maintenance
- ✅ Rend le code plus lisible
- ✅ Prépare pour les futures évolutions

### 2.2 Refactoring vs Réécriture

| Refactoring | Réécriture |
|-------------|------------|
| Modifications incrémentales | Tout recommencer de zéro |
| Comportement préservé | Nouveau comportement possible |
| Faible risque | Risque élevé |
| Tests existants passent | Nouveaux tests nécessaires |
| Continue en parallèle du développement | Bloque tout le reste |

**Règle d'or :** Préférez le refactoring progressif à la réécriture totale.

### 2.3 Code Smells (Mauvaises Odeurs)

Les **code smells** sont des indices qu'un code a besoin de refactoring.

**Analogie :** Comme une mauvaise odeur signale que quelque chose ne va pas, un code smell signale un problème potentiel.

**Exemples de code smells :**
- Méthodes trop longues (> 20 lignes)
- Classes trop grandes (> 300 lignes)
- Duplication de code
- Noms peu clairs
- Trop de paramètres (> 3)
- Commentaires excessifs
- Code mort (jamais exécuté)
- Conditions complexes imbriquées

---

## 3. Règles Fondamentales du Refactoring

### Règle 1 : Tests d'abord
```ts
// ✅ Avant de refactorer, avoir des tests
describe('calculateTotal', () => {
  it('should calculate total with tax', () => {
    const result = calculateTotal([
      { price: 100, quantity: 2 }
    ], 0.2);
    expect(result).toBe(240); // 200 + 20% taxe
  });
});

// Maintenant on peut refactorer en toute sécurité
```

**Si pas de tests :** Ajoutez-en avant de refactorer !

### Règle 2 : Petits pas
```ts
// ✅ Refactoring en petites étapes

// Étape 1 : Extraire une constante
const TAX_RATE = 0.2;

// Étape 2 : Extraire une fonction
function calculateTax(amount: number): number {
  return amount * TAX_RATE;
}

// Étape 3 : Simplifier la fonction principale
function calculateTotal(items: Item[]): number {
  const subtotal = items.reduce((sum, item) => sum + item.price * item.quantity, 0);
  const tax = calculateTax(subtotal);
  return subtotal + tax;
}

// Vérifier les tests après chaque étape ✅
```

### Règle 3 : Un seul refactoring à la fois
```ts
// ❌ Mauvais : tout refactorer d'un coup
// - Renommer des variables
// - Extraire des fonctions
// - Changer l'architecture
// - Modifier la logique
// → Trop de changements = impossible de débugger

// ✅ Bon : un refactoring, commit, tests, puis le suivant
// 1. Renommer une variable → commit
// 2. Extraire une fonction → commit
// 3. Simplifier une condition → commit
```

### Règle 4 : Ne pas ajouter de fonctionnalités

```ts
// ❌ Mauvais : refactorer ET ajouter une feature
function calculateTotal(items: Item[]): number {
  const subtotal = items.reduce(...);
  const tax = calculateTax(subtotal);
  const discount = calculateDiscount(subtotal); // ⚠️ Nouvelle feature !
  return subtotal + tax - discount;
}

// ✅ Bon : refactorer OU ajouter une feature, pas les deux
// 1. D'abord refactorer
// 2. Puis ajouter la feature dans un commit séparé
```

---

## 4. Techniques de Refactoring Classiques

### 4.1 Extract Method (Extraire une méthode)

**Quand :** Une fonction fait trop de choses ou est trop longue.

```ts
// ❌ Avant : fonction longue
function processOrder(order: Order): void {
  // Validation
  if (!order.items || order.items.length === 0) {
    throw new Error("Order is empty");
  }
  for (const item of order.items) {
    if (item.quantity <= 0) {
      throw new Error("Invalid quantity");
    }
  }

  // Calcul du total
  let total = 0;
  for (const item of order.items) {
    total += item.price * item.quantity;
  }
  const tax = total * 0.2;
  total += tax;

  // Application de remises
  if (order.customer.isPremium) {
    total *= 0.9;
  }
  if (total > 100) {
    total -= 10;
  }

  // Sauvegarde
  database.save(order);

  // Email
  sendEmail(order.customer.email, `Total: ${total}`);
}

// ✅ Après : fonctions extraites
function processOrder(order: Order): void {
  validateOrder(order);
  const total = calculateTotal(order);
  saveOrder(order);
  notifyCustomer(order, total);
}

function validateOrder(order: Order): void {
  if (!order.items || order.items.length === 0) {
    throw new Error("Order is empty");
  }
  for (const item of order.items) {
    if (item.quantity <= 0) {
      throw new Error("Invalid quantity");
    }
  }
}

function calculateTotal(order: Order): number {
  const subtotal = order.items.reduce((sum, item) => sum + item.price * item.quantity, 0);
  const tax = subtotal * 0.2;
  let total = subtotal + tax;

  if (order.customer.isPremium) {
    total *= 0.9;
  }
  if (total > 100) {
    total -= 10;
  }

  return total;
}

function saveOrder(order: Order): void {
  database.save(order);
}

function notifyCustomer(order: Order, total: number): void {
  sendEmail(order.customer.email, `Total: ${total}`);
}
```

### 4.2 Rename (Renommer)

**Quand :** Les noms ne sont pas clairs ou trompeurs.

```ts
// ❌ Avant : noms peu clairs
function calc(a: any[], b: number): number {
  let t = 0;
  for (const x of a) {
    t += x.p * x.q;
  }
  return t * (1 + b);
}

// ✅ Après : noms explicites
function calculateOrderTotalWithTax(items: OrderItem[], taxRate: number): number {
  let subtotal = 0;
  for (const item of items) {
    subtotal += item.price * item.quantity;
  }
  return subtotal * (1 + taxRate);
}
```

### 4.3 Extract Constant (Extraire une constante)

**Quand :** Des valeurs magiques (magic numbers) apparaissent dans le code.

```ts
// ❌ Avant : magic numbers
function calculatePrice(basePrice: number): number {
  const tax = basePrice * 0.2;
  const shipping = basePrice > 50 ? 0 : 5;
  return basePrice + tax + shipping;
}

// ✅ Après : constantes nommées
const TAX_RATE = 0.2;
const FREE_SHIPPING_THRESHOLD = 50;
const SHIPPING_COST = 5;

function calculatePrice(basePrice: number): number {
  const tax = basePrice * TAX_RATE;
  const shipping = basePrice > FREE_SHIPPING_THRESHOLD ? 0 : SHIPPING_COST;
  return basePrice + tax + shipping;
}
```

### 4.4 Replace Conditional with Polymorphism

**Quand :** Des if/else multiples sur un type.

```ts
// ❌ Avant : conditionnelles multiples
function calculateShipping(order: Order): number {
  if (order.shippingMethod === "standard") {
    return order.weight * 0.5;
  } else if (order.shippingMethod === "express") {
    return order.weight * 1.5 + 10;
  } else if (order.shippingMethod === "overnight") {
    return order.weight * 3 + 25;
  }
  return 0;
}

// ✅ Après : polymorphisme
interface ShippingStrategy {
  calculate(weight: number): number;
}

class StandardShipping implements ShippingStrategy {
  calculate(weight: number): number {
    return weight * 0.5;
  }
}

class ExpressShipping implements ShippingStrategy {
  calculate(weight: number): number {
    return weight * 1.5 + 10;
  }
}

class OvernightShipping implements ShippingStrategy {
  calculate(weight: number): number {
    return weight * 3 + 25;
  }
}

class Order {
  constructor(
    public weight: number,
    private shippingStrategy: ShippingStrategy
  ) {}

  calculateShipping(): number {
    return this.shippingStrategy.calculate(this.weight);
  }
}
```

### 4.5 Introduce Parameter Object

**Quand :** Trop de paramètres (> 3).

```ts
// ❌ Avant : trop de paramètres
function createUser(
  firstName: string,
  lastName: string,
  email: string,
  password: string,
  age: number,
  country: string,
  city: string,
  zipCode: string
): User {
  // ...
}

// Appel illisible
createUser("Alice", "Dupont", "alice@example.com", "pass123", 25, "France", "Paris", "75001");

// ✅ Après : objet de paramètres
interface CreateUserParams {
  firstName: string;
  lastName: string;
  email: string;
  password: string;
  age: number;
  address: {
    country: string;
    city: string;
    zipCode: string;
  };
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
  address: {
    country: "France",
    city: "Paris",
    zipCode: "75001"
  }
});
```

### 4.6 Replace Magic Number with Named Constant

```ts
// ❌ Avant
if (user.age >= 18 && user.age <= 65) {
  // ...
}

if (order.total > 100) {
  applyDiscount(order, 10);
}

// ✅ Après
const MIN_ADULT_AGE = 18;
const MAX_WORKING_AGE = 65;
const FREE_SHIPPING_THRESHOLD = 100;
const DISCOUNT_AMOUNT = 10;

if (user.age >= MIN_ADULT_AGE && user.age <= MAX_WORKING_AGE) {
  // ...
}

if (order.total > FREE_SHIPPING_THRESHOLD) {
  applyDiscount(order, DISCOUNT_AMOUNT);
}
```

### 4.7 Decompose Conditional (Décomposer les conditions)

```ts
// ❌ Avant : condition complexe
if (
  date.getMonth() > 5 &&
  date.getMonth() < 9 &&
  (user.isPremium || user.totalOrders > 10)
) {
  applyDiscount();
}

// ✅ Après : fonctions nommées
function isSummer(date: Date): boolean {
  const month = date.getMonth();
  return month > 5 && month < 9;
}

function isEligibleForDiscount(user: User): boolean {
  return user.isPremium || user.totalOrders > 10;
}

if (isSummer(date) && isEligibleForDiscount(user)) {
  applyDiscount();
}
```

---

## 5. Processus de Refactoring Sécurisé

### Étape 1 : Identifier le code smell
```ts
// Code smell détecté : fonction trop longue avec duplication
function calculateOrderPrice(order: Order): number {
  let total = 0;
  for (const item of order.items) {
    total += item.price * item.quantity;
  }

  let tax = 0;
  for (const item of order.items) {
    tax += item.price * item.quantity * 0.2;
  }

  return total + tax;
}
```

### Étape 2 : Écrire des tests (si absents)
```ts
describe('calculateOrderPrice', () => {
  it('should calculate price with tax', () => {
    const order = {
      items: [
        { price: 100, quantity: 2 },
        { price: 50, quantity: 1 }
      ]
    };
    const result = calculateOrderPrice(order);
    expect(result).toBe(300); // (200 + 50) + 20% = 300
  });

  it('should handle empty orders', () => {
    const order = { items: [] };
    const result = calculateOrderPrice(order);
    expect(result).toBe(0);
  });
});
```

### Étape 3 : Refactorer par petits pas
```ts
// Étape 3.1 : Extraire constante
const TAX_RATE = 0.2;

// Tests → ✅ Passent

// Étape 3.2 : Extraire fonction de calcul du subtotal
function calculateSubtotal(items: OrderItem[]): number {
  return items.reduce((sum, item) => sum + item.price * item.quantity, 0);
}

function calculateOrderPrice(order: Order): number {
  const subtotal = calculateSubtotal(order.items);
  
  let tax = 0;
  for (const item of order.items) {
    tax += item.price * item.quantity * TAX_RATE;
  }

  return subtotal + tax;
}

// Tests → ✅ Passent

// Étape 3.3 : Simplifier le calcul de la taxe
function calculateOrderPrice(order: Order): number {
  const subtotal = calculateSubtotal(order.items);
  const tax = subtotal * TAX_RATE;
  return subtotal + tax;
}

// Tests → ✅ Passent

// Étape 3.4 : Extraire fonction finale
function calculateTax(subtotal: number): number {
  return subtotal * TAX_RATE;
}

function calculateOrderPrice(order: Order): number {
  const subtotal = calculateSubtotal(order.items);
  const tax = calculateTax(subtotal);
  return subtotal + tax;
}

// Tests → ✅ Passent
```

### Étape 4 : Vérifier et commit
```bash
# Lancer tous les tests
npm test

# Si tout est vert ✅
git add .
git commit -m "refactor: simplify order price calculation"
```

---

## 6. Cas d'usage métier : Refactoring d'un service Angular

### Avant refactoring

```ts
// ❌ Code smell : trop de responsabilités, duplication, couplage fort
@Injectable({ providedIn: 'root' })
export class UserService {
  constructor(private http: HttpClient) {}

  createUser(data: any): Observable<any> {
    // Validation inline
    if (!data.email || !data.email.includes('@')) {
      throw new Error('Invalid email');
    }
    if (!data.password || data.password.length < 8) {
      throw new Error('Password too short');
    }

    // Transformation
    const user = {
      firstName: data.firstName.trim(),
      lastName: data.lastName.trim(),
      email: data.email.toLowerCase().trim(),
      password: data.password
    };

    // Appel HTTP
    return this.http.post('/api/users', user).pipe(
      tap(() => {
        // Log inline
        console.log('User created:', user.email);
        
        // Analytics inline
        if (window['gtag']) {
          window['gtag']('event', 'user_created');
        }

        // Notification inline
        alert('User created successfully!');
      }),
      catchError(error => {
        console.error('Error creating user:', error);
        alert('Error creating user');
        return throwError(error);
      })
    );
  }
}
```

### Après refactoring (étape par étape)

```ts
// ✅ Étape 1 : Extraire la validation
interface CreateUserDto {
  firstName: string;
  lastName: string;
  email: string;
  password: string;
}

class UserValidator {
  validate(data: CreateUserDto): void {
    this.validateEmail(data.email);
    this.validatePassword(data.password);
  }

  private validateEmail(email: string): void {
    if (!email || !email.includes('@')) {
      throw new Error('Invalid email');
    }
  }

  private validatePassword(password: string): void {
    if (!password || password.length < 8) {
      throw new Error('Password must be at least 8 characters');
    }
  }
}

// ✅ Étape 2 : Extraire la transformation
class UserMapper {
  toCreateDto(data: any): CreateUserDto {
    return {
      firstName: data.firstName?.trim() || '',
      lastName: data.lastName?.trim() || '',
      email: data.email?.toLowerCase().trim() || '',
      password: data.password || ''
    };
  }
}

// ✅ Étape 3 : Extraire le logging
@Injectable({ providedIn: 'root' })
export class LoggerService {
  log(message: string, data?: any): void {
    console.log(`[INFO] ${message}`, data);
  }

  error(message: string, error?: any): void {
    console.error(`[ERROR] ${message}`, error);
  }
}

// ✅ Étape 4 : Extraire les analytics
@Injectable({ providedIn: 'root' })
export class AnalyticsService {
  trackEvent(eventName: string, data?: any): void {
    if (window['gtag']) {
      window['gtag']('event', eventName, data);
    }
  }
}

// ✅ Étape 5 : Extraire les notifications
@Injectable({ providedIn: 'root' })
export class NotificationService {
  success(message: string): void {
    // Remplacer alert par une vraie notification (snackbar, toast...)
    alert(message);
  }

  error(message: string): void {
    alert(`Error: ${message}`);
  }
}

// ✅ Étape 6 : Service refactoré avec injection de dépendances
@Injectable({ providedIn: 'root' })
export class UserService {
  private validator = new UserValidator();
  private mapper = new UserMapper();

  constructor(
    private http: HttpClient,
    private logger: LoggerService,
    private analytics: AnalyticsService,
    private notification: NotificationService
  ) {}

  createUser(data: any): Observable<User> {
    const dto = this.mapper.toCreateDto(data);
    this.validator.validate(dto);

    return this.http.post<User>('/api/users', dto).pipe(
      tap(user => this.handleSuccess(user)),
      catchError(error => this.handleError(error))
    );
  }

  private handleSuccess(user: User): void {
    this.logger.log('User created', { email: user.email });
    this.analytics.trackEvent('user_created');
    this.notification.success('User created successfully!');
  }

  private handleError(error: any): Observable<never> {
    this.logger.error('Error creating user', error);
    this.notification.error('Failed to create user');
    return throwError(() => error);
  }
}
```

**Résultat :**
- ✅ SRP respecté (chaque classe a une responsabilité)
- ✅ Testable (facile de mocker les dépendances)
- ✅ Réutilisable (validation, logging, etc. réutilisables)
- ✅ Maintenable (facile de modifier une partie)

---

## 7. Erreurs courantes & Comment les éviter

### Erreur 1 : Refactorer sans tests

```ts
// ❌ Dangereux : refactorer sans filet de sécurité
function complexCalculation(data: any): number {
  // Refactoring de cette fonction sans tests
  // → Risque de casser le comportement
}

// ✅ Sécurisé : tests d'abord
describe('complexCalculation', () => {
  it('should return correct result', () => {
    expect(complexCalculation({ a: 5, b: 10 })).toBe(15);
  });
});

// Maintenant on peut refactorer en toute sécurité
```

### Erreur 2 : Refactorer et ajouter des features

```ts
// ❌ Mélanger refactoring et nouvelle feature
function processOrder(order: Order): void {
  // Refactoring + ajout de la gestion des coupons
  const discount = applyCoupon(order.coupon); // Nouvelle feature !
  // ...
}

// ✅ Séparer : d'abord refactorer, puis ajouter
// Commit 1 : refactoring
// Commit 2 : ajout de la feature coupons
```

### Erreur 3 : Refactorings trop ambitieux

```ts
// ❌ Tout refactorer d'un coup
// - Renommer 50 variables
// - Extraire 20 fonctions
// - Changer l'architecture
// → Impossible de débugger si ça casse

// ✅ Refactorings incrémentaux
// Commit 1 : Renommer 5 variables
// Commit 2 : Extraire 2 fonctions
// Commit 3 : ...
```

---

## 8. Exercices

### Exercice 1 : Refactorer un système de validation

Refactorez ce code :
```ts
function validateForm(form: any): boolean {
  if (!form.email || !form.email.includes('@') || form.email.length < 5) {
    return false;
  }
  if (!form.password || form.password.length < 8 || !/[0-9]/.test(form.password)) {
    return false;
  }
  if (!form.age || form.age < 18 || form.age > 150) {
    return false;
  }
  return true;
}
```

### Exercice 2 : Refactorer un service NestJS

Appliquez les principes SOLID et refactorez ce service :
```ts
@Injectable()
export class OrderService {
  async createOrder(data: any): Promise<any> {
    // Validation, calcul, sauvegarde, email, tout dans une méthode
  }
}
```

---

## 9. Comportement Senior

### 9.1 Refactoring continu

```ts
// ✅ Règle du scout : laisser le code plus propre
// Chaque fois que vous touchez du code, améliorez-le légèrement

// Avant de modifier une fonction :
// 1. Renommer une variable peu claire
// 2. Extraire une constante magique
// 3. Simplifier une condition
// Puis faire votre modification
```

### 9.2 Refactoring dans les code reviews

```ts
// ✅ En code review, suggérer des refactorings
// "Cette fonction pourrait être découpée en 3 petites fonctions"
// "Ces magic numbers devraient être des constantes"
// "Ce code est dupliqué, on pourrait factoriser"
```

### 9.3 Balance refactoring / features

```ts
// ✅ Règle des 80/20
// 80% du temps : features
// 20% du temps : refactoring et amélioration

// Chaque sprint :
// - Stories de features
// - Tech debt stories (refactoring)
```

---

## 10. Résumé

### Principes clés

1. **Tests d'abord** : Toujours avoir des tests avant de refactorer
2. **Petits pas** : Refactorings incrémentaux et commits fréquents
3. **Un changement à la fois** : Pas de mélange refactoring/features
4. **Comportement préservé** : Ne pas changer ce que fait le code
5. **Code smells** : Les identifier et les corriger progressivement

### Techniques essentielles

- Extract Method
- Rename
- Extract Constant
- Replace Conditional with Polymorphism
- Introduce Parameter Object
- Decompose Conditional

### Processus

1. Identifier le code smell
2. Écrire des tests (si absents)
3. Refactorer par petits pas
4. Vérifier les tests après chaque étape
5. Commit

### En une phrase

> **Le refactoring est l'amélioration continue de la structure du code sans changer son comportement, effectuée par petits pas sécurisés par des tests.**

---

## 11. Ressources Externes

### Livres
- **Refactoring** - Martin Fowler (livre de référence)
- **Clean Code** - Robert C. Martin
- **Working Effectively with Legacy Code** - Michael Feathers

### Sites
- [Refactoring Guru](https://refactoring.guru/refactoring) (catalogue de refactorings)
- [SourceMaking - Refactoring](https://sourcemaking.com/refactoring)

### Vidéos
- [Refactoring - Fireship](https://www.youtube.com/watch?v=D4auWwMsEnY) (anglais)
- [Refactoring en pratique](https://www.youtube.com/watch?v=vhYK3pDUijk) (français)

### Outils
- **ESLint** : Détection automatique de code smells
- **SonarQube** : Analyse de qualité du code
- **IDE Refactoring Tools** : VS Code, WebStorm (refactoring automatisé)
