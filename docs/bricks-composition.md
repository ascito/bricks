# Bricks Composition - Documentation

Ce document explique comment composer des structures avec le système Bricks.

## Structure d'une composition

Une **composition** est un fichier JSON décrivant un assemblage de briques.

**Important** : La structure est **plate** (flat). Toutes les briques sont au même niveau dans le tableau `bricks`. Les relations parent-enfant sont exprimées via des **références par ID** dans les slots.

```json
{
  "id": "ma-composition",
  "name": "Ma Composition",
  "bricks": [
    { "id": "brique-1", "brick": "...", "inputs": { ... } },
    { "id": "brique-2", "brick": "...", "inputs": { ... } },
    { "id": "brique-3", "brick": "...", "inputs": { ... } }
  ]
}
```

### Champs de la composition

| Champ | Type | Description |
|-------|------|-------------|
| `id` | string | Identifiant unique de la composition |
| `name` | string | Nom lisible |
| `bricks` | array | Liste plate de toutes les briques |

## Structure d'une brique dans la composition

Chaque élément du tableau `bricks` :

```json
{
  "id": "mon-bouton",
  "brick": "button",
  "inputs": {
    "label": "Cliquer ici",
    "variant": "primary"
  }
}
```

| Champ | Type | Description |
|-------|------|-------------|
| `id` | string | Identifiant unique (utilisé pour les références dans les slots) |
| `brick` | string | Type de brique (doit exister dans le catalogue) |
| `inputs` | object | Paramètres de la brique (selon son schema) |
| `slots` | object | Références vers d'autres briques (voir section Slots) |

## Les Slots (références)

Les **slots** permettent de lier des briques entre elles via des **références par ID**.

### Principe

Une brique peut avoir des "emplacements" (slots) qui référencent d'autres briques :
- `card` a les slots : `header`, `body`, `footer`
- `form` a le slot : `children`
- `stack` a le slot : `children`

### Syntaxe

```json
{
  "id": "ma-card",
  "brick": "card",
  "inputs": { "variant": "elevated" },
  "slots": {
    "body": ["contenu-1", "contenu-2"]
  }
}
```

Le slot `body` contient des **IDs** qui référencent d'autres briques du même tableau `bricks`. Ce n'est **pas** de l'imbrication JSON, c'est un système de **références**.

## Exemple complet : Page de login

```json
{
  "id": "login-page",
  "name": "Page de connexion",
  "bricks": [
    {
      "id": "wrapper",
      "brick": "center",
      "inputs": { "minHeight": "100vh" },
      "slots": { "children": ["card"] }
    },
    {
      "id": "card",
      "brick": "card",
      "inputs": { "variant": "elevated", "padding": "lg" },
      "slots": { "body": ["title", "form"] }
    },
    {
      "id": "title",
      "brick": "heading",
      "inputs": { "content": "Connexion", "level": 1 }
    },
    {
      "id": "form",
      "brick": "form",
      "inputs": { "action": "/login", "method": "POST" },
      "slots": { "children": ["email", "password", "submit"] }
    },
    {
      "id": "email",
      "brick": "form-field",
      "inputs": { "label": "Email", "name": "email", "type": "email", "required": true }
    },
    {
      "id": "password",
      "brick": "form-field",
      "inputs": { "label": "Mot de passe", "name": "password", "type": "password", "required": true }
    },
    {
      "id": "submit",
      "brick": "button",
      "inputs": { "label": "Se connecter", "type": "submit", "variant": "primary", "fullWidth": true }
    }
  ]
}
```

### Structure plate vs arborescence logique

Le JSON est **plat** (7 briques au même niveau), mais les slots créent une **arborescence logique** :

```
center (wrapper) ──ref→ card
card ──ref→ title, form
form ──ref→ email, password, submit
```

L'interpréteur résout les références pour construire l'arbre de rendu.

## Exemple : Hero avec CTA

```json
{
  "id": "hero-section",
  "name": "Section Hero",
  "bricks": [
    {
      "id": "hero",
      "brick": "hero",
      "inputs": {
        "title": "Bienvenue sur notre site",
        "subtitle": "La meilleure solution pour vos besoins",
        "image": "https://example.com/hero.jpg",
        "overlay": "dark",
        "overlayOpacity": 0.5,
        "textColor": "light",
        "height": "lg",
        "align": "center"
      },
      "slots": { "actions": ["cta-primary", "cta-secondary"] }
    },
    {
      "id": "cta-primary",
      "brick": "button",
      "inputs": { "label": "Commencer", "variant": "primary", "size": "lg" }
    },
    {
      "id": "cta-secondary",
      "brick": "button",
      "inputs": { "label": "En savoir plus", "variant": "outline", "size": "lg" }
    }
  ]
}
```

## Bonnes pratiques

1. **IDs uniques** : Chaque brique doit avoir un `id` unique dans la composition
2. **Consulter le schema** : Vérifier les inputs requis et les valeurs autorisées
3. **Slots disponibles** : Tous les composites n'ont pas les mêmes slots (consulter le catalogue)
4. **Références valides** : Les IDs dans les slots doivent correspondre à des briques existantes
5. **Structure plate** : Toutes les briques au même niveau, jamais de JSON imbriqué