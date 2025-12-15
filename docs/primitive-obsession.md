# Primitive Obsession (Anti-Pattern)

Summary
- Primitive Obsession is the anti-pattern of overusing primitive types (strings, numbers) instead of creating domain-specific types, leading to duplicated validation, unclear intent, and brittle code.

Symptoms
- Many functions take raw primitives for domain concepts (email: string, money: number).
- Scattered validation logic and duplicated checks.

TypeScript example (anti-pattern)
```ts
function sendInvoice(toEmail: string, amountCents: number) {
  // validate email/amount in many places
}
```

Refactor (use Value Objects)
```ts
class Email {
  private constructor(public readonly value: string) {}
  static create(v: string) { if (!/^[^@\s]+@[^@\s]+\.[^@\s]+$/.test(v)) throw new Error('invalid'); return new Email(v); }
}

class Money {
  constructor(public readonly cents: number, public readonly currency = 'USD') {
    if (!Number.isInteger(cents) || cents < 0) throw new Error('invalid money');
  }
}

function sendInvoice(to: Email, amount: Money) {
  // callers must construct AM and Email, so values are validated once
}
```

Benefits of fixing
- Centralized validation, clearer APIs, fewer bugs, and improved domain model expressiveness.

When not to refactor
- For throwaway code or trivial scripts where overhead outweighs benefit.
