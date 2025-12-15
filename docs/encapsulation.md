# Encapsulation

Summary
- Encapsulation bundles data and the methods that operate on that data, hiding internal representation behind a well-defined interface.

Why it matters
- Limits coupling, clarifies responsibilities, and enables safe changes to internals.

TypeScript example
```ts
class Rectangle {
  constructor(private width: number, private height: number) {}

  area() { return this.width * this.height; }

  scale(factor: number) { this.width *= factor; this.height *= factor; }
}

const r = new Rectangle(3, 4);
console.log(r.area());
r.scale(2);
```

Good encapsulation practices
- Use private/protected fields where available.
- Expose behavior, not representations.
- Prefer methods that express domain intent (e.g., `deposit()` vs `setBalance()`).

When to break encapsulation
- For performance-critical internals after careful profiling; prefer explicit API for fast paths.
