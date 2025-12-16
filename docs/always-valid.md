# Always Valid

## Summary

Objects should be constructed and mutated so they always represent a valid state; invalid states should be unrepresentable.

## Why this matters

- Reduces scattered runtime checks and duplicated validation logic.
- Makes reasoning about state simpler and tests more focused.
- Prevents whole classes of bugs by enforcing domain rules at construction/mutation time.

## When to use / Use-cases

- Domain types with strong invariants (email, money, percentages).
- Aggregates and entities where invalid state could lead to inconsistent behavior (bank accounts, orders).

## When not to use / Anti-signals

- Throwaway scripts or prototypes where the overhead of strict types outweighs short-term productivity.
- When runtime constraints force accepting partially-constructed data (e.g., streaming parsers) — prefer guarded builders or Result types.

## How it works (concept)

Validation and guarding happen at the boundaries: constructors, factories, and mutators. Consumers must use the safe creation APIs so any instance they receive already satisfies invariants.

## How to implement (practical steps)

1. Prefer private constructors and factory functions for validated creation.
2. Validate inputs early and return explicit errors (throws, Result/Either types, or Option types depending on language style).
3. Keep mutators private or validate inside public methods so invariants cannot be broken by consumers.
4. Use types to make invalid states unrepresentable (discriminated unions, sealed classes).

## Bad Example (TypeScript) ❌

```ts
class User { constructor(public email: string) {} }
// callers can construct User('not-an-email') and system will only fail later
```

## Good Example (TypeScript) ✅

```ts
class Email {
  private constructor(public readonly value: string) {}
  static create(value: string): Email {
    if (!/^[^@\s]+@[^@\s]+\.[^@\s]+$/.test(value)) {
      throw new Error('Invalid email');
    }
    return new Email(value);
  }
}

const e = Email.create('alice@example.com');
```

### Aggregate example (Bank account)

```ts
class BankAccount {
  private balance: number;
  constructor(initialCents: number) {
    if (initialCents < 0) throw new Error('initial must be >= 0');
    this.balance = initialCents;
  }
  deposit(cents: number) { if (cents <= 0) throw new Error('must be positive'); this.balance += cents; }
  withdraw(cents: number) { if (cents <= 0 || cents > this.balance) throw new Error('invalid withdraw'); this.balance -= cents; }
  getBalance() { return this.balance; }
}
```

## Testing guidance

- Unit-test factory/constructor validation paths and all public mutators.
- Include property-based tests when ranges or combinatorial invariants exist.

## Trade-offs and pitfalls

- Requires upfront design and may add boilerplate (factory methods or wrappers).
- Throwing vs returning errors should follow the project's error-handling style.

## Quick checklist ✅

- [ ] Are invalid states prevented at construction or mutation?
- [ ] Are public mutators validated or restricted?
- [ ] Are there unit tests for each invariant?

## Related principles

- [Object Invariants](object-invariants.md)
- [Value Object](value-object.md)
