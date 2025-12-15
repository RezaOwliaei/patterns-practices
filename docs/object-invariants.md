# Object Invariants

Summary
- Object invariants are rules that must always hold true for an object's state (e.g., non-negative balance, valid email address).

Enforcement techniques
- Check in constructors/factories.
- Keep mutators private or validate inside them.
- Use guardian objects or aggregate roots to coordinate changes across multiple objects.

TypeScript example (invariant inside class)
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

Invariants spanning multiple objects
- Use a higher-level guardian (see Guardian pattern) to validate transitions across multiple entities in a single transaction.

Testing invariants
- Unit test constructors/factories and all public mutators to ensure invariant preservation.
