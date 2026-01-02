# Contributing to Bricks

Thank you for your interest in contributing to the Bricks specification!

## How to Contribute

### Reporting Issues

- Use GitHub Issues to report bugs or suggest improvements
- Check existing issues before creating a new one
- Provide clear descriptions and examples

### Proposing Changes

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/my-proposal`)
3. Make your changes
4. Submit a Pull Request

### Adding New Bricks

When proposing a new brick:

1. Create the schema file in the appropriate category
2. Include at least 2 examples
3. Add comprehensive tags
4. Document all inputs with descriptions
5. Consider edge cases and validation rules

**Brick Schema Template:**

```json
{
  "id": "my-brick",
  "version": "1.0.0",
  "category": "ui.composite",
  "description": "Clear description of what this brick does",
  "inputs": {
    "type": "object",
    "properties": {
      "requiredField": {
        "type": "string",
        "description": "What this field does"
      },
      "optionalField": {
        "type": "string",
        "default": "default value",
        "description": "What this field does"
      }
    },
    "required": ["requiredField"]
  },
  "tags": ["ui", "composite", "relevant", "tags"],
  "examples": [
    {
      "name": "Basic usage",
      "inputs": {
        "requiredField": "example value"
      }
    },
    {
      "name": "Advanced usage",
      "inputs": {
        "requiredField": "example",
        "optionalField": "custom value"
      }
    }
  ]
}
```

### Specification Changes

For changes to the core specification:

1. Open an issue first to discuss the change
2. Provide rationale and use cases
3. Consider backward compatibility
4. Update relevant documentation

## Code of Conduct

- Be respectful and constructive
- Focus on the technical merits
- Welcome newcomers

## Questions?

Open an issue with the "question" label.

---

*One brick at a time.*
