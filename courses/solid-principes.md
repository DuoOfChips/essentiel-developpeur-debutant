# SOLID : Les 5 Principes de la Conception Orientée Objet

## Introduction

### Objectifs du cours
Après avoir lu ce cours, vous serez capable de :
- Comprendre les 5 principes SOLID
- Appliquer chaque principe dans votre code TypeScript
- Identifier les violations des principes SOLID
- Refactorer du code pour respecter SOLID
- Créer du code maintenable, testable et évolutif

### Scope de la notion
SOLID permet de :
- **Maintenir** le code facilement
- **Tester** le code efficacement
- **Évol uer** le code sans tout casser
- **Comprendre** le code rapidement
- **Collaborer** sereinement en équipe

Dans les webapps métier (Angular/NestJS), SOLID est essentiel pour :
- Structurer les services et composants
- Faciliter l'injection de dépendances
- Permettre les tests unitaires
- Réduire le couplage entre modules

---

## 1. Qu'est-ce que SOLID ?

SOLID est un acronyme pour **5 principes** de conception orientée objet :

- **S** - Single Responsibility Principle (SRP)
- **O** - Open/Closed Principle (OCP)
- **L** - Liskov Substitution Principle (LSP)
- **I** - Interface Segregation Principle (ISP)
- **D** - Dependency Inversion Principle (DIP)

**Créé par :** Robert C. Martin (Uncle Bob) dans les années 2000

**Objectif :** Créer du code **flexible**, **maintenable** et **évolutif**

---

## 2. S - Single Responsibility Principle (SRP)

### Principe
> **Une classe ne doit avoir qu'une seule raison de changer**
> 
> Une classe = une responsabilité = un seul axe de changement

### Analogie
Un **couteau suisse** 🔪 vs un **couteau de chef** 👨‍🍳 :
- Le couteau suisse fait tout : couper, visser, ouvrir des bouteilles
- Le couteau de chef fait une chose : couper, mais il le fait parfaitement
- Si vous voulez améliorer la coupe, vous modifiez le couteau de chef, pas le couteau suisse

### Violation du SRP

```ts
// ❌ Violation : cette classe a TROP de responsabilités
class UserService {
  // Responsabilité 1 : Gestion des utilisateurs
  createUser(userData: any): User {
    const user = new User(userData);
    return user;
  }

  // Responsabilité 2 : Validation
  validateEmail(email: string): boolean {
    return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
  }

  // Responsabilité 3 : Persistance
  saveToDatabase(user: User): void {
    // Code de sauvegarde en base
  }

  // Responsabilité 4 : Envoi d'emails
  sendWelcomeEmail(user: User): void {
    // Code d'envoi d'email
  }

  // Responsabilité 5 : Logging
  logUserCreation(user: User): void {
    console.log(`User ${user.id} created`);
  }

  // Responsabilité 6 : Génération de rapports
  generateUserReport(user: User): string {
    return `Report for ${user.name}`;
  }
}

// ⚠️ Problème : Si on change la façon de logger, on doit modifier UserService
// ⚠️ Problème : Si on change la base de données, on doit modifier UserService
// ⚠️ Problème : Difficile à tester (trop de dépendances)
```

### Respect du SRP

```ts
// ✅ Une classe = une responsabilité

// Responsabilité : Validation
class UserValidator {
  validateEmail(email: string): boolean {
    return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
  }

  validateAge(age: number): boolean {
    return age >= 18 && age <= 150;
  }

  validate(userData: any): boolean {
    return this.validateEmail(userData.email) && 
           this.validateAge(userData.age);
  }
}

// Responsabilité : Persistance
class UserRepository {
  private users: Map<string, User> = new Map();

  save(user: User): void {
    this.users.set(user.id, user);
  }

  findById(id: string): User | null {
    return this.users.get(id) || null;
  }

  findAll(): User[] {
    return Array.from(this.users.values());
  }
}

// Responsabilité : Envoi d'emails
class EmailService {
  sendWelcomeEmail(user: User): void {
    console.log(`Envoi email de bienvenue à ${user.email}`);
  }

  sendPasswordResetEmail(user: User): void {
    console.log(`Envoi email de réinitialisation à ${user.email}`);
  }
}

// Responsabilité : Logging
class Logger {
  log(message: string): void {
    console.log(`[INFO] ${message}`);
  }

  error(message: string): void {
    console.error(`[ERROR] ${message}`);
  }
}

// Responsabilité : Orchestration (gestion métier)
class UserService {
  constructor(
    private validator: UserValidator,
    private repository: UserRepository,
    private emailService: EmailService,
    private logger: Logger
  ) {}

  createUser(userData: any): User {
    // Valider
    if (!this.validator.validate(userData)) {
      throw new Error("Invalid user data");
    }

    // Créer
    const user = new User(userData);

    // Sauvegarder
    this.repository.save(user);

    // Envoyer email
    this.emailService.sendWelcomeEmail(user);

    // Logger
    this.logger.log(`User ${user.id} created`);

    return user;
  }
}
```

**Avantages :**
- ✅ Facile à tester (chaque classe séparément)
- ✅ Facile à modifier (changement isolé)
- ✅ Facile à réutiliser (composants indépendants)
- ✅ Facile à comprendre (responsabilité claire)

---

## 3. O - Open/Closed Principle (OCP)

### Principe
> **Les classes doivent être ouvertes à l'extension, mais fermées à la modification**
> 
> On doit pouvoir ajouter de nouvelles fonctionnalités sans modifier le code existant

### Analogie
Un **smartphone** 📱 avec des **applications** :
- Le téléphone est fermé (vous ne modifiez pas l'OS)
- Mais vous pouvez ajouter des apps (extension)
- Chaque app ajoute une fonctionnalité sans casser les autres

### Violation de l'OCP

```ts
// ❌ Violation : ajouter un nouveau type de paiement = modifier la classe
class PaymentProcessor {
  processPayment(order: Order, paymentType: string): void {
    if (paymentType === "creditCard") {
      console.log("Traitement par carte bancaire");
      // Logique spécifique carte
    } else if (paymentType === "paypal") {
      console.log("Traitement par PayPal");
      // Logique spécifique PayPal
    } else if (paymentType === "bitcoin") {
      // ⚠️ Ajout = modification du code existant
      console.log("Traitement par Bitcoin");
    }
    // ⚠️ Et si on ajoute Apple Pay ? Encore une modification !
  }
}

// ⚠️ Problème : Chaque nouveau moyen de paiement = modifier cette classe
// ⚠️ Risque de régression : on peut casser les paiements existants
```

### Respect de l'OCP

```ts
// ✅ Ouvert à l'extension, fermé à la modification

// Interface définissant le contrat
interface PaymentMethod {
  process(order: Order): void;
}

// Implémentations concrètes
class CreditCardPayment implements PaymentMethod {
  process(order: Order): void {
    console.log(`Paiement CB pour ${order.total}€`);
    // Logique carte bancaire
  }
}

class PayPalPayment implements PaymentMethod {
  process(order: Order): void {
    console.log(`Paiement PayPal pour ${order.total}€`);
    // Logique PayPal
  }
}

class BitcoinPayment implements PaymentMethod {
  process(order: Order): void {
    console.log(`Paiement Bitcoin pour ${order.total}€`);
    // Logique Bitcoin
  }
}

// ✅ Nouvelle méthode de paiement = nouvelle classe (pas de modification)
class ApplePayPayment implements PaymentMethod {
  process(order: Order): void {
    console.log(`Paiement Apple Pay pour ${order.total}€`);
  }
}

// Processeur qui utilise le polymorphisme
class PaymentProcessor {
  processPayment(order: Order, method: PaymentMethod): void {
    method.process(order);
    // ✅ Pas besoin de modifier ce code pour ajouter un nouveau moyen de paiement
  }
}

// Utilisation
const processor = new PaymentProcessor();
const order = new Order(100);

processor.processPayment(order, new CreditCardPayment());
processor.processPayment(order, new PayPalPayment());
processor.processPayment(order, new ApplePayPayment()); // Nouvelle méthode sans modification
```

**Avantages :**
- ✅ Pas de risque de régression
- ✅ Ajout de fonctionnalités sans modifier l'existant
- ✅ Code stable et prévisible

---

## 4. L - Liskov Substitution Principle (LSP)

### Principe
> **Les objets d'une classe dérivée doivent pouvoir remplacer les objets de la classe de base sans altérer le bon fonctionnement du programme**
> 
> Si B hérite de A, on doit pouvoir utiliser B partout où on attend A

### Analogie
Une **télécommande** 🎮 :
- Si vous achetez une télécommande universelle
- Elle doit fonctionner exactement comme l'originale
- Appuyer sur "volume +" doit augmenter le volume, pas changer de chaîne

### Violation du LSP

```ts
// ❌ Violation de Liskov
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
    this.height = width; // ⚠️ Modifie aussi la hauteur !
  }

  setHeight(height: number): void {
    this.width = height; // ⚠️ Modifie aussi la largeur !
    this.height = height;
  }
}

// Test
function testRectangle(rect: Rectangle): void {
  rect.setWidth(5);
  rect.setHeight(10);
  console.log(`Aire attendue: 50, Aire réelle: ${rect.getArea()}`);
}

const rect = new Rectangle(0, 0);
testRectangle(rect); // "Aire attendue: 50, Aire réelle: 50" ✅

const square = new Square(0, 0);
testRectangle(square); // "Aire attendue: 50, Aire réelle: 100" ❌
// ⚠️ Le carré ne peut pas substituer le rectangle !
```

### Respect du LSP

```ts
// ✅ Respect de Liskov : pas d'héritage, composition

interface Shape {
  getArea(): number;
  getPerimeter(): number;
}

class Rectangle implements Shape {
  constructor(private width: number, private height: number) {}

  setWidth(width: number): void {
    this.width = width;
  }

  setHeight(height: number): void {
    this.height = height;
  }

  getArea(): number {
    return this.width * this.height;
  }

  getPerimeter(): number {
    return 2 * (this.width + this.height);
  }
}

class Square implements Shape {
  constructor(private side: number) {}

  setSide(side: number): void {
    this.side = side;
  }

  getArea(): number {
    return this.side ** 2;
  }

  getPerimeter(): number {
    return 4 * this.side;
  }
}

// Fonction qui attend une Shape
function printShapeInfo(shape: Shape): void {
  console.log(`Aire: ${shape.getArea()}`);
  console.log(`Périmètre: ${shape.getPerimeter()}`);
}

// ✅ Les deux implémentations fonctionnent correctement
printShapeInfo(new Rectangle(5, 10));
printShapeInfo(new Square(5));
```

**Autre exemple :**

```ts
// ❌ Violation
class Bird {
  fly(): void {
    console.log("Je vole");
  }
}

class Penguin extends Bird {
  fly(): void {
    throw new Error("Les pingouins ne volent pas !"); // ⚠️ Casse le contrat
  }
}

// ✅ Respect du LSP
interface Bird {
  move(): void;
}

class FlyingBird implements Bird {
  move(): void {
    this.fly();
  }

  private fly(): void {
    console.log("Je vole");
  }
}

class SwimmingBird implements Bird {
  move(): void {
    this.swim();
  }

  private swim(): void {
    console.log("Je nage");
  }
}
```

---

## 5. I - Interface Segregation Principle (ISP)

### Principe
> **Aucun client ne devrait être forcé de dépendre de méthodes qu'il n'utilise pas**
> 
> Préférez plusieurs interfaces spécifiques plutôt qu'une interface générale

### Analogie
Un **restaurant** 🍽️ :
- Menu végétarien pour les végétariens
- Menu sans gluten pour les intolérants
- Menu enfant pour les enfants
- Plutôt qu'un seul menu énorme avec tout

### Violation de l'ISP

```ts
// ❌ Violation : interface trop large
interface Worker {
  work(): void;
  eat(): void;
  sleep(): void;
  attendMeeting(): void;
  writeCode(): void;
  designUI(): void;
  manageTeam(): void;
}

// ⚠️ Un développeur ne manage pas forcément une équipe
class Developer implements Worker {
  work(): void {
    console.log("Je travaille");
  }

  eat(): void {
    console.log("Je mange");
  }

  sleep(): void {
    console.log("Je dors");
  }

  attendMeeting(): void {
    console.log("Je participe à une réunion");
  }

  writeCode(): void {
    console.log("J'écris du code");
  }

  designUI(): void {
    throw new Error("Je ne fais pas de design"); // ⚠️ Forcé d'implémenter
  }

  manageTeam(): void {
    throw new Error("Je ne manage pas"); // ⚠️ Forcé d'implémenter
  }
}
```

### Respect de l'ISP

```ts
// ✅ Interfaces ségrégées (spécifiques)
interface Workable {
  work(): void;
}

interface Eatable {
  eat(): void;
}

interface Sleepable {
  sleep(): void;
}

interface Codable {
  writeCode(): void;
}

interface Designable {
  designUI(): void;
}

interface Manageable {
  manageTeam(): void;
  attendMeeting(): void;
}

// Chaque classe implémente seulement ce dont elle a besoin
class Developer implements Workable, Eatable, Sleepable, Codable {
  work(): void {
    console.log("Je travaille");
  }

  eat(): void {
    console.log("Je mange");
  }

  sleep(): void {
    console.log("Je dors");
  }

  writeCode(): void {
    console.log("J'écris du code");
  }
}

class Designer implements Workable, Eatable, Sleepable, Designable {
  work(): void {
    console.log("Je travaille");
  }

  eat(): void {
    console.log("Je mange");
  }

  sleep(): void {
    console.log("Je dors");
  }

  designUI(): void {
    console.log("Je crée des designs");
  }
}

class Manager implements Workable, Eatable, Sleepable, Manageable {
  work(): void {
    console.log("Je travaille");
  }

  eat(): void {
    console.log("Je mange");
  }

  sleep(): void {
    console.log("Je dors");
  }

  manageTeam(): void {
    console.log("Je manage l'équipe");
  }

  attendMeeting(): void {
    console.log("Je participe aux réunions");
  }
}
```

**Autre exemple : Repository**

```ts
// ❌ Violation : interface trop large
interface Repository<T> {
  findById(id: string): Promise<T>;
  findAll(): Promise<T[]>;
  create(entity: T): Promise<T>;
  update(id: string, entity: T): Promise<T>;
  delete(id: string): Promise<void>;
  search(query: string): Promise<T[]>;
  export(): Promise<string>;
  import(data: string): Promise<void>;
  backup(): Promise<void>;
  restore(backup: string): Promise<void>;
}

// ⚠️ Un simple repository de lecture n'a pas besoin de create/update/delete

// ✅ Interfaces ségrégées
interface Readable<T> {
  findById(id: string): Promise<T>;
  findAll(): Promise<T[]>;
}

interface Writable<T> {
  create(entity: T): Promise<T>;
  update(id: string, entity: T): Promise<T>;
  delete(id: string): Promise<void>;
}

interface Searchable<T> {
  search(query: string): Promise<T[]>;
}

interface Exportable {
  export(): Promise<string>;
  import(data: string): Promise<void>;
}

// Repository en lecture seule
class ReadOnlyRepository<T> implements Readable<T> {
  async findById(id: string): Promise<T> {
    // ...
  }

  async findAll(): Promise<T[]> {
    // ...
  }
}

// Repository complet
class FullRepository<T> implements Readable<T>, Writable<T>, Searchable<T> {
  async findById(id: string): Promise<T> { /* ... */ }
  async findAll(): Promise<T[]> { /* ... */ }
  async create(entity: T): Promise<T> { /* ... */ }
  async update(id: string, entity: T): Promise<T> { /* ... */ }
  async delete(id: string): Promise<void> { /* ... */ }
  async search(query: string): Promise<T[]> { /* ... */ }
}
```

---

## 6. D - Dependency Inversion Principle (DIP)

### Principe
> **Les modules de haut niveau ne doivent pas dépendre des modules de bas niveau. Les deux doivent dépendre d'abstractions.**
> 
> **Les abstractions ne doivent pas dépendre des détails. Les détails doivent dépendre des abstractions.**

### Analogie
Une **prise électrique** 🔌 :
- Votre lampe (haut niveau) ne dépend pas d'une centrale électrique spécifique (bas niveau)
- Elle dépend d'une **abstraction** : la prise murale
- Vous pouvez changer de fournisseur d'électricité sans changer votre lampe

### Violation du DIP

```ts
// ❌ Violation : dépendance directe sur l'implémentation concrète

// Module bas niveau
class MySQLDatabase {
  connect(): void {
    console.log("Connexion à MySQL");
  }

  query(sql: string): any {
    console.log(`Exécution: ${sql}`);
    return [];
  }
}

// Module haut niveau dépend du module bas niveau
class UserService {
  private database: MySQLDatabase; // ⚠️ Dépendance concrète !

  constructor() {
    this.database = new MySQLDatabase(); // ⚠️ Instanciation directe !
  }

  getUser(id: string): User {
    const result = this.database.query(`SELECT * FROM users WHERE id = ${id}`);
    return result;
  }
}

// ⚠️ Problème : Si on veut passer à PostgreSQL, on doit modifier UserService
// ⚠️ Problème : Impossible de tester UserService sans vraie base de données
```

### Respect du DIP

```ts
// ✅ Abstraction (interface)
interface Database {
  connect(): void;
  query(sql: string): any;
}

// Implémentations concrètes (détails)
class MySQLDatabase implements Database {
  connect(): void {
    console.log("Connexion à MySQL");
  }

  query(sql: string): any {
    console.log(`MySQL: ${sql}`);
    return [];
  }
}

class PostgreSQLDatabase implements Database {
  connect(): void {
    console.log("Connexion à PostgreSQL");
  }

  query(sql: string): any {
    console.log(`PostgreSQL: ${sql}`);
    return [];
  }
}

class MongoDatabase implements Database {
  connect(): void {
    console.log("Connexion à MongoDB");
  }

  query(sql: string): any {
    console.log(`MongoDB: ${sql}`);
    return [];
  }
}

// ✅ Module haut niveau dépend de l'abstraction
class UserService {
  constructor(private database: Database) {} // ✅ Injection de dépendance

  getUser(id: string): User {
    const result = this.database.query(`SELECT * FROM users WHERE id = ${id}`);
    return result;
  }
}

// Utilisation : on injecte la dépendance
const mysqlService = new UserService(new MySQLDatabase());
const postgresService = new UserService(new PostgreSQLDatabase());
const mongoService = new UserService(new MongoDatabase());

// ✅ Facile de changer de base de données
// ✅ Facile de tester avec un mock
class MockDatabase implements Database {
  connect(): void {}
  query(sql: string): any {
    return { id: "1", name: "Test User" };
  }
}

const testService = new UserService(new MockDatabase());
```

**Autre exemple : Notification**

```ts
// ✅ Abstraction
interface NotificationSender {
  send(message: string, recipient: string): void;
}

// Implémentations
class EmailSender implements NotificationSender {
  send(message: string, recipient: string): void {
    console.log(`Email à ${recipient}: ${message}`);
  }
}

class SmsSender implements NotificationSender {
  send(message: string, recipient: string): void {
    console.log(`SMS à ${recipient}: ${message}`);
  }
}

// Service dépend de l'abstraction
class NotificationService {
  constructor(private sender: NotificationSender) {}

  notify(message: string, recipient: string): void {
    this.sender.send(message, recipient);
  }
}

// Injection de dépendances
const emailNotif = new NotificationService(new EmailSender());
const smsNotif = new NotificationService(new SmsSender());
```

---

## 7. Cas d'usage métier complet (tous les principes SOLID)

```ts
// === SRP : Chaque classe a une responsabilité ===

// Validation
interface Validator<T> {
  validate(entity: T): boolean;
}

class UserValidator implements Validator<User> {
  validate(user: User): boolean {
    return user.email.includes('@') && user.age >= 18;
  }
}

// Persistance
interface Repository<T> {
  save(entity: T): Promise<void>;
  findById(id: string): Promise<T | null>;
}

class UserRepository implements Repository<User> {
  private users: Map<string, User> = new Map();

  async save(user: User): Promise<void> {
    this.users.set(user.id, user);
  }

  async findById(id: string): Promise<User | null> {
    return this.users.get(id) || null;
  }
}

// Notification
interface NotificationSender {
  send(message: string, recipient: string): Promise<void>;
}

class EmailNotification implements NotificationSender {
  async send(message: string, recipient: string): Promise<void> {
    console.log(`Email à ${recipient}: ${message}`);
  }
}

// === OCP : Ouvert à l'extension, fermé à la modification ===
// On peut ajouter de nouveaux types de notifications sans modifier le code existant

class SmsNotification implements NotificationSender {
  async send(message: string, recipient: string): Promise<void> {
    console.log(`SMS à ${recipient}: ${message}`);
  }
}

// === LSP : Les implémentations respectent le contrat ===
// EmailNotification et SmsNotification peuvent substituer NotificationSender

// === ISP : Interfaces spécifiques ===
// Validator, Repository, NotificationSender sont des interfaces séparées

// === DIP : Dépendances injectées (abstractions) ===
class UserService {
  constructor(
    private validator: Validator<User>,
    private repository: Repository<User>,
    private notificationSender: NotificationSender
  ) {}

  async registerUser(userData: any): Promise<User> {
    const user = new User(userData);

    // Valider
    if (!this.validator.validate(user)) {
      throw new Error("Invalid user");
    }

    // Sauvegarder
    await this.repository.save(user);

    // Notifier
    await this.notificationSender.send(
      "Bienvenue !",
      user.email
    );

    return user;
  }
}

// Utilisation avec injection de dépendances
const userService = new UserService(
  new UserValidator(),
  new UserRepository(),
  new EmailNotification()
);

// Facile de changer les dépendances
const userServiceWithSms = new UserService(
  new UserValidator(),
  new UserRepository(),
  new SmsNotification() // ✅ Juste changer l'implémentation
);
```

---

## 8. Erreurs courantes & Comment les éviter

### Erreur 1 : Violer plusieurs principes à la fois

```ts
// ❌ Viole SRP, OCP, DIP
class OrderProcessor {
  processOrder(order: Order): void {
    // Validation (SRP)
    if (order.items.length === 0) throw new Error();

    // Calcul (SRP)
    let total = 0;
    for (const item of order.items) {
      total += item.price;
    }

    // Paiement (SRP + OCP)
    if (order.paymentMethod === "card") {
      // ...
    } else if (order.paymentMethod === "paypal") {
      // ...
    }

    // Base de données (SRP + DIP)
    const mysql = new MySQLDatabase();
    mysql.save(order);

    // Email (SRP + DIP)
    const gmail = new GmailService();
    gmail.send(order.customerEmail);
  }
}
```

### Erreur 2 : Sur-ingénierie (trop de SOLID)

```ts
// ❌ Trop complexe pour un cas simple
interface NumberAdder {
  add(a: number, b: number): number;
}

class SimpleNumberAdder implements NumberAdder {
  add(a: number, b: number): number {
    return a + b;
  }
}

// ✅ Pour des opérations simples, une fonction suffit
function add(a: number, b: number): number {
  return a + b;
}
```

**Règle :** Appliquez SOLID quand c'est pertinent, pas systématiquement.

---

## 9. Exercices

### Exercice 1 : Refactoring SOLID

Refactorez cette classe pour respecter tous les principes SOLID :

```ts
class BlogService {
  createPost(title: string, content: string, authorEmail: string): void {
    // Validation
    if (title.length < 5) throw new Error("Title too short");
    if (!authorEmail.includes('@')) throw new Error("Invalid email");

    // Création
    const post = {
      id: Math.random().toString(),
      title,
      content,
      authorEmail,
      createdAt: new Date()
    };

    // Sauvegarde MySQL
    const mysql = new MySQLConnection();
    mysql.query(`INSERT INTO posts ...`);

    // Email
    console.log(`Email sent to ${authorEmail}`);

    // Log
    console.log(`Post ${post.id} created`);
  }
}
```

### Exercice 2 : Système de paiement SOLID

Créez un système de paiement respectant SOLID avec :
- Différents moyens de paiement (carte, PayPal, virement)
- Validation des montants
- Sauvegarde des transactions
- Notifications aux clients
- Génération de factures

---

## 10. Comportement Senior

### 10.1 SOLID dans NestJS (Dependency Injection)

```ts
// ✅ NestJS applique naturellement SOLID

@Injectable()
export class UserService {
  constructor(
    private readonly userRepository: UserRepository,
    private readonly emailService: EmailService,
    private readonly logger: Logger
  ) {}

  async create(createUserDto: CreateUserDto): Promise<User> {
    const user = await this.userRepository.save(createUserDto);
    await this.emailService.sendWelcome(user);
    this.logger.log(`User ${user.id} created`);
    return user;
  }
}
```

### 10.2 SOLID dans Angular

```ts
// ✅ Services Angular avec injection de dépendances

@Injectable({ providedIn: 'root' })
export class UserService {
  constructor(
    private http: HttpClient,
    private logger: LoggerService
  ) {}

  getUser(id: string): Observable<User> {
    this.logger.log(`Fetching user ${id}`);
    return this.http.get<User>(`/api/users/${id}`);
  }
}
```

### 10.3 Tests facilitéspar SOLID

```ts
// ✅ Test facile grâce à DIP
describe('UserService', () => {
  it('should create user', async () => {
    // Mocks
    const mockRepo: Repository<User> = {
      save: jest.fn(),
      findById: jest.fn()
    };

    const mockNotif: NotificationSender = {
      send: jest.fn()
    };

    const service = new UserService(
      new UserValidator(),
      mockRepo,
      mockNotif
    );

    await service.registerUser({ email: 'test@test.com', age: 25 });

    expect(mockRepo.save).toHaveBeenCalled();
    expect(mockNotif.send).toHaveBeenCalled();
  });
});
```

---

## 11. Résumé

| Principe | En une phrase | Bénéfice |
|----------|---------------|----------|
| **SRP** | Une classe = une responsabilité | Maintenabilité |
| **OCP** | Ouvert extension, fermé modification | Évolutivité |
| **LSP** | Les enfants substituables aux parents | Fiabilité |
| **ISP** | Interfaces spécifiques, pas générales | Simplicité |
| **DIP** | Dépendre d'abstractions, pas de concret | Testabilité |

### Checklist SOLID

Avant de valider votre code :
- [ ] **SRP** : Chaque classe a une seule responsabilité
- [ ] **OCP** : Peut-on ajouter des fonctionnalités sans modifier l'existant ?
- [ ] **LSP** : Les classes dérivées respectent le contrat parent
- [ ] **ISP** : Les interfaces sont spécifiques et ciblées
- [ ] **DIP** : Les dépendances sont injectées (pas instanciées)

### En une phrase

> **SOLID est un ensemble de 5 principes pour créer du code orienté objet maintenable, testable et évolutif en favorisant la séparation des responsabilités, l'extension sans modification, la substitution fiable, les interfaces ciblées et l'inversion des dépendances.**

---

## 12. Ressources Externes

### Articles
- [SOLID Principles](https://en.wikipedia.org/wiki/SOLID)
- [SOLID Principles in TypeScript](https://medium.com/@severinperez/writing-flexible-code-with-the-single-responsibility-principle-b71c4f3f883f)

### Vidéos
- [SOLID Principles - Fireship](https://www.youtube.com/watch?v=pTB30aXS77U) (anglais, 10min)
- [SOLID en 7 minutes](https://www.youtube.com/watch?v=7EmboKQH8lM) (français)
- [Clean Code - Uncle Bob](https://www.youtube.com/watch?v=7EmboKQH8lM)

### Livres
- **Clean Code** - Robert C. Martin
- **Clean Architecture** - Robert C. Martin
- **Agile Software Development, Principles, Patterns, and Practices** - Robert C. Martin

### Sites interactifs
- [SOLID Principles Explained](https://www.digitalocean.com/community/conceptual_articles/s-o-l-i-d-the-first-five-principles-of-object-oriented-design)
- [Refactoring Guru - SOLID](https://refactoring.guru/design-patterns/solid)
