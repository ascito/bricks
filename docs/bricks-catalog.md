# Bricks Catalog - Documentation

Ce document explique le fichier `bricks-catalog.json`, la base de connaissance du système Bricks.

## Qu'est-ce que Bricks ?

**Bricks** est un système de composition universel basé sur des composants JSON. On assemble des "briques" validées par des schémas pour créer des structures complexes.

```
Composition JSON → Moteur Bricks → Output (selon l'implémentation)
```

Le système est **agnostique du rendu** : une même composition peut être interprétée pour générer du HTML, du PDF, des données structurées, ou tout autre format selon le moteur utilisé.

## Qu'est-ce qu'une brique ?

Une **brique** est un composant abstrait qui :
- Reçoit des **inputs** (paramètres JSON typés)
- Est validée par un **schema JSON**
- Produit un **output** selon l'interpréteur utilisé

Exemple : la brique `button` reçoit `{ "label": "Cliquer", "variant": "primary" }` et représente un bouton avec ces propriétés. Le rendu final dépend de l'implémentation (HTML, composant React, élément PDF, etc.).

## Les 3 catégories

| Catégorie | Description | Exemples |
|-----------|-------------|----------|
| `primitive` | Composants UI atomiques | button, input, image, icon, badge, text, heading |
| `composite` | Composants composés de plusieurs primitives | card, hero, form, navbar, modal, accordion |
| `intelligence` | Briques qui font appel à l'IA | ai-generate |

## Structure du catalogue JSON

Le fichier `bricks-catalog.json` est un **array** de briques. Chaque brique a cette structure :

```json
{
  "id": "button",
  "category": "primitive",
  "description": "Bouton cliquable avec variantes",
  "schema": { ... },
  "examples": [ ... ]
}
```

### Champs

| Champ | Type | Description |
|-------|------|-------------|
| `id` | string | Identifiant unique de la brique (ex: `button`, `hero`, `card`) |
| `category` | string | `primitive`, `composite` ou `intelligence` |
| `description` | string | Description courte de la brique |
| `schema` | object | JSON Schema définissant les inputs acceptés |
| `examples` | array | Exemples d'inputs pré-remplis |

## Comment lire un schema

Le `schema` d'une brique est un **JSON Schema** qui définit les inputs possibles.

### Exemple : schema de `button`

```json
{
  "type": "object",
  "properties": {
    "label": {
      "type": "string",
      "description": "Texte du bouton"
    },
    "variant": {
      "type": "string",
      "enum": ["primary", "secondary", "outline", "ghost"],
      "default": "primary"
    },
    "size": {
      "type": "string",
      "enum": ["sm", "md", "lg"],
      "default": "md"
    },
    "disabled": {
      "type": "boolean",
      "default": false
    },
    "href": {
      "type": "string",
      "description": "URL si le bouton est un lien"
    }
  },
  "required": ["label"]
}
```

### Interpréter le schema

| Élément | Signification |
|---------|---------------|
| `properties` | Liste des inputs possibles |
| `type` | Type de la valeur (`string`, `boolean`, `number`, `array`, `object`) |
| `enum` | Valeurs autorisées (liste fermée) |
| `default` | Valeur par défaut si non fournie |
| `required` | Inputs obligatoires |
| `description` | Explication de l'input |

## Exemple complet d'une brique

```json
{
  "id": "badge",
  "category": "primitive",
  "description": "Badge ou tag informatif avec variantes",
  "schema": {
    "type": "object",
    "properties": {
      "content": {
        "type": "string",
        "description": "Texte du badge"
      },
      "variant": {
        "type": "string",
        "enum": ["primary", "secondary", "success", "warning", "danger"],
        "default": "primary"
      },
      "size": {
        "type": "string",
        "enum": ["xs", "sm", "md", "lg"],
        "default": "md"
      }
    },
    "required": ["content"]
  },
  "examples": [
    { "content": "Nouveau", "variant": "success" },
    { "content": "En promo", "variant": "warning", "size": "lg" }
  ]
}
```

Pour utiliser cette brique dans une composition, on fournit :

```json
{
  "id": "mon-badge",
  "brick": "badge",
  "inputs": {
    "content": "Nouveau",
    "variant": "success"
  }
}
```

## Philosophie

Le système Bricks repose sur quelques principes :

1. **Tout est brique** : Chaque élément est un composant réutilisable
2. **Validation stricte** : Les inputs sont validés par des schemas JSON
3. **Agnosticisme** : Le format JSON est indépendant du rendu final
4. **Sémantique** : Les briques décrivent "quoi", pas "comment"