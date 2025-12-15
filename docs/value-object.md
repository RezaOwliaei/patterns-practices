# Value Object (VO) Pattern

Summary
- Value Objects are small, immutable objects defined by their attributes (value equality), not identity. They represent descriptive aspects of the domain (e.g., money, coordinates, email).

Characteristics
- Immutable, replaceable, equality based on contained data, often lightweight.

TypeScript example
```ts
class Money {
  constructor(public readonly cents: number, public readonly currency: string) {}

  equals(other: Money) {
    return this.cents === other.cents && this.currency === other.currency;
  }

  add(other: Money) {
    if (this.currency !== other.currency) throw new Error('currency mismatch');
    return new Money(this.cents + other.cents, this.currency);
  }
}

const a = new Money(1000, 'USD');
const b = a.add(new Money(250, 'USD'));

console.log(a.equals(b)); // false
```

Usage
- Use VOs for types that have no identity, for safer APIs, and to avoid primitive obsession.

Benefits
- Clearer intent, fewer errors (units, currency), easier testing, thread-safety due to immutability.
