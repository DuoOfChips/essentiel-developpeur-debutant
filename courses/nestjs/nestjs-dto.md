# NestJS DTOs : Data Transfer Objects

## Introduction

### Objectifs du cours
Ce cours vous permettra de :
- Comprendre ce qu'est un DTO et pourquoi l'utiliser
- Créer des DTOs pour vos endpoints
- Différencier les DTOs de création, mise à jour et réponse
- Organiser vos DTOs de manière professionnelle

### Ce que vous saurez faire après ce cours
- Créer des DTOs pour structurer vos données d'API
- Utiliser les DTOs dans vos controllers et services
- Séparer les DTOs par cas d'usage (create, update, response)
- Typer correctement vos endpoints avec TypeScript

### Scope de la notion
Les DTOs (Data Transfer Objects) sont des objets utilisés pour définir la structure des données qui transitent entre le client et le serveur. Ils servent de contrat d'interface, facilitent la validation, et permettent de séparer la représentation des données de la logique métier. Dans une webapp métier, les DTOs garantissent que les données envoyées et reçues respectent un format précis.

---

## Prérequis

Avant ce cours, vous devez connaître :
- [NestJS : Installation et premier projet](./nestjs-installation-premier-projet.md)
- [TypeScript : Types et interfaces](../type-vs-interface.md)
- [HTTP et APIs](../http-api-fondamentaux.md)

---

## Définitions et Concepts Clés

### DTO (Data Transfer Object)
Un **DTO** est une classe TypeScript simple qui définit la structure des données transférées via le réseau. Il n'a pas de logique métier, seulement des propriétés.

**Analogie** : Un DTO est comme un formulaire papier avec des cases à remplir. Le formulaire définit quelles informations sont attendues (nom, prénom, âge), mais ne fait aucun traitement. Il sert juste à transporter les informations du point A au point B.

### Entity vs DTO
- **Entity** : Représente les données en base de données (structure BDD)
- **DTO** : Représente les données en transit (structure API)

**Exemple concret** : Dans une webapp de gestion d'utilisateurs :
- **Entity User** : id, email, passwordHash, createdAt, updatedAt, lastLoginAt
- **DTO CreateUser** : email, password (pas d'id, pas de hash, pas de dates)
- **DTO UserResponse** : id, email, createdAt (pas de password!)

**Pourquoi séparer ?**
- Sécurité : Ne jamais exposer le passwordHash dans une réponse
- Flexibilité : L'API peut évoluer sans changer la BDD
- Validation : Des règles différentes pour créer vs modifier

### Séparation des DTOs par cas d'usage
Dans une API REST professionnelle, on crée différents DTOs :
- **CreateDTO** : Données pour créer une ressource (POST)
- **UpdateDTO** : Données pour modifier (PUT/PATCH)
- **ResponseDTO** : Données retournées au client (GET)
- **QueryDTO** : Paramètres de filtrage/recherche

**Analogie** : C'est comme avoir différents formulaires administratifs :
- Formulaire de création de compte (tous les champs requis)
- Formulaire de modification (champs optionnels)
- Carte d'identité (seulement les infos publiques)

---

## Pourquoi Utiliser des DTOs ?

### Problèmes sans DTOs

```typescript
// ❌ MAUVAIS - Utiliser l'entity directement
@Post()
create(@Body() user: User) {  // User = entity de la BDD
  return this.usersService.create(user);
}

// Client peut envoyer n'importe quoi :
{
  "email": "test@example.com",
  "password": "12345",
  "isAdmin": true,          // ⚠️ Dangereux! Le client s'auto-promeut admin
  "balance": 1000000,       // ⚠️ Le client se donne de l'argent
  "id": 999                 // ⚠️ Le client choisit son ID
}
```

**Problèmes** :
- ❌ Aucune validation
- ❌ Le client peut envoyer des champs sensibles
- ❌ Pas de typage strict
- ❌ Risques de sécurité

### Avec DTOs

```typescript
// ✅ BON - DTO dédié
export class CreateUserDto {
  email: string;
  password: string;
  // C'est TOUT. Le client ne peut envoyer que ça.
}

@Post()
create(@Body() createUserDto: CreateUserDto) {
  return this.usersService.create(createUserDto);
}

// Le client ne peut envoyer QUE :
{
  "email": "test@example.com",
  "password": "12345"
}
// Tout autre champ sera ignoré ou rejeté
```

**Avantages** :
- ✅ Contrat d'interface clair
- ✅ Validation automatique (avec class-validator)
- ✅ Sécurité accrue
- ✅ Documentation automatique (avec Swagger)
- ✅ Auto-complétion dans l'IDE

---

## Créer des DTOs

### Structure des dossiers

```
src/
  users/
    dto/
      create-user.dto.ts      # DTO pour créer
      update-user.dto.ts      # DTO pour modifier
      user-response.dto.ts    # DTO pour réponse (optionnel)
      query-user.dto.ts       # DTO pour filtres (optionnel)
    entities/
      user.entity.ts          # Entity de la BDD
    users.controller.ts
    users.service.ts
    users.module.ts
```

### DTO de création : CreateDTO

**Exemple : Créer un produit**

`src/products/dto/create-product.dto.ts` :

```typescript
export class CreateProductDto {
  name: string;
  description: string;
  price: number;
  categoryId: number;
  inStock: boolean;
}
```

**Usage dans le controller** :

```typescript
import { Controller, Post, Body } from '@nestjs/common';
import { CreateProductDto } from './dto/create-product.dto';

@Controller('products')
export class ProductsController {
  @Post()
  create(@Body() createProductDto: CreateProductDto) {
    // createProductDto est automatiquement typé
    console.log(createProductDto.name);  // ✅ Auto-complétion
    console.log(createProductDto.price); // ✅ Auto-complétion
    
    return this.productsService.create(createProductDto);
  }
}
```

### DTO de mise à jour : UpdateDTO

**Différence clé** : Tous les champs sont **optionnels** (partial).

`src/products/dto/update-product.dto.ts` :

**Option 1 : Manuellement**
```typescript
export class UpdateProductDto {
  name?: string;
  description?: string;
  price?: number;
  categoryId?: number;
  inStock?: boolean;
}
```

**Option 2 : Avec PartialType (recommandé)**
```typescript
import { PartialType } from '@nestjs/mapped-types';
import { CreateProductDto } from './create-product.dto';

export class UpdateProductDto extends PartialType(CreateProductDto) {}
// Tous les champs de CreateProductDto deviennent optionnels automatiquement
```

**Usage** :

```typescript
@Put(':id')
update(
  @Param('id') id: string,
  @Body() updateProductDto: UpdateProductDto
) {
  // Le client peut envoyer seulement le prix, ou seulement le nom, etc.
  return this.productsService.update(+id, updateProductDto);
}
```

**Exemple de requête** :
```json
{
  "price": 99.99
}
// Seul le prix est modifié, le reste ne change pas
```

### DTO de réponse : ResponseDTO (optionnel mais recommandé)

**But** : Contrôler exactement ce qui est retourné au client.

`src/users/dto/user-response.dto.ts` :

```typescript
export class UserResponseDto {
  id: number;
  email: string;
  firstName: string;
  lastName: string;
  createdAt: Date;
  
  // ✅ Le client ne voit PAS :
  // - password
  // - passwordHash
  // - resetToken
  // - internalNotes
}
```

**Transformation dans le service** :

```typescript
@Injectable()
export class UsersService {
  async findOne(id: number): Promise<UserResponseDto> {
    const user = await this.userRepository.findOne(id);
    
    // Mapper Entity → DTO
    const responseDto: UserResponseDto = {
      id: user.id,
      email: user.email,
      firstName: user.firstName,
      lastName: user.lastName,
      createdAt: user.createdAt,
    };
    
    return responseDto;
  }
}
```

### DTO de requête/query : QueryDTO

**But** : Typer les paramètres de filtrage/recherche.

`src/products/dto/query-product.dto.ts` :

```typescript
export class QueryProductDto {
  category?: string;
  minPrice?: number;
  maxPrice?: number;
  inStock?: boolean;
  sortBy?: 'name' | 'price' | 'createdAt';
  sortOrder?: 'ASC' | 'DESC';
  page?: number;
  limit?: number;
}
```

**Usage** :

```typescript
@Get()
findAll(@Query() query: QueryProductDto) {
  // GET /products?category=electronics&minPrice=100&maxPrice=500
  return this.productsService.findAll(query);
}
```

---

## Cas d'Usage Concrets

### Cas 1 : API de gestion d'utilisateurs

**Entity** :
```typescript
// src/users/entities/user.entity.ts
export class User {
  id: number;
  email: string;
  passwordHash: string;
  firstName: string;
  lastName: string;
  role: 'user' | 'admin';
  isActive: boolean;
  resetToken?: string;
  lastLoginAt?: Date;
  createdAt: Date;
  updatedAt: Date;
}
```

**CreateUserDto** :
```typescript
// src/users/dto/create-user.dto.ts
export class CreateUserDto {
  email: string;
  password: string;       // En clair, sera hashé côté serveur
  firstName: string;
  lastName: string;
  // Pas de role, pas d'id, pas de dates → définis côté serveur
}
```

**UpdateUserDto** :
```typescript
// src/users/dto/update-user.dto.ts
import { PartialType, OmitType } from '@nestjs/mapped-types';
import { CreateUserDto } from './create-user.dto';

// UpdateUserDto sans le password pour sécurité
export class UpdateUserDto extends PartialType(
  OmitType(CreateUserDto, ['password'] as const)
) {}

// DTO séparé pour changer le mot de passe (meilleure sécurité)
// src/users/dto/change-password.dto.ts
export class ChangePasswordDto {
  currentPassword: string;
  newPassword: string;
  confirmPassword: string;
}
```

**UserResponseDto** :
```typescript
// src/users/dto/user-response.dto.ts
export class UserResponseDto {
  id: number;
  email: string;
  firstName: string;
  lastName: string;
  role: string;
  isActive: boolean;
  createdAt: Date;
  // ✅ Pas de passwordHash, resetToken, etc.
}
```

**Controller** :
```typescript
@Controller('users')
export class UsersController {
  @Post()
  async create(@Body() createUserDto: CreateUserDto): Promise<UserResponseDto> {
    return this.usersService.create(createUserDto);
  }

  @Get(':id')
  async findOne(@Param('id') id: string): Promise<UserResponseDto> {
    return this.usersService.findOne(+id);
  }

  @Patch(':id')
  async update(
    @Param('id') id: string,
    @Body() updateUserDto: UpdateUserDto
  ): Promise<UserResponseDto> {
    return this.usersService.update(+id, updateUserDto);
  }
}
```

### Cas 2 : API e-commerce avec filtres

**Product Entity** :
```typescript
export class Product {
  id: number;
  name: string;
  description: string;
  price: number;
  costPrice: number;      // Prix d'achat (privé)
  stock: number;
  category: string;
  imageUrl: string;
  isActive: boolean;
  createdAt: Date;
}
```

**CreateProductDto** :
```typescript
export class CreateProductDto {
  name: string;
  description: string;
  price: number;
  costPrice: number;      // Admin peut définir le prix d'achat
  stock: number;
  category: string;
  imageUrl?: string;
}
```

**ProductResponseDto** (pour le public) :
```typescript
export class ProductResponseDto {
  id: number;
  name: string;
  description: string;
  price: number;
  // ✅ Pas de costPrice (info privée)
  // ✅ Pas de stock exact, juste disponibilité
  inStock: boolean;
  category: string;
  imageUrl?: string;
}
```

**QueryProductDto** :
```typescript
export class QueryProductDto {
  category?: string;
  minPrice?: number;
  maxPrice?: number;
  search?: string;
  inStock?: boolean;
  page?: number;
  limit?: number;
}
```

**Controller avec filtrage** :
```typescript
@Controller('products')
export class ProductsController {
  @Get()
  async findAll(@Query() query: QueryProductDto): Promise<ProductResponseDto[]> {
    // GET /products?category=electronics&minPrice=100&inStock=true&page=1&limit=20
    return this.productsService.findAll(query);
  }

  @Get(':id')
  async findOne(@Param('id') id: string): Promise<ProductResponseDto> {
    return this.productsService.findOne(+id);
  }
}
```

**Service avec transformation** :
```typescript
@Injectable()
export class ProductsService {
  async findAll(query: QueryProductDto): Promise<ProductResponseDto[]> {
    const products = await this.productRepository.find({
      where: {
        ...(query.category && { category: query.category }),
        ...(query.inStock !== undefined && { 
          stock: MoreThan(query.inStock ? 0 : -1) 
        }),
      },
      skip: (query.page - 1) * query.limit,
      take: query.limit,
    });

    // Transformer Entity → DTO (masquer costPrice)
    return products.map(p => this.toResponseDto(p));
  }

  private toResponseDto(product: Product): ProductResponseDto {
    return {
      id: product.id,
      name: product.name,
      description: product.description,
      price: product.price,
      inStock: product.stock > 0,
      category: product.category,
      imageUrl: product.imageUrl,
    };
  }
}
```

### Cas 3 : DTOs imbriqués

**Commande avec articles** :

```typescript
// src/orders/dto/create-order-item.dto.ts
export class CreateOrderItemDto {
  productId: number;
  quantity: number;
  // Le prix est calculé côté serveur, pas envoyé par le client
}

// src/orders/dto/create-order.dto.ts
export class CreateOrderDto {
  items: CreateOrderItemDto[];  // DTO imbriqué
  shippingAddress: string;
  paymentMethod: 'card' | 'paypal';
}

// Exemple de requête
{
  "items": [
    { "productId": 1, "quantity": 2 },
    { "productId": 5, "quantity": 1 }
  ],
  "shippingAddress": "123 Rue Example, Paris",
  "paymentMethod": "card"
}
```

---

## Helpers NestJS pour DTOs

### PartialType : Rendre tous les champs optionnels

```typescript
import { PartialType } from '@nestjs/mapped-types';

class UpdateProductDto extends PartialType(CreateProductDto) {}
// Équivalent à créer manuellement avec tous les champs optionnels
```

### PickType : Sélectionner certains champs

```typescript
import { PickType } from '@nestjs/mapped-types';

class UpdateEmailDto extends PickType(CreateUserDto, ['email'] as const) {}
// Seulement le champ email de CreateUserDto
```

### OmitType : Exclure certains champs

```typescript
import { OmitType } from '@nestjs/mapped-types';

class UpdateUserWithoutPasswordDto extends OmitType(CreateUserDto, ['password'] as const) {}
// Tous les champs SAUF password
```

### IntersectionType : Combiner plusieurs DTOs

```typescript
import { IntersectionType } from '@nestjs/mapped-types';

class PaginatedQueryDto {
  page: number;
  limit: number;
}

class QueryProductDto extends IntersectionType(
  FilterProductDto,
  PaginatedQueryDto
) {}
// Combine les champs des deux DTOs
```

---

## Erreurs Courantes & Comment les Éviter

### Erreur 1 : Utiliser l'entity comme DTO

```typescript
// ❌ MAUVAIS
@Post()
create(@Body() user: User) {  // Entity utilisée directement
  return this.usersService.create(user);
}

// ✅ BON
@Post()
create(@Body() createUserDto: CreateUserDto) {
  return this.usersService.create(createUserDto);
}
```

**Pourquoi ?** L'entity contient des champs qui ne doivent jamais être exposés (passwordHash, etc.).

### Erreur 2 : Même DTO pour create et update

```typescript
// ❌ MAUVAIS - Tous les champs requis pour update aussi
export class ProductDto {
  name: string;      // Obligatoire
  price: number;     // Obligatoire
  description: string; // Obligatoire
}

// ✅ BON - DTOs séparés
export class CreateProductDto {
  name: string;      // Obligatoire à la création
  price: number;
  description: string;
}

export class UpdateProductDto extends PartialType(CreateProductDto) {
  // Tous optionnels pour update
}
```

### Erreur 3 : Exposer des données sensibles

```typescript
// ❌ MAUVAIS
@Get(':id')
async findOne(@Param('id') id: string): Promise<User> {
  return this.usersService.findOne(+id);
  // Retourne TOUT, y compris passwordHash!
}

// ✅ BON
@Get(':id')
async findOne(@Param('id') id: string): Promise<UserResponseDto> {
  const user = await this.usersService.findOne(+id);
  return {
    id: user.id,
    email: user.email,
    // Pas de passwordHash
  };
}
```

### Erreur 4 : Oublier de typer les @Query

```typescript
// ❌ MAUVAIS - Pas de typage
@Get()
findAll(@Query() query: any) {  // any = pas de validation!
  return this.productsService.findAll(query);
}

// ✅ BON
@Get()
findAll(@Query() query: QueryProductDto) {
  return this.productsService.findAll(query);
}
```

### Erreur 5 : DTOs avec logique métier

```typescript
// ❌ MAUVAIS - Logique dans le DTO
export class CreateProductDto {
  name: string;
  price: number;

  calculateTax(): number {  // ❌ Mauvais!
    return this.price * 0.2;
  }
}

// ✅ BON - DTO simple, logique dans le service
export class CreateProductDto {
  name: string;
  price: number;
  // Pas de méthodes, juste des propriétés
}

// Logique dans le service
@Injectable()
export class ProductsService {
  calculateTax(price: number): number {
    return price * 0.2;
  }
}
```

---

## Exercices Pratiques

### Exercice 1 : DTOs pour une API de livres (Obligatoire)

**Objectif** : Créer les DTOs pour une API de bibliothèque.

**Entity** :
```typescript
class Book {
  id: number;
  title: string;
  author: string;
  isbn: string;
  price: number;
  costPrice: number;
  stock: number;
  publishedDate: Date;
  description: string;
  createdAt: Date;
}
```

**Tâches** :
1. Créez `CreateBookDto` (sans id, dates auto, pas de costPrice)
2. Créez `UpdateBookDto` avec PartialType
3. Créez `BookResponseDto` (sans costPrice)
4. Créez `QueryBookDto` (author?, minPrice?, maxPrice?, inStock?)

### Exercice 2 : DTOs imbriqués pour commande (Recommandé)

**Objectif** : Créer des DTOs pour un système de commande.

**Tâches** :
1. Créez `CreateOrderItemDto` (productId, quantity)
2. Créez `CreateOrderDto` (items: CreateOrderItemDto[], shippingAddress, notes?)
3. Testez avec le controller

**Exemple de requête attendu** :
```json
{
  "items": [
    { "productId": 1, "quantity": 2 },
    { "productId": 3, "quantity": 1 }
  ],
  "shippingAddress": "123 Rue Test",
  "notes": "Livraison urgente"
}
```

### Exercice 3 : PickType et OmitType (Facultatif)

**Objectif** : Utiliser les helpers NestJS.

**Tâches** :
1. Avec `CreateUserDto` (email, password, firstName, lastName)
2. Créez `ChangePasswordDto` avec PickType (seulement password)
3. Créez `UpdateProfileDto` avec OmitType (tous sauf password)

---

## Comportement Senior

### Bonnes pratiques

**1. Un DTO par cas d'usage**
```
dto/
  create-product.dto.ts
  update-product.dto.ts
  product-response.dto.ts
  query-product.dto.ts
```

**2. Nommage cohérent**
```typescript
// ✅ BON - Convention claire
CreateProductDto
UpdateProductDto
ProductResponseDto
QueryProductDto

// ❌ MAUVAIS - Nommage incohérent
ProductInput
ProductUpdate
ReturnProduct
ProductFilters
```

**3. Utiliser PartialType pour UpdateDTO**
```typescript
// ✅ BON - DRY
export class UpdateProductDto extends PartialType(CreateProductDto) {}

// ❌ MAUVAIS - Duplication
export class UpdateProductDto {
  name?: string;
  price?: number;
  // ... copie de CreateProductDto avec ? partout
}
```

**4. Séparer les concerns**
```typescript
// ✅ BON - DTO différent pour réponse publique
export class PublicUserDto {
  id: number;
  username: string;
  // Pas d'email, pas de données sensibles
}

export class PrivateUserDto {
  id: number;
  username: string;
  email: string;
  role: string;
}
```

**5. Documentation avec JSDoc**
```typescript
export class CreateProductDto {
  /**
   * Nom du produit
   * @example "Laptop Dell XPS 15"
   */
  name: string;

  /**
   * Prix en euros TTC
   * @example 1499.99
   */
  price: number;
}
```

### Patterns avancés

**Pattern 1 : Factory pour transformation**
```typescript
export class UserMapper {
  static toResponseDto(user: User): UserResponseDto {
    return {
      id: user.id,
      email: user.email,
      fullName: `${user.firstName} ${user.lastName}`,
      createdAt: user.createdAt,
    };
  }

  static toResponseDtoList(users: User[]): UserResponseDto[] {
    return users.map(u => this.toResponseDto(u));
  }
}

// Usage dans le service
async findAll(): Promise<UserResponseDto[]> {
  const users = await this.userRepository.find();
  return UserMapper.toResponseDtoList(users);
}
```

**Pattern 2 : DTO avec valeurs par défaut**
```typescript
export class QueryProductDto {
  category?: string;
  page: number = 1;
  limit: number = 10;
  sortBy: 'name' | 'price' = 'name';
  sortOrder: 'ASC' | 'DESC' = 'ASC';
}
```

---

## Résumé

### Qu'avez-vous appris ?

1. **DTOs** : Objets pour définir la structure des données en transit
2. **Séparation** : CreateDTO, UpdateDTO, ResponseDTO, QueryDTO
3. **Sécurité** : Ne jamais exposer de données sensibles
4. **Helpers** : PartialType, PickType, OmitType, IntersectionType
5. **Transformation** : Entity → DTO dans les services

### Quand utiliser les DTOs ?

**✅ Utilisez toujours des DTOs pour** :
- Endpoints publics (API REST)
- Validation de données entrantes
- Masquer des données sensibles
- Documenter l'API

**❌ Pas nécessaire pour** :
- Communication interne entre services (même app)
- Scripts internes
- Tests unitaires simples

### Prochaines étapes

- **[Validation avec class-validator](./nestjs-validation.md)** - Ajouter validation aux DTOs
- **[Transformation avec class-transformer](./nestjs-transformation.md)** - Transformer automatiquement
- **[Swagger : Documentation API](./nestjs-swagger.md)** - Documenter avec DTOs

### Points clés à retenir

> Les DTOs définissent la structure des données API. Créez des DTOs séparés pour create, update et response. Utilisez PartialType pour UpdateDTO. Ne jamais exposer l'entity directement. Les DTOs protègent votre API et documentent vos contrats.

---

## Ressources Externes

### Documentation officielle
- 📘 [NestJS DTOs](https://docs.nestjs.com/controllers#request-payloads) - Documentation officielle
- 📘 [Mapped Types](https://docs.nestjs.com/openapi/mapped-types) - Helpers NestJS

### Articles
- 📝 [DTO Pattern in NestJS](https://dev.to/nestjs/dto-pattern-in-nestjs-4f9h)
- 📝 [Entity vs DTO](https://medium.com/@dk.prdctn/entity-vs-dto-8f59b53e0b00)

### Vidéos (anglais)
- 🎥 [NestJS DTOs Explained](https://www.youtube.com/watch?v=VwWsvDdqW7o)
- 🎥 [DTO Validation in NestJS](https://www.youtube.com/watch?v=qr_J2KwKOTc)
