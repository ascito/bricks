# Bricks Specification

**A Universal Composition System for AI-Generated Content**

[![Status](https://img.shields.io/badge/status-draft-yellow.svg)](https://github.com/ascito/bricks)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

---

## Abstract

Bricks is a declarative composition specification that enables AI systems to produce validated, secure, and consistent output by selecting and configuring predefined components rather than generating arbitrary code.

This specification defines:
- A **schema format** for declaring reusable components (bricks)
- A **composition format** for assembling bricks into pages
- **Conformance requirements** for rendering engines
- A **collaboration model** between humans and AI

## Status of This Document

This is a **Working Draft** specification. It is intended for review and feedback from implementers and the broader community.

## Table of Contents

1. [Introduction](#introduction)
2. [Terminology](#terminology)
3. [Specification](#specification)
4. [Brick Catalog](#brick-catalog)
5. [Examples](#examples)
6. [Security Considerations](#security-considerations)
7. [Contributing](#contributing)

## Introduction

### The Problem

When AI generates code directly, the output is:
- **Unpredictable**: Different prompts yield inconsistent results
- **Insecure**: Potential for XSS, injection, and other vulnerabilities
- **Unmaintainable**: No design system, no consistency
- **Unverifiable**: Impossible to audit or validate

### The Solution

Bricks inverts the paradigm:

```
BEFORE: Prompt → AI → Raw Code → (bugs, vulnerabilities, inconsistencies)

AFTER:  Prompt → AI → Brick Selection + Configuration → Engine → Validated Output
```

The AI doesn't need to generate when it **already knows**. The brick catalog is structured knowledge. The AI applies what it knows: selecting the right bricks, configuring them correctly. The rendering engine produces the output.

**The entire Bricks structure is designed for LLMs**: JSON schemas, typed inputs, clear naming, hierarchical categories, examples. Everything is optimized to be parsed, understood, and applied by language models.

### Design Principles

1. **Declarative over Imperative**: Describe what, not how
2. **Composition over Generation**: Assemble validated parts
3. **Constraints over Freedom**: Limit choices to ensure quality
4. **Human Control**: AI proposes, human validates

### Vision

Bricks aims to become a **foundational standard** for human-AI collaboration in content generation, similar to how other simple yet powerful specifications transformed their domains:

| Year | Standard | Impact |
|------|----------|--------|
| 2000 | REST | Standardized web APIs |
| 2001 | JSON | Standardized data exchange |
| 2006 | jQuery | Standardized DOM manipulation |
| **2026** | **Bricks** | **Standardizes AI-assisted content creation** |

The specification is intentionally:

- **Simple**: JSON-based, no special syntax to learn
- **Universal**: Language-agnostic, framework-agnostic
- **Durable**: Built on stable foundations (JSON Schema, semantic HTML)
- **Extensible**: New bricks can be added without breaking existing ones

We believe the era of AI generating arbitrary code is a transition phase. The future is **structured knowledge** that AI can reliably apply. Bricks is that structure.

## Terminology

| Term | Definition |
|------|------------|
| **Brick** | A reusable component with a defined schema, inputs, and deterministic output |
| **Primitive** | An atomic brick with no dependencies (button, input, text) |
| **Composite** | A brick composed of other bricks (card, form, modal) |
| **Composition** | An assembly of bricks forming a page or fragment |
| **Schema** | The formal definition of a brick's inputs and outputs |
| **Engine** | A conforming implementation that renders compositions |

## Specification

The full technical specification is available in [`SPECIFICATION.md`](SPECIFICATION.md).

Key sections:
- [1. Brick Schema](SPECIFICATION.md#1-brick-schema)
- [2. Type System](SPECIFICATION.md#2-type-system)
- [3. Composition Format](SPECIFICATION.md#3-composition-format)
- [4. Rendering](SPECIFICATION.md#4-rendering)
- [5. Validation](SPECIFICATION.md#5-validation)
- [6. Conformance](SPECIFICATION.md#6-conformance)

## Brick Catalog

This repository includes a reference catalog of bricks organized by category:

```
bricks/
├── primitives/
│   ├── ui/          # button, badge, icon, image, text...
│   ├── form/        # input, select, checkbox, radio...
│   ├── layout/      # container, grid, stack, center...
│   └── data/        # variable, loop, conditional, slot...
│
├── composites/
│   ├── ui/          # card, accordion, modal, tabs, hero...
│   ├── form/        # form, login-form, search-input...
│   └── layout/      # header, footer, page-layout...
│
└── meta/            # seo, fonts
```

See [`bricks/`](bricks/) for the complete catalog with schemas and examples.

## Examples

Complete composition examples are available in [`examples/`](examples/):

- [`landing-page.json`](examples/landing-page.json) - Marketing landing page
- [`login-page.json`](examples/login-page.json) - Authentication page
- [`blog-post.json`](examples/blog-post.json) - Article page

## Security Considerations

Bricks addresses security by design:

1. **No arbitrary code execution**: AI cannot generate executable code
2. **Input sanitization**: All string inputs are sanitized by the engine
3. **Schema validation**: Invalid inputs are rejected before rendering
4. **Audit trail**: Every composition is traceable and auditable

See [Security](SPECIFICATION.md#7-security-considerations) in the specification.

## Human-AI Collaboration

Bricks defines a collaboration workflow where:
- **Human** provides vision, content, and validation
- **AI** selects bricks, proposes structures, executes technically

See [`WORKFLOW.md`](WORKFLOW.md) for the complete collaboration guide.

## Contributing

We welcome contributions! Please see [`CONTRIBUTING.md`](CONTRIBUTING.md) for guidelines.

## Acknowledgments

This specification was developed by [Ascito](https://ascito.fr), based on a page builder system created in 2018 and adapted for the era of Large Language Models.

## License

This specification is released under the [MIT License](LICENSE).

---

*One brick at a time.*
