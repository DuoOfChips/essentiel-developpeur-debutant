# Composition vs Héritage : Pourquoi Préférer la Composition

## Introduction

### Objectifs du cours
Après avoir lu ce cours, vous serez capable de :
- Comprendre les différences entre composition et héritage
- Identifier quand utiliser la composition plutôt que l'héritage
- Appliquer le principe "Composition over Inheritance"
- Refactorer du code basé sur l'héritage vers la composition
- Éviter les pièges classiques de l'héritage

### Scope de la notion
Ce cours permet de :
- **Structurer** du code flexible et maintenable
- **Éviter** les hiérarchies de classes rigides et fragiles
- **Favoriser** la réutilisation de code sans couplage fort
- **Faciliter** les tests et l'évolution du code
- Créer des architectures **modulaires** et **évolutives**

Dans les webapps métier (Angular/NestJS), préférer la composition permet :
- Des services modulaires et testables
- Une meilleure injection de dépendances
- Un code plus flexible face aux changements métier
- Moins de bugs liés aux effets de bord de l'héritage

---

## 1. Rappels

Avant ce cours, vous devez maîtriser :

### Classes et objets
```ts
class User {
  constructor(public name: string, public email: string) {}
}
```

### Héritage de base
```ts
class Admin extends User {
  constructor(name: string, email: string, public role: string) {
    super(name, email);
  }
}
```

---

## 2. Définitions et Concepts Clés

### 2.1 Qu'est-ce que l'Héritage ?

L'**héritage** est un mécanisme où une classe enfant **hérite** des propriétés et méthodes d'une classe parent.

**Analogie de la vie quotidienne :**
Un **chat** 🐱 est un **animal** :
- Tous les animaux mangent, dorment, respirent
- Le chat hérite de ces comportements
- Le chat ajoute des comportements spécifiques : miauler, grimper

```ts
class Animal {
  eat(): void {
    console.log("Je mange");
  }
}

class Cat extends Animal {
  meow(): void {
    console.log("Miaou!");
  }
}

const cat = new Cat();
cat.eat(); // Hérité de Animal
cat.meow(); // Spécifique à Cat
```

**Relation :** Un chat **EST UN** animal (relation "is-a")

### 2.2 Qu'est-ce que la Composition ?

La **composition** est un mécanisme où une classe **contient** d'autres objets pour réutiliser leur fonctionnalité.

**Analogie de la vie quotidienne :**
Une **voiture** 🚗 n'est pas un moteur, mais elle **a un** moteur :
- La voiture contient un moteur
- La voiture contient des roues
- La voiture contient un système de freinage

```ts
class Engine {
  start(): void {
    console.log("Moteur démarré");
  }
}

class Brake {
  apply(): void {
    console.log("Freins appliqués");
  }
}

class Car {
  private engine: Engine;
  private brake: Brake;

  constructor() {
    this.engine = new Engine();
    this.brake = new Brake();
  }

  startCar(): void {
    this.engine.start();
  }

  stopCar(): void {
    this.brake.apply();
  }
}
```

**Relation :** Une voiture **A UN** moteur (relation "has-a")

### 2.3 Principe "Composition over Inheritance"

> **"Favorisez la composition d'objets plutôt que l'héritage de classes"**
> — Gang of Four (Design Patterns, 1994)

**Pourquoi ?**
- ✅ Flexibilité : Changer les composants à l'exécution
- ✅ Découplage : Moins de dépendances
- ✅ Testabilité : Facile de mocker les dépendances
- ✅ Évolution : Ajouter des comportements sans casser le code existant

---

## 3. Les Problèmes de l'Héritage

### 3.1 Hiérarchies rigides et fragiles

```ts
// ❌ Problème : hiérarchie rigide
class Animal {
  move(): void {
    console.log("Je me déplace");
  }
}

class Bird extends Animal {
  fly(): void {
    console.log("Je vole");
  }
}

class Penguin extends Bird {
  // ⚠️ Problème : un pingouin ne vole pas!
  fly(): void {
    throw new Error("Les pingouins ne volent pas");
  }
}
```

**Problème :** Le pingouin hérite de `fly()` alors qu'il ne devrait pas voler.

### 3.2 Le problème du diamant

```ts
// ❌ Problème : héritage multiple (impossible en TypeScript)
class Swimmer {
  swim(): void { }
}

class Flyer {
  fly(): void { }
}

// ⚠️ TypeScript ne supporte pas l'héritage multiple
// class Duck extends Swimmer, Flyer { }
```

**Problème :** Un canard nage ET vole, mais on ne peut pas hériter de deux classes.

### 3.3 Couplage fort

```ts
// ❌ Problème : couplage fort
class Employee {
  constructor(public name: string) {}

  work(): void {
    console.log(`${this.name} travaille`);
  }
}

class Manager extends Employee {
  manage(): void {
    console.log(`${this.name} manage`);
  }
}

class Developer extends Employee {
  code(): void {
    console.log(`${this.name} code`);
  }
}

// ⚠️ Si on change Employee, tous les enfants sont impactés
```

### 3.4 Violation du principe de Liskov

```ts
// ❌ Violation de Liskov Substitution Principle
class Rectangle {
  constructor(protected width: number, protected height: number) {}

  setWidth(width: number): void {
    this.width = width;
  }

  setHeight(height: number): void {
    this.height = height;
  }

  getArea(): number {
    return this.width * this.height;
  }
}

class Square extends Rectangle {
  setWidth(width: number): void {
    this.width = width;
    this.height = width; // ⚠️ Modifie aussi la hauteur
  }

  setHeight(height: number): void {
    this.width = height;
    this.height = height; // ⚠️ Modifie aussi la largeur
  }
}

function testRectangle(rect: Rectangle): void {
  rect.setWidth(5);
  rect.setHeight(10);
  console.log(rect.getArea()); // On s'attend à 50
}

const square = new Square(5, 5);
testRectangle(square); // 100 au lieu de 50 ⚠️
```

**Problème :** On ne peut pas substituer un carré à un rectangle sans changer le comportement.

---

## 4. La Composition comme Solution

### 4.1 Exemple : Système d'employés avec composition

```ts
// ✅ Solution : Composition

// Comportements séparés
interface Workable {
  work(): void;
}

interface Codable {
  code(): void;
}

interface Manageable {
  manage(): void;
}

// Implémentations concrètes
class WorkBehavior implements Workable {
  work(): void {
    console.log("Je travaille");
  }
}

class CodeBehavior implements Codable {
  code(): void {
    console.log("Je code");
  }
}

class ManageBehavior implements Manageable {
  manage(): void {
    console.log("Je manage une équipe");
  }
}

// Employés composés
class Employee {
  private workBehavior: Workable;

  constructor(public name: string, workBehavior: Workable) {
    this.workBehavior = workBehavior;
  }

  performWork(): void {
    this.workBehavior.work();
  }
}

class Developer {
  private workBehavior: Workable;
  private codeBehavior: Codable;

  constructor(
    public name: string,
    workBehavior: Workable,
    codeBehavior: Codable
  ) {
    this.workBehavior = workBehavior;
    this.codeBehavior = codeBehavior;
  }

  work(): void {
    this.workBehavior.work();
  }

  code(): void {
    this.codeBehavior.code();
  }
}

class Manager {
  private workBehavior: Workable;
  private manageBehavior: Manageable;

  constructor(
    public name: string,
    workBehavior: Workable,
    manageBehavior: Manageable
  ) {
    this.workBehavior = workBehavior;
    this.manageBehavior = manageBehavior;
  }

  work(): void {
    this.workBehavior.work();
  }

  manage(): void {
    this.manageBehavior.manage();
  }
}

// Utilisation
const dev = new Developer("Alice", new WorkBehavior(), new CodeBehavior());
dev.work();
dev.code();

const manager = new Manager("Bob", new WorkBehavior(), new ManageBehavior());
manager.work();
manager.manage();
```

**Avantages :**
- ✅ Flexibilité : on peut changer les comportements à l'exécution
- ✅ Réutilisation : les comportements sont indépendants
- ✅ Testabilité : facile de mocker les comportements

### 4.2 Exemple : Système de notifications

```ts
// ✅ Composition pour les notifications

interface NotificationSender {
  send(message: string, recipient: string): void;
}

class EmailSender implements NotificationSender {
  send(message: string, recipient: string): void {
    console.log(`Email envoyé à ${recipient}: ${message}`);
  }
}

class SmsSender implements NotificationSender {
  send(message: string, recipient: string): void {
    console.log(`SMS envoyé à ${recipient}: ${message}`);
  }
}

class PushSender implements NotificationSender {
  send(message: string, recipient: string): void {
    console.log(`Push notification envoyée à ${recipient}: ${message}`);
  }
}

// Service composé
class NotificationService {
  private senders: NotificationSender[];

  constructor(senders: NotificationSender[]) {
    this.senders = senders;
  }

  notify(message: string, recipient: string): void {
    for (const sender of this.senders) {
      sender.send(message, recipient);
    }
  }

  addSender(sender: NotificationSender): void {
    this.senders.push(sender);
  }
}

// Utilisation
const notificationService = new NotificationService([
  new EmailSender(),
  new SmsSender()
]);

notificationService.notify("Votre commande est expédiée", "alice@example.com");

// Ajouter dynamiquement un nouveau canal
notificationService.addSender(new PushSender());
```

---

## 5. Cas d'usage métier concrets

### 5.1 Système de paiement e-commerce

```ts
// ✅ Composition : stratégies de paiement

interface PaymentStrategy {
  processPayment(amount: number): boolean;
}

class CreditCardPayment implements PaymentStrategy {
  constructor(private cardNumber: string, private cvv: string) {}

  processPayment(amount: number): boolean {
    console.log(`Paiement de ${amount}€ par carte ${this.cardNumber}`);
    return true;
  }
}

class PayPalPayment implements PaymentStrategy {
  constructor(private email: string) {}

  processPayment(amount: number): boolean {
    console.log(`Paiement de ${amount}€ via PayPal (${this.email})`);
    return true;
  }
}

class BankTransferPayment implements PaymentStrategy {
  constructor(private iban: string) {}

  processPayment(amount: number): boolean {
    console.log(`Paiement de ${amount}€ par virement (${this.iban})`);
    return true;
  }
}

class Order {
  constructor(
    public id: string,
    public amount: number,
    private paymentStrategy: PaymentStrategy
  ) {}

  checkout(): void {
    const success = this.paymentStrategy.processPayment(this.amount);
    if (success) {
      console.log(`Commande ${this.id} confirmée`);
    }
  }

  changePaymentMethod(newStrategy: PaymentStrategy): void {
    this.paymentStrategy = newStrategy;
  }
}

// Utilisation
const order = new Order(
  "O001",
  150,
  new CreditCardPayment("1234567890123456", "123")
);
order.checkout();

// Changement de méthode de paiement
order.changePaymentMethod(new PayPalPayment("alice@example.com"));
order.checkout();
```

### 5.2 Logger système (Angular/NestJS)

```ts
// ✅ Composition : système de logging

interface LogTransport {
  log(level: string, message: string): void;
}

class ConsoleTransport implements LogTransport {
  log(level: string, message: string): void {
    console.log(`[${level}] ${message}`);
  }
}

class FileTransport implements LogTransport {
  constructor(private filename: string) {}

  log(level: string, message: string): void {
    // Écrire dans un fichier
    console.log(`[FILE:${this.filename}] [${level}] ${message}`);
  }
}

class RemoteTransport implements LogTransport {
  constructor(private apiUrl: string) {}

  log(level: string, message: string): void {
    // Envoyer à un serveur distant
    console.log(`[REMOTE:${this.apiUrl}] [${level}] ${message}`);
  }
}

class Logger {
  private transports: LogTransport[];

  constructor(transports: LogTransport[]) {
    this.transports = transports;
  }

  info(message: string): void {
    this.log("INFO", message);
  }

  warn(message: string): void {
    this.log("WARN", message);
  }

  error(message: string): void {
    this.log("ERROR", message);
  }

  private log(level: string, message: string): void {
    for (const transport of this.transports) {
      transport.log(level, message);
    }
  }
}

// Configuration en développement
const devLogger = new Logger([
  new ConsoleTransport()
]);

// Configuration en production
const prodLogger = new Logger([
  new ConsoleTransport(),
  new FileTransport("/var/log/app.log"),
  new RemoteTransport("https://api.logging.com")
]);

prodLogger.info("Application démarrée");
prodLogger.error("Erreur de connexion à la base de données");
```

### 5.3 Système de validation (forms Angular)

```ts
// ✅ Composition : validateurs réutilisables

interface Validator<T> {
  validate(value: T): string | null;
}

class EmailValidator implements Validator<string> {
  validate(value: string): string | null {
    if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(value)) {
      return "Email invalide";
    }
    return null;
  }
}

class MinLengthValidator implements Validator<string> {
  constructor(private minLength: number) {}

  validate(value: string): string | null {
    if (value.length < this.minLength) {
      return `Minimum ${this.minLength} caractères requis`;
    }
    return null;
  }
}

class RangeValidator implements Validator<number> {
  constructor(private min: number, private max: number) {}

  validate(value: number): string | null {
    if (value < this.min || value > this.max) {
      return `La valeur doit être entre ${this.min} et ${this.max}`;
    }
    return null;
  }
}

class FormField<T> {
  private validators: Validator<T>[];
  private value: T;

  constructor(initialValue: T, validators: Validator<T>[]) {
    this.value = initialValue;
    this.validators = validators;
  }

  setValue(value: T): void {
    this.value = value;
  }

  getValue(): T {
    return this.value;
  }

  validate(): string[] {
    const errors: string[] = [];
    for (const validator of this.validators) {
      const error = validator.validate(this.value);
      if (error) {
        errors.push(error);
      }
    }
    return errors;
  }

  isValid(): boolean {
    return this.validate().length === 0;
  }
}

// Utilisation
const emailField = new FormField("", [
  new EmailValidator(),
  new MinLengthValidator(5)
]);

emailField.setValue("test");
console.log(emailField.validate()); // ["Email invalide"]

emailField.setValue("alice@example.com");
console.log(emailField.isValid()); // true

const ageField = new FormField(15, [
  new RangeValidator(18, 100)
]);

console.log(ageField.validate()); // ["La valeur doit être entre 18 et 100"]
```

---

## 6. Quand utiliser l'Héritage ?

L'héritage n'est pas toujours mauvais. Il est approprié dans certains cas :

### 6.1 Cas valides pour l'héritage

**1. Relation "EST UN" claire et stable**
```ts
// ✅ Bon usage de l'héritage
abstract class Shape {
  abstract getArea(): number;
  abstract getPerimeter(): number;
}

class Circle extends Shape {
  constructor(private radius: number) {
    super();
  }

  getArea(): number {
    return Math.PI * this.radius ** 2;
  }

  getPerimeter(): number {
    return 2 * Math.PI * this.radius;
  }
}

class Rectangle extends Shape {
  constructor(private width: number, private height: number) {
    super();
  }

  getArea(): number {
    return this.width * this.height;
  }

  getPerimeter(): number {
    return 2 * (this.width + this.height);
  }
}
```

**2. Framework ou bibliothèque imposant l'héritage**
```ts
// Angular Component
import { Component } from '@angular/core';

@Component({
  selector: 'app-user',
  template: '<div>User Component</div>'
})
export class UserComponent {
  // Angular impose l'héritage de Component
}
```

**3. Hiérarchie simple et peu profonde**
```ts
// ✅ Un niveau d'héritage acceptable
class HttpError extends Error {
  constructor(public statusCode: number, message: string) {
    super(message);
  }
}

class NotFoundError extends HttpError {
  constructor(message: string = "Not Found") {
    super(404, message);
  }
}
```

### 6.2 Questions à se poser

Avant d'utiliser l'héritage, demandez-vous :

1. **La relation est-elle vraiment "EST UN" ?**
   - Oui → Peut-être héritage
   - Non → Composition

2. **La hiérarchie est-elle stable ?**
   - Oui → Peut-être héritage
   - Non → Composition

3. **Y a-t-il un risque de hiérarchie profonde ?**
   - Oui → Composition
   - Non → Peut-être héritage

4. **Ai-je besoin de changer le comportement à l'exécution ?**
   - Oui → Composition
   - Non → Peut-être héritage

---

## 7. Erreurs courantes & Comment les éviter

### Erreur 1 : Héritage pour réutiliser du code

```ts
// ❌ Mauvais : héritage pour réutilisation
class EmailSender {
  send(to: string, subject: string, body: string): void {
    // ...
  }
}

class UserService extends EmailSender {
  // ⚠️ UserService n'EST PAS un EmailSender!
  createUser(user: User): void {
    // ...
    this.send(user.email, "Welcome", "Bienvenue!");
  }
}

// ✅ Bon : composition
class UserService {
  constructor(private emailSender: EmailSender) {}

  createUser(user: User): void {
    // ...
    this.emailSender.send(user.email, "Welcome", "Bienvenue!");
  }
}
```

### Erreur 2 : Hiérarchies trop profondes

```ts
// ❌ Hiérarchie trop profonde
class Entity { }
class Person extends Entity { }
class Employee extends Person { }
class Developer extends Employee { }
class SeniorDeveloper extends Developer { }
class TechLead extends SeniorDeveloper { }
// ⚠️ 6 niveaux de profondeur!

// ✅ Composition plate
class Person {
  constructor(
    public id: string,
    public name: string,
    private role: Role,
    private skills: Skill[]
  ) {}
}
```

### Erreur 3 : Modifier le comportement parent

```ts
// ❌ Modifier le comportement du parent
class Parent {
  doSomething(): void {
    console.log("Parent action");
  }
}

class Child extends Parent {
  doSomething(): void {
    // ⚠️ Changement complet du comportement
    console.log("Completely different action");
  }
}

// ✅ Composition avec interface
interface Action {
  execute(): void;
}

class ParentAction implements Action {
  execute(): void {
    console.log("Parent action");
  }
}

class ChildAction implements Action {
  execute(): void {
    console.log("Child action");
  }
}
```

---

## 8. Exercices

### Exercice 1 : Refactoring d'un système de véhicules

Refactorez cette hiérarchie vers la composition :
```ts
class Vehicle {
  move(): void { }
}

class LandVehicle extends Vehicle {
  drive(): void { }
}

class WaterVehicle extends Vehicle {
  sail(): void { }
}

class AmphibiousVehicle extends ??? {
  // ⚠️ Problème : doit hériter de LandVehicle ET WaterVehicle
}
```

### Exercice 2 : Système de permissions

Créez un système de permissions avec composition :
- Différents types de permissions (READ, WRITE, DELETE, ADMIN)
- Utilisateurs avec combinaisons de permissions
- Possibilité d'ajouter/retirer des permissions dynamiquement

---

## 9. Comportement Senior

### 9.1 Interfaces plutôt que classes abstraites

```ts
// ✅ Préférer les interfaces
interface Repository<T> {
  findById(id: string): Promise<T | null>;
  save(entity: T): Promise<void>;
  delete(id: string): Promise<void>;
}

class UserRepository implements Repository<User> {
  // Implémentation
}

class ProductRepository implements Repository<Product> {
  // Implémentation
}
```

### 9.2 Injection de dépendances

```ts
// ✅ Dépendances injectées
class OrderService {
  constructor(
    private paymentService: PaymentService,
    private emailService: EmailService,
    private orderRepository: OrderRepository
  ) {}

  async createOrder(order: Order): Promise<void> {
    await this.orderRepository.save(order);
    await this.paymentService.process(order);
    await this.emailService.sendConfirmation(order);
  }
}
```

### 9.3 Principe de responsabilité unique avec composition

```ts
// ✅ Chaque classe a une responsabilité
class PriceCalculator {
  calculate(items: OrderItem[]): number {
    // Calcul du prix
  }
}

class TaxCalculator {
  calculate(price: number, taxRate: number): number {
    // Calcul de la taxe
  }
}

class DiscountCalculator {
  calculate(price: number, user: User): number {
    // Calcul de la réduction
  }
}

class OrderPricingService {
  constructor(
    private priceCalculator: PriceCalculator,
    private taxCalculator: TaxCalculator,
    private discountCalculator: DiscountCalculator
  ) {}

  calculateTotal(order: Order): number {
    const basePrice = this.priceCalculator.calculate(order.items);
    const discount = this.discountCalculator.calculate(basePrice, order.user);
    const priceAfterDiscount = basePrice - discount;
    const tax = this.taxCalculator.calculate(priceAfterDiscount, 0.2);
    return priceAfterDiscount + tax;
  }
}
```

---

## 10. Résumé

### Ce que vous avez appris

- **Héritage** : Relation "EST UN", fragile et rigide
- **Composition** : Relation "A UN", flexible et modulaire
- **Principe** : Favoriser la composition plutôt que l'héritage
- **Problèmes de l'héritage** : Hiérarchies rigides, couplage fort, difficulté d'évolution
- **Avantages de la composition** : Flexibilité, testabilité, réutilisation

### Quand utiliser quoi

| Critère | Héritage | Composition |
|---------|----------|-------------|
| Relation | "EST UN" claire | "A UN" ou "UTILISE UN" |
| Flexibilité | ❌ Faible | ✅ Élevée |
| Couplage | ❌ Fort | ✅ Faible |
| Testabilité | ⚠️ Moyenne | ✅ Facile |
| Changement à l'exécution | ❌ Non | ✅ Oui |
| Profondeur | ⚠️ Max 2-3 niveaux | ✅ Plate |

### Règle d'or

> **Préférez la composition par défaut. N'utilisez l'héritage que si vous avez une raison très forte.**

---

## 11. Ressources Externes

### Articles
- [Composition over Inheritance](https://en.wikipedia.org/wiki/Composition_over_inheritance)
- [Prefer Composition Over Inheritance - Medium](https://medium.com/@severinperez/writing-flexible-code-with-the-single-responsibility-principle-b71c4f3f883f)

### Vidéos
- [Composition vs Inheritance - Fun Fun Function](https://www.youtube.com/watch?v=wfMtDGfHWpA) (anglais)
- [Design Patterns: Composition Over Inheritance](https://www.youtube.com/watch?v=hxGOiiR9ZKg) (anglais)

### Livres
- **Design Patterns** - Gang of Four (principe fondateur)
- **Clean Code** - Robert C. Martin (chapitre sur l'OOP)
- **Head First Design Patterns** - Freeman & Freeman
