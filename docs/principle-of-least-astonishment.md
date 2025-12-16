# Principle of Least Astonishment

## Summary

The Principle of Least Astonishment (POLA) states that software should behave in ways users and developers reasonably expect.

## Why this matters

- Reduces surprises and prevents subtle bugs caused by unexpected behavior.
- Improves API adoption through predictable semantics.

## When to use / Use-cases

- Public APIs, libraries, and code that will be used by many developers.

## When not to use / Anti-signals

- When low-level, highly-optimized code needs to trade off explicitness for performance (use explicit documentation and tests).

## How it works (concept)

Prefer intuitive defaults, consistent naming, and predictable side-effects. When ambiguity exists, choose explicit behavior or clearly document surprises.

## Bad Example (TypeScript) ❌

```ts
// surprising API (mutates global state silently)
function addToCache(key: string, value: any) { globalCache[key] = value; }
```

## Good Example (TypeScript) ✅

```ts
// less surprising: return a new cache or clearly document mutation
function withAddedCache(cache: Record<string, any>, key: string, value: any) {
  return { ...cache, [key]: value };
}
```

## Testing guidance

- Test documented behaviors and ensure no undocumented side-effects exist.

## Trade-offs and pitfalls

- Ergonomics can sometimes push toward implicit behavior; choose consistency and document intentionally.

## Quick checklist ✅

- [ ] Are defaults intuitive and documented?
- [ ] Are naming and behavior consistent across the API?
- [ ] Are surprising behaviors clearly documented and tested?

## Related principles

- [Pit of Success](pit-of-success.md)
