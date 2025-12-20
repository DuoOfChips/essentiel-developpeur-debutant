# Encapsulation : Protéger et Contrôler l'Accès aux Données

## Introduction

### Objectifs du cours
Après avoir lu ce cours, vous serez capable de :
- Comprendre le principe d'encapsulation en POO
- Utiliser les modificateurs d'accès (`public`, `private`, `protected`)
- Créer des getters et setters pour contrôler l'accès aux données
- Protéger l'intégrité des objets
- Appliquer l'encapsulation dans des contextes métier réels

### Scope de la notion
L'encapsulation permet de :
- **Protéger** les données sensibles contre les modifications non contrôlées
- **Cacher** les détails d'implémentation
- **Valider** les données avant leur modification
- **Maintenir** un état cohérent des objets
- Créer des APIs publiques claires et stables

Dans les webapps métier, l'encapsulation est cruciale pour :
- Garantir l'intégrité des données métier (prix, quantités, montants...)
- Empêcher des modifications non autorisées
- Faciliter l'évolution du code sans casser les clients
- Centraliser la logique de validation

---

## 1. Rappels

Avant ce cours, vous devriez maîtriser :
- Les classes et objets (voir cours POO)
- Les propriétés et méthodes
- Le mot-clé `this`

```ts
class User {
  firstName: string;
  lastName: string;

  constructor(firstName: string, lastName: string) {
    this.firstName = firstName;
    this.lastName = lastName;
  }
}
```

---

## 2. Définitions et Concepts Clés

### 2.1 Qu'est-ce que l'Encapsulation ?

L'**encapsulation** est le principe de **cacher les détails internes** d'un objet et de n'exposer que ce qui est nécessaire via une interface publique.

**Analogie de la vie quotidienne :**
Une **voiture** 🚗 :
- Vous utilisez le **volant**, les **pédales**, le **levier de vitesse** (interface publique)
- Vous ne voyez pas et ne manipulez pas directement le **moteur**, la **transmission**, les **injecteurs** (implémentation cachée)
- Si le constructeur change le moteur pour un modèle plus performant, vous continuez à conduire de la même façon

De même, en programmation :
- L'utilisateur de votre classe accède aux **méthodes publiques**
- Il ne doit pas accéder directement aux **propriétés internes**
- Vous pouvez changer l'implémentation sans impacter les utilisateurs

### 2.2 Les Modificateurs d'Accès

TypeScript propose 3 modificateurs d'accès :

| Modificateur | Accès depuis la classe | Accès depuis les enfants | Accès depuis l'extérieur |
|--------------|------------------------|--------------------------|--------------------------|
| `public`     | ✅ Oui                 | ✅ Oui                    | ✅ Oui                    |
| `protected`  | ✅ Oui                 | ✅ Oui                    | ❌ Non                    |
| `private`    | ✅ Oui                 | ❌ Non                    | ❌ Non                    |

**Par défaut**, tout est `public` en TypeScript.

### 2.3 Information Hiding (Masquage de l'information)

Le but de l'encapsulation est de **cacher** comment les choses fonctionnent à l'intérieur et d'exposer seulement ce qui est nécessaire.

**Avantages :**
- ✅ Simplification de l'API publique
- ✅ Liberté de modifier l'implémentation
- ✅ Protection contre les erreurs de manipulation
- ✅ Validation centralisée des données

---

## 3. Ce qui se passe dans l'ordinateur

### 3.1 Niveau compilateur

Les modificateurs d'accès (`public`, `private`, `protected`) sont des **vérifications au moment de la compilation** en TypeScript.

```ts
class BankAccount {
  private balance: number = 0;

  deposit(amount: number): void {
    this.balance += amount; // ✅ OK dans la classe
  }
}

const account = new BankAccount();
account.balance = 1000000; // ❌ Erreur de compilation
```

**Important :** Une fois compilé en JavaScript, ces protections **disparaissent** ! Le code JavaScript généré n'a pas de concept de `private`.

```js
// JavaScript généré
class BankAccount {
  constructor() {
    this.balance = 0;
  }
  deposit(amount) {
    this.balance += amount;
  }
}
```

### 3.2 Private Fields JavaScript (ES2022)

JavaScript moderne (ES2022) introduit les **vraies propriétés privées** avec `#` :

```ts
class BankAccount {
  #balance: number = 0; // Vraiment privé, même en JS

  deposit(amount: number): void {
    this.#balance += amount;
  }

  getBalance(): number {
    return this.#balance;
  }
}

const account = new BankAccount();
console.log(account.#balance); // ❌ Erreur même en JavaScript runtime!
```

**Différence :**
- `private` TypeScript → Protection à la compilation uniquement
- `#` JavaScript → Protection à la compilation ET au runtime

---

## 4. Déroulé du cours : Encapsulation en pratique

### 4.1 Public : Accès libre

```ts
class User {
  public firstName: string; // Explicite (optionnel)
  public lastName: string;

  constructor(firstName: string, lastName: string) {
    this.firstName = firstName;
    this.lastName = lastName;
  }

  public getFullName(): string {
    return `${this.firstName} ${this.lastName}`;
  }
}

const user = new User("Alice", "Dupont");
console.log(user.firstName); // ✅ OK
user.firstName = "Bob"; // ✅ OK
```

**Quand utiliser `public` :**
- Données sans logique de validation
- Méthodes faisant partie de l'API publique
- Par défaut pour les méthodes exposées

### 4.2 Private : Accès restreint à la classe

```ts
class BankAccount {
  private balance: number = 0;
  private accountNumber: string;

  constructor(accountNumber: string) {
    this.accountNumber = accountNumber;
  }

  public deposit(amount: number): void {
    if (amount <= 0) {
      throw new Error("Le montant doit être positif");
    }
    this.balance += amount;
  }

  public withdraw(amount: number): void {
    if (amount > this.balance) {
      throw new Error("Solde insuffisant");
    }
    this.balance -= amount;
  }

  public getBalance(): number {
    return this.balance;
  }
}

const account = new BankAccount("FR123456");
account.deposit(100);
console.log(account.getBalance()); // 100

// ❌ Erreurs de compilation
// account.balance = 1000000;
// account.accountNumber = "FR000000";
```

**Avantages :**
- ✅ Impossible de modifier le solde sans passer par `deposit` ou `withdraw`
- ✅ Validation centralisée
- ✅ Intégrité des données garantie

### 4.3 Protected : Accès dans la classe et les enfants

```ts
class Animal {
  protected name: string;
  protected age: number;

  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }

  protected makeSound(): void {
    console.log("Some generic sound");
  }
}

class Dog extends Animal {
  private breed: string;

  constructor(name: string, age: number, breed: string) {
    super(name, age);
    this.breed = breed;
  }

  public bark(): void {
    // ✅ OK : accès aux membres protected du parent
    console.log(`${this.name} (${this.breed}) says: Woof!`);
    this.makeSound();
  }
}

const dog = new Dog("Rex", 3, "Labrador");
dog.bark(); // ✅ OK
// dog.name; // ❌ Erreur : protected
// dog.makeSound(); // ❌ Erreur : protected
```

**Quand utiliser `protected` :**
- Propriétés/méthodes utilisées par les classes enfants
- Partage de logique dans une hiérarchie de classes
- Éviter la duplication dans les sous-classes

### 4.4 Getters et Setters

Les **getters** et **setters** permettent un accès contrôlé aux propriétés privées :

```ts
class Product {
  private _price: number;
  private _stock: number;

  constructor(private name: string, price: number, stock: number) {
    this._price = price;
    this._stock = stock;
  }

  // Getter : lecture seule
  public get price(): number {
    return this._price;
  }

  // Setter : validation avant modification
  public set price(value: number) {
    if (value < 0) {
      throw new Error("Le prix ne peut pas être négatif");
    }
    if (value > 1000000) {
      throw new Error("Prix trop élevé (max: 1 000 000€)");
    }
    this._price = value;
  }

  public get stock(): number {
    return this._stock;
  }

  public set stock(value: number) {
    if (value < 0) {
      throw new Error("Le stock ne peut pas être négatif");
    }
    this._stock = value;
  }

  // Propriété calculée (getter sans setter)
  public get isAvailable(): boolean {
    return this._stock > 0;
  }

  public get totalValue(): number {
    return this._price * this._stock;
  }
}

const product = new Product("Laptop", 1000, 10);

// Utilisation comme des propriétés normales
console.log(product.price); // 1000
product.price = 1200; // ✅ Validation OK
console.log(product.isAvailable); // true
console.log(product.totalValue); // 12000

// product.price = -500; // ❌ Error: Le prix ne peut pas être négatif
```

**Avantages des getters/setters :**
- ✅ Validation automatique
- ✅ Calculs à la demande
- ✅ Syntaxe naturelle (pas de `getPrice()` / `setPrice()`)
- ✅ Possibilité d'ajouter de la logique plus tard

### 4.5 Readonly : Propriétés en lecture seule

```ts
class User {
  public readonly id: string;
  public readonly createdAt: Date;
  public name: string;

  constructor(id: string, name: string) {
    this.id = id;
    this.createdAt = new Date();
    this.name = name;
  }

  public updateName(newName: string): void {
    this.name = newName; // ✅ OK
    // this.id = "new-id"; // ❌ Erreur : readonly
  }
}

const user = new User("U001", "Alice");
console.log(user.id); // "U001"
user.name = "Bob"; // ✅ OK
// user.id = "U002"; // ❌ Erreur : readonly
// user.createdAt = new Date(); // ❌ Erreur : readonly
```

**Quand utiliser `readonly` :**
- Identifiants uniques (id, uuid)
- Dates de création/modification
- Configuration immuable
- Constantes d'instance

---

## 5. Cas d'usage métier concrets

### 5.1 Système de paiement sécurisé

```ts
class CreditCard {
  private cardNumber: string;
  private cvv: string;
  private expirationDate: Date;
  private balance: number;

  constructor(cardNumber: string, cvv: string, expirationDate: Date, balance: number) {
    this.validateCardNumber(cardNumber);
    this.validateCVV(cvv);
    this.cardNumber = cardNumber;
    this.cvv = cvv;
    this.expirationDate = expirationDate;
    this.balance = balance;
  }

  // Masquer le numéro de carte (sauf 4 derniers chiffres)
  public getMaskedCardNumber(): string {
    const lastFour = this.cardNumber.slice(-4);
    return `**** **** **** ${lastFour}`;
  }

  public isExpired(): boolean {
    return new Date() > this.expirationDate;
  }

  public charge(amount: number, cvv: string): boolean {
    if (this.isExpired()) {
      throw new Error("Carte expirée");
    }

    if (cvv !== this.cvv) {
      throw new Error("CVV invalide");
    }

    if (amount > this.balance) {
      return false; // Solde insuffisant
    }

    this.balance -= amount;
    return true;
  }

  private validateCardNumber(cardNumber: string): void {
    if (cardNumber.length !== 16 || !/^\d+$/.test(cardNumber)) {
      throw new Error("Numéro de carte invalide");
    }
  }

  private validateCVV(cvv: string): void {
    if (cvv.length !== 3 || !/^\d+$/.test(cvv)) {
      throw new Error("CVV invalide");
    }
  }

  // ❌ PAS de getter pour cardNumber ou cvv (sécurité!)
}

const card = new CreditCard("1234567890123456", "123", new Date("2025-12-31"), 1000);
console.log(card.getMaskedCardNumber()); // "**** **** **** 3456"

// card.cardNumber; // ❌ Erreur : private
// card.cvv; // ❌ Erreur : private
card.charge(100, "123"); // ✅ OK
```

### 5.2 Gestion d'utilisateurs avec validation

```ts
class User {
  private _email: string;
  private _age: number;
  private passwordHash: string;

  constructor(
    public readonly id: string,
    email: string,
    password: string,
    age: number
  ) {
    this.email = email; // Utilise le setter
    this.age = age; // Utilise le setter
    this.passwordHash = this.hashPassword(password);
  }

  public get email(): string {
    return this._email;
  }

  public set email(value: string) {
    if (!this.isValidEmail(value)) {
      throw new Error("Email invalide");
    }
    this._email = value;
  }

  public get age(): number {
    return this._age;
  }

  public set age(value: number) {
    if (value < 18) {
      throw new Error("L'utilisateur doit avoir au moins 18 ans");
    }
    if (value > 150) {
      throw new Error("Âge invalide");
    }
    this._age = value;
  }

  public changePassword(oldPassword: string, newPassword: string): void {
    if (!this.verifyPassword(oldPassword)) {
      throw new Error("Ancien mot de passe incorrect");
    }

    if (newPassword.length < 8) {
      throw new Error("Le mot de passe doit contenir au moins 8 caractères");
    }

    this.passwordHash = this.hashPassword(newPassword);
  }

  public verifyPassword(password: string): boolean {
    return this.hashPassword(password) === this.passwordHash;
  }

  private isValidEmail(email: string): boolean {
    return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
  }

  private hashPassword(password: string): string {
    // Simplification - en production, utiliser bcrypt
    return `hashed_${password}`;
  }
}

const user = new User("U001", "alice@example.com", "password123", 25);
console.log(user.email); // "alice@example.com"

user.email = "newemail@example.com"; // ✅ OK
// user.email = "invalid-email"; // ❌ Error: Email invalide
// user.age = 15; // ❌ Error: L'utilisateur doit avoir au moins 18 ans
```

### 5.3 Gestion de stock e-commerce

```ts
class Inventory {
  private items: Map<string, InventoryItem> = new Map();

  public addProduct(productId: string, name: string, quantity: number, minStock: number): void {
    if (this.items.has(productId)) {
      throw new Error(`Produit ${productId} déjà existant`);
    }

    this.items.set(productId, new InventoryItem(productId, name, quantity, minStock));
  }

  public increaseStock(productId: string, quantity: number): void {
    const item = this.getItem(productId);
    item.increase(quantity);
  }

  public decreaseStock(productId: string, quantity: number): void {
    const item = this.getItem(productId);
    item.decrease(quantity);
  }

  public getStockLevel(productId: string): number {
    return this.getItem(productId).quantity;
  }

  public needsRestock(productId: string): boolean {
    return this.getItem(productId).needsRestock();
  }

  public getLowStockProducts(): InventoryItem[] {
    return Array.from(this.items.values()).filter(item => item.needsRestock());
  }

  private getItem(productId: string): InventoryItem {
    const item = this.items.get(productId);
    if (!item) {
      throw new Error(`Produit ${productId} introuvable`);
    }
    return item;
  }
}

class InventoryItem {
  private _quantity: number;

  constructor(
    public readonly productId: string,
    public readonly name: string,
    quantity: number,
    private readonly minStock: number
  ) {
    this._quantity = quantity;
  }

  public get quantity(): number {
    return this._quantity;
  }

  public increase(amount: number): void {
    if (amount <= 0) {
      throw new Error("La quantité doit être positive");
    }
    this._quantity += amount;
  }

  public decrease(amount: number): void {
    if (amount <= 0) {
      throw new Error("La quantité doit être positive");
    }
    if (amount > this._quantity) {
      throw new Error(`Stock insuffisant (disponible: ${this._quantity})`);
    }
    this._quantity -= amount;
  }

  public needsRestock(): boolean {
    return this._quantity < this.minStock;
  }
}

const inventory = new Inventory();
inventory.addProduct("P001", "Laptop", 50, 10);
inventory.addProduct("P002", "Mouse", 5, 20);

console.log(inventory.getStockLevel("P001")); // 50
inventory.decreaseStock("P001", 10);
console.log(inventory.getStockLevel("P001")); // 40

console.log(inventory.needsRestock("P002")); // true (5 < 20)
console.log(inventory.getLowStockProducts()); // [InventoryItem for P002]
```

---

## 6. Erreurs courantes & Comment les éviter

### Erreur 1 : Tout mettre en public

```ts
// ❌ Mauvais : tout est public
class BankAccount {
  public balance: number = 0;
  public accountNumber: string;

  constructor(accountNumber: string) {
    this.accountNumber = accountNumber;
  }
}

const account = new BankAccount("FR123");
account.balance = 1000000; // ⚠️ Aucune validation!

// ✅ Bon : propriétés privées avec accès contrôlé
class BankAccount {
  private balance: number = 0;
  private accountNumber: string;

  constructor(accountNumber: string) {
    this.accountNumber = accountNumber;
  }

  public deposit(amount: number): void {
    if (amount > 0) {
      this.balance += amount;
    }
  }

  public getBalance(): number {
    return this.balance;
  }
}
```

### Erreur 2 : Exposer des getters qui retournent des objets mutables

```ts
// ❌ Problème : l'objet retourné peut être modifié
class Order {
  private items: Product[] = [];

  public getItems(): Product[] {
    return this.items; // ⚠️ Retourne la référence directe!
  }
}

const order = new Order();
const items = order.getItems();
items.push(new Product()); // ⚠️ Modifie l'état interne!

// ✅ Solution 1 : retourner une copie
class Order {
  private items: Product[] = [];

  public getItems(): Product[] {
    return [...this.items]; // Copie superficielle
  }
}

// ✅ Solution 2 : retourner readonly
class Order {
  private items: Product[] = [];

  public getItems(): readonly Product[] {
    return this.items;
  }
}
```

### Erreur 3 : Ne pas valider dans les setters

```ts
// ❌ Setter sans validation
class Product {
  private _price: number = 0;

  public set price(value: number) {
    this._price = value; // ⚠️ Accepte n'importe quoi!
  }
}

const product = new Product();
product.price = -100; // ⚠️ Prix négatif accepté!

// ✅ Setter avec validation
class Product {
  private _price: number = 0;

  public set price(value: number) {
    if (value < 0) {
      throw new Error("Le prix ne peut pas être négatif");
    }
    this._price = value;
  }
}
```

### Erreur 4 : Utiliser `protected` au lieu de `private` sans raison

```ts
// ❌ Protected sans besoin d'héritage
class User {
  protected email: string; // Pourquoi protected ?
  protected password: string; // Dangereux!
}

// ✅ Private par défaut
class User {
  private email: string;
  private passwordHash: string;
}
```

### Erreur 5 : Oublier readonly pour les propriétés immuables

```ts
// ❌ ID modifiable
class User {
  public id: string;
  public createdAt: Date;

  constructor(id: string) {
    this.id = id;
    this.createdAt = new Date();
  }
}

const user = new User("U001");
user.id = "U002"; // ⚠️ L'ID ne devrait jamais changer!

// ✅ Utiliser readonly
class User {
  public readonly id: string;
  public readonly createdAt: Date;

  constructor(id: string) {
    this.id = id;
    this.createdAt = new Date();
  }
}
```

---

## 7. Exercices

### Exercice 1 : Compte bancaire sécurisé

Créez une classe `BankAccount` avec :
- Propriétés privées : `accountNumber`, `balance`, `owner`
- Méthodes publiques : `deposit()`, `withdraw()`, `transfer()`, `getBalance()`
- Validation : montants positifs, solde suffisant
- Historique des transactions (privé, accessible via getter)

### Exercice 2 : Gestion de notes d'étudiants

Créez une classe `Student` avec :
- Propriétés : `id` (readonly), `name`, `grades` (privé)
- Méthode `addGrade()` avec validation (note entre 0 et 20)
- Getter `average` qui calcule la moyenne
- Getter `isPassing` (moyenne >= 10)
- Méthode privée `calculateAverage()`

---

## 8. Comportement Senior

### 8.1 Règle de base : Private par défaut

```ts
// ✅ Approche senior
class UserService {
  private users: Map<string, User> = new Map();
  private emailValidator: EmailValidator;

  constructor() {
    this.emailValidator = new EmailValidator();
  }

  public createUser(email: string, name: string): User {
    // Logique métier
  }

  private validateUser(user: User): boolean {
    // Validation interne
  }
}
```

**Règle :** Commencez toujours par `private`, passez à `protected` ou `public` seulement si nécessaire.

### 8.2 Éviter les getters/setters inutiles

```ts
// ❌ Getters/setters sans logique = inutile
class Product {
  private _name: string;

  public get name(): string {
    return this._name;
  }

  public set name(value: string) {
    this._name = value;
  }
}

// ✅ Si pas de logique, utilisez public
class Product {
  public name: string;
}

// ✅ Utilisez getters/setters seulement avec de la logique
class Product {
  private _price: number;

  public get price(): number {
    return this._price;
  }

  public set price(value: number) {
    if (value < 0) throw new Error("Prix invalide");
    this._price = value;
  }
}
```

### 8.3 Immutabilité quand c'est possible

```ts
// ✅ Classe immuable
class Money {
  constructor(
    public readonly amount: number,
    public readonly currency: string
  ) {}

  public add(other: Money): Money {
    if (this.currency !== other.currency) {
      throw new Error("Devises incompatibles");
    }
    return new Money(this.amount + other.amount, this.currency);
  }
}

const price1 = new Money(100, "EUR");
const price2 = new Money(50, "EUR");
const total = price1.add(price2); // Nouvel objet
```

### 8.4 Documentation des APIs publiques

```ts
class PaymentService {
  /**
   * Traite un paiement par carte bancaire
   * @param amount Montant en euros (doit être positif)
   * @param cardNumber Numéro de carte (16 chiffres)
   * @param cvv Code CVV (3 chiffres)
   * @returns true si le paiement est accepté, false sinon
   * @throws Error si les paramètres sont invalides
   */
  public processPayment(amount: number, cardNumber: string, cvv: string): boolean {
    // ...
  }
}
```

---

## 9. Résumé

### Ce que vous avez appris

- **Encapsulation** : Cacher les détails internes, exposer une API publique
- **`private`** : Accessible uniquement dans la classe
- **`protected`** : Accessible dans la classe et ses enfants
- **`public`** : Accessible partout (par défaut)
- **`readonly`** : Propriété en lecture seule
- **Getters/Setters** : Accès contrôlé avec validation
- **`#` (JavaScript)** : Vraies propriétés privées au runtime

### Quand utiliser l'encapsulation

✅ **Toujours encapsuler :**
- Données sensibles (mots de passe, numéros de carte...)
- Données avec logique de validation (prix, quantités...)
- État interne qui doit rester cohérent
- Implémentation qui peut changer

❌ **Encapsulation moins critique :**
- DTOs (Data Transfer Objects) simples
- Objets de configuration
- Structures de données pures

### Principe de base

> **Rendez tout privé par défaut, puis exposez progressivement ce qui est nécessaire.**

---

## 10. Ressources Externes

### Documentation
- [TypeScript Access Modifiers](https://www.typescriptlang.org/docs/handbook/2/classes.html#member-visibility)
- [JavaScript Private Fields](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/Private_class_fields)

### Vidéos
- [Encapsulation en POO - Grafikart](https://grafikart.fr/tutoriels/encapsulation-1235) (français)
- [TypeScript Private vs Public - Web Dev Simplified](https://www.youtube.com/watch?v=EJFPy9TvHyU) (anglais)

### Articles
- [Why Encapsulation Matters](https://stackify.com/oop-concept-for-beginners-what-is-encapsulation/)
- [Information Hiding](https://en.wikipedia.org/wiki/Information_hiding)
