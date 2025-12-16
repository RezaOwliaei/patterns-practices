# Tell, Don't Ask

## Summary

Tell, Don't Ask encourages sending commands to objects (telling them what to do) instead of querying their state and making decisions externally.

## Why this matters

- Keeps behavior and data together, reinforcing encapsulation.
- Reduces duplicated logic and coupling between components.

## When to use / Use-cases

- When an object can perform the operation that affects its state (e.g., carts, orders, aggregates).

## When not to use / Anti-signals

- Small utility structs or immutable value objects where querying and computing externally is fine.

## How it works (concept)

Expose command-like methods on the object so callers express intent (e.g., `increaseQty`) and let the object enforce its own invariants.

## Bad Example (TypeScript) ❌

```ts
class Cart {
  private items: { id: string; qty: number }[] = [];
  getItems() { return this.items; }
}

function increaseQty(cart: Cart, id: string) {
  const items = cart.getItems();
  const item = items.find(i => i.id === id);
  if (item) item.qty += 1; // external mutation
}
```

## Good Example (TypeScript) ✅

```ts
class Cart {
  private items: Map<string, number> = new Map();

  addItem(id: string, qty = 1) {
    this.items.set(id, (this.items.get(id) || 0) + qty);
  }

  increaseQty(id: string, delta = 1) {
    if (!this.items.has(id)) throw new Error('item not found');
    this.items.set(id, this.items.get(id)! + delta);
  }

  removeItem(id: string) { this.items.delete(id); }
}

const cart = new Cart();
cart.addItem('p1');
cart.increaseQty('p1'); // tell the cart what to do
```

## Testing guidance

- Test object commands directly and assert invariants, rather than testing external manipulations.

## Trade-offs and pitfalls

- May require more expressive interfaces and a small number of wrapper methods; keep them intentional and focused.

## Quick checklist ✅

- [ ] Are state-changing operations expressed as commands on the object?
- [ ] Are invariants enforced inside the object methods?
- [ ] Are command methods covered by tests?

## Related principles

- [Encapsulation](encapsulation.md)
- [Guardian (Invariant Protection)](guardian-invariant-protection.md)
