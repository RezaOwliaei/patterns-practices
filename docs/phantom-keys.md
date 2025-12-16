# Phantom Keys (TypeScript)

## Summary

Phantom keys are type-level-only keys added to object shapes or mapped types to convey compile-time information (e.g., roles, permissions, or flags) without adding runtime properties.

## Why this matters

- Allow compile-time tracking of capabilities or metadata while keeping runtime objects minimal.
- Provide safer APIs when certain keys express capabilities that should be validated by the type system.

## When to use / Use-cases

- Tagging objects with compile-time capabilities (e.g., `HasRead`, `HasWrite`).
- Enforcing compile-time constraints for configuration objects that are validated at startup.

## When not to use / Anti-signals

- When runtime inspection of keys is required.
- When it complicates type signatures excessively for little safety gain.

## How it works (concept)

Use mapped types and optional phantom keys (often typed as `never` or optional branded symbols) to represent capabilities in the type system. At runtime these keys can be absent or unused.

## How to implement (practical steps)

1. Create a union of capability keys (string literal types) or define symbol markers for capabilities.
2. Create mapped utility types that optionally add those keys with a phantom type (often `never` or an optional `unique symbol` index).
3. Use conditional types to narrow or enforce capability-bearing shapes.

## Best practices

- Prefer `unique symbol` markers for phantom keys when possible instead of plain string keys to avoid accidental collisions and runtime leakage.
- Keep phantom keys type-only and centralize any `as` casts or factories in a single module.
- Add `tsd` type-level tests to ensure incompatible capability combinations are rejected at compile time.

### Example: string key vs unique symbol

**String-based (less safe)**

```ts
// string-based phantom keys can collide and may appear at runtime
type Capabilities = 'canRead' | 'canWrite';

type WithCaps<C extends Capabilities> = { name: string } & { [K in C]?: never };

type ReadOnly = WithCaps<'canRead'>;

const r: ReadOnly = { name: 'x', canRead: undefined } // runtime key exists; less safe
```

**Symbol-based (recommended)**

```ts
// use a unique symbol for the phantom key — cannot be set accidentally at runtime
declare const CAN_READ: unique symbol;

type WithCapSym = { name: string } & { readonly [CAN_READ]?: true };

type ReadOnlySym = WithCapSym;

const r2: ReadOnlySym = { name: 'x' } // safe; no accidental runtime key
```

Using `unique symbol` reduces the risk that consumers accidentally set or rely on runtime keys and helps provide stronger nominal distinctions at the type level.

## Bad Example (TypeScript) ❌

```ts
// encoding capabilities as runtime keys increases surface area
const obj = { name: 'x', canRead: true, canWrite: false }
```

## Good Example (TypeScript) ✅

```ts
type Capabilities = 'canRead' | 'canWrite';

type WithCaps<C extends Capabilities> = { name: string } & { [K in C]?: never };

// Type-level examples
type ReadOnly = WithCaps<'canRead'>;

type Writer = WithCaps<'canWrite'>;

// At runtime, the object remains minimal and capabilities inform type checks
const r: ReadOnly = { name: 'x' };
```

## Testing guidance

- Add type-level tests where the compiler must accept/reject certain capability combinations.
- Test runtime validation paths that depend on capability contracts.

## Trade-offs and pitfalls

- Complex type-level machinery can be hard to maintain; prefer simpler alternatives when possible.
- Phantom keys don't exist at runtime—if you need runtime checks, add explicit validators.

## Quick checklist ✅

- [ ] Are phantom keys used for compile-time concerns only?
- [ ] Are accompanying runtime validators added when needed?
- [ ] Are type-level tests present for important constraints?

## Related principles

- [Phantom Types](phantom-types.md)
- [Phantom Property](phantom-property.md)
