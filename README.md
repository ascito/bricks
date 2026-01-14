# Bricks Spec

> Spécification du système de composition Bricks - JSON + Markdown uniquement

## Contenu

```
bricks-spec/
├── catalog.json              # 93 briques avec leurs schemas
├── docs/
│   ├── bricks-catalog.md     # Qu'est-ce qu'une brique
│   └── bricks-composition.md # Comment composer des pages
└── examples/
    └── landing-simple.json   # Exemple de composition
```

## Utilisation

Ce repo contient la **spécification pure** du système Bricks :
- Pas de code d'implémentation
- Uniquement JSON (schemas, catalogue) et Markdown (documentation)
- Peut servir de base de connaissances pour LLMs (RAG / file_search)

## Catalogue

Le fichier `catalog.json` contient les 93 briques disponibles :
- **Primitives** : button, input, image, heading, text, link, icon, badge...
- **Composites** : hero, header, footer, card, accordion, tabs, modal...
- **Intelligence** : ai-generate

Chaque brique a :
- `id` : Identifiant unique
- `category` : primitive | composite | intelligence
- `description` : Description courte
- `schema` : JSON Schema des inputs
- `examples` : Exemples d'utilisation

## Documentation

- [Qu'est-ce qu'une brique](docs/bricks-catalog.md)
- [Comment composer](docs/bricks-composition.md)

## Liens

- **GitHub** : [ascito/bricks](https://github.com/ascito/bricks)
