---
category: principle
tags: [design]
title: Encapsulation
---

# Encapsulation

## Summary

Encapsulation bundles data and the methods that operate on it, hiding internal representation behind a well-defined interface.

## Why this matters

- Limits coupling between modules and classes.
- Clarifies responsibilities and makes replacing internals safe.
- Enables controlled mutation and clearer invariants.

## When to use / Use-cases

- Any domain object that manages its own state (bank accounts, carts, sessions).
- Libraries or modules exposing public APIs where internal representation may change.

## When not to use / Anti-signals

- Over-encapsulating for trivial data-only types adds friction without benefit.
- Premature encapsulation where clear value objects suffice.

## How it works (concept)

Expose behaviors (methods with clear intent) instead of raw internal state. Keep internals private or protected and present a small, intention-revealing surface.

## How to implement (practical steps)

1. Make fields private/protected where language features allow it.
2. Prefer methods that express domain actions (e.g., `deposit`, `scale`) over setters that expose representation.
3. Return intention-bearing results or copy immutable data where necessary to avoid exposing internals.

## Bad Example (TypeScript) ❌

```ts
class Rectangle {
  constructor(
    public width: number,
    public height: number,
  ) {}
}
// callers can mutate fields directly and assume representation
```

## Good Example (TypeScript) ✅

```ts
class Rectangle {
  constructor(
    private width: number,
    private height: number,
  ) {}
  area() {
    return this.width * this.height;
  }
  scale(factor: number) {
    this.width *= factor;
    this.height *= factor;
  }
}

const r = new Rectangle(3, 4);
console.log(r.area());
r.scale(2);
```

## Testing guidance

- Test public behaviors (area, scale) rather than internal fields.
- Use black-box tests to ensure invariants hold after public operations.

## Trade-offs and pitfalls

- Breaking encapsulation for perf should be justified by benchmarks.
- Excessive indirection to protect internals can lead to unwieldy APIs.

## Quick checklist ✅

- [ ] Are internal fields non-public where possible?
- [ ] Does API expose intentful methods rather than raw data?
- [ ] Are public behaviors covered by tests?

## Related principles

- [Polite Object](../idioms/polite-object.md)
- [Always Valid](always-valid.md)
