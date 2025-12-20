# Programmation Orientée Objet : Classes et Objets

## Introduction

### Objectifs du cours
Après avoir lu ce cours, vous serez capable de :
- Comprendre les concepts fondamentaux de la POO (Programmation Orientée Objet)
- Créer et utiliser des classes en TypeScript
- Instancier des objets et manipuler leurs propriétés et méthodes
- Comprendre le rôle du constructeur et de `this`
- Structurer du code métier avec une approche orientée objet

### Scope de la notion
La POO permet de :
- **Organiser** le code en entités logiques (utilisateur, produit, commande...)
- **Réutiliser** du code grâce aux classes
- **Modéliser** le domaine métier de façon naturelle
- **Encapsuler** les données et comportements ensemble
- Faciliter la **maintenance** et l'**évolution** du code

Dans le contexte des webapps métier, la POO est essentielle pour :
- Modéliser les entités business (User, Product, Order, Invoice...)
- Créer des services réutilisables (EmailService, PaymentService...)
- Structurer le code frontend (Angular) et backend (NestJS)

---

## 1. Rappels

Avant de plonger dans la POO, assurons-nous que vous maîtrisez :

### Objets littéraux en JavaScript/TypeScript
```ts
const user = {
  name: "Alice",
  email: "alice@example.com",
  age: 25
};
```

### Fonctions
```ts
function greet(name: string): string {
  return `Hello, ${name}!`;
}
```

Ces concepts sont les briques de base. La POO va plus loin en **structurant** et **organisant** ces briques.

---

## 2. Définitions et Concepts Clés

### 2.1 Qu'est-ce que la POO ?

La **Programmation Orientée Objet** est un **paradigme de programmation** qui organise le code autour d'**objets** plutôt que de fonctions et de logique.

**Analogie de la vie quotidienne :**
Imagine une voiture 🚗 :
- Elle a des **propriétés** : couleur, marque, vitesse actuelle, niveau d'essence
- Elle a des **comportements** : démarrer, accélérer, freiner, tourner

En POO, on modélise cette voiture comme un **objet** qui regroupe ces propriétés et comportements.

### 2.2 Classe

Une **classe** est un **plan** (blueprint) ou **moule** qui définit :
- Les **propriétés** (attributs, données)
- Les **méthodes** (comportements, actions)

**Analogie :** Une classe est comme le plan architectural d'une maison. Le plan ne peut pas être habité, mais il décrit comment construire la maison.

```ts
class Voiture {
  // Propriétés
  marque: string;
  couleur: string;
  vitesse: number;

  // Méthode
  accelerer(): void {
    this.vitesse += 10;
  }
}
```

### 2.3 Objet (Instance)

Un **objet** est une **instance concrète** d'une classe.

**Analogie :** Si la classe est le plan de la maison, l'objet est la maison réelle construite à partir de ce plan. Vous pouvez construire plusieurs maisons (objets) à partir du même plan (classe).

```ts
const maVoiture = new Voiture(); // Création d'un objet
const voitureDeJean = new Voiture(); // Autre objet, même classe
```

### 2.4 Propriétés (Attributs)

Les **propriétés** sont les **données** stockées dans un objet.

```ts
class User {
  firstName: string;
  lastName: string;
  email: string;
  age: number;
}
```

### 2.5 Méthodes

Les **méthodes** sont les **fonctions** définies dans une classe qui décrivent les comportements.

```ts
class User {
  firstName: string;
  lastName: string;

  getFullName(): string {
    return `${this.firstName} ${this.lastName}`;
  }
}
```

### 2.6 Constructeur

Le **constructeur** est une méthode spéciale appelée lors de la création d'un objet. Il initialise les propriétés.

```ts
class User {
  firstName: string;
  lastName: string;

  constructor(firstName: string, lastName: string) {
    this.firstName = firstName;
    this.lastName = lastName;
  }
}

const user = new User("Alice", "Dupont");
```

### 2.7 Le mot-clé `this`

`this` fait référence à l'**instance actuelle** de la classe.

**Analogie :** Dans une conversation, "je" ou "moi" fait référence à vous-même. `this` dans une classe fait référence à l'objet lui-même.

```ts
class Counter {
  count: number = 0;

  increment(): void {
    this.count++; // this = l'instance actuelle
  }
}
```

---

## 3. Ce qui se passe dans l'ordinateur

### 3.1 Allocation mémoire

Quand vous créez un objet avec `new` :

```ts
const user = new User("Alice", "Dupont");
```

**Étapes dans la mémoire :**

1. **Allocation** : La machine réserve un bloc de mémoire pour stocker l'objet
2. **Initialisation** : Le constructeur est appelé et initialise les propriétés
3. **Référence** : La variable `user` stocke l'**adresse mémoire** (référence) de l'objet

```
Mémoire Heap:
┌─────────────────────┐
│ Adresse: 0x1A3F    │
│ ┌─────────────────┐ │
│ │ firstName: "Alice" │
│ │ lastName: "Dupont" │
│ └─────────────────┘ │
└─────────────────────┘
       ↑
       │
   user (0x1A3F)
```

### 3.2 Passage par référence

Les objets sont **passés par référence**, pas par valeur :

```ts
const user1 = new User("Alice", "Dupont");
const user2 = user1; // user2 pointe vers le même objet

user2.firstName = "Bob";
console.log(user1.firstName); // "Bob" ⚠️ Modifié !
```

**Important :** `user1` et `user2` pointent vers le **même emplacement mémoire**.

---

## 4. Déroulé du cours : Classes et Objets en TypeScript

### 4.1 Créer une classe simple

```ts
class Product {
  name: string;
  price: number;
  inStock: boolean;

  constructor(name: string, price: number, inStock: boolean = true) {
    this.name = name;
    this.price = price;
    this.inStock = inStock;
  }

  displayInfo(): string {
    return `${this.name} - ${this.price}€ ${this.inStock ? '✅' : '❌'}`;
  }

  applyDiscount(percentage: number): void {
    this.price = this.price * (1 - percentage / 100);
  }
}
```

**Utilisation :**
```ts
const laptop = new Product("MacBook Pro", 2500, true);
console.log(laptop.displayInfo()); // "MacBook Pro - 2500€ ✅"

laptop.applyDiscount(10);
console.log(laptop.price); // 2250
```

### 4.2 Syntaxe raccourcie du constructeur (TypeScript)

TypeScript offre une syntaxe plus concise :

```ts
// ❌ Syntaxe verbeuse
class User {
  firstName: string;
  lastName: string;

  constructor(firstName: string, lastName: string) {
    this.firstName = firstName;
    this.lastName = lastName;
  }
}

// ✅ Syntaxe raccourcie (équivalente)
class User {
  constructor(
    public firstName: string,
    public lastName: string
  ) {}
}
```

**Les modificateurs `public`, `private`, `protected` seront vus dans le cours sur l'encapsulation.**

### 4.3 Méthodes et logique métier

Les méthodes doivent encapsuler la **logique métier** :

```ts
class Order {
  constructor(
    public items: Product[],
    public customer: User
  ) {}

  calculateTotal(): number {
    return this.items.reduce((sum, item) => sum + item.price, 0);
  }

  applyTax(rate: number): number {
    const total = this.calculateTotal();
    return total * (1 + rate);
  }

  canShip(): boolean {
    return this.items.every(item => item.inStock);
  }

  getInvoice(): string {
    return `
      Facture pour ${this.customer.firstName} ${this.customer.lastName}
      Total: ${this.calculateTotal()}€
      Statut: ${this.canShip() ? 'Prêt à expédier' : 'En attente de stock'}
    `;
  }
}
```

**Utilisation :**
```ts
const customer = new User("Alice", "Dupont");
const laptop = new Product("Laptop", 1000, true);
const mouse = new Product("Mouse", 50, false);

const order = new Order([laptop, mouse], customer);

console.log(order.calculateTotal()); // 1050
console.log(order.canShip()); // false (mouse pas en stock)
console.log(order.getInvoice());
```

### 4.4 Propriétés calculées (getters)

Les **getters** permettent de calculer des valeurs à la demande :

```ts
class User {
  constructor(
    public firstName: string,
    public lastName: string,
    public birthYear: number
  ) {}

  // Getter
  get fullName(): string {
    return `${this.firstName} ${this.lastName}`;
  }

  get age(): number {
    const currentYear = new Date().getFullYear();
    return currentYear - this.birthYear;
  }
}

const user = new User("Alice", "Dupont", 1995);
console.log(user.fullName); // "Alice Dupont" (pas de parenthèses!)
console.log(user.age); // 29
```

**Avantage :** La valeur est toujours à jour sans stocker une donnée redondante.

### 4.5 Setters (modification contrôlée)

Les **setters** permettent de contrôler la modification d'une propriété :

```ts
class Product {
  private _price: number;

  constructor(name: string, price: number) {
    this._price = price;
  }

  get price(): number {
    return this._price;
  }

  set price(value: number) {
    if (value < 0) {
      throw new Error("Le prix ne peut pas être négatif");
    }
    this._price = value;
  }
}

const product = new Product("Laptop", 1000);
product.price = 1200; // ✅ OK
product.price = -500; // ❌ Error: Le prix ne peut pas être négatif
```

### 4.6 Méthodes statiques

Les **méthodes statiques** appartiennent à la **classe** et non aux instances :

```ts
class MathUtils {
  static readonly PI = 3.14159;

  static calculateCircleArea(radius: number): number {
    return this.PI * radius * radius;
  }

  static convertCelsiusToFahrenheit(celsius: number): number {
    return (celsius * 9/5) + 32;
  }
}

// Utilisation sans instanciation
console.log(MathUtils.calculateCircleArea(5)); // 78.54
console.log(MathUtils.convertCelsiusToFahrenheit(20)); // 68
```

**Quand utiliser les méthodes statiques ?**
- Fonctions utilitaires qui ne dépendent pas d'un état d'instance
- Factory methods (voir cours Design Patterns)
- Constantes et configurations

---

## 5. Cas d'usage métier concrets

### 5.1 Modélisation d'un système de commande e-commerce

```ts
class Customer {
  constructor(
    public id: string,
    public firstName: string,
    public lastName: string,
    public email: string,
    public isPremium: boolean = false
  ) {}

  get fullName(): string {
    return `${this.firstName} ${this.lastName}`;
  }
}

class Product {
  constructor(
    public id: string,
    public name: string,
    public price: number,
    public stock: number
  ) {}

  isAvailable(quantity: number): boolean {
    return this.stock >= quantity;
  }

  reduceStock(quantity: number): void {
    if (!this.isAvailable(quantity)) {
      throw new Error(`Stock insuffisant pour ${this.name}`);
    }
    this.stock -= quantity;
  }
}

class OrderItem {
  constructor(
    public product: Product,
    public quantity: number
  ) {}

  get subtotal(): number {
    return this.product.price * this.quantity;
  }
}

class Order {
  private items: OrderItem[] = [];

  constructor(
    public id: string,
    public customer: Customer,
    public createdAt: Date = new Date()
  ) {}

  addItem(product: Product, quantity: number): void {
    if (!product.isAvailable(quantity)) {
      throw new Error(`${product.name} n'est pas disponible en quantité ${quantity}`);
    }
    this.items.push(new OrderItem(product, quantity));
  }

  calculateSubtotal(): number {
    return this.items.reduce((sum, item) => sum + item.subtotal, 0);
  }

  calculateDiscount(): number {
    if (this.customer.isPremium) {
      return this.calculateSubtotal() * 0.1; // 10% réduction
    }
    return 0;
  }

  calculateTotal(): number {
    return this.calculateSubtotal() - this.calculateDiscount();
  }

  confirmOrder(): void {
    // Réduire le stock pour chaque produit
    for (const item of this.items) {
      item.product.reduceStock(item.quantity);
    }
    console.log(`Commande ${this.id} confirmée pour ${this.customer.fullName}`);
    console.log(`Total: ${this.calculateTotal()}€`);
  }
}
```

**Utilisation :**
```ts
const customer = new Customer("C001", "Alice", "Dupont", "alice@example.com", true);
const laptop = new Product("P001", "MacBook Pro", 2500, 10);
const mouse = new Product("P002", "Magic Mouse", 80, 50);

const order = new Order("O001", customer);
order.addItem(laptop, 1);
order.addItem(mouse, 2);

console.log(`Sous-total: ${order.calculateSubtotal()}€`); // 2660
console.log(`Réduction: ${order.calculateDiscount()}€`); // 266
console.log(`Total: ${order.calculateTotal()}€`); // 2394

order.confirmOrder();
```

### 5.2 Service d'authentification (NestJS/Angular)

```ts
class AuthService {
  private users: Map<string, User> = new Map();

  register(email: string, password: string, firstName: string, lastName: string): User {
    if (this.users.has(email)) {
      throw new Error("Utilisateur déjà existant");
    }

    const user = new User(email, this.hashPassword(password), firstName, lastName);
    this.users.set(email, user);
    return user;
  }

  login(email: string, password: string): User | null {
    const user = this.users.get(email);
    if (!user) {
      return null;
    }

    const hashedPassword = this.hashPassword(password);
    if (user.passwordHash === hashedPassword) {
      return user;
    }

    return null;
  }

  private hashPassword(password: string): string {
    // Simplification - en production, utiliser bcrypt
    return `hashed_${password}`;
  }
}

class User {
  constructor(
    public email: string,
    public passwordHash: string,
    public firstName: string,
    public lastName: string
  ) {}
}
```

---

## 6. Erreurs courantes & Comment les éviter

### Erreur 1 : Oublier `new` lors de l'instanciation

```ts
class User {
  constructor(public name: string) {}
}

// ❌ Erreur
const user = User("Alice"); // TypeError: Class constructor cannot be invoked without 'new'

// ✅ Correct
const user = new User("Alice");
```

### Erreur 2 : Oublier `this` dans les méthodes

```ts
class Counter {
  count: number = 0;

  increment(): void {
    // ❌ Erreur : count n'existe pas dans ce scope
    count++;

    // ✅ Correct
    this.count++;
  }
}
```

### Erreur 3 : Modifier une référence au lieu de créer un nouvel objet

```ts
class Product {
  constructor(public name: string, public price: number) {}
}

const product1 = new Product("Laptop", 1000);
const product2 = product1; // ⚠️ Même référence !

product2.price = 1500;
console.log(product1.price); // 1500 (modifié aussi!)

// ✅ Solution : créer une nouvelle instance ou cloner
const product3 = new Product(product1.name, product1.price);
```

### Erreur 4 : Ne pas typer les propriétés

```ts
// ❌ Mauvaise pratique
class User {
  name; // Type implicite 'any'
  age;
}

// ✅ Bonne pratique
class User {
  name: string;
  age: number;
}
```

### Erreur 5 : Mettre trop de logique dans le constructeur

```ts
// ❌ Constructeur trop complexe
class User {
  constructor(public email: string) {
    // Ne pas faire d'appels réseau dans le constructeur
    this.validateEmail();
    this.fetchUserData();
    this.initializePermissions();
  }
}

// ✅ Séparer l'initialisation
class User {
  constructor(public email: string) {
    this.validateEmail();
  }

  async initialize(): Promise<void> {
    await this.fetchUserData();
    this.initializePermissions();
  }
}
```

---

## 7. Exercices

### Exercice 1 : Système de bibliothèque

Créez un système de gestion de bibliothèque avec :
- Une classe `Book` (titre, auteur, ISBN, disponible)
- Une classe `Member` (nom, numéro de membre, livres empruntés)
- Une classe `Library` qui gère les emprunts et retours

**Fonctionnalités attendues :**
- Emprunter un livre (si disponible)
- Retourner un livre
- Lister les livres disponibles
- Calculer les frais de retard (0.50€ par jour)

### Exercice 2 : Système de gestion de tâches

Créez un gestionnaire de tâches avec :
- Une classe `Task` (titre, description, statut, priorité, date de création)
- Une classe `Project` (nom, liste de tâches)
- Méthodes pour filtrer les tâches par statut, priorité
- Calculer le pourcentage de complétion du projet

---

## 8. Comportement Senior

### 8.1 Préférer la composition à l'héritage

```ts
// ✅ Bon : composition
class EmailService {
  send(to: string, subject: string, body: string): void {
    // ...
  }
}

class NotificationService {
  constructor(private emailService: EmailService) {}

  notifyUser(user: User, message: string): void {
    this.emailService.send(user.email, "Notification", message);
  }
}

// ❌ À éviter : héritage pour réutilisation
class NotificationService extends EmailService {
  notifyUser(user: User, message: string): void {
    this.send(user.email, "Notification", message);
  }
}
```

### 8.2 Principes SOLID dès le début

- **Single Responsibility** : Une classe = une responsabilité
- **Open/Closed** : Ouvert à l'extension, fermé à la modification
- Utiliser des **interfaces** pour définir des contrats

```ts
// ✅ SRP respecté
class UserValidator {
  validate(user: User): boolean {
    return this.isEmailValid(user.email) && this.isAgeValid(user.age);
  }

  private isEmailValid(email: string): boolean {
    return email.includes('@');
  }

  private isAgeValid(age: number): boolean {
    return age >= 18;
  }
}

class UserRepository {
  save(user: User): void {
    // Sauvegarder en base
  }
}

class UserService {
  constructor(
    private validator: UserValidator,
    private repository: UserRepository
  ) {}

  createUser(user: User): void {
    if (!this.validator.validate(user)) {
      throw new Error("Utilisateur invalide");
    }
    this.repository.save(user);
  }
}
```

### 8.3 Nommer les classes avec précision

```ts
// ❌ Noms vagues
class Manager { }
class Helper { }
class Utility { }

// ✅ Noms précis
class UserAuthenticationManager { }
class EmailFormatHelper { }
class DateUtility { }
```

### 8.4 Éviter les "God Objects"

Une classe ne doit pas tout faire :

```ts
// ❌ God Object
class UserManager {
  createUser() { }
  deleteUser() { }
  validateEmail() { }
  sendEmail() { }
  hashPassword() { }
  generateToken() { }
  saveToDatabase() { }
  // ... 50 autres méthodes
}

// ✅ Responsabilités séparées
class UserService { }
class EmailService { }
class AuthenticationService { }
class UserRepository { }
```

---

## 9. Résumé

### Ce que vous avez appris

- **Classe** : Plan/moule définissant propriétés et méthodes
- **Objet** : Instance concrète d'une classe
- **Constructeur** : Méthode d'initialisation appelée avec `new`
- **this** : Référence à l'instance actuelle
- **Méthodes** : Comportements encapsulés dans la classe
- **Propriétés** : Données stockées dans l'objet
- **Getters/Setters** : Accès et modification contrôlés
- **Méthodes statiques** : Appartiennent à la classe, pas aux instances

### Quand utiliser la POO

✅ **Utilisez la POO quand :**
- Vous modélisez des entités métier (User, Product, Order...)
- Vous avez des données ET comportements liés
- Vous voulez réutiliser du code structuré
- Vous construisez des applications complexes (Angular, NestJS)

❌ **Ne forcez pas la POO quand :**
- Une simple fonction suffit
- Vous manipulez des données sans comportement
- L'état est géré globalement (Redux, NgRx...)

### Quand ne PAS s'en servir

- Pour des opérations utilitaires simples → préférer des fonctions pures
- Pour de la transformation de données → préférer map/filter/reduce
- Pour des configurations → préférer des objets littéraux ou const

---

## 10. Ressources Externes

### Documentation officielle
- [TypeScript Classes](https://www.typescriptlang.org/docs/handbook/2/classes.html)
- [MDN: JavaScript Classes](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Classes)

### Vidéos
- [POO en TypeScript - Grafikart](https://grafikart.fr/tutoriels/poo-typescript-1234) (français)
- [TypeScript OOP - Fireship](https://www.youtube.com/watch?v=8d8aDqxTWrY) (anglais)
- [Object Oriented Programming - freeCodeCamp](https://www.youtube.com/watch?v=PFmuCDHHpwk) (anglais)

### Articles
- [OOP vs Functional Programming](https://www.educative.io/blog/functional-programming-vs-oop)
- [When to use OOP](https://stackoverflow.blog/2020/09/02/if-everyone-hates-it-why-is-oop-still-so-widely-spread/)

### Cours interactifs
- [Codecademy - Learn TypeScript](https://www.codecademy.com/learn/learn-typescript)
- [Exercism - TypeScript Track](https://exercism.org/tracks/typescript)
