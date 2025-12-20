# Interfaces vs Classes Abstraites : Quand Utiliser Quoi

## Introduction

### Objectifs du cours
Après avoir lu ce cours, vous serez capable de :
- Comprendre les différences entre interfaces et classes abstraites
- Choisir entre interface et classe abstraite selon le contexte
- Utiliser les interfaces pour définir des contrats
- Utiliser les classes abstraites pour partager du code commun
- Appliquer ces concepts dans Angular et NestJS

### Scope de la notion
Ce cours permet de :
- **Définir des contrats** clairs entre composants
- **Abstraire** la logique commune
- **Découpler** le code avec des interfaces
- **Structurer** des hiérarchies de classes modulaires
- Faciliter les **tests** et le **mocking**

Dans les webapps métier (Angular/NestJS) :
- Les interfaces définissent les contrats de services
- Les classes abstraites partagent la logique commune
- L'injection de dépendances repose sur les interfaces
- Les DTOs (Data Transfer Objects) utilisent des interfaces

---

## 1. Rappels

### Classes
```ts
class User {
  constructor(public name: string, public email: string) {}
  
  greet(): string {
    return `Hello, ${this.name}`;
  }
}
```

### Héritage
```ts
class Admin extends User {
  constructor(name: string, email: string, public role: string) {
    super(name, email);
  }
}
```

---

## 2. Définitions et Concepts Clés

### 2.1 Qu'est-ce qu'une Interface ?

Une **interface** est un **contrat** qui définit la structure (propriétés et méthodes) qu'une classe doit respecter, **sans fournir d'implémentation**.

**Analogie de la vie quotidienne :**
Une **prise électrique** 🔌 :
- Elle définit un **standard** : taille, forme, voltage
- N'importe quel appareil respectant ce standard peut se brancher
- La prise ne contient pas l'électricité, elle définit comment y accéder

```ts
interface Chargeable {
  charge(): void;
  getBatteryLevel(): number;
}

class Phone implements Chargeable {
  private battery: number = 50;

  charge(): void {
    this.battery = 100;
  }

  getBatteryLevel(): number {
    return this.battery;
  }
}

class Laptop implements Chargeable {
  private battery: number = 30;

  charge(): void {
    this.battery = 100;
  }

  getBatteryLevel(): number {
    return this.battery;
  }
}
```

**Caractéristiques :**
- ✅ Définit un contrat (signature)
- ❌ Pas d'implémentation
- ✅ Peut être implémentée par plusieurs classes
- ✅ Une classe peut implémenter plusieurs interfaces
- ✅ Légère et flexible

### 2.2 Qu'est-ce qu'une Classe Abstraite ?

Une **classe abstraite** est une classe **partiellement implémentée** qui sert de base à d'autres classes. Elle ne peut pas être instanciée directement.

**Analogie de la vie quotidienne :**
Un **moule à gâteau** 🎂 :
- Il définit la forme de base (structure commune)
- On peut ajouter des ingrédients spécifiques (implémentation concrète)
- On ne peut pas manger le moule, seulement le gâteau final

```ts
abstract class Shape {
  constructor(protected color: string) {}

  // Méthode abstraite (doit être implémentée)
  abstract getArea(): number;

  // Méthode concrète (partagée par tous)
  describe(): string {
    return `Une forme ${this.color} avec une aire de ${this.getArea()}`;
  }
}

class Circle extends Shape {
  constructor(color: string, private radius: number) {
    super(color);
  }

  getArea(): number {
    return Math.PI * this.radius ** 2;
  }
}

class Rectangle extends Shape {
  constructor(color: string, private width: number, private height: number) {
    super(color);
  }

  getArea(): number {
    return this.width * this.height;
  }
}

const circle = new Circle("rouge", 5);
console.log(circle.describe()); // "Une forme rouge avec une aire de 78.54"
// const shape = new Shape("bleu"); // ❌ Erreur : classe abstraite
```

**Caractéristiques :**
- ✅ Peut contenir de l'implémentation
- ✅ Peut avoir des propriétés avec valeurs
- ✅ Peut avoir un constructeur
- ❌ Ne peut pas être instanciée directement
- ❌ Une classe ne peut hériter que d'une seule classe abstraite
- ⚠️ Couplage plus fort qu'une interface

---

## 3. Différences Clés

| Critère | Interface | Classe Abstraite |
|---------|-----------|------------------|
| **Implémentation** | ❌ Aucune | ✅ Partielle ou complète |
| **Propriétés** | ✅ Types uniquement | ✅ Avec valeurs initiales |
| **Constructeur** | ❌ Non | ✅ Oui |
| **Méthodes** | Signature uniquement | Signature + implémentation |
| **Héritage multiple** | ✅ Oui (plusieurs interfaces) | ❌ Non (une seule classe) |
| **Mot-clé** | `implements` | `extends` |
| **Compilation JS** | ❌ Disparaît | ✅ Reste (classe JS) |
| **Couplage** | ✅ Faible | ⚠️ Plus fort |
| **Usage** | Contrat, API publique | Partage de code commun |

---

## 4. Déroulé du cours : Quand utiliser quoi

### 4.1 Utiliser une Interface

**Quand :**
- Vous voulez définir un **contrat** sans imposer d'implémentation
- Plusieurs classes non liées doivent partager la même API
- Vous voulez permettre l'implémentation multiple
- Vous voulez un couplage faible (injection de dépendances)
- Vous définissez des DTOs ou des modèles de données

**Exemple 1 : Contrat de service**
```ts
interface Logger {
  log(message: string): void;
  error(message: string): void;
  warn(message: string): void;
}

class ConsoleLogger implements Logger {
  log(message: string): void {
    console.log(`[LOG] ${message}`);
  }

  error(message: string): void {
    console.error(`[ERROR] ${message}`);
  }

  warn(message: string): void {
    console.warn(`[WARN] ${message}`);
  }
}

class FileLogger implements Logger {
  constructor(private filename: string) {}

  log(message: string): void {
    // Écrire dans un fichier
  }

  error(message: string): void {
    // Écrire dans un fichier d'erreurs
  }

  warn(message: string): void {
    // Écrire dans un fichier d'avertissements
  }
}

class RemoteLogger implements Logger {
  constructor(private apiUrl: string) {}

  log(message: string): void {
    // Envoyer au serveur
  }

  error(message: string): void {
    // Envoyer au serveur
  }

  warn(message: string): void {
    // Envoyer au serveur
  }
}

// Utilisation avec injection de dépendances
class UserService {
  constructor(private logger: Logger) {}

  createUser(user: User): void {
    this.logger.log(`Création de l'utilisateur ${user.name}`);
    // ...
  }
}

// Flexibilité : facile de changer l'implémentation
const service1 = new UserService(new ConsoleLogger());
const service2 = new UserService(new FileLogger("/var/log/app.log"));
```

**Exemple 2 : DTOs (Data Transfer Objects)**
```ts
// Interface pour les données
interface CreateUserDto {
  firstName: string;
  lastName: string;
  email: string;
  password: string;
}

interface UpdateUserDto {
  firstName?: string;
  lastName?: string;
  email?: string;
}

interface UserResponseDto {
  id: string;
  firstName: string;
  lastName: string;
  email: string;
  createdAt: Date;
}

// Utilisation dans un contrôleur NestJS
class UserController {
  constructor(private userService: UserService) {}

  async create(dto: CreateUserDto): Promise<UserResponseDto> {
    return this.userService.create(dto);
  }

  async update(id: string, dto: UpdateUserDto): Promise<UserResponseDto> {
    return this.userService.update(id, dto);
  }
}
```

**Exemple 3 : Interfaces multiples**
```ts
interface Readable {
  read(): string;
}

interface Writable {
  write(data: string): void;
}

interface Closeable {
  close(): void;
}

// Une classe peut implémenter plusieurs interfaces
class File implements Readable, Writable, Closeable {
  private content: string = "";
  private isOpen: boolean = true;

  read(): string {
    if (!this.isOpen) throw new Error("Fichier fermé");
    return this.content;
  }

  write(data: string): void {
    if (!this.isOpen) throw new Error("Fichier fermé");
    this.content += data;
  }

  close(): void {
    this.isOpen = false;
  }
}
```

### 4.2 Utiliser une Classe Abstraite

**Quand :**
- Vous voulez partager du **code commun** entre classes liées
- Vous avez une logique métier à réutiliser
- Vous voulez un **constructeur** avec initialisation commune
- Les classes enfants partagent des **propriétés** communes
- Vous avez une hiérarchie logique claire

**Exemple 1 : Logique commune**
```ts
abstract class HttpError extends Error {
  constructor(
    public statusCode: number,
    message: string,
    public timestamp: Date = new Date()
  ) {
    super(message);
  }

  // Méthode commune à toutes les erreurs HTTP
  toJSON() {
    return {
      statusCode: this.statusCode,
      message: this.message,
      timestamp: this.timestamp
    };
  }

  // Méthode abstraite à implémenter
  abstract getErrorType(): string;
}

class NotFoundError extends HttpError {
  constructor(resource: string) {
    super(404, `${resource} not found`);
  }

  getErrorType(): string {
    return "NOT_FOUND";
  }
}

class UnauthorizedError extends HttpError {
  constructor(message: string = "Unauthorized") {
    super(401, message);
  }

  getErrorType(): string {
    return "UNAUTHORIZED";
  }
}

class BadRequestError extends HttpError {
  constructor(message: string) {
    super(400, message);
  }

  getErrorType(): string {
    return "BAD_REQUEST";
  }
}

// Utilisation
try {
  throw new NotFoundError("User");
} catch (error) {
  if (error instanceof HttpError) {
    console.log(error.toJSON()); // Méthode commune
    console.log(error.getErrorType()); // Méthode spécifique
  }
}
```

**Exemple 2 : Repository pattern**
```ts
abstract class BaseRepository<T> {
  protected items: Map<string, T> = new Map();

  // Méthodes communes implémentées
  findById(id: string): T | null {
    return this.items.get(id) || null;
  }

  findAll(): T[] {
    return Array.from(this.items.values());
  }

  save(id: string, entity: T): void {
    this.items.set(id, entity);
  }

  delete(id: string): void {
    this.items.delete(id);
  }

  // Méthode abstraite spécifique
  abstract validate(entity: T): boolean;
}

interface User {
  id: string;
  email: string;
  name: string;
}

class UserRepository extends BaseRepository<User> {
  validate(user: User): boolean {
    return (
      user.email.includes('@') &&
      user.name.length > 0
    );
  }

  // Méthode spécifique aux utilisateurs
  findByEmail(email: string): User | null {
    return this.findAll().find(u => u.email === email) || null;
  }
}

interface Product {
  id: string;
  name: string;
  price: number;
}

class ProductRepository extends BaseRepository<Product> {
  validate(product: Product): boolean {
    return (
      product.name.length > 0 &&
      product.price > 0
    );
  }

  // Méthode spécifique aux produits
  findByPriceRange(min: number, max: number): Product[] {
    return this.findAll().filter(p => p.price >= min && p.price <= max);
  }
}
```

**Exemple 3 : Component Angular avec logique commune**
```ts
import { Component, OnInit, OnDestroy } from '@angular/core';
import { Subject } from 'rxjs';

// Classe abstraite pour gérer la destruction automatique
abstract class BaseComponent implements OnInit, OnDestroy {
  protected destroy$ = new Subject<void>();

  ngOnInit(): void {
    this.onInit();
  }

  ngOnDestroy(): void {
    this.destroy$.next();
    this.destroy$.complete();
    this.onDestroy();
  }

  // Hooks optionnels pour les enfants
  protected onInit(): void {}
  protected onDestroy(): void {}
}

@Component({
  selector: 'app-user-list',
  template: '<div>User List</div>'
})
class UserListComponent extends BaseComponent {
  users: User[] = [];

  protected onInit(): void {
    // Logique d'initialisation
    // Les observables utiliseront destroy$ pour se désabonner automatiquement
  }

  protected onDestroy(): void {
    // Nettoyage spécifique si nécessaire
  }
}
```

### 4.3 Combiner Interface et Classe Abstraite

On peut aussi combiner les deux approches :

```ts
// Interface définit le contrat
interface Repository<T> {
  findById(id: string): Promise<T | null>;
  findAll(): Promise<T[]>;
  save(entity: T): Promise<void>;
  delete(id: string): Promise<void>;
}

// Classe abstraite fournit l'implémentation de base
abstract class BaseRepository<T> implements Repository<T> {
  protected abstract tableName: string;

  async findById(id: string): Promise<T | null> {
    // Implémentation commune avec la base de données
    return null; // Simplifié
  }

  async findAll(): Promise<T[]> {
    // Implémentation commune
    return [];
  }

  async save(entity: T): Promise<void> {
    // Implémentation commune
  }

  async delete(id: string): Promise<void> {
    // Implémentation commune
  }

  // Méthode helper protégée
  protected async query(sql: string): Promise<any> {
    // Logique de requête partagée
  }
}

// Repository concret
class UserRepository extends BaseRepository<User> {
  protected tableName = "users";

  // Méthodes spécifiques aux utilisateurs
  async findByEmail(email: string): Promise<User | null> {
    return this.query(`SELECT * FROM ${this.tableName} WHERE email = ?`);
  }
}

// On peut maintenant utiliser Repository<T> comme type
class UserService {
  constructor(private userRepo: Repository<User>) {}
  // Découplé de l'implémentation concrète
}
```

---

## 5. Cas d'usage métier concrets

### 5.1 Système de paiement (Interface)

```ts
// Interface : contrat pour les fournisseurs de paiement
interface PaymentProvider {
  processPayment(amount: number, currency: string): Promise<PaymentResult>;
  refund(transactionId: string, amount: number): Promise<void>;
  getTransactionStatus(transactionId: string): Promise<TransactionStatus>;
}

interface PaymentResult {
  success: boolean;
  transactionId: string;
  message?: string;
}

enum TransactionStatus {
  PENDING = "PENDING",
  COMPLETED = "COMPLETED",
  FAILED = "FAILED",
  REFUNDED = "REFUNDED"
}

// Implémentations multiples
class StripeProvider implements PaymentProvider {
  async processPayment(amount: number, currency: string): Promise<PaymentResult> {
    // Logique Stripe
    return {
      success: true,
      transactionId: "stripe_tx_123"
    };
  }

  async refund(transactionId: string, amount: number): Promise<void> {
    // Logique de remboursement Stripe
  }

  async getTransactionStatus(transactionId: string): Promise<TransactionStatus> {
    // Récupérer le statut depuis Stripe
    return TransactionStatus.COMPLETED;
  }
}

class PayPalProvider implements PaymentProvider {
  async processPayment(amount: number, currency: string): Promise<PaymentResult> {
    // Logique PayPal
    return {
      success: true,
      transactionId: "paypal_tx_456"
    };
  }

  async refund(transactionId: string, amount: number): Promise<void> {
    // Logique de remboursement PayPal
  }

  async getTransactionStatus(transactionId: string): Promise<TransactionStatus> {
    // Récupérer le statut depuis PayPal
    return TransactionStatus.COMPLETED;
  }
}

// Service découplé de l'implémentation
class PaymentService {
  constructor(private provider: PaymentProvider) {}

  async checkout(order: Order): Promise<void> {
    const result = await this.provider.processPayment(
      order.total,
      order.currency
    );

    if (result.success) {
      order.transactionId = result.transactionId;
      console.log("Paiement réussi");
    }
  }
}

// Facile de changer de fournisseur
const stripeService = new PaymentService(new StripeProvider());
const paypalService = new PaymentService(new PayPalProvider());
```

### 5.2 Gestion de notifications (Classe Abstraite)

```ts
// Classe abstraite avec logique commune
abstract class NotificationChannel {
  constructor(protected retryCount: number = 3) {}

  // Méthode template avec logique commune
  async send(message: string, recipient: string): Promise<boolean> {
    let attempts = 0;
    
    while (attempts < this.retryCount) {
      try {
        await this.doSend(message, recipient);
        await this.logSuccess(recipient);
        return true;
      } catch (error) {
        attempts++;
        if (attempts >= this.retryCount) {
          await this.logFailure(recipient, error);
          return false;
        }
        await this.wait(1000 * attempts); // Backoff exponentiel
      }
    }
    
    return false;
  }

  // Méthode abstraite : chaque canal implémente son envoi
  protected abstract doSend(message: string, recipient: string): Promise<void>;

  // Méthodes communes partagées
  protected async logSuccess(recipient: string): Promise<void> {
    console.log(`Message envoyé avec succès à ${recipient}`);
  }

  protected async logFailure(recipient: string, error: any): Promise<void> {
    console.error(`Échec d'envoi à ${recipient}:`, error);
  }

  protected async wait(ms: number): Promise<void> {
    return new Promise(resolve => setTimeout(resolve, ms));
  }
}

class EmailChannel extends NotificationChannel {
  protected async doSend(message: string, recipient: string): Promise<void> {
    // Logique spécifique à l'email
    console.log(`Envoi email à ${recipient}: ${message}`);
    // Simuler un envoi
  }
}

class SmsChannel extends NotificationChannel {
  protected async doSend(message: string, recipient: string): Promise<void> {
    // Logique spécifique au SMS
    console.log(`Envoi SMS à ${recipient}: ${message}`);
  }
}

class PushChannel extends NotificationChannel {
  protected async doSend(message: string, recipient: string): Promise<void> {
    // Logique spécifique aux push notifications
    console.log(`Envoi push à ${recipient}: ${message}`);
  }
}

// Utilisation
const email = new EmailChannel(3);
await email.send("Votre commande est prête", "alice@example.com");
```

### 5.3 Service Angular/NestJS (Interface + Classe Abstraite)

```ts
// Interface : contrat du service
interface CrudService<T> {
  findAll(): Promise<T[]>;
  findOne(id: string): Promise<T>;
  create(dto: any): Promise<T>;
  update(id: string, dto: any): Promise<T>;
  remove(id: string): Promise<void>;
}

// Classe abstraite : implémentation de base
abstract class BaseCrudService<T> implements CrudService<T> {
  protected abstract repository: Repository<T>;
  protected abstract entityName: string;

  async findAll(): Promise<T[]> {
    return this.repository.findAll();
  }

  async findOne(id: string): Promise<T> {
    const entity = await this.repository.findById(id);
    if (!entity) {
      throw new Error(`${this.entityName} with ID ${id} not found`);
    }
    return entity;
  }

  async create(dto: any): Promise<T> {
    const entity = await this.repository.save(dto);
    return entity;
  }

  async update(id: string, dto: any): Promise<T> {
    await this.findOne(id); // Vérifier l'existence
    const updated = await this.repository.update(id, dto);
    return updated;
  }

  async remove(id: string): Promise<void> {
    await this.findOne(id); // Vérifier l'existence
    await this.repository.delete(id);
  }
}

// Service concret
class UserService extends BaseCrudService<User> {
  protected repository: Repository<User>;
  protected entityName = "User";

  constructor(repository: Repository<User>) {
    super();
    this.repository = repository;
  }

  // Méthodes métier spécifiques
  async findByEmail(email: string): Promise<User | null> {
    return this.repository.findByEmail(email);
  }

  async activateUser(id: string): Promise<void> {
    const user = await this.findOne(id);
    user.isActive = true;
    await this.repository.save(user);
  }
}
```

---

## 6. Erreurs courantes & Comment les éviter

### Erreur 1 : Utiliser une classe abstraite comme interface

```ts
// ❌ Mauvais : classe abstraite sans implémentation
abstract class Logger {
  abstract log(message: string): void;
  abstract error(message: string): void;
}

// ✅ Bon : interface pour un simple contrat
interface Logger {
  log(message: string): void;
  error(message: string): void;
}
```

**Règle :** Si vous n'avez pas de code à partager, utilisez une interface.

### Erreur 2 : Interface avec implémentation (impossible)

```ts
// ❌ Erreur : les interfaces ne peuvent pas avoir d'implémentation
interface Calculator {
  add(a: number, b: number): number {
    return a + b; // ❌ Erreur TypeScript
  }
}

// ✅ Bon : classe abstraite pour partager l'implémentation
abstract class Calculator {
  add(a: number, b: number): number {
    return a + b;
  }

  abstract multiply(a: number, b: number): number;
}
```

### Erreur 3 : Hériter de plusieurs classes abstraites

```ts
// ❌ Impossible en TypeScript
abstract class A { }
abstract class B { }

class C extends A, B { } // ❌ Erreur

// ✅ Solution : interfaces multiples + une classe abstraite
interface IA { }
interface IB { }

abstract class BaseC { }

class C extends BaseC implements IA, IB { }
```

### Erreur 4 : Ne pas marquer les méthodes comme abstract

```ts
// ❌ Méthode non implémentée mais pas marquée abstract
abstract class Shape {
  getArea(): number {
    // Pas d'implémentation mais pas abstract
    return 0; // Valeur par défaut peu utile
  }
}

// ✅ Bon : forcer l'implémentation
abstract class Shape {
  abstract getArea(): number;
}
```

---

## 7. Exercices

### Exercice 1 : Système de cache

Créez un système de cache avec :
- Interface `Cache<T>` avec méthodes `get`, `set`, `delete`, `has`
- Implémentations : `MemoryCache`, `LocalStorageCache`
- Classe abstraite `BaseCache<T>` avec logique de statistiques (hits, misses)

### Exercice 2 : Validators

Créez un système de validation avec :
- Interface `Validator<T>` avec méthode `validate(value: T): ValidationResult`
- Classe abstraite `BaseValidator<T>` avec gestion des messages d'erreur
- Validators concrets : `EmailValidator`, `AgeValidator`, `PasswordValidator`

---

## 8. Comportement Senior

### 8.1 Préférer les interfaces pour le découplage

```ts
// ✅ Interface pour découplage
interface EmailService {
  send(to: string, subject: string, body: string): Promise<void>;
}

class UserService {
  constructor(private emailService: EmailService) {}
  // Découplé de l'implémentation
}
```

### 8.2 Classes abstraites pour le code partagé uniquement

```ts
// ✅ Classe abstraite uniquement si code à partager
abstract class BaseEntity {
  id: string;
  createdAt: Date;
  updatedAt: Date;

  constructor() {
    this.id = generateId();
    this.createdAt = new Date();
    this.updatedAt = new Date();
  }

  touch(): void {
    this.updatedAt = new Date();
  }
}
```

### 8.3 Documentation claire

```ts
/**
 * Interface définissant un fournisseur de stockage.
 * Permet de découpler l'application du système de stockage utilisé.
 */
interface StorageProvider {
  /**
   * Récupère une valeur par sa clé
   * @param key Clé de la valeur
   * @returns La valeur ou null si inexistante
   */
  get(key: string): Promise<string | null>;

  /**
   * Stocke une valeur
   * @param key Clé de stockage
   * @param value Valeur à stocker
   */
  set(key: string, value: string): Promise<void>;
}
```

---

## 9. Résumé

### Tableau de décision

| Besoin | Interface | Classe Abstraite |
|--------|-----------|------------------|
| Définir un contrat | ✅ Oui | ⚠️ Possible |
| Partager du code | ❌ Non | ✅ Oui |
| Héritage multiple | ✅ Oui | ❌ Non |
| Constructeur | ❌ Non | ✅ Oui |
| Propriétés avec valeurs | ❌ Non | ✅ Oui |
| Découplage | ✅ Fort | ⚠️ Moyen |
| Test/Mock | ✅ Facile | ⚠️ Moyen |

### Règles d'or

1. **Par défaut, utilisez une interface** pour définir des contrats
2. **Utilisez une classe abstraite** seulement si vous avez du code à partager
3. **Combinez les deux** si nécessaire (classe abstraite implémentant une interface)
4. **Préférez la composition** à l'héritage quand c'est possible

### En une phrase

> **Les interfaces définissent des contrats flexibles et découplés, tandis que les classes abstraites partagent du code commun dans une hiérarchie.**

---

## 10. Ressources Externes

### Documentation
- [TypeScript Interfaces](https://www.typescriptlang.org/docs/handbook/2/objects.html)
- [TypeScript Abstract Classes](https://www.typescriptlang.org/docs/handbook/2/classes.html#abstract-classes-and-members)

### Articles
- [Interface vs Abstract Class](https://medium.com/@viktor.kukurba/interface-vs-abstract-class-in-typescript-2664c0f2b1c)
- [When to use Abstract Class vs Interface](https://www.c-sharpcorner.com/article/when-to-use-interface-and-abstract-class/)

### Vidéos
- [TypeScript Interfaces - Fireship](https://www.youtube.com/watch?v=JHTCf1R_pyY) (anglais)
- [Abstract Classes in TypeScript](https://www.youtube.com/watch?v=Kv_8yB7VVMg) (anglais)
