# Software Design Principles and Patterns

This repository is a collection of software design principles and patterns, explained in a simple and practical way. The goal is to provide a quick reference for developers to understand and apply these concepts in their daily work.

## Why?

As "vibe coding" is becoming a habit, we need to take more care of quality. This repository aims to provide a structured way to learn and apply established principles and patterns to improve the quality of our software.

Each principle or pattern has its own document with a detailed explanation, examples, and use cases.

## Principles and Patterns

Here is the list of principles and patterns currently documented:

*   [Always-Valid Principle](docs/always-valid.md)
*   [Encapsulation](docs/encapsulation.md)
*   [Exhaustiveness Principle](docs/exhaustiveness-principle.md)
*   [Guardian (Invariant Protection)](docs/guardian-invariant-protection.md)
*   [Object Invariants](docs/object-invariants.md)
*   [Pit of Success](docs/pit-of-success.md)
*   [Polite Object](docs/polite-object.md)
*   [Principle of Least Astonishment](docs/principle-of-least-astonishment.md)
*   [Tell, Don't Ask](docs/tell-dont-ask.md)
*   [Value Object](docs/value-object.md)

## Anti-Patterns

*   [API Pollution](docs/api-pollution.md)
*   [Primitive Obsession](docs/primitive-obsession.md)

## Documentation Template

To maintain consistency across all documents, please use the following template when creating new principle or pattern documents.

### Instructions

1.  Create a new markdown file in the `docs/` directory (e.g., `docs/new-pattern.md`).
2.  Use the template below as a starting point.
3.  Fill in each section with relevant information about the principle or pattern.
4.  Keep explanations concise and provide clear, practical code examples.
5.  Add a link to the new document in the main `README.md` file under the appropriate section.

### Template

```markdown
# [Principle or Pattern Name]

## Summary

A brief, one-sentence summary of the concept.

## Key Symptoms / Why it matters

- A bulleted list of symptoms that this pattern addresses or reasons why this principle is important.
- Keep it short and to the point.

## How to Avoid / How to Implement

A numbered list of actionable steps to avoid the anti-pattern or implement the pattern.

## Example

A code block with a clear "before" (polluted/bad) and "after" (clean/good) example. Use TypeScript for consistency.

### Bad Example

```ts
// Code demonstrating the problem
```

### Good Example

```ts
// Code demonstrating the solution
```

## Related Principles

- A bulleted list of links to related principles or patterns within this repository.
- Example: `[Encapsulation](encapsulation.md)`
```

## Contributing

Contributions are welcome! If you want to add a new principle or pattern, or improve an existing one, please open an issue or submit a pull request. Before adding a new principle or pattern, please check the [TODO list](todo.md) to see if it's already planned.