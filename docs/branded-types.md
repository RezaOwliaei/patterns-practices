---
category: ts-pattern
tags: [typescript, types]
title: Branded Types (TypeScript)
---

# Branded Types (TypeScript)

## Summary

Branded types add nominal (opaque) typing to TypeScript by combining primitives with a unique brand so values that share the same underlying primitive aren't accidentally mixed.

## Why this matters

- Prevents accidental mixing of semantically different values that use the same primitive (e.g., `UserId` vs `OrderId`).
- Makes intent explicit in function signatures and APIs.
- Helps catch class-of-bugs at compile time instead of runtime.

## When to use / Use-cases

- Identifiers and external keys that are represented as the same primitive type (string, number) but must not be interchanged.
- APIs where additional type-safety improves developer confidence and reduces accidental misuse.

## When not to use / Anti-signals

- Small throwaway scripts or prototypes where boilerplate outweighs benefit.
- When runtime enforcement of types is required (branded types are erased at runtime).

## How it works (concept)

A branded type is an intersection between a primitive and an opaque "brand" object. TypeScript uses the brand to distinguish the new type at compile time, but at runtime the value is the original primitive.

## How to implement (practical steps)

1. Define a unique brand (use `unique symbol` or a literal brand key).
2. Define your branded alias by intersecting with the brand.
3. Provide constructors/factories that validate input and return the branded type (use `as` cast only inside validated factories).
4. Avoid unsafe casts outside of trusted factories; prefer distinct creators for conversion.

## Bad Example (TypeScript) ❌

```ts
function deleteUser(userId: string) {
  // caller could accidentally pass an orderId string
}
```

## Good Example (TypeScript) ✅

```ts
// brand definition
declare const USER_ID: unique symbol;
export type UserId = string & { readonly [USER_ID]: true };

// safe factory
export function createUserId(s: string): UserId {
  if (!/^u_\d+$/.test(s)) throw new Error('invalid user id');
  return s as UserId;
}

// usage
function deleteUser(userId: UserId) {
  // userId must be created via createUserId()
}

const id = createUserId('u_42');
deleteUser(id);
// deleteUser('u_42'); // Type error: string is not assignable to UserId
```

> Note: branded types are erased at runtime — `UserId` is still a `string` at runtime, so if you need runtime type safety consider wrapping in a small Value Object instead.

## Testing guidance

- Unit-test factories and validation logic thoroughly.
- Consider using a TypeScript type-testing tool (e.g., `tsd`) to assert type-level expectations where helpful.

## Trade-offs and pitfalls

- Brands are a compile-time-only safety net; they can be bypassed with `as` casts.
- Extra boilerplate for each domain primitive.
- Not a replacement for runtime validation when inputs come from untrusted sources.

## Quick checklist ✅

- [ ] Defined a unique brand for the type.
- [ ] Provided a validated factory for creating the branded value.
- [ ] Avoided unsafe casts in application code.
- [ ] Added tests for factory/validation logic.

## Related principles

- [Value Object](value-object.md) — for cases where runtime safety or added behavior is desirable
- [Primitive Obsession](primitive-obsession.md)
- [Always Valid](always-valid.md)
