# Phase 5 Course Creation Guide

## Overview
This guide documents the structure and standards established for Phase 5 — Architecture Web courses. Use this as a reference when creating the remaining courses.

## Completed Work

### 1. hard-skills.md Restructuring ✅
- Reorganized Phase 5 into 7 subsections
- Added 115 fine-grained topics
- Progressive difficulty: Fondamentaux → Intermédiaire → Avancé
- Logical learning path

### 2. Template Courses Created ✅

**5 comprehensive courses completed:**
1. `angular-installation-premier-projet.md` (2,816 words, 15 sections)
2. `angular-signals.md` (3,549 words, 15 sections)
3. `angular-composants-bases.md` (2,496 words, 13 sections)
4. `nestjs-installation-premier-projet.md` (3,839 words, 16 sections)
5. `nestjs-dto.md` (2,740 words, 12 sections)

**Total: 15,440 words of production-ready content**

## Course Structure Template

Every course MUST include these sections in this order:

### 1. Title
```markdown
# [Technology] [Topic] : [Subtitle if needed]
```
Example: `# Angular Signals : Nouvelle Gestion d'État Réactive`

### 2. Introduction
```markdown
## Introduction

### Objectifs du cours
Ce cours vous permettra de :
- [Objective 1]
- [Objective 2]
- [Objective 3]

### Ce que vous saurez faire après ce cours
- [Skill 1]
- [Skill 2]
- [Skill 3]

### Scope de la notion
[Paragraph explaining what this concept is, why it matters, and where it fits in webapp development]
```

### 3. Prérequis
```markdown
## Prérequis

Avant ce cours, vous devez connaître :
- [Link to prerequisite course 1]
- [Link to prerequisite course 2]
- [Link to external knowledge if needed]
```

### 4. Définitions et Concepts Clés
```markdown
## Définitions et Concepts Clés

### [Concept 1]
**[Term]** est [definition].

**Analogie** : [Real-world analogy that everyone can understand]

**Exemple concret** : [Business/webapp example]
```

**Important:** Include analogies from everyday life for EVERY major concept.

Examples of good analogies:
- Restaurant (servers = controllers, cooks = services)
- LEGO blocks (components)
- Thermometer (signals)
- Administrative forms (DTOs)
- Toolbox (CLI)

### 5. Optional: Ce qui se passe dans l'ordinateur
```markdown
## Ce qui se passe dans l'ordinateur

[Explain memory, CPU, compilation process when relevant]

```mermaid
[Diagram showing architecture or flow]
```
```

Use this section for:
- Compilation/build processes
- Memory management
- Network requests
- Change detection mechanisms

### 6. Core Content
Structure progressively with subsections:
- Start simple
- Build complexity
- Show code examples
- Include TypeScript ONLY
- Use real webapp scenarios

**Common subsections:**
- Creating/Installing
- Basic usage
- Advanced patterns
- Integration with other features

**Code example standards:**
```typescript
// ❌ MAUVAIS - [Why it's bad]
[bad code]

// ✅ BON - [Why it's good]
[good code]
```

### 7. Cas d'Usage Concrets
Include 2-3 real-world scenarios:
- E-commerce (products, cart, orders)
- CRM (users, customers, contacts)
- Inventory management
- Authentication/Authorization
- Dashboard/Analytics
- Task management

**Format:**
```markdown
### Cas 1 : [Scenario name]

**Context:** [Describe the business need]

**Implementation:**
```typescript
[Full working example]
```

**Explanation:** [Why this approach]
```

### 8. Erreurs Courantes & Comment les Éviter
```markdown
## Erreurs Courantes & Comment les Éviter

### Erreur 1 : [Error description]

**Problème** : [What goes wrong]

```typescript
// ❌ MAUVAIS
[Code that causes the error]
```

**Erreur** : `[Error message]`

**Solution** :
```typescript
// ✅ BON
[Correct code]
```

**Pourquoi ?** [Explanation]
```

Include at least 5 common errors.

### 9. Exercices Pratiques
```markdown
## Exercices Pratiques

### Exercice 1 : [Title] (Obligatoire)

**Objectif** : [What to achieve]

**Tâches** :
1. [Step 1]
2. [Step 2]
3. [Step 3]

**Validation** : [How to know it works]

### Exercice 2 : [Title] (Recommandé)

[Same structure]

### Exercice 3 : [Title] (Facultatif)

[Same structure - more challenging]
```

### 10. Comportement Senior
```markdown
## Comportement Senior

### Bonnes pratiques

**1. [Practice name]**
```typescript
// ✅ BON - [Description]
[code]

// ❌ MAUVAIS - [Description]
[code]
```

### Astuces de développeur expérimenté
- [Tip 1]
- [Tip 2]

### Patterns avancés
[Advanced patterns if relevant]
```

Include:
- Professional recommendations
- Industry best practices
- Code quality tips
- Documentation practices
- Tools and extensions

### 11. Résumé
```markdown
## Résumé

### Qu'avez-vous appris ?
1. **[Topic 1]** : [Summary]
2. **[Topic 2]** : [Summary]

### Quand utiliser [this feature] ?

**✅ Utilisez [feature] pour** :
- [Use case 1]
- [Use case 2]

**❌ Ne pas utiliser pour** :
- [Anti-pattern 1]
- [Anti-pattern 2]

### Prochaines étapes
- **[Link to next course]** - [What it covers]
- **[Link to related course]** - [What it covers]

### Points clés à retenir

> [Single paragraph summary of the most important concepts]
```

### 12. Ressources Externes
```markdown
## Ressources Externes

### Documentation officielle
- 📘 [Official Docs](URL) - Description

### Vidéos (français)
- 🎥 [Video Title - Author](URL)

### Vidéos (anglais)
- 🎥 [Video Title - Author](URL)

### Articles et tutoriels
- 📝 [Article Title](URL)

### Outils
- 🛠️ [Tool Name](URL) - Description

### Communauté
- 💬 [Discord/Reddit/Stack Overflow](URL)
```

**Priority:** French resources first, then English.

## Quality Standards

### TypeScript Only
- ✅ All code examples in TypeScript
- ❌ No JavaScript examples
- Use proper types and interfaces

### Length
- Minimum: 2,000 words
- Target: 2,500-3,500 words
- Maximum: 4,000 words

### Sections
- Minimum: 12 sections (all required)
- Average: 13-16 sections

### Code Examples
- Minimum: 10 code blocks
- Mix of ❌ MAUVAIS and ✅ BON examples
- Real-world, runnable code

### Analogies
- At least 3 real-world analogies
- Simple, understandable by anyone
- Relevant to the concept

### Exercises
- 3 exercises minimum
- Progressive difficulty
- Clear validation criteria

## Technical Requirements

### Angular Courses
- Use Angular 20+ features
- Standalone components by default
- Signals for state when applicable
- Modern best practices

### NestJS Courses
- Use NestJS 11 features
- Latest decorators
- TypeScript strict mode
- Security best practices

### Formatting
- Markdown format
- Mermaid diagrams where helpful
- Tables for comparisons
- Proper code formatting with syntax highlighting

## Content Guidelines

### Tone
- Professional but approachable
- Clear and pedagogical
- Encouraging for beginners
- Challenging for advanced topics

### Examples
- Real business scenarios
- Webapp-focused
- Complete, working code
- Properly commented

### Errors
- Show common mistakes
- Explain why they're wrong
- Provide clear solutions
- Include error messages

## Remaining Courses

### High Priority (Next to Create)

**Angular:**
1. Templates and data binding
2. Directives (structural)
3. Directives (attribute)
4. Property and event binding
5. Component communication (@Input/@Output)
6. Services
7. Dependency Injection
8. HTTP Client basics
9. RxJS Observables intro
10. Forms (reactive)

**NestJS:**
1. Validation (class-validator)
2. Controllers advanced
3. Providers and Services
4. TypeORM setup
5. Entities and repositories
6. Authentication basics
7. JWT implementation
8. Guards
9. Interceptors
10. Pipes

### Medium Priority

Continue following the hard-skills.md structure.

### Lower Priority

Advanced topics, performance optimization, microservices.

## Validation Checklist

Before marking a course as complete, verify:

- [ ] All 12 required sections present
- [ ] 2,000+ words
- [ ] TypeScript code only
- [ ] 3+ real-world analogies
- [ ] 10+ code examples
- [ ] 5+ common errors documented
- [ ] 3 exercises (obligatoire, recommandé, facultatif)
- [ ] Senior tips included
- [ ] External resources (French + English)
- [ ] Links to prerequisites work
- [ ] Links to next steps exist (even if courses not created yet)
- [ ] Mermaid diagrams where applicable
- [ ] Real business use cases
- [ ] Code is runnable and correct
- [ ] No security vulnerabilities
- [ ] Follows Angular 20+ / NestJS 11 conventions

## Common Mistakes to Avoid

1. ❌ Using JavaScript instead of TypeScript
2. ❌ Skipping analogies
3. ❌ Too short (< 2,000 words)
4. ❌ No real-world examples
5. ❌ Missing exercise levels
6. ❌ Outdated Angular/NestJS patterns
7. ❌ No error examples
8. ❌ Missing external resources
9. ❌ Broken internal links
10. ❌ Code that doesn't run

## Tips for Efficient Creation

1. **Start with the template structure** - Copy sections from existing courses
2. **Research the topic** - Read official docs first
3. **Find analogies** - Think about everyday objects/situations
4. **Write code examples** - Test them to make sure they work
5. **Include screenshots** - If showing UI changes
6. **Link everything** - Prerequisites, next steps, external resources
7. **Use tables** - For comparisons and checklists
8. **Add diagrams** - Mermaid for architecture and flow
9. **Test exercises** - Make sure they're achievable
10. **Proofread** - Check for typos and broken links

## Example References

Use these 5 completed courses as templates:
1. **Installation course:** `angular-installation-premier-projet.md`
2. **Advanced concept:** `angular-signals.md`
3. **Fundamental concept:** `angular-composants-bases.md`
4. **Backend setup:** `nestjs-installation-premier-projet.md`
5. **Design pattern:** `nestjs-dto.md`

## Success Metrics

A course is successful when:
- ✅ A beginner can follow it and complete exercises
- ✅ Code examples work without modification
- ✅ Analogies make complex concepts clear
- ✅ Errors are preventable with the guidance provided
- ✅ Senior tips add real value
- ✅ External resources enhance learning
- ✅ It fits logically in the learning path

## File Naming Convention

```
[framework]-[topic-in-kebab-case].md
```

Examples:
- ✅ `angular-http-client-bases.md`
- ✅ `nestjs-typeorm-setup.md`
- ✅ `angular-rxjs-observables.md`
- ❌ `angular_http.md`
- ❌ `Angular-HTTP-Client.md`
- ❌ `http-client.md` (missing framework prefix)

## Directory Structure

```
courses/
  angular/
    angular-*.md
  nestjs/
    nestjs-*.md
  fullstack/
    fullstack-*.md
```

## Conclusion

Follow this guide to maintain consistency across all Phase 5 courses. When in doubt, refer to the 5 completed template courses. Quality over quantity - each course should be comprehensive and production-ready.

**Remember:** These courses are designed to make learners quickly operational with strong fundamentals and senior-level practices from day one.
