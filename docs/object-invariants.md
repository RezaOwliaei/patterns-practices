---
category: principle
tags: [design, safety]
title: Object Invariants
---

# Object Invariants

## Summary

Object invariants are rules that must always hold for an object's state (e.g., non-negative balance, valid email format).

## Why this matters

- Invariants ensure object validity across operations and improve system correctness.
- They make it possible to reason about behavior without checking conditions everywhere.

## When to use / Use-cases

- Core domain objects with correctness constraints (percentages, balances, identifiers).

## When not to use / Anti-signals

- For transient DTOs or data-transfer shapes where validation is performed upstream.

## How it works (concept)

Enforce invariants at creation and mutation points; use guardians to coordinate multi-object invariants when necessary.

## How to implement (practical steps)

1. Validate in constructors or factory methods.
2. Restrict mutators (use private setters or guarded public methods).
3. Use guardians/aggregate roots for invariants spanning multiple objects.

## Example (TypeScript)

```ts
class Percentage {
  private constructor(private readonly value: number) {}
  static create(value: number) {
    if (value < 0 || value > 100) throw new Error('percentage out of range');
    return new Percentage(value);
  }
  get() { return this.value; }
}

const p = Percentage.create(42);
```

## Testing guidance

- Unit test constructors/factories and public operations that may break invariants.
- Add integration tests for guardian-coordinated transitions.

## Trade-offs and pitfalls

- Overly strict invariants can make refactoring harder; evaluate cost vs benefit.

## Quick checklist ✅

- [ ] Are core invariants enforced at creation and mutation?
- [ ] Is cross-object coordination handled by a guardian when needed?
- [ ] Are invariants covered by tests?

## Related principles

- [Always Valid](always-valid.md)
- [Guardian (Invariant Protection)](guardian-invariant-protection.md)
