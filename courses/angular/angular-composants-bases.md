# Angular Composants : Bases et Création

## Introduction

### Objectifs du cours
Ce cours vous permettra de :
- Comprendre ce qu'est un composant Angular
- Créer des composants manuellement et avec Angular CLI
- Organiser vos composants dans une architecture modulaire
- Maîtriser la structure d'un composant (classe, template, styles)

### Ce que vous saurez faire après ce cours
- Créer des composants standalone (Angular 20+)
- Structurer votre application en composants réutilisables
- Séparer la logique de présentation
- Organiser vos fichiers de composants efficacement

### Scope de la notion
Les composants sont les briques fondamentales d'une application Angular. Chaque composant encapsule une partie de l'interface utilisateur avec sa logique, son template HTML et ses styles CSS. Dans une webapp métier, vous aurez des dizaines voire des centaines de composants (header, menu, tableau, formulaire, carte produit, etc.). Maîtriser les composants est essentiel pour construire des applications maintenables.

---

## Prérequis

Avant ce cours, vous devez connaître :
- [Angular : Installation et premier projet](./angular-installation-premier-projet.md)
- [TypeScript : Classes et interfaces](../type-vs-interface.md)
- [HTML et CSS de base](https://developer.mozilla.org/fr/docs/Learn/HTML)

---

## Définitions et Concepts Clés

### Composant
Un **composant** est une classe TypeScript décorée avec `@Component` qui contrôle une portion de l'écran. Il combine :
- **Template** (HTML) : Ce qui est affiché
- **Classe** (TypeScript) : La logique et les données
- **Styles** (CSS/SCSS) : L'apparence

**Analogie** : Un composant est comme une pièce LEGO. Chaque pièce a une forme (template), une couleur (styles), et peut avoir des fonctionnalités spéciales (classe). Vous assemblez ces pièces pour construire votre application.

### Composant Standalone (Angular 20+)
Un **composant standalone** est un composant qui n'a pas besoin d'être déclaré dans un NgModule. Il peut importer directement ses dépendances.

**Avant (avec NgModule)** :
```typescript
// Fallait déclarer le composant dans un module
@NgModule({
  declarations: [MyComponent],
  imports: [CommonModule]
})
export class MyModule {}
```

**Maintenant (standalone - Angular 20+)** :
```typescript
// Le composant importe directement ce dont il a besoin
@Component({
  standalone: true,
  imports: [CommonModule]
})
export class MyComponent {}
```

**Avantage** : Plus simple, moins de boilerplate, meilleure tree-shaking.

### Selector
Le **selector** est le nom de la balise HTML du composant. C'est comme ça qu'on l'utilise dans un template.

```typescript
@Component({
  selector: 'app-product-card',  // ← Selector
  ...
})
export class ProductCardComponent {}
```

**Usage dans un template** :
```html
<app-product-card></app-product-card>
```

**Convention de nommage** : `app-` ou `prefix-` suivi du nom du composant en kebab-case.

### Encapsulation de styles
Angular encapsule les styles par défaut, ce qui signifie que les styles d'un composant n'affectent QUE ce composant.

**Exemple** :
```css
/* product-card.component.css */
h2 {
  color: blue;  /* Seulement les h2 de ProductCardComponent seront bleus */
}
```

**Analogie** : C'est comme chaque pièce d'une maison a sa propre décoration. La couleur du salon n'affecte pas la chambre.

---

## Anatomie d'un Composant

### Structure des fichiers

Un composant Angular 20+ génère typiquement 3 fichiers :

```
product-card/
  product-card.component.ts       # Logique TypeScript
  product-card.component.html     # Template HTML
  product-card.component.css      # Styles CSS
  product-card.component.spec.ts  # Tests (optionnel)
```

### Le fichier TypeScript (.ts)

**Structure minimale** :

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-product-card',
  standalone: true,
  imports: [],
  templateUrl: './product-card.component.html',
  styleUrl: './product-card.component.css'
})
export class ProductCardComponent {
  // Propriétés (données)
  productName: string = 'Laptop Dell';
  price: number = 1500;
  inStock: boolean = true;

  // Méthodes (logique)
  addToCart(): void {
    console.log(`${this.productName} ajouté au panier`);
  }
}
```

**Décomposition du décorateur `@Component`** :
- `selector` : Nom de la balise HTML
- `standalone: true` : Composant autonome (Angular 20+)
- `imports` : Dépendances du composant
- `templateUrl` : Chemin vers le fichier HTML
- `styleUrl` : Chemin vers le fichier CSS

### Le fichier HTML (.html)

```html
<div class="product-card">
  <h2>{{ productName }}</h2>
  <p class="price">{{ price }} €</p>
  
  <div class="stock" *ngIf="inStock">
    ✅ En stock
  </div>
  
  <button (click)="addToCart()">
    Ajouter au panier
  </button>
</div>
```

**Éléments clés** :
- `{{ productName }}` : Interpolation - affiche la propriété
- `*ngIf="inStock"` : Directive structurelle - affichage conditionnel
- `(click)="addToCart()"` : Event binding - gère le clic

### Le fichier CSS (.css)

```css
.product-card {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 20px;
  margin: 10px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.product-card h2 {
  color: #333;
  font-size: 1.5em;
  margin-bottom: 10px;
}

.price {
  font-size: 1.2em;
  font-weight: bold;
  color: #2c3e50;
}

.stock {
  color: #27ae60;
  margin: 10px 0;
}

button {
  background-color: #3498db;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 4px;
  cursor: pointer;
  font-size: 1em;
}

button:hover {
  background-color: #2980b9;
}
```

---

## Créer un Composant

### Méthode 1 : Avec Angular CLI (Recommandé)

**Commande de base** :
```bash
ng generate component product-card

# Version courte
ng g c product-card
```

**Cela génère** :
```
src/app/product-card/
  product-card.component.ts
  product-card.component.html
  product-card.component.css
  product-card.component.spec.ts  # Tests
```

**Options utiles** :

```bash
# Créer sans fichier de test
ng g c product-card --skip-tests

# Créer dans un dossier spécifique
ng g c components/shared/product-card

# Créer avec inline template (pas de fichier .html séparé)
ng g c product-card --inline-template

# Créer avec inline style (pas de fichier .css séparé)
ng g c product-card --inline-style

# Créer avec les deux inline
ng g c product-card --inline-template --inline-style

# Flat : ne pas créer de dossier
ng g c product-card --flat
```

### Méthode 2 : Manuellement (pour comprendre)

**Étape 1** : Créer le fichier TypeScript

```typescript
// src/app/greeting/greeting.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-greeting',
  standalone: true,
  template: `
    <div class="greeting">
      <h1>{{ message }}</h1>
      <button (click)="changeMessage()">Changer</button>
    </div>
  `,
  styles: [`
    .greeting {
      text-align: center;
      padding: 20px;
    }
    h1 {
      color: #3498db;
    }
  `]
})
export class GreetingComponent {
  message: string = 'Bonjour!';

  changeMessage(): void {
    this.message = 'Hello!';
  }
}
```

**Étape 2** : Utiliser dans un autre composant

```typescript
// app.component.ts
import { Component } from '@angular/core';
import { GreetingComponent } from './greeting/greeting.component';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [GreetingComponent],  // ← Importer le composant
  template: `
    <h1>Mon Application</h1>
    <app-greeting></app-greeting>
  `
})
export class AppComponent {}
```

---

## Types de Composants

### Composant de Présentation (Presentational/Dumb)

**Rôle** : Afficher des données, pas de logique métier.

```typescript
import { Component, Input, Output, EventEmitter } from '@angular/core';

interface Product {
  id: number;
  name: string;
  price: number;
}

@Component({
  selector: 'app-product-item',
  standalone: true,
  template: `
    <div class="product">
      <h3>{{ product.name }}</h3>
      <p>{{ product.price }} €</p>
      <button (click)="onAddClick()">Ajouter</button>
    </div>
  `,
  styles: [`
    .product {
      border: 1px solid #ccc;
      padding: 15px;
      margin: 10px 0;
    }
  `]
})
export class ProductItemComponent {
  @Input({ required: true }) product: Product;  // Données reçues du parent (required dès Angular 17+)
  @Output() addToCart = new EventEmitter<Product>();  // Événement vers le parent

  onAddClick(): void {
    this.addToCart.emit(this.product);
  }
}
```

**Caractéristiques** :
- ✅ Reçoit des données via `@Input()`
- ✅ Émet des événements via `@Output()`
- ✅ Pas d'appels HTTP
- ✅ Pas de services injectés
- ✅ Réutilisable facilement

### Composant Container (Smart)

**Rôle** : Contient la logique métier, appelle les services, gère l'état.

```typescript
import { Component, OnInit } from '@angular/core';
import { CommonModule } from '@angular/common';
import { ProductItemComponent } from './product-item/product-item.component';

interface Product {
  id: number;
  name: string;
  price: number;
}

@Component({
  selector: 'app-product-list',
  standalone: true,
  imports: [CommonModule, ProductItemComponent],
  template: `
    <div class="product-list">
      <h2>Nos Produits</h2>
      
      <div *ngIf="isLoading">Chargement...</div>
      
      <app-product-item
        *ngFor="let product of products"
        [product]="product"
        (addToCart)="handleAddToCart($event)"
      ></app-product-item>
      
      <div class="cart-info">
        Panier: {{ cartCount }} articles
      </div>
    </div>
  `,
  styles: [`
    .product-list {
      max-width: 800px;
      margin: 0 auto;
    }
    .cart-info {
      position: fixed;
      bottom: 20px;
      right: 20px;
      background: #3498db;
      color: white;
      padding: 15px;
      border-radius: 8px;
    }
  `]
})
export class ProductListComponent implements OnInit {
  products: Product[] = [];
  isLoading = false;
  cartCount = 0;

  ngOnInit(): void {
    this.loadProducts();
  }

  loadProducts(): void {
    this.isLoading = true;
    
    // Simulation d'appel API
    setTimeout(() => {
      this.products = [
        { id: 1, name: 'Laptop', price: 1500 },
        { id: 2, name: 'Souris', price: 25 },
        { id: 3, name: 'Clavier', price: 75 }
      ];
      this.isLoading = false;
    }, 1000);
  }

  handleAddToCart(product: Product): void {
    console.log('Ajout au panier:', product);
    this.cartCount++;
  }
}
```

**Caractéristiques** :
- ✅ Gère l'état (loading, data, errors)
- ✅ Appelle les services
- ✅ Contient la logique métier
- ✅ Orchestre les composants de présentation

---

## Organisation des Composants

### Structure recommandée

```
src/app/
  components/           # Composants réutilisables
    shared/             # Composants partagés entre features
      button/
        button.component.ts
        button.component.html
        button.component.css
      card/
        card.component.ts
        ...
    
  features/             # Composants par fonctionnalité métier
    products/
      product-list/
        product-list.component.ts
        ...
      product-detail/
        product-detail.component.ts
        ...
      product-form/
        product-form.component.ts
        ...
    
    orders/
      order-list/
      order-detail/
    
  layout/               # Composants de mise en page
    header/
      header.component.ts
      ...
    footer/
      footer.component.ts
      ...
    sidebar/
      sidebar.component.ts
      ...
```

### Exemple complet : Feature Products

**1. Composant de liste (container)** :

```typescript
// features/products/product-list/product-list.component.ts
import { Component, OnInit } from '@angular/core';
import { CommonModule } from '@angular/common';
import { ProductCardComponent } from '../product-card/product-card.component';

@Component({
  selector: 'app-product-list',
  standalone: true,
  imports: [CommonModule, ProductCardComponent],
  templateUrl: './product-list.component.html',
  styleUrl: './product-list.component.css'
})
export class ProductListComponent implements OnInit {
  products = [
    { id: 1, name: 'Laptop', price: 1500, image: 'laptop.jpg' },
    { id: 2, name: 'Souris', price: 25, image: 'mouse.jpg' },
    { id: 3, name: 'Clavier', price: 75, image: 'keyboard.jpg' }
  ];

  ngOnInit(): void {
    console.log('Composant de liste initialisé');
  }

  onProductClick(productId: number): void {
    console.log('Produit cliqué:', productId);
    // Navigation vers la page détail
  }
}
```

```html
<!-- features/products/product-list/product-list.component.html -->
<div class="products-container">
  <h1>Nos Produits</h1>
  
  <div class="products-grid">
    <app-product-card
      *ngFor="let product of products"
      [product]="product"
      (productClick)="onProductClick($event)"
    ></app-product-card>
  </div>
</div>
```

**2. Composant carte produit (présentation)** :

```typescript
// features/products/product-card/product-card.component.ts
import { Component, Input, Output, EventEmitter } from '@angular/core';

interface Product {
  id: number;
  name: string;
  price: number;
  image: string;
}

@Component({
  selector: 'app-product-card',
  standalone: true,
  templateUrl: './product-card.component.html',
  styleUrl: './product-card.component.css'
})
export class ProductCardComponent {
  @Input({ required: true }) product!: Product;
  @Output() productClick = new EventEmitter<number>();

  onClick(): void {
    this.productClick.emit(this.product.id);
  }
}
```

```html
<!-- features/products/product-card/product-card.component.html -->
<div class="card" (click)="onClick()">
  <img [src]="product.image" [alt]="product.name">
  <h3>{{ product.name }}</h3>
  <p class="price">{{ product.price }} €</p>
  <button>Voir détails</button>
</div>
```

---

## Lifecycle Hooks Essentiels

### ngOnInit
S'exécute après la création du composant.

```typescript
import { Component, OnInit } from '@angular/core';

export class MyComponent implements OnInit {
  data: any[] = [];

  ngOnInit(): void {
    // Charger les données au démarrage
    this.loadData();
  }

  loadData(): void {
    console.log('Chargement des données...');
  }
}
```

**Usage** : Initialisation, chargement de données, souscriptions.

### ngOnDestroy
S'exécute avant la destruction du composant.

```typescript
import { Component, OnDestroy } from '@angular/core';

export class MyComponent implements OnDestroy {
  ngOnDestroy(): void {
    // Nettoyage : unsubscribe, clear timers, etc.
    console.log('Composant détruit');
  }
}
```

**Usage** : Nettoyage, unsubscribe des Observables.

---

## Erreurs Courantes & Comment les Éviter

### Erreur 1 : Oublier d'importer le composant

```typescript
// ❌ MAUVAIS
@Component({
  selector: 'app-parent',
  standalone: true,
  imports: [],  // ProductCardComponent manquant!
  template: `<app-product-card></app-product-card>`
})
```

**Erreur** : `'app-product-card' is not a known element`

**Solution** :
```typescript
// ✅ BON
@Component({
  selector: 'app-parent',
  standalone: true,
  imports: [ProductCardComponent],  // ✅ Importé
  template: `<app-product-card></app-product-card>`
})
```

### Erreur 2 : Mauvais selector

```typescript
// ❌ MAUVAIS
@Component({
  selector: 'ProductCard',  // PascalCase ❌
  ...
})
```

**Solution** :
```typescript
// ✅ BON
@Component({
  selector: 'app-product-card',  // kebab-case ✅
  ...
})
```

### Erreur 3 : Propriétés non initialisées

```typescript
// ❌ MAUVAIS
export class MyComponent {
  title: string;  // ⚠️ Pas initialisé
}
```

**Solution** :
```typescript
// ✅ BON - Option 1
export class MyComponent {
  title: string = 'Mon titre';
}

// ✅ BON - Option 2
export class MyComponent {
  title!: string;  // Utiliser ! si initialisé plus tard
}
```

### Erreur 4 : Modifier des @Input dans le composant enfant

```typescript
// ❌ MAUVAIS
export class ChildComponent {
  @Input() data!: any;

  modifyData(): void {
    this.data.name = 'Nouveau nom';  // ⚠️ Mutation de l'input
  }
}

// ✅ BON - Émettre un événement
export class ChildComponent {
  @Input() data!: any;
  @Output() dataChange = new EventEmitter<any>();

  requestChange(): void {
    this.dataChange.emit({ ...this.data, name: 'Nouveau nom' });
  }
}
```

---

## Exercices Pratiques

### Exercice 1 : Créer un composant de carte utilisateur (Obligatoire)

**Objectif** : Créer un composant réutilisable.

**Tâches** :
1. Générez `user-card` avec Angular CLI
2. Ajoutez des propriétés : name, email, role
3. Créez un template avec ces infos
4. Stylisez la carte (border, padding, etc.)
5. Utilisez le composant dans AppComponent

**Validation** : La carte s'affiche correctement.

### Exercice 2 : Liste de produits avec filtrage (Recommandé)

**Objectif** : Container + composants de présentation.

**Tâches** :
1. Créez `ProductListComponent` (container)
2. Créez `ProductItemComponent` (présentation)
3. Dans ProductList, affichez une liste de 5 produits
4. Passez chaque produit à ProductItem via `@Input()`
5. Ajoutez un bouton "Détails" qui émet un événement

### Exercice 3 : Composant avec toggle (Facultatif)

**Objectif** : Gérer un état local.

**Tâches** :
1. Créez `ToggleComponent`
2. Propriété `isOn: boolean = false`
3. Bouton qui toggle l'état
4. Affichage conditionnel : "ON" ou "OFF"
5. Changement de couleur selon l'état

---

## Comportement Senior

### Bonnes pratiques

**1. Un composant = Une responsabilité**
```typescript
// ✅ BON - Composant focalisé
@Component({
  selector: 'app-user-avatar',
  template: `<img [src]="avatarUrl" [alt]="userName">`
})
export class UserAvatarComponent {
  @Input() avatarUrl!: string;
  @Input() userName!: string;
}

// ❌ MAUVAIS - Trop de responsabilités
@Component({
  selector: 'app-user-everything',
  template: `<!-- Avatar + Profile + Settings + Orders + ... -->`
})
```

**2. Nommage cohérent**
```
✅ product-card.component.ts
✅ user-list.component.ts
✅ order-detail.component.ts

❌ ProductCard.ts
❌ list.ts
❌ detail-order.component.ts
```

**3. Composants petits et réutilisables**
```typescript
// ✅ BON - Petits composants réutilisables
<app-button [text]="'Sauvegarder'" (click)="save()"></app-button>
<app-button [text]="'Annuler'" [type]="'secondary'" (click)="cancel()"></app-button>

// ❌ MAUVAIS - Bouton dupliqué partout
<button class="primary" (click)="save()">Sauvegarder</button>
<button class="secondary" (click)="cancel()">Annuler</button>
```

**4. Documentation avec JSDoc**
```typescript
/**
 * Composant d'affichage de carte produit
 * @example
 * <app-product-card [product]="myProduct" (click)="handleClick()"></app-product-card>
 */
@Component({
  selector: 'app-product-card',
  ...
})
export class ProductCardComponent {
  /** Données du produit à afficher */
  @Input() product!: Product;
  
  /** Émis quand l'utilisateur clique sur la carte */
  @Output() cardClick = new EventEmitter<Product>();
}
```

---

## Résumé

### Qu'avez-vous appris ?

1. **Composants** : Briques de construction d'Angular
2. **Structure** : Classe + Template + Styles
3. **Standalone** : Composants autonomes dans Angular 20+
4. **Types** : Présentation vs Container
5. **Organisation** : Structure de dossiers claire

### Quand créer un nouveau composant ?

**✅ Créez un composant quand** :
- Réutilisation (utilisé 2+ fois)
- Complexité (> 100 lignes de template)
- Responsabilité distincte
- Testabilité

**❌ Pas besoin si** :
- Utilisé une seule fois et simple
- Juste quelques lignes HTML
- Pas de logique spécifique

### Prochaines étapes

- **[Templates : Syntaxe et data binding](./angular-templates-data-binding.md)** - Maîtriser les templates
- **[Communication parent-enfant : @Input](./angular-input.md)** - Passer des données
- **[Communication enfant-parent : @Output](./angular-output.md)** - Émettre des événements

### Points clés à retenir

> Les composants encapsulent template, logique et styles. Angular 20+ utilise des composants standalone. Séparez les composants de présentation (dumb) des containers (smart). Organisez vos composants par fonctionnalité. Un composant = une responsabilité.

---

## Ressources Externes

### Documentation officielle
- 📘 [Angular Components](https://angular.dev/guide/components) - Guide officiel
- 📘 [Component API](https://angular.dev/api/core/Component) - Référence API

### Vidéos (français)
- 🎥 [Les Composants Angular - Grafikart](https://www.youtube.com/watch?v=bT5jF7Z0wWw)

### Vidéos (anglais)
- 🎥 [Angular Components Explained](https://www.youtube.com/watch?v=23o0evRtrFI)
- 🎥 [Component Architecture](https://www.youtube.com/watch?v=8iBB48RpF6U)

### Articles
- 📝 [Component Best Practices](https://angular.io/guide/styleguide#components)
- 📝 [Smart vs Presentational Components](https://blog.angular-university.io/angular-2-smart-components-vs-presentation-components-whats-the-difference-when-to-use-each-and-why/)
