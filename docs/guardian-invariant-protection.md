# Guardian (Invariant Protection)

Summary
- A Guardian is an entity responsible for protecting invariants across one or more objects. In Domain-Driven Design this often maps to an Aggregate Root.

Responsibilities
- Coordinate operations that span multiple objects.
- Ensure invariants and transactional consistency.

TypeScript example (aggregate root)
```ts
class OrderLine { constructor(public readonly sku: string, public qty: number) {} }

class Order {
  private lines: Map<string, OrderLine> = new Map();

  addLine(sku: string, qty: number) {
    if (qty <= 0) throw new Error('invalid qty');
    const existing = this.lines.get(sku);
    if (existing) existing.qty += qty;
    else this.lines.set(sku, new OrderLine(sku, qty));
  }

  removeLine(sku: string) {
    if (!this.lines.has(sku)) throw new Error('not found');
    this.lines.delete(sku);
  }

  getTotalItems() { return Array.from(this.lines.values()).reduce((s, l) => s + l.qty, 0); }
}

// `Order` is the guardian that ensures order-level invariants (non-negative qty, coherent totals)
```

Notes
- Guardians centralize domain rules and reduce scattered checks. They make it easier to reason about state transitions and invariants.
