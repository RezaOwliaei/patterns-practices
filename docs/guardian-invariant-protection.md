# Guardian (Invariant Protection)

## Summary

A Guardian (often an Aggregate Root in DDD) coordinates operations across related objects and enforces invariants at the aggregate level.

## Why this matters

- Ensures consistency when operations span multiple entities.
- Reduces scattered validations and accidental invariant violations.

## When to use / Use-cases

- Operations that affect multiple domain objects and require consistent business rules (orders with multiple lines, transactions).

## When not to use / Anti-signals

- Small, independent objects where coordination is unnecessary or would add complexity.

## How it works (concept)

The guardian is the authoritative coordinator of state transitions for a group of related objects. It exposes guarded operations that maintain aggregate-level invariants and hides low-level mutators.

## How to implement (practical steps)

1. Identify the aggregate boundary and the invariants that must hold across it.
2. Make mutations go through the guardian; avoid direct access to child entities from outside.
3. Validate combined state transitions within the guardian before committing changes.

## Example (TypeScript)

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

## Testing guidance

- Test multi-entity operations as integration-style unit tests to ensure the guardian preserves invariants.

## Trade-offs and pitfalls

- Can become a god object if it takes on too many responsibilities — keep guardians focused on invariants and coordination.

## Quick checklist ✅

- [ ] Is there a clear aggregate boundary?
- [ ] Do external components perform operations via the guardian, not by mutating children directly?
- [ ] Are cross-entity invariants covered by tests?

## Related principles

- [Object Invariants](object-invariants.md)
- [Tell, Don't Ask](tell-dont-ask.md)
