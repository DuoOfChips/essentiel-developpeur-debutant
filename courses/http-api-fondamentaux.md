# HTTP et APIs : Fondamentaux des Communications Web

## 1. Contexte et définitions des concepts clés

### 1.1 Qu'est-ce que HTTP ?

**HTTP** (HyperText Transfer Protocol) est le protocole de communication qui permet aux navigateurs et serveurs de s'échanger des données sur le web.

**Analogie :** HTTP est comme le système postal :
- **Client** (navigateur) = Vous envoyez une lettre (requête)
- **Serveur** = Bureau de poste qui répond
- **HTTP** = Les règles du service postal

### 1.2 Qu'est-ce qu'une API ?

**API** (Application Programming Interface) est une interface qui permet à deux applications de communiquer.

**API REST** : Architecture la plus courante pour les APIs web
- **RE**presentational **S**tate **T**ransfer
- Utilise HTTP pour transférer des données
- Format JSON généralement

### 1.3 Définitions importantes

- **Client** : Application qui demande (navigateur, app mobile)
- **Serveur** : Application qui répond (backend)
- **Requête** (Request) : Demande envoyée par le client
- **Réponse** (Response) : Réponse du serveur
- **Endpoint** : URL d'une ressource (`/api/users`)
- **Méthode HTTP** : Type d'action (GET, POST, PUT, DELETE)
- **Status Code** : Code de réponse (200, 404, 500)
- **Headers** : Métadonnées de la requête/réponse
- **Body** : Données envoyées/reçues
- **JSON** : Format d'échange de données

## 2. Quand et pourquoi utiliser HTTP et les APIs

### 2.1 Cas d'usage quotidiens

**Toute application web utilise HTTP :**
- Charger une page web
- Se connecter à un site
- Envoyer un formulaire
- Récupérer des données
- Uploader un fichier

**APIs partout :**
- Application météo → API météo
- Carte interactive → API Google Maps
- Paiement en ligne → API Stripe
- Authentification → API Auth
- Base de données → API backend

### 2.2 Architecture Client-Serveur

```
┌─────────────┐                    ┌─────────────┐
│   CLIENT    │                    │   SERVEUR   │
│  (Angular)  │  ──── HTTP ───→    │  (NestJS)   │
│             │                    │             │
│             │  ←─── JSON ────    │             │
└─────────────┘                    └─────────────┘
```

**Avantages :**
- ✅ Séparation frontend/backend
- ✅ Plusieurs clients (web, mobile, desktop)
- ✅ Évolutivité indépendante
- ✅ Équipes séparées

## 3. Comment cela se passe du point de vue matériel

### 3.1 Cycle de vie d'une requête HTTP

```
1. CLIENT génère requête
   ↓
2. RÉSEAU transmet (Internet)
   ↓
3. SERVEUR reçoit et traite
   ↓
4. SERVEUR génère réponse
   ↓
5. RÉSEAU transmet retour
   ↓
6. CLIENT reçoit et affiche
```

**Exemple concret :**
```
1. Utilisateur clique "Se connecter"
2. Angular envoie POST /api/auth/login
3. Requête voyage via Internet (TCP/IP)
4. NestJS reçoit, valide credentials
5. NestJS génère JWT token
6. Réponse voyage retour
7. Angular reçoit token, stocke, redirige
```

### 3.2 Structure d'une requête HTTP

```
POST /api/users HTTP/1.1
Host: api.example.com
Content-Type: application/json
Authorization: Bearer eyJhbGc...

{
  "name": "Alice",
  "email": "alice@example.com"
}
```

**Composants :**
- **Méthode** : POST
- **Path** : /api/users
- **Headers** : Content-Type, Authorization
- **Body** : Données JSON

### 3.3 Structure d'une réponse HTTP

```
HTTP/1.1 201 Created
Content-Type: application/json
Location: /api/users/123

{
  "id": 123,
  "name": "Alice",
  "email": "alice@example.com",
  "createdAt": "2024-11-26T10:00:00Z"
}
```

**Composants :**
- **Status** : 201 Created
- **Headers** : Content-Type, Location
- **Body** : Données créées

## 4. Méthodes HTTP (Verbes)

### 4.1 Les 5 méthodes principales

| Méthode | Action | Idempotent* | Exemple |
|---------|--------|-------------|---------|
| **GET** | Lire | ✅ | Récupérer utilisateurs |
| **POST** | Créer | ❌ | Créer utilisateur |
| **PUT** | Remplacer | ✅ | Remplacer utilisateur complet |
| **PATCH** | Modifier | ❌ | Modifier email seulement |
| **DELETE** | Supprimer | ✅ | Supprimer utilisateur |

*Idempotent = Appeler plusieurs fois = même résultat

### 4.2 GET : Récupérer des données

**Caractéristiques :**
- Ne modifie JAMAIS les données
- Pas de body dans la requête
- Paramètres dans l'URL

**Exemples TypeScript :**
```ts
// Récupérer tous les utilisateurs
const users = await fetch('/api/users');

// Récupérer un utilisateur spécifique
const user = await fetch('/api/users/123');

// Avec paramètres de recherche
const results = await fetch('/api/products?category=electronics&minPrice=100');
```

### 4.3 POST : Créer une ressource

**Caractéristiques :**
- Crée une nouvelle ressource
- Body contient les données
- Retourne souvent 201 Created

**Exemple :**
```ts
const response = await fetch('/api/users', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    name: 'Alice',
    email: 'alice@example.com'
  })
});

const newUser = await response.json();
// { id: 123, name: "Alice", ... }
```

### 4.4 PUT : Remplacer complètement

**Exemple :**
```ts
await fetch('/api/users/123', {
  method: 'PUT',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    name: 'Alice Updated',
    email: 'alice.new@example.com',
    age: 30
    // Toutes les propriétés doivent être fournies
  })
});
```

### 4.5 PATCH : Modification partielle

**Exemple :**
```ts
await fetch('/api/users/123', {
  method: 'PATCH',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    email: 'alice.new@example.com'
    // Seulement les champs à modifier
  })
});
```

### 4.6 DELETE : Supprimer

**Exemple :**
```ts
await fetch('/api/users/123', {
  method: 'DELETE'
});
// Généralement retourne 204 No Content
```

## 5. Status Codes (Codes de statut)

### 5.1 Catégories

| Plage | Signification | Exemples |
|-------|---------------|----------|
| **1xx** | Information | 100 Continue |
| **2xx** | Succès | 200 OK, 201 Created |
| **3xx** | Redirection | 301 Moved, 304 Not Modified |
| **4xx** | Erreur client | 400 Bad Request, 404 Not Found |
| **5xx** | Erreur serveur | 500 Internal Error |

### 5.2 Codes essentiels à connaître

**Succès (2xx) :**
- **200 OK** : Requête réussie
- **201 Created** : Ressource créée
- **204 No Content** : Succès sans contenu (DELETE)

**Erreurs client (4xx) :**
- **400 Bad Request** : Données invalides
- **401 Unauthorized** : Pas authentifié
- **403 Forbidden** : Pas autorisé
- **404 Not Found** : Ressource inexistante
- **422 Unprocessable Entity** : Validation échouée

**Erreurs serveur (5xx) :**
- **500 Internal Server Error** : Erreur serveur générique
- **502 Bad Gateway** : Proxy/Gateway erreur
- **503 Service Unavailable** : Service temporairement indisponible

### 5.3 Gestion des status codes

**Client TypeScript :**
```ts
async function getUser(id: number): Promise<User> {
  const response = await fetch(`/api/users/${id}`);
  
  if (response.status === 404) {
    throw new Error('User not found');
  }
  
  if (!response.ok) { // ok = status 200-299
    throw new Error(`HTTP ${response.status}: ${response.statusText}`);
  }
  
  return response.json();
}
```

**Serveur NestJS :**
```ts
@Get(':id')
async getUser(@Param('id') id: number): Promise<User> {
  const user = await this.userService.findById(id);
  
  if (!user) {
    throw new NotFoundException('User not found'); // 404
  }
  
  return user; // 200
}

@Post()
@HttpCode(201) // Explicite
async createUser(@Body() dto: CreateUserDto): Promise<User> {
  return this.userService.create(dto);
}
```

## 6. JSON : Format d'échange

### 6.1 Qu'est-ce que JSON ?

**JSON** (JavaScript Object Notation) : Format texte pour échanger des données

**Exemple :**
```json
{
  "id": 123,
  "name": "Alice",
  "email": "alice@example.com",
  "isActive": true,
  "roles": ["user", "admin"],
  "profile": {
    "age": 30,
    "city": "Paris"
  }
}
```

### 6.2 Conversion TypeScript ↔ JSON

**Objet → JSON (Sérialisation) :**
```ts
const user = {
  id: 123,
  name: "Alice"
};

const json = JSON.stringify(user);
// '{"id":123,"name":"Alice"}'
```

**JSON → Objet (Désérialisation) :**
```ts
const json = '{"id":123,"name":"Alice"}';
const user = JSON.parse(json);
// { id: 123, name: "Alice" }
```

## 7. Exemples concrets d'API

### 7.1 API CRUD complète

**Interface TypeScript :**
```ts
interface User {
  id: number;
  name: string;
  email: string;
}

interface CreateUserDto {
  name: string;
  email: string;
}
```

**Service client (Angular) :**
```ts
@Injectable()
export class UserService {
  private apiUrl = 'https://api.example.com/users';
  
  // GET /users - Liste
  async getUsers(): Promise<User[]> {
    const response = await fetch(this.apiUrl);
    return response.json();
  }
  
  // GET /users/:id - Un utilisateur
  async getUser(id: number): Promise<User> {
    const response = await fetch(`${this.apiUrl}/${id}`);
    
    if (!response.ok) {
      throw new Error('User not found');
    }
    
    return response.json();
  }
  
  // POST /users - Créer
  async createUser(dto: CreateUserDto): Promise<User> {
    const response = await fetch(this.apiUrl, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(dto)
    });
    
    return response.json();
  }
  
  // PATCH /users/:id - Modifier
  async updateUser(id: number, updates: Partial<User>): Promise<User> {
    const response = await fetch(`${this.apiUrl}/${id}`, {
      method: 'PATCH',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(updates)
    });
    
    return response.json();
  }
  
  // DELETE /users/:id - Supprimer
  async deleteUser(id: number): Promise<void> {
    await fetch(`${this.apiUrl}/${id}`, {
      method: 'DELETE'
    });
  }
}
```

**Controller serveur (NestJS) :**
```ts
@Controller('users')
export class UserController {
  constructor(private userService: UserService) {}
  
  @Get()
  async findAll(): Promise<User[]> {
    return this.userService.findAll();
  }
  
  @Get(':id')
  async findOne(@Param('id') id: number): Promise<User> {
    const user = await this.userService.findById(id);
    if (!user) {
      throw new NotFoundException();
    }
    return user;
  }
  
  @Post()
  @HttpCode(201)
  async create(@Body() dto: CreateUserDto): Promise<User> {
    return this.userService.create(dto);
  }
  
  @Patch(':id')
  async update(
    @Param('id') id: number,
    @Body() updates: Partial<User>
  ): Promise<User> {
    return this.userService.update(id, updates);
  }
  
  @Delete(':id')
  @HttpCode(204)
  async delete(@Param('id') id: number): Promise<void> {
    await this.userService.delete(id);
  }
}
```

### 7.2 Authentification avec JWT

**Login :**
```ts
// Client
async login(email: string, password: string): Promise<string> {
  const response = await fetch('/api/auth/login', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ email, password })
  });
  
  const data = await response.json();
  return data.token; // JWT
}

// Serveur
@Post('login')
async login(@Body() dto: LoginDto): Promise<{ token: string }> {
  const user = await this.authService.validateUser(dto.email, dto.password);
  
  if (!user) {
    throw new UnauthorizedException('Invalid credentials');
  }
  
  const token = this.authService.generateToken(user);
  return { token };
}
```

**Requêtes authentifiées :**
```ts
// Client - Ajouter token dans header
async getProfile(token: string): Promise<User> {
  const response = await fetch('/api/users/me', {
    headers: {
      'Authorization': `Bearer ${token}`
    }
  });
  
  return response.json();
}

// Serveur - Vérifier token
@Get('me')
@UseGuards(AuthGuard)
async getProfile(@Request() req): Promise<User> {
  return req.user; // Extrait du token
}
```

## 8. Erreurs et pièges à éviter

### 8.1 Erreurs fréquentes

| Erreur | Impact | Solution |
|--------|--------|----------|
| **Oublier await** | Promise non résolue | Toujours await fetch() |
| **Pas de gestion erreur** | Crash silencieux | try/catch ou .catch() |
| **Mauvaise méthode HTTP** | Sémantique incorrecte | GET=lire, POST=créer, etc. |
| **CORS non configuré** | Requête bloquée | Configurer CORS serveur |
| **Token expiré** | 401 Unauthorized | Refresh token |
| **Pas de Content-Type** | Serveur refuse | Toujours headers |

### 8.2 Pièges courants

**Piège 1 : Oublier de parser JSON**
```ts
// ❌ Erreur
const response = await fetch('/api/users');
console.log(response); // Object Response, pas les données

// ✅ Correct
const response = await fetch('/api/users');
const users = await response.json();
console.log(users); // Données
```

**Piège 2 : Ne pas vérifier status**
```ts
// ❌ Erreur - 404 non géré
const response = await fetch('/api/users/999');
const user = await response.json(); // Plante

// ✅ Correct
const response = await fetch('/api/users/999');
if (!response.ok) {
  throw new Error(`HTTP ${response.status}`);
}
const user = await response.json();
```

**Piège 3 : Modifier avec GET**
```ts
// ❌ Mauvais - GET ne doit pas modifier
fetch('/api/users/delete?id=123'); // GET

// ✅ Bon - DELETE pour supprimer
fetch('/api/users/123', { method: 'DELETE' });
```

## 9. Résumé de l'essentiel

### Points clés

1. **HTTP = Communication web**
   - Client envoie requête
   - Serveur renvoie réponse
   - Format JSON généralement

2. **Méthodes HTTP**
   - GET : Lire
   - POST : Créer
   - PUT/PATCH : Modifier
   - DELETE : Supprimer

3. **Status codes**
   - 2xx : Succès
   - 4xx : Erreur client
   - 5xx : Erreur serveur

4. **APIs REST**
   - Endpoints clairs (`/api/users`)
   - Méthodes HTTP sémantiques
   - JSON pour données
   - Stateless

### Checklist API call

- [ ] Bonne méthode HTTP ?
- [ ] URL correcte ?
- [ ] Headers nécessaires ?
- [ ] Body si POST/PUT/PATCH ?
- [ ] Gestion d'erreur ?
- [ ] await ou .then() ?
- [ ] Parse JSON ?

---

**En une phrase :**

> HTTP est le protocole de communication web qui permet aux clients (navigateurs, apps) d'échanger des données avec les serveurs via des requêtes typées (GET, POST, etc.) retournant des status codes et du JSON, formant la base des APIs REST modernes.
