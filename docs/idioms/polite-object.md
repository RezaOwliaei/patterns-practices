---
category: idiom
tags: [design]
title: Polite Object (Idiom)
---

# Polite Object (Idiom)

## Summary

Polite Objects present clear, intention-revealing APIs, avoid surprising side-effects, and cooperate with callers rather than exposing internals.

## Why this matters

- Makes code easier to reason about and compose safely.
- Prevents consumers from accidentally violating invariants.

## When to use / Use-cases

- Public-facing objects or APIs that will be used by many parts of the codebase.

## When not to use / Anti-signals

- Simple data carriers or DTOs where exposing data is intentional and light-weight.

## How it works (concept)

Design methods that express intent and avoid returning mutable internal structures. Prefer small, focused methods and intention-bearing return types.

## Bad Example (TypeScript) ❌

```ts
class Session {
  public data: any = {};
}
// external code mutates session.data arbitrarily
```

## Good Example (TypeScript) ✅

```ts
class Session {
  private data: Record<string, unknown> = {};
  get<T>(key: string): T | undefined {
    return this.data[key] as T | undefined;
  }
  set(key: string, value: unknown) {
    this.data[key] = value;
  }
  clear() {
    this.data = {};
  }
}

// API speaks intent and keeps internal structure private
```

## Testing guidance

- Test behavior through the public API (get/set/clear) and ensure no external mutation can change internals unexpectedly.

## Trade-offs and pitfalls

- Over-encapsulation for trivial cases may add unnecessary complexity.

## Quick checklist ✅

- [ ] Does API express intent rather than raw data access?
- [ ] Are mutable internals protected from callers?
- [ ] Are behaviors tested through public methods?

## Related principles

- [Encapsulation](../principles/encapsulation.md)
