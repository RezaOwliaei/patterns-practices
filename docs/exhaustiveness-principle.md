---
category: principle
tags: [design, types]
title: Exhaustiveness Principle
---

# Exhaustiveness Principle

## Summary

Explicitly handle every possible case or variant in a design so that adding cases fails fast at compile-time or test-time rather than producing silent runtime bugs.

## Why this matters

- Prevents silent regressions when new variants are introduced.
- Ensures maintainers consider every branch and its implications.

## When to use / Use-cases

- Switches over discriminated unions or enums.
- Error handling flows where missing cases should be surfaced.

## When not to use / Anti-signals

- For trivial one-off scripts where exhaustive handling is unnecessary overhead.

## How it works (concept)

Use language features (discriminated unions, exhaustive checks) or systematic patterns (Result/Either) so missing handling becomes a compile or test-time error.

## How to implement (practical steps)

1. Prefer discriminated unions or tagged enums for variant types.
2. Add a default branch that asserts `never` (or equivalent) so the compiler checks exhaustiveness.
3. Add unit tests that exercise each variant.

## Good Example (TypeScript) ✅

```ts
type Shape =
  | { kind: 'circle'; r: number }
  | { kind: 'rect'; w: number; h: number };

function area(s: Shape) {
  switch (s.kind) {
    case 'circle': return Math.PI * s.r * s.r;
    case 'rect': return s.w * s.h;
    default: {
      const _exhaustive: never = s; // compile-time check for missing cases
      return _exhaustive;
    }
  }
}
```

## Testing guidance

- Add unit tests that pass each variant to the handler.
- Use static type checks (TS, Flow) in CI to ensure no missing cases.

## Trade-offs and pitfalls

- Small overhead when adding a new variant (forcing you to update handlers) — this is intentional.

## Quick checklist ✅

- [ ] Are variants expressed as discriminated unions or enums?
- [ ] Is there a compile-time or test-time guard for missing cases?
- [ ] Are all variants tested?

## Related principles

- [Exhaustiveness Principle](exhaustiveness-principle.md)
