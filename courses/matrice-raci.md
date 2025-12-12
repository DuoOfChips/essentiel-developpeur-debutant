# Matrice RACI : Clarifier les Rôles et Responsabilités

## 1. Contexte et définitions des concepts clés

### 1.1 Qu'est-ce que la Matrice RACI ?

**RACI** est un acronyme qui définit 4 types de responsabilités :
- **R**esponsible (Réalisateur) : Qui fait le travail
- **A**ccountable (Approbateur) : Qui valide et est ultimement responsable
- **C**onsulted (Consulté) : Qui donne son avis avant
- **I**nformed (Informé) : Qui est tenu au courant après

**Objectif :** Clarifier qui fait quoi dans un projet pour éviter confusion, conflits et travail en double.

**Analogie simple :**
> Dans un restaurant : Le Chef (A) est responsable du plat, le Cuisinier (R) le prépare, le Sommelier (C) est consulté pour le vin, et le Serveur (I) est informé pour le service.

### 1.2 Principes fondamentaux

**Règle d'or :**
- Chaque tâche doit avoir UN SEUL Accountable
- Une tâche peut avoir plusieurs Responsible
- Trop de C ou I = overhead de communication

**Objectifs :**
- Éliminer la confusion des rôles
- Éviter que personne ne fasse le travail
- Éviter que plusieurs personnes fassent la même chose
- Clarifier qui décide

### 1.3 Définitions importantes

- **Responsible (R)** : Fait le travail, peut être plusieurs personnes
- **Accountable (A)** : Rend des comptes, UN SEUL par tâche
- **Consulted (C)** : Communication bidirectionnelle avant
- **Informed (I)** : Communication unidirectionnelle après
- **Matrice** : Tableau croisant tâches × rôles
- **Stakeholder** : Partie prenante du projet

## 2. Quand et pourquoi utiliser la Matrice RACI

### 2.1 Pourquoi RACI est indispensable

**Sans RACI (confusion) :**
- ❌ "Je pensais que c'était toi qui devais faire ça"
- ❌ Travail fait en double par 2 personnes
- ❌ Personne ne prend de décision
- ❌ Trop de réunions avec trop de monde
- ❌ Blocages car on ne sait pas qui valide
- ❌ Responsabilité diluée

**Avec RACI :**
- ✅ Chacun sait ce qu'il doit faire
- ✅ Pas de duplication de travail
- ✅ Décisions rapides (A est clair)
- ✅ Communication efficace (C et I définis)
- ✅ Responsabilité claire
- ✅ Moins de conflits

### 2.2 Cas d'usage idéaux

**RACI est parfait pour :**
- Nouveau projet avec équipe formée
- Réorganisation d'équipe
- Projet avec beaucoup de parties prenantes
- Processus complexes avec multiples intervenants
- Onboarding de nouveaux membres
- Résolution de conflits de responsabilité

**Exemples concrets :**
```
✅ Développement d'une nouvelle feature
✅ Processus de déploiement en production
✅ Gestion des incidents
✅ Processus de code review
✅ Mise en place d'un nouveau tool
✅ Recrutement d'un développeur
```

### 2.3 Signaux qu'il faut un RACI

```
🚨 "Ce n'est pas mon travail"
🚨 "Je pensais que c'était fait"
🚨 "Qui doit valider ça ?"
🚨 "On a fait le travail en double"
🚨 "Personne ne m'a informé"
🚨 "Trop de gens dans cette réunion"

→ Il est temps de créer un RACI !
```

## 3. Les 4 rôles RACI en détail

### 3.1 R - Responsible (Réalisateur)

**Définition :**
Personne(s) qui fait/font le travail. Exécute la tâche.

**Caractéristiques :**
- Fait le travail concret
- Peut être plusieurs personnes
- Rend compte au Accountable
- A les compétences techniques nécessaires

**Questions à se poser :**
```
❓ Qui a les compétences pour faire ça ?
❓ Qui va effectivement coder/écrire/créer ?
❓ Cette personne a-t-elle le temps ?
```

**Exemples :**
```
Tâche: Développer l'API de login
R: Alice (Développeur Backend)

Tâche: Rédiger documentation technique
R: Bob (Tech Writer) + Charlie (Dev Lead)

Tâche: Créer maquettes UI
R: Diana (Designer)
```

**⚠️ Erreur courante :**
```
❌ R = Manager (qui ne fait pas le travail)
✅ R = Personne qui fait réellement le travail
```

### 3.2 A - Accountable (Approbateur/Décideur)

**Définition :**
Personne ultimement responsable et qui approuve le travail. UN SEUL par tâche.

**Caractéristiques :**
- **UN SEUL** par tâche (règle absolue)
- Valide que le travail est bien fait
- Prend les décisions finales
- Rend compte aux stakeholders
- Peut déléguer le travail (R) mais garde la responsabilité

**Questions à se poser :**
```
❓ Qui sera tenu responsable si ça échoue ?
❓ Qui a le pouvoir de décision finale ?
❓ Qui peut dire "Go" ou "No go" ?
```

**Exemples :**
```
Tâche: Développer l'API de login
A: Product Owner

Tâche: Déployer en production
A: Tech Lead

Tâche: Valider le design
A: Design Lead
```

**Règle d'or :**
> Si personne n'est Accountable = Personne n'est vraiment responsable
> Si plusieurs Accountable = Paralysie décisionnelle

**A vs R :**
```
A: "Je m'assure que c'est fait et bien fait"
R: "Je fais le travail"

Exemple:
Feature Login:
A: Product Owner (s'assure que ça répond au besoin)
R: Développeur (code l'implémentation)
```

### 3.3 C - Consulted (Consulté)

**Définition :**
Personne(s) consultée(s) AVANT que le travail soit fait. Communication bidirectionnelle.

**Caractéristiques :**
- Donne son avis/expertise
- Consulté avant décision
- Communication aller-retour
- Peut influencer le résultat
- Pas de pouvoir de décision final

**Questions à se poser :**
```
❓ De qui avons-nous besoin d'input avant ?
❓ Qui a l'expertise nécessaire ?
❓ Qui sera impacté et doit donner son avis ?
```

**Exemples :**
```
Tâche: Choisir l'architecture API
C: Senior Developer, DevOps Engineer, Security Lead

Tâche: Définir le processus de release
C: Dev Team, QA Team

Tâche: Valider le design
C: Product Owner, Développeurs Front-end
```

**⚠️ Attention :**
```
❌ Trop de C = Processus lent
❌ C qui veut être A = Conflit

✅ C limité aux vraies expertises nécessaires
✅ C donne avis mais A décide
```

### 3.4 I - Informed (Informé)

**Définition :**
Personne(s) informée(s) APRÈS que le travail est fait. Communication unidirectionnelle.

**Caractéristiques :**
- Tenu au courant du résultat
- Après la décision/réalisation
- Communication à sens unique
- Pas d'input demandé
- Besoin de savoir pour leur travail

**Questions à se poser :**
```
❓ Qui doit être au courant du résultat ?
❓ Qui sera impacté par cette décision ?
❓ Qui utilisera l'information après ?
```

**Exemples :**
```
Tâche: Déployer en production
I: Support Team, Sales Team, Management

Tâche: Nouvelle feature livrée
I: Marketing, Customer Success

Tâche: Bug critique corrigé
I: Product Manager, Clients affectés
```

**⚠️ Attention :**
```
❌ Trop de I = Spam d'emails
❌ I qui veut être C = Frustration

✅ I limité à qui a vraiment besoin de savoir
✅ Communication claire et concise
```

## 4. Comment créer une Matrice RACI

### 4.1 Processus en 6 étapes

**Étape 1 : Lister les activités/tâches**
```
Projet: Développement nouvelle feature

Tâches:
1. Définir les requirements
2. Créer les maquettes UI
3. Développer le back-end
4. Développer le front-end
5. Écrire les tests
6. Code review
7. Déployer en staging
8. Tests QA
9. Déployer en production
10. Communiquer aux utilisateurs
```

**Étape 2 : Identifier les rôles/personnes**
```
Rôles:
- Product Owner (PO)
- Tech Lead (TL)
- Développeur Backend (Dev BE)
- Développeur Frontend (Dev FE)
- QA Engineer (QA)
- DevOps Engineer (DevOps)
- Marketing Manager (MM)
```

**Étape 3 : Créer la matrice vide**
```
            | PO | TL | Dev BE | Dev FE | QA | DevOps | MM |
------------|----|----|--------|--------|----| -------|----| 
Requirements|    |    |        |        |    |        |    |
Maquettes   |    |    |        |        |    |        |    |
...         |    |    |        |        |    |        |    |
```

**Étape 4 : Assigner les R (Responsible)**
Pour chaque tâche, qui fait le travail ?

**Étape 5 : Assigner le A (Accountable)**
Pour chaque tâche, UN SEUL qui est ultimement responsable.

**Étape 6 : Assigner C et I**
Qui doit être consulté avant ? Qui doit être informé après ?

### 4.2 Exemple complet : Feature de paiement

```
┌────────────────────────┬────┬────┬────────┬────────┬────┬────────┬────┐
│ Tâche                  │ PO │ TL │ Dev BE │ Dev FE │ QA │ DevOps │ MM │
├────────────────────────┼────┼────┼────────┼────────┼────┼────────┼────┤
│ 1. Requirements        │ A  │ C  │   C    │   C    │ C  │   I    │ I  │
├────────────────────────┼────┼────┼────────┼────────┼────┼────────┼────┤
│ 2. Choix solution      │ C  │ A  │   R    │   C    │ I  │   C    │ I  │
│    technique           │    │    │        │        │    │        │    │
├────────────────────────┼────┼────┼────────┼────────┼────┼────────┼────┤
│ 3. Intégration Stripe  │ I  │ A  │   R    │        │ I  │   C    │    │
│    API                 │    │    │        │        │    │        │    │
├────────────────────────┼────┼────┼────────┼────────┼────┼────────┼────┤
│ 4. Dev UI paiement     │ C  │ A  │   C    │   R    │ I  │        │    │
├────────────────────────┼────┼────┼────────┼────────┼────┼────────┼────┤
│ 5. Tests automatisés   │ I  │ C  │   R    │   R    │ A  │        │    │
├────────────────────────┼────┼────┼────────┼────────┼────┼────────┼────┤
│ 6. Code Review         │ I  │ A  │   C    │   C    │ C  │        │    │
├────────────────────────┼────┼────┼────────┼────────┼────┼────────┼────┤
│ 7. Tests QA            │ C  │ C  │   I    │   I    │ R,A│        │    │
├────────────────────────┼────┼────┼────────┼────────┼────┼────────┼────┤
│ 8. Déploiement prod    │ C  │ A  │   I    │   I    │ I  │   R    │ I  │
├────────────────────────┼────┼────┼────────┼────────┼────┼────────┼────┤
│ 9. Communication       │ C  │ I  │        │        │    │        │ R,A│
│    utilisateurs        │    │    │        │        │    │        │    │
└────────────────────────┴────┴────┴────────┴────────┴────┴────────┴────┘

Légende:
R = Responsible (fait le travail)
A = Accountable (responsable final, UN SEUL)
C = Consulted (consulté avant)
I = Informed (informé après)
```

### 4.3 Validation de la matrice

**Checklist de validation :**

**Pour chaque ligne (tâche) :**
- [ ] Au moins un R (quelqu'un fait le travail)
- [ ] UN SEUL A (responsable final)
- [ ] Pas trop de C (< 4 personnes idéalement)
- [ ] Pas trop de I (limiter le bruit)

**Pour chaque colonne (personne) :**
- [ ] Charge de travail réaliste (pas 20 R pour une personne)
- [ ] Pas de A sans R correspondant
- [ ] Équilibre des responsabilités

**Questions de validation :**
```
❓ Qui fait réellement le travail ? (R)
❓ Qui sera blâmé si ça échoue ? (A)
❓ De qui avons-nous besoin d'avis avant ? (C)
❓ Qui doit savoir après ? (I)
```

## 5. Exemples par type de projet

### 5.1 Feature Development (Développement)

```
┌─────────────────────┬────┬────────┬────────┬────┬─────────┐
│ Tâche               │ PO │ Dev 1  │ Dev 2  │ QA │ SM      │
├─────────────────────┼────┼────────┼────────┼────┼─────────┤
│ User Story          │ R,A│   C    │   C    │ C  │   I     │
├─────────────────────┼────┼────────┼────────┼────┼─────────┤
│ Estimation          │ C  │  R,A   │   R    │ I  │   I     │
├─────────────────────┼────┼────────┼────────┼────┼─────────┤
│ Développement       │ C  │   R    │   R    │ I  │   I     │
├─────────────────────┼────┼────────┼────────┼────┼─────────┤
│ Code Review         │ I  │  R,A   │   R    │ I  │   I     │
├─────────────────────┼────┼────────┼────────┼────┼─────────┤
│ Tests               │ I  │   C    │   C    │ R,A│   I     │
├─────────────────────┼────┼────────┼────────┼────┼─────────┤
│ Démo Sprint Review  │ A  │   R    │   R    │ C  │   R     │
└─────────────────────┴────┴────────┴────────┴────┴─────────┘
```

### 5.2 Incident Management (Gestion d'incident)

```
┌──────────────────────┬──────────┬────────┬────────┬────┐
│ Tâche                │ On-Call  │ TL     │ DevOps │ PO │
├──────────────────────┼──────────┼────────┼────────┼────┤
│ Détection incident   │  R,A     │   I    │   I    │ I  │
├──────────────────────┼──────────┼────────┼────────┼────┤
│ Investigation        │  R       │   C    │   C    │ I  │
├──────────────────────┼──────────┼────────┼────────┼────┤
│ Correction           │  R       │   A    │   C    │ I  │
├──────────────────────┼──────────┼────────┼────────┼────┤
│ Tests correction     │  R       │   C    │   C    │ I  │
├──────────────────────┼──────────┼────────┼────────┼────┤
│ Déploiement hotfix   │  R       │   A    │   R    │ I  │
├──────────────────────┼──────────┼────────┼────────┼────┤
│ Post-mortem          │  R       │   A    │   C    │ C  │
└──────────────────────┴──────────┴────────┴────────┴────┘
```

### 5.3 Recrutement développeur

```
┌──────────────────────┬────┬──────┬──────┬────────┐
│ Tâche                │ RH │ TL   │ Dev  │ CTO    │
├──────────────────────┼────┼──────┼──────┼────────┤
│ Job description      │ R  │  C   │  C   │  A     │
├──────────────────────┼────┼──────┼──────┼────────┤
│ Publication offre    │ R,A│  I   │      │  I     │
├──────────────────────┼────┼──────┼──────┼────────┤
│ Screening CV         │ R  │  C   │      │  A     │
├──────────────────────┼────┼──────┼──────┼────────┤
│ Test technique       │ C  │  R,A │  C   │  I     │
├──────────────────────┼────┼──────┼──────┼────────┤
│ Entretien technique  │ I  │  R,A │  R   │  C     │
├──────────────────────┼────┼──────┼──────┼────────┤
│ Décision finale      │ C  │  C   │  C   │  A     │
├──────────────────────┼────┼──────┼──────┼────────┤
│ Offre contractuelle  │ R,A│  C   │      │  C     │
└──────────────────────┴────┴──────┴──────┴────────┘
```

## 6. Outils pour créer un RACI

### 6.1 Outils simples

**Excel / Google Sheets :**
```
✅ Gratuit et accessible
✅ Facile à partager
✅ Formules pour valider (UN SEUL A)
✅ Filtres et tris
```

**Template Google Sheets :**
```
=COUNTIF(B2:H2,"A")  → Doit être = 1
=COUNTIF(B2:H2,"R")  → Doit être ≥ 1
```

**Confluence / Notion :**
```
✅ Documentation centralisée
✅ Versionning
✅ Commentaires
✅ Intégration avec Jira
```

### 6.2 Template Markdown

```markdown
## RACI Matrix - Feature Login

| Tâche | PO | Dev | QA | Description |
|-------|----|----|----|----|
| Requirements | A | C | C | Définir besoins |
| Development | C | R,A | I | Coder feature |
| Testing | C | C | R,A | Tests QA |
| Deployment | I | R | I | Deploy prod |

**Légende:**
- R: Responsible (fait)
- A: Accountable (responsable)
- C: Consulted (consulté)
- I: Informed (informé)
```

## 7. Erreurs et pièges à éviter

### 7.1 Erreurs fréquentes

| Erreur | Description | Impact | Solution |
|--------|-------------|--------|----------|
| **Plusieurs A** | 2+ Accountable par tâche | Confusion, décisions lentes | UN SEUL A par tâche |
| **Pas de A** | Aucun Accountable | Personne responsable | Toujours un A |
| **Trop de C** | 10 personnes consultées | Processus paralysé | Max 3-4 C |
| **Trop de I** | 20 personnes informées | Spam, bruit | I limité au besoin |
| **A qui ne peut pas décider** | A sans pouvoir réel | Blocages | A = vraie autorité |
| **R sans compétence** | Mauvaise assignation | Travail mal fait | R = compétences |
| **RACI non communiqué** | Matrice dans un tiroir | Inutile | Partager et expliquer |

### 7.2 Pièges courants

**Piège 1 : RACI trop détaillé**
```
❌ RACI avec 100 tâches microscopiques
   → Overhead énorme
   → Personne ne l'utilise

✅ RACI macro (10-20 tâches principales)
   → Gérable
   → Utilisé réellement
```

**Piège 2 : Confusion A et R**
```
❌ Manager est R pour tout
   (alors qu'il ne fait pas le travail)

✅ Clarification:
   R = Fait physiquement le travail
   A = Responsable du résultat
   
Exemple:
Tâche: Coder API
R: Développeur (écrit le code)
A: Tech Lead (s'assure que c'est bien fait)
```

**Piège 3 : RACI = Hiérarchie**
```
❌ "Le chef est toujours A"
   → Bottleneck
   → Microgestion

✅ A = Meilleure personne pour décider
   → Peut être junior si compétent
   → Empowerment de l'équipe
```

**Piège 4 : C qui veut être A**
```
Symptôme:
"Je suis juste Consulté ? Je devrais être Accountable!"

Résolution:
- Clarifier: C donne expertise, A décide
- C influence mais ne décide pas
- Si vraiment compétent → Promouvoir à A
```

### 7.3 Antipatterns

**1. "Everybody is Accountable"**
```
❌ Toute l'équipe est A pour tout
   → Personne n'est vraiment responsable
   → Diffusion de responsabilité

✅ UN SEUL A par tâche
```

**2. "CC Everyone"**
```
❌ Tout le monde en I pour tout
   → Information overload
   → Emails ignorés

✅ I limité aux vraiment concernés
```

**3. "Consultation Hell"**
```
❌ 15 personnes en C
   → Impossible d'avancer
   → Paralysie par comité

✅ C limité à 2-4 experts clés
```

## 8. Résumé de l'essentiel

### Points clés à retenir

1. **RACI = 4 rôles clairs**
   - R: Fait le travail (peut être plusieurs)
   - A: Responsable final (UN SEUL obligatoire)
   - C: Consulté avant (expertise)
   - I: Informé après (besoin de savoir)

2. **Règles absolues**
   - UN SEUL A par tâche
   - Au moins un R par tâche
   - Limiter C et I (éviter overhead)

3. **Bénéfices**
   - Clarté des responsabilités
   - Décisions rapides
   - Moins de conflits
   - Communication efficace

4. **Création en 6 étapes**
   1. Lister tâches
   2. Identifier rôles
   3. Créer matrice
   4. Assigner R
   5. Assigner A (UN SEUL)
   6. Assigner C et I

### Checklist RACI

**Validation matrice :**
- [ ] Chaque tâche a AU MOINS un R
- [ ] Chaque tâche a UN SEUL A
- [ ] Pas plus de 3-4 C par tâche
- [ ] I limité aux vraiment concernés
- [ ] Charge équilibrée par personne
- [ ] Matrice partagée avec l'équipe
- [ ] Revue régulière (trimestre)

### Questions décisionnelles rapides

```
Pour chaque tâche et personne:

R: "Cette personne fait-elle physiquement le travail ?"
   OUI → R
   
A: "Cette personne est-elle ultimement responsable ?"
   "Sera-t-elle blâmée si ça échoue ?"
   OUI → A (UN SEUL)
   
C: "Avons-nous besoin de l'avis de cette personne AVANT ?"
   "A-t-elle l'expertise nécessaire ?"
   OUI → C
   
I: "Cette personne doit-elle savoir APRÈS ?"
   "L'information lui est-elle nécessaire ?"
   OUI → I
```

### Template décision pour une tâche

```
Tâche: [Nom de la tâche]

Responsible (R): [Qui fait le travail ?]
  - [Personne 1] parce que [compétence]
  - [Personne 2] parce que [compétence]

Accountable (A): [Qui est responsable ?] UN SEUL
  - [Personne] parce que [autorité/responsabilité]

Consulted (C): [Qui consulter avant ?] Max 3-4
  - [Expert 1] parce que [expertise]
  - [Expert 2] parce que [expertise]

Informed (I): [Qui informer après ?] Limité
  - [Stakeholder 1] parce que [impact]
  - [Stakeholder 2] parce que [besoin]
```

### Communication de la matrice

**Email d'annonce :**
```
Objet: RACI Matrix - Feature Paiement

Bonjour l'équipe,

Afin de clarifier les rôles et responsabilités sur la feature
Paiement, j'ai créé une matrice RACI (lien ci-dessous).

Rappel:
- R: Tu fais le travail
- A: Tu es responsable du résultat final
- C: On te consulte avant de décider
- I: On t'informe après

Merci de la consulter et de me faire vos retours d'ici [date].

Lien: [URL de la matrice]
```

---

**En une phrase :**

> La Matrice RACI est un outil de clarification des responsabilités qui définit pour chaque tâche qui est Responsible (fait le travail), Accountable (responsable final, UN SEUL), Consulted (consulté avant) et Informed (informé après), éliminant ainsi la confusion, les doublons de travail et les blocages décisionnels tout en améliorant l'efficacité de la communication.

**Pour être employable :**

**Tu DOIS savoir :**
- ✅ Expliquer ce que signifie RACI
- ✅ Différencier R et A clairement
- ✅ Comprendre pourquoi UN SEUL A par tâche
- ✅ Créer une matrice RACI simple
- ✅ Identifier les erreurs dans un RACI (plusieurs A, trop de C)
- ✅ Utiliser RACI pour clarifier responsabilités

**Vocabulaire à maîtriser absolument :**
RACI, Responsible, Accountable, Consulted, Informed, Matrice de responsabilités, Stakeholder, Règle du UN SEUL A, Overhead de communication.
