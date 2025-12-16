---
category: pattern
tags: [design, types]
title: Value Object (VO) Pattern
---

# Value Object (VO) Pattern

## Summary

Value Objects are small, immutable types defined by their attributes (value equality) rather than identity; examples include money, email, and coordinates.

## Why this matters

- Makes equality explicit and avoids identity-related bugs.
- Encapsulates domain-specific behavior and validation near the value.

## When to use / Use-cases

- For domain concepts that have no identity and are defined purely by their data (money, measurements, email addresses).

## When not to use / Anti-signals

- For entities that require identity (users, orders) or where mutability is a necessary feature.

## How it works (concept)

Make the type immutable, implement value-based equality, and provide behavior that lives on the value (e.g., Money.add). Keep validation and representation consistent inside the VO.

## Example (TypeScript)

```ts
class Money {
  constructor(
    public readonly cents: number,
    public readonly currency: string,
  ) {}

  equals(other: Money) {
    return this.cents === other.cents && this.currency === other.currency;
  }

  add(other: Money) {
    if (this.currency !== other.currency) throw new Error("currency mismatch");
    return new Money(this.cents + other.cents, this.currency);
  }
}

const a = new Money(1000, "USD");
const b = a.add(new Money(250, "USD"));

console.log(a.equals(b)); // false
```

## Testing guidance

- Test equality, immutability, and domain behavior (e.g., add, conversions).

## Trade-offs and pitfalls

- Many small classes can increase code volume; balance between explicit domain types and pragmatism.

## Quick checklist ✅

- [ ] Is the VO immutable and tested for equality?
- [ ] Does it encapsulate validation and domain behavior?
- [ ] Is currency/unit mismatch handled explicitly?

## Related principles

- [Primitive Obsession](../anti-patterns/primitive-obsession.md)
