# Bricks Specification

**Version 1.0.0 - Working Draft**

**Editors:**
- Ascito (contact@ascito.fr)

**Abstract:**
This document specifies the Bricks composition system, a declarative format for defining reusable UI components and assembling them into pages.

---

## Table of Contents

1. [Brick Schema](#1-brick-schema)
2. [Type System](#2-type-system)
3. [Composition Format](#3-composition-format)
4. [Rendering](#4-rendering)
5. [Validation](#5-validation)
6. [Conformance](#6-conformance)
7. [Security Considerations](#7-security-considerations)
8. [Privacy Considerations](#8-privacy-considerations)

---

## 1. Brick Schema

### 1.1 Overview

A **brick** is the fundamental unit of composition. Each brick is defined by a JSON schema that specifies its identity, inputs, outputs, and behavior.

### 1.2 Schema Structure

A brick schema MUST contain the following fields:

```json
{
  "id": "string",
  "version": "string",
  "category": "string",
  "description": "string",
  "inputs": { },
  "tags": [ ]
}
```

A brick schema MAY contain the following optional fields:

```json
{
  "outputs": { },
  "examples": [ ],
  "compatibility": { },
  "deprecated": false,
  "since": "string"
}
```

### 1.3 Field Definitions

#### 1.3.1 `id` (required)

A unique identifier for the brick. MUST be a lowercase string using only alphanumeric characters and hyphens.

**Pattern:** `^[a-z][a-z0-9-]*$`

**Examples:** `button`, `card`, `login-form`, `date-picker`

#### 1.3.2 `version` (required)

The semantic version of the brick schema.

**Pattern:** `^[0-9]+\.[0-9]+\.[0-9]+$`

**Example:** `1.0.0`

#### 1.3.3 `category` (required)

The hierarchical category of the brick. Categories follow a dot-notation format.

**Valid categories:**

| Category | Description |
|----------|-------------|
| `ui.primitive` | Atomic UI elements |
| `ui.composite` | Assembled UI components |
| `form.primitive` | Atomic form elements |
| `form.composite` | Assembled form components |
| `layout.primitive` | Atomic layout elements |
| `layout.composite` | Assembled layout components |
| `data.primitive` | Data binding primitives |
| `meta` | Meta information (SEO, fonts) |

#### 1.3.4 `description` (required)

A human-readable description of the brick's purpose.

#### 1.3.5 `inputs` (required)

An object defining the brick's input parameters. See [Section 2: Type System](#2-type-system).

#### 1.3.6 `outputs` (optional)

An object defining events or slots that the brick exposes.

#### 1.3.7 `tags` (required)

An array of strings for categorization and search.

#### 1.3.8 `examples` (optional)

An array of example configurations demonstrating brick usage.

```json
{
  "examples": [
    {
      "name": "Example name",
      "description": "Optional description",
      "inputs": { }
    }
  ]
}
```

### 1.4 Complete Example

```json
{
  "id": "button",
  "version": "1.0.0",
  "category": "ui.primitive",
  "description": "Interactive button with multiple variants and sizes",
  "inputs": {
    "type": "object",
    "properties": {
      "label": {
        "type": "string",
        "description": "Button text"
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
      "href": {
        "type": "string",
        "nullable": true,
        "description": "URL if button is a link"
      },
      "disabled": {
        "type": "boolean",
        "default": false
      }
    },
    "required": ["label"]
  },
  "tags": ["ui", "primitive", "interactive", "action"],
  "examples": [
    {
      "name": "Primary button",
      "inputs": {
        "label": "Submit",
        "variant": "primary"
      }
    },
    {
      "name": "Link button",
      "inputs": {
        "label": "Learn more",
        "variant": "outline",
        "href": "/about"
      }
    }
  ]
}
```

---

## 2. Type System

### 2.1 Primitive Types

| Type | JSON Type | Description |
|------|-----------|-------------|
| `string` | string | Text value |
| `number` | number | Numeric value (integer or float) |
| `integer` | number | Integer value only |
| `boolean` | boolean | True or false |
| `null` | null | Null value |

### 2.2 Complex Types

#### 2.2.1 `object`

A structured type with named properties.

```json
{
  "type": "object",
  "properties": {
    "name": { "type": "string" },
    "age": { "type": "integer" }
  },
  "required": ["name"]
}
```

#### 2.2.2 `array`

An ordered list of values.

```json
{
  "type": "array",
  "items": {
    "type": "string"
  },
  "minItems": 1,
  "maxItems": 10
}
```

#### 2.2.3 `enum`

A value constrained to a predefined set.

```json
{
  "type": "string",
  "enum": ["small", "medium", "large"],
  "default": "medium"
}
```

### 2.3 Special Types

#### 2.3.1 `brick`

A reference to another brick (for composition).

```json
{
  "type": "brick",
  "accepts": ["button", "link", "icon"]
}
```

#### 2.3.2 `slot`

A container for child bricks.

```json
{
  "type": "slot",
  "name": "content",
  "accepts": ["*"]
}
```

#### 2.3.3 `html`

Raw HTML content (sanitized by engine).

```json
{
  "type": "html",
  "description": "Rich text content"
}
```

### 2.4 Modifiers

| Modifier | Type | Description |
|----------|------|-------------|
| `required` | boolean | Field must be provided |
| `default` | any | Default value if not provided |
| `nullable` | boolean | Field accepts null |
| `description` | string | Human-readable description |
| `deprecated` | boolean | Field is deprecated |

### 2.5 Constraints

#### String Constraints

| Constraint | Description |
|------------|-------------|
| `minLength` | Minimum string length |
| `maxLength` | Maximum string length |
| `pattern` | Regex pattern to match |
| `format` | Predefined format (email, url, date, etc.) |

#### Number Constraints

| Constraint | Description |
|------------|-------------|
| `minimum` | Minimum value (inclusive) |
| `maximum` | Maximum value (inclusive) |
| `exclusiveMinimum` | Minimum value (exclusive) |
| `exclusiveMaximum` | Maximum value (exclusive) |
| `multipleOf` | Value must be multiple of this |

#### Array Constraints

| Constraint | Description |
|------------|-------------|
| `minItems` | Minimum array length |
| `maxItems` | Maximum array length |
| `uniqueItems` | All items must be unique |

---

## 3. Composition Format

### 3.1 Overview

A **composition** is an assembly of bricks forming a page or fragment. Compositions are JSON documents that reference brick schemas and provide input values.

### 3.2 Composition Structure

```json
{
  "name": "string",
  "version": "string",
  "description": "string",
  "bricks": [ ]
}
```

### 3.3 Brick Reference

Each brick in a composition is referenced by its `id` and configured with `inputs`:

```json
{
  "brick": "hero",
  "inputs": {
    "title": "Welcome",
    "subtitle": "Discover our services"
  }
}
```

### 3.4 Nested Composition

Bricks can contain other bricks via slots:

```json
{
  "brick": "card",
  "inputs": {
    "variant": "elevated"
  },
  "children": [
    {
      "brick": "heading",
      "inputs": { "level": 3, "content": "Card Title" }
    },
    {
      "brick": "text",
      "inputs": { "content": "Card description" }
    },
    {
      "brick": "button",
      "inputs": { "label": "Action" }
    }
  ]
}
```

### 3.5 Data Binding

Compositions can reference external data using template syntax:

```json
{
  "brick": "card",
  "inputs": {
    "title": "{{ data.product.name }}",
    "price": "{{ data.product.price }}"
  }
}
```

#### 3.5.1 Binding Syntax

| Syntax | Description |
|--------|-------------|
| `{{ data.path }}` | Access data by path |
| `{{ data.items[0] }}` | Access array item |
| `{{ data.items[*].name }}` | Map array property |

### 3.6 Conditional Rendering

Bricks can be conditionally rendered:

```json
{
  "brick": "banner",
  "condition": "{{ data.promo.active }}",
  "inputs": { }
}
```

### 3.7 Iteration

Bricks can be repeated for each item in an array:

```json
{
  "brick": "card",
  "repeat": "{{ data.products }}",
  "as": "product",
  "inputs": {
    "title": "{{ product.name }}",
    "image": "{{ product.image }}"
  }
}
```

### 3.8 Complete Composition Example

```json
{
  "name": "landing-page",
  "version": "1.0.0",
  "description": "Marketing landing page",
  "data": {
    "source": "datas.json"
  },
  "bricks": [
    {
      "brick": "seo",
      "inputs": {
        "title": "{{ data.site.title }}",
        "description": "{{ data.site.description }}"
      }
    },
    {
      "brick": "header",
      "inputs": {
        "logo": "{{ data.site.logo }}",
        "navigation": "{{ data.navigation }}"
      }
    },
    {
      "brick": "hero",
      "inputs": {
        "title": "{{ data.hero.title }}",
        "subtitle": "{{ data.hero.subtitle }}",
        "cta": {
          "label": "{{ data.hero.cta.label }}",
          "href": "{{ data.hero.cta.href }}"
        }
      }
    },
    {
      "brick": "section",
      "inputs": { "background": "light" },
      "children": [
        {
          "brick": "section-header",
          "inputs": {
            "title": "Our Services",
            "eyebrow": "What we do"
          }
        },
        {
          "brick": "card",
          "repeat": "{{ data.services }}",
          "as": "service",
          "inputs": {
            "title": "{{ service.name }}",
            "description": "{{ service.description }}",
            "icon": "{{ service.icon }}"
          }
        }
      ]
    },
    {
      "brick": "footer",
      "inputs": {
        "copyright": "{{ data.site.copyright }}"
      }
    }
  ]
}
```

---

## 4. Rendering

### 4.1 Rendering Process

A conforming engine MUST implement the following rendering process:

1. **Parse**: Load and parse the composition JSON
2. **Resolve**: Resolve data bindings and references
3. **Validate**: Validate all brick inputs against schemas
4. **Render**: Transform each brick to the output format
5. **Assemble**: Combine rendered bricks into final output

### 4.2 Determinism

Rendering MUST be deterministic:
- Same composition + same data = same output
- No random values unless explicitly seeded
- No external state dependencies

### 4.3 Output Formats

Engines MAY support multiple output formats:

| Format | MIME Type | Description |
|--------|-----------|-------------|
| HTML | text/html | Static HTML with inline CSS |
| HTML+CSS | text/html | HTML with linked stylesheets |
| React | application/javascript | React components |
| Vue | application/javascript | Vue components |
| JSON | application/json | Intermediate representation |

### 4.4 Brick Isolation

Each brick MUST be rendered in isolation:
- No global state leakage
- Scoped CSS (prefixed classes or CSS modules)
- No side effects on other bricks

---

## 5. Validation

### 5.1 Schema Validation

Before rendering, engines MUST validate:

1. All required fields are present
2. All values match their declared types
3. All constraints are satisfied
4. All brick references are valid

### 5.2 Validation Errors

Validation errors MUST include:

```json
{
  "valid": false,
  "errors": [
    {
      "path": "bricks[0].inputs.title",
      "code": "required_field",
      "message": "Field 'title' is required"
    }
  ]
}
```

### 5.3 Error Codes

| Code | Description |
|------|-------------|
| `required_field` | Required field is missing |
| `invalid_type` | Value type doesn't match schema |
| `invalid_enum` | Value not in allowed enum |
| `constraint_violation` | Value violates constraint |
| `unknown_brick` | Referenced brick doesn't exist |
| `invalid_reference` | Data reference cannot be resolved |

---

## 6. Conformance

### 6.1 Conformance Levels

| Level | Requirements |
|-------|--------------|
| **Bricks Basic** | Schema parsing, validation, HTML rendering |
| **Bricks Standard** | + Data binding, conditionals, iteration |
| **Bricks Full** | + Multiple output formats, theming, plugins |

### 6.2 Conformance Requirements

A conforming engine:

1. MUST parse valid brick schemas without error
2. MUST reject invalid schemas with appropriate errors
3. MUST validate compositions against schemas
4. MUST produce identical output for identical input
5. MUST sanitize HTML content to prevent XSS
6. SHOULD support all bricks in the reference catalog

### 6.3 Conformance Testing

Implementations SHOULD pass the Bricks Conformance Test Suite (to be published).

---

## 7. Security Considerations

### 7.1 Threat Model

Bricks is designed to mitigate threats from AI-generated content:

| Threat | Mitigation |
|--------|------------|
| Code injection | AI cannot generate executable code |
| XSS attacks | All HTML content is sanitized |
| Schema bypass | Strict validation before rendering |
| Data exfiltration | No network access during rendering |

### 7.2 Sanitization

Engines MUST sanitize:

- All `string` inputs that may contain HTML
- All `html` type inputs using a whitelist approach
- All URLs to prevent javascript: and data: schemes

### 7.3 Content Security Policy

Rendered output SHOULD be compatible with strict CSP:
- No inline scripts
- No eval()
- No external resource loading without explicit allowlist

---

## 8. Privacy Considerations

### 8.1 Data Handling

Engines MUST NOT:
- Transmit composition data to external services
- Log sensitive input values
- Cache personal data without consent

### 8.2 Audit Trail

Engines SHOULD provide:
- Composition version history
- Rendering audit logs
- Data access logs

---

## Appendix A: JSON Schema

The normative JSON Schema for brick definitions is available at:
`schemas/brick.schema.json`

## Appendix B: Implementation

This specification is intentionally **implementation-agnostic**.

A conforming engine can be built in any language or framework:
- PHP, Python, Node.js, Go, Rust...
- React, Vue, Svelte, vanilla JS...
- Server-side, client-side, or hybrid

The specification is designed to be clear enough that **an AI system can generate a conforming engine from scratch**, validating the core principle: *AI assembles validated components, it doesn't generate arbitrary code.*

This is the ultimate proof of concept: if an AI can build the engine from the spec, the spec is complete.

## Appendix C: Changelog

### Version 1.0.0 (Draft)
- Initial specification

---

*This document is a Working Draft and subject to change.*
