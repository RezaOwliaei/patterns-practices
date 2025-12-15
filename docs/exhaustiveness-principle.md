# Exhaustiveness Principle

Summary
- The Exhaustiveness Principle requires explicit handling of all possible cases or conditions in a design (e.g., all enum variants, all error branches).

Benefits
- Prevents silent failures when new cases are introduced, and encourages deliberate handling of unexpected input.

TypeScript example (exhaustive switch)
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

Patterns
- Use discriminated unions, Result/Either types, and exhaustive checks during compile time.
