# Phantom Property (TypeScript)

## Summary

A phantom property is a type-only property added to a type or interface to carry compile-time metadata or a marker; it is not relied on at runtime.

## Why this matters

- Provides a simple way to differentiate otherwise-identical types.
- Helpful for disambiguation, nominal typing, or carrying state-level markers.

## When to use / Use-cases

- Distinguishing two objects with the same shape but different semantics.
- Embedding a compile-time-only marker for libraries or type-level utilities.

## When not to use / Anti-signals

- When you need runtime checking or when the marker would cause runtime collisions.

## How it works (concept)

Add a property that is only present in the type system (often optional, `readonly`, and with a type like `never` or a branded symbol). Since TypeScript erases types at runtime, the property doesn’t affect emitted JS.

## How to implement (practical steps)

1. Choose a name for the phantom property (e.g., `__brand`, `__phantom`) or better, use a `unique symbol` constant.
2. Keep it optional or `readonly` and give it a marker type (prefer `unique symbol` over strings — see reasons below).
3. Use factories or type aliases to create instances of the marked type; avoid setting the phantom property at runtime.

## Best practices

- **Prefer `unique symbol` over string keys**: unique symbols are unforgeable and cannot be collided with by accident, which makes them safer for cross-module libraries.
- **Keep phantom properties type-only**: do not write or depend on them at runtime; they should only exist to guide the type system.
- **Centralize creation**: create instances through factories that validate input and hide any unsafe `as` casts.

### Example: using a `unique symbol` for a phantom property

```ts
// recommended approach
declare const USER_BRAND: unique symbol;

interface User { id: string; name: string; readonly [USER_BRAND]?: true }
interface Admin { id: string; name: string; readonly [USER_BRAND]?: false }

function isUser(u: User | Admin): u is User { return (u as any)[USER_BRAND] !== false }
```

**Why `unique symbol`?**

- Symbols are unique at the language level and cannot be created by accident from strings.
- Using `unique symbol` avoids accidental name collisions and reduces the chance that runtime code will set or depend on the phantom property.

## Bad Example (TypeScript) ❌

```ts
// two seemingly identical types — easy to mix up
interface User { id: string; name: string }
interface Admin { id: string; name: string }
```

## Good Example (TypeScript) ✅

```ts
declare const USER_BRAND: unique symbol;

interface User { id: string; name: string; readonly [USER_BRAND]?: true }
interface Admin { id: string; name: string; readonly [USER_BRAND]?: false }

function isUser(u: User | Admin): u is User { return (u as any)[USER_BRAND] !== false }
```

> Note: Keep phantom properties out of runtime code — use them for type-level discrimination only.

## Testing guidance

- Test behavior of factories and runtime logic that depends on type guards; type-level differences can be asserted using tools like `tsd`.

## Trade-offs and pitfalls

- Naming collisions or misuse can lead to brittle code if you begin to rely on phantom properties at runtime.

## Quick checklist ✅

- [ ] Phantom property names are unique and unlikely to collide.
- [ ] No runtime code writes or depends on the phantom property.
- [ ] Type tests cover the intended discrimination.

## Related principles

- [Phantom Types](phantom-types.md)
- [Branded Types (TypeScript)](branded-types.md)
