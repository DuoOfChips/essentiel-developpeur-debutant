# gestion des erreurs en programmation

## Introduction à la gestion des erreurs

### Qu’est-ce qu’une erreur en programmation ?

Une erreur est un problème qui survient lors de l’exécution d’un programme et qui empêche ce dernier de continuer normalement. Les erreurs peuvent provenir de différentes sources :
- Erreur de syntaxe : le code n’est pas écrit correctement (ex : `if x = 5` au lieu de `if (x === 5)`).
- Erreur d’exécution (runtime) : le code est syntaxiquement correct mais provoque un problème lors de l’exécution (ex : division par zéro, accès à une variable non définie).
- Erreur logique : le code s’exécute sans planter, mais le résultat n’est pas celui attendu (ex : mauvaise formule de calcul).

### Pourquoi gérer les erreurs ?

Sans gestion des erreurs, une erreur peut :
- Faire planter l’application.
- Perdre des données utilisateur.
- Rendre l’expérience utilisateur mauvaise.
- Difficile à diagnostiquer pour un développeur.

La gestion des erreurs permet de :
- Anticiper les problèmes et éviter les plantages.
- Informer l’utilisateur avec un message clair.
- Logger les erreurs pour comprendre ce qui s’est passé.
- Corriger et réagir automatiquement quand c’est possible.

## Fonctionnement interne des erreurs dans un ordinateur

Quand votre programme s’exécute :
1. Le code source (ce que vous écrivez) est transformé en instructions machine par le compilateur ou l’interpréteur.
2. Chaque instruction est exécutée séquentiellement par le processeur.
3. Si une instruction provoque une situation non prévue (ex : division par zéro), le processeur interrompt l’exécution normale et signale une exception.
4. L’ordinateur construit un objet erreur (souvent appelé exception) avec des informations comme :
    - Type d’erreur
    - Message
    - Stack trace (trace des appels de fonction qui ont conduit à l’erreur)
5. Si cette exception n’est pas capturée, le programme plante.

En TypeScript/JavaScript, les erreurs sont des objets de type Error ou des classes héritées.

> On verra ensemble plus tard ce qu'est un `objet` ou une `classe`

## Syntaxe de base pour gérer les erreurs en TypeScript

En TypeScript, on utilise principalement :

```ts
try {
    // Code qui peut provoquer une erreur
} catch (error) {
    // Code pour gérer l'erreur
} finally {
    // Code exécuté dans tous les cas (optionnel)
}
```


### lecture d’un fichier JSON

```ts
function parseJson(jsonString: string): object | null {
    try {
        const data = JSON.parse(jsonString);
        return data;
    } catch (error) {
        console.error("Erreur lors de l'analyse du JSON :", (error as Error).message);
        return null;
    }
}

// Usage
const result = parseJson('{"nom": "Vincent"}');
console.log(result);
```


`try` : on met le code qui pourrait échouer.

`catch` : on capture l’erreur et on la traite.

`finally` : (optionnel) code exécuté toujours, qu’il y ait eu erreur ou pas.

## Cas d’usage concrets en applications

| Cas d’usage            | Exemple TypeScript                        | Avantage                                      |
| ---------------------- | ----------------------------------------- | --------------------------------------------- |
| Lecture de fichiers    | Parser un JSON provenant d’une API        | Empêche le crash si le fichier est corrompu   |
| Accès à une API        | Gestion des erreurs HTTP (`404`, `500`)   | Informer l’utilisateur et relancer la requête |
| Formulaire utilisateur | Vérifier la validité d’une saisie         | Message clair sur le champ incorrect          |
| Base de données        | Tentative de connexion ou requête échouée | Retry automatique ou message clair            |


### appel API

```ts
async function fetchUserData(userId: string) {
    try {
        const response = await fetch(`https://api.example.com/users/${userId}`);
        if (!response.ok) {
            throw new Error(`Erreur HTTP : ${response.status}`);
        }
        const data = await response.json();
        return data;
    } catch (error) {
        console.error("Impossible de récupérer les données :", (error as Error).message);
        return null;
    }
}
```

## Erreurs courantes à connaître

### Erreurs fréquentes en TypeScript/JavaScript

- ReferenceError : variable ou fonction inexistante
- TypeError : type incorrect (ex : null.foo())
- SyntaxError : code mal écrit
- RangeError : valeur hors limites (ex : new Array(-1))
- Error personnalisé : erreurs métiers (ex : utilisateur non autorisé)

### Conseils pour les détecter

- Lire attentivement le message d’erreur
- Vérifier la stack trace
- Reproduire le problème étape par étape
- Ajouter des logs pour comprendre le contexte
- Utiliser ChatGPT

> Attention : chatGPT est a double tranchant. Soit il résoud vite l'erreur soit il vous embourbe dans une complexité plus pronfonde. Il vaut mieux apprendre à lire les messages d'erreur et les comprendre.

## Bonnes pratiques pour la gestion des erreurs

### Écriture du code

- Toujours anticiper les erreurs possibles
- Ne jamais laisser une erreur « silencieuse » (quand on le voit)
- Préférer les exceptions explicites plutôt que de retourner `null` ou `undefined` partout
    - `null` et `undefined` peuvent être des valeurs *normales* en fonction du contexte métier.
    - Il vaut bien mieux que le programme hurle de souffrance en crachant le message d'erreur. Au moins on le sait.
- Utiliser TypeScript typé pour éviter certains types d’erreurs à la compilation

### Lecture et interprétation

- Utiliser console.error pour loguer les erreurs
    - Le message aura un style typique de l'erreur avec du rouge affiché dans la console.
- Ajouter un message clair et contextualisé
    - Les objets d'erreur comportent bien souvent un message d'erreur plus ou moins explicite. Autant l'utiliser.
- Ne pas afficher le stack trace à l’utilisateur final (sauf debug)
    - Risque important pour la sécurité de l'application.

### Structure d’erreur avancée (niveau plus avancé)

Créer des classes d’erreurs personnalisées pour mieux organiser le code :

```ts
class ApiError extends Error {
    constructor(public status: number, message: string) {
        super(message);
        this.name = "ApiError";
    }
}

async function getData() {
    try {
        throw new ApiError(404, "Utilisateur non trouvé");
    } catch (error) {
        if (error instanceof ApiError) {
            console.error(`API Error [${error.status}]: ${error.message}`);
        }
    }
}
```

## Avantages et limites de la gestion des erreurs

Avantages : 
- Évite le crash de l’application
- Facilite la maintenance et le debug
- Permet des réactions automatiques (retry, fallback)
- Améliore l’expérience utilisateur

Limites / pièges :
- Trop de try/catch peut rendre le code illisible
- Cacher les erreurs (ex : `catch` {} vide) est dangereux
- Les erreurs logiques ne sont pas toujours détectables par `try/catch`

## Conseils pour faciliter la résolution des erreurs

- Toujours logger l’erreur avec contexte (fonction, paramètre, timestamp)
- Structurer vos erreurs fortement conseillé : utiliser des classes et types
- Ne jamais ignorer les erreurs (warning aussi en théorie)
- Ajouter des tests unitaires et d’intégration pour reproduire les erreurs *volontairement*
    - Permet de s'assurer que le cas d'erreur est géré par l'application et ne provoque pas de crash
- Documenter les messages d’erreurs pour l’équipe et les utilisateurs

## Résumé pratique

- Une erreur = un problème qui interrompt le flux normal
- `try/catch/finally` = structure de base pour gérer les erreurs
- Lire et interpréter les messages d’erreurs est essentiel
- Créer des classes d’erreurs personnalisées pour vos besoins est souvent une bonne idée
- Logger et documenter les erreurs facilite le debug, la maintenance et l'évolutivité.