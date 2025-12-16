---
category: anti-pattern
tags: [design]
title: Primitive Obsession (Anti-Pattern)
---

# Primitive Obsession (Anti-Pattern)

## Summary

Primitive Obsession is the habit of overusing raw primitives (strings, numbers) for domain concepts instead of modeling them as domain types, leading to duplicated validation and unclear intent.

## Why this matters

- Leads to scattered validation and duplicated checks.
- Makes intent less obvious and code more brittle.

## When to use / Use-cases

- Replace primitives with domain types when the value has domain-specific rules (email, money, id formats).

## When not to use / Anti-signals

- Throwaway scripts or small throwaway code where the cost of creating types outweighs the benefit.

## How it works (concept)

Introduce Value Objects or small domain types that encapsulate validation and behavior so the rest of the codebase deals with meaningful types instead of raw primitives.

## Bad Example (TypeScript) ❌

```ts
function sendInvoice(toEmail: string, amountCents: number) {
  // validate email/amount in many places
}
```

## Good Example (TypeScript) ✅

```ts
class Email {
  private constructor(public readonly value: string) {}
  static create(v: string) {
    if (!/^[^@\s]+@[^@\s]+\.[^@\s]+$/.test(v)) throw new Error("invalid");
    return new Email(v);
  }
}

class Money {
  constructor(
    public readonly cents: number,
    public readonly currency = "USD",
  ) {
    if (!Number.isInteger(cents) || cents < 0) throw new Error("invalid money");
  }
}

function sendInvoice(to: Email, amount: Money) {
  // callers must construct Money and Email, so values are validated once
}
```

## Testing guidance

- Test the Value Object constructors and behavior; fewer tests are needed elsewhere because values are validated up-front.

## Trade-offs and pitfalls

- Increased type/count of small classes; balance cost vs benefit for each domain concept.

## Quick checklist ✅

- [ ] Is domain-specific validation centralized in a single type?
- [ ] Are primitives replaced with Value Objects where they carry domain semantics?
- [ ] Are Value Objects thoroughly unit-tested?

## Related principles

- [Value Object](../patterns/value-object.md)
