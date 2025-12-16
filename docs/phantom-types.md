# Phantom Types (TypeScript)

## Summary

Phantom types use type parameters or unused type-level markers to carry extra compile-time information without changing runtime representation.

## Why this matters

- Provide stronger type-safety for logically distinct variants that share the same runtime shape.
- Catch mix-ups (e.g., different units, states, or IDs) at compile time.
- No runtime cost—useful in performance-sensitive code.

## When to use / Use-cases

- Distinguishing units of measure (meters vs seconds).
- Representing state in state machines (e.g., `State<'Started'>`).
- Safe APIs that should prevent mixing semantically different primitives.

## When not to use / Anti-signals

- When runtime checks are required for untrusted inputs (phantom types are erased at runtime).
- Small throwaway code where extra typing adds unnecessary overhead.

## How it works (concept)

Introduce a generic parameter or a marker type that is only used for typing and never appears in emitted JavaScript. The compiler enforces distinctions but the runtime value remains the same as the underlying type.

## How to implement (practical steps)

1. Define a phantom marker type (usually an unused generic parameter or an intersection with a phantom interface).
2. Provide validated constructors/factories that assert the constraints and return the phantom type via a type cast in a localized spot.
3. Prefer `tsd` or other type-tests to document expected type relationships.

## Best practices

- Prefer `unique symbol` markers or branded aliases when you need cross-module nominal distinctions; symbols reduce collision risk compared to string markers.
- Centralize unsafe `as` casts in factories and expose only the safe constructors and type aliases from a single module.
- Use `tsd` tests to assert assignability and to document the intended type constraints.

### Example: symbol-branded phantom type

```ts
// create a unique symbol to use as a phantom brand
declare const METER_MARK: unique symbol;
export type Meter = number & { readonly [METER_MARK]?: true };

export function createMeter(n: number): Meter {
  // validate if needed
  return n as Meter;
}

// usage
function addMeters(a: Meter, b: Meter) { return createMeter(a + b) }
```

Using a `unique symbol` brand gives you stronger nominal safety across modules compared to plain phantom markers while still incurring no runtime overhead.

## Bad Example (TypeScript) ❌

```ts
// mixing units is easy with plain numbers
function addMeters(a: number, b: number) { return a + b }
```

## Good Example (TypeScript) ✅

```ts
// phantom marker approach
type Phantom<T> = { readonly __phantom?: T };
export type Meter = number & Phantom<'Meter'>;
export type Second = number & Phantom<'Second'>;

function createMeter(n: number): Meter { return n as Meter }
function createSecond(n: number): Second { return n as Second }

function addMeters(a: Meter, b: Meter) { return createMeter(a + b) }

const m = createMeter(3);
const s = createSecond(5);
// addMeters(m, s); // Type error — prevents accidental mixing
```

## Testing guidance

- Unit-test factories and any runtime validation.
- Add small type-level tests with `tsd` to assert that incompatible phantom types are not assignable.

## Trade-offs and pitfalls

- Phantom types are compile-time-only and can be bypassed with casts.
- Requires discipline to centralize casts inside factories.

## Quick checklist ✅

- [ ] Phantom markers are defined and used consistently.
- [ ] Factories validate inputs and localize unsafe `as` casts.
- [ ] Type-level tests cover key invariants.

## Related principles

- [Branded Types (TypeScript)](branded-types.md)
- [Primitive Obsession](primitive-obsession.md)
