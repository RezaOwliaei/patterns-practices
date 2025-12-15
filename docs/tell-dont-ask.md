# Tell, Don't Ask

Summary
- The "Tell, Don't Ask" principle encourages sending commands to objects (telling them what to do) rather than querying their state and making decisions externally (asking).

Motivation
- Keeps behavior and data together (encapsulation).
- Prevents logic duplication across the codebase.
- Makes code more robust by preventing external components from assuming internal representation.

When to use
- Whenever an object can perform an operation that affects its own state.

TypeScript example (anti-pattern)
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

TypeScript example (Tell, Don't Ask)
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

Benefits
- Stronger encapsulation, fewer coupling points, clearer intent.

Trade-offs
- Requires designing richer object interfaces; may add small wrapper methods.

Further reading
- Apply with Command-like methods and aggregate roots in domain models.
