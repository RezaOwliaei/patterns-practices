# Always Valid

Summary
- The "Always Valid" pattern/principle states that objects should be constructed and mutated so they always represent a valid state. Invalid states should be unrepresentable.

Motivation
- Avoids runtime checks scattered across the codebase.
- Shifts correctness to construction and domain rules.

How to enforce
- Validate in constructors/factory functions.
- Use private fields and only expose safe operations.

TypeScript example (factory + private constructor)
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

// consumers must use Email.create, so Email instances are always valid
const e = Email.create('alice@example.com');
```

TypeScript example (aggregate with invariants)
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

Benefits
- Fewer validation sites, predictable invariants, easier reasoning and testing.

Trade-offs
- More upfront design. Sometimes requires factory functions or discriminated unions for complex validation.

Notes
- Consider Result/Option wrappers for languages or codebases that prefer non-throwing flows.
