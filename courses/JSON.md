# Le format JSON

## Qu’est-ce que le JSON ?

JSON signifie JavaScript Object Notation.
C’est un format de données léger, lisible par les humains, facilement manipulable par les machines, utilisé partout dans les systèmes informatiques modernes.

Il sert principalement à échanger des données entre deux systèmes (navigateurs, serveurs, API, applications mobiles, objets connectés, etc.).

## Petit point historique

1999–2001 : Douglas Crockford (développeur JavaScript) formalise le format JSON à partir des littéraux d’objets JavaScript.

2006 : Le format devient un standard via RFC 4627.

2013 : Standardisation renforcée via RFC 7159.

2017 : JSON devient un standard international : ECMA-404.

Aujourd’hui, JSON a remplacé SML dans la majorité des usages web.

## Pourquoi JSON est-il incontournable aujourd’hui ?

JSON est utilisé dans :

Les API web (REST, GraphQL, etc.)

Les applications mobiles (Android / Apple iOS)

Les applications desktop modernes

Les bases de données NoSQL (MongoDB, Firebase, DynamoDB, Elasticsearch)

Les microservices et systèmes distribués

Les IoT / objets connectés

Les configurations d’outils et frameworks (package.json, tsconfig.json, composer.json…)

## Structure d’un JSON

JSON repose sur quelques concepts simples.

| Type      | Exemple               | Notes                                     |
| --------- | --------------------- | ----------------------------------------- |
| `string`  | `"Bonjour"`           | Doit toujours être entre guillemets `" "` |
| `number`  | `42`, `3.14`          | Pas de distingue entre int/float          |
| `boolean` | `true` / `false`      | Tout en minuscule                         |
| `null`    | `null`                | Valeur vide                               |
| `object`  | `{ "cle": "valeur" }` | Collection de paires clé/valeur           |
| `array`   | `[1, 2, 3]`           | Liste ordonnée                            |

```json
{
  "id": 123,
  "nom": "Vincent",
  "estPremium": true,
  "compétences": ["Angular", "ASP.NET", "Architecture"],
  "profil": {
    "age": 29,
    "ville": "Paris"
  }
}
```

## Cas d’usage réels et pratiques

- Dans une API Web (REST)
- Dans les fichiers de configuration
- Dans certaines bases de données (ex: MongoDB)

```json
[
  {
    "id": 1,
    "nom": "Clavier mécanique",
    "prix": 89.90,
    "stock": 12
  },
  {
    "id": 2,
    "nom": "Souris ergonomique",
    "prix": 49.99,
    "stock": 5
  }
]
```

## JSON dans l’architecture d’une application moderne

```text
[ Frontend Web / Mobile ]
          ↓   JSON
[ API REST / GraphQL / Gateway ]
          ↓   JSON
[ Microservices ]
          ↓   JSON
[ BDD NoSQL / Services externes ]
```

## Erreurs courantes avec le JSON

| Erreur                                     | Exemple                      | Pourquoi                                                                  |
| ------------------------------------------ | ---------------------------- | ------------------------------------------------------------------------- |
| ❌ Virer les guillemets autour des strings  | `name: Vincent`              | JSON → les clés et strings doivent être entre `"`                         |
| ❌ Ajouter une virgule finale               | `{ "name": "Vincent", }`     | JSON ne tolère pas les virgules finales                                   |
| ❌ Commentaires interdits                   | `// Ceci est un commentaire` | JSON pur n’autorise *aucun commentaire*                                   |
| ❌ Doubler les types (date, regex, classe…) | `new Date()`                 | JSON ne sait encoder que **string, number, boolean, null, array, object** |
| ❌ Structure non valide                     | `[ { ... } , ]`              | Comme pour les objets, pas de `,` final                                   |
| ❌ Caractères spéciaux non échappés         | `"nom": "Vincent "le boss""` | Les guillemets doivent être échappés                                      |

## Avantages de JSON

- Simple, lisible, human friendly
- Ultra compatible (tous les langages modernes)
- Léger (plus court que XML)
- Excellent pour les API Web
- Facile à parser et rapide
- Utilisé comme format universel

## Défauts de JSON

- Pas de commentaires (dans le standard)
- Pas typé → peut créer des erreurs silencieuses
- Ne supporte pas les dates, regex, fonctions → nécessite une transformation en string
- Pas adapté à la validation de schémas complexes seul → on utilise JSON Schema
- Pas sécurisé si mal géré (risques d’injection JSON)

## Conseils et bonnes pratiques

### Structure et cohérence

- Toujours utiliser des clés en anglais (standard industriel)
- Utiliser camelCase : firstName
- Fournir un schéma clair dans les API
- Éviter les objets trop profonds → préférer les structures plates

### Validation des données

Toujours valider côté backend avec :
- JSON Schema
- Zod
- Yup
- Ajv

Exemple de validation avec JSON Schema :

```json
{
  "type": "object",
  "properties": {
    "email": { "type": "string", "format": "email" },
    "age": { "type": "number", "minimum": 18 }
  },
  "required": ["email"]
}
```

### Optimisation

- Ne pas envoyer des données inutiles (KISS / YAGNI)
- Favoriser la normalisation (listes + références)
- Minifier le JSON en production

### Sécurité

- Ne jamais faire confiance au JSON client
- Toujours valider, assainir, vérifier les types

Protéger les API contre :
- JSON Injection
- Data Overposting
- Mass Assignment

### Débogage

Outils recommandés :
- [https://jsonlint.com](https://jsonlint.com)
- VSCode + extension JSON Tools
- Postman / Insomnia pour tester des API
