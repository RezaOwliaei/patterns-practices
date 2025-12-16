# Pit of Success (Idiom)

## Summary

Design APIs so that the easiest path for users is also the correct and safe one, making mistakes less likely.

## Why this matters

- Reduces common errors and encourages consistent, maintainable usage.
- Lowers onboarding friction for new users of libraries and frameworks.

## When to use / Use-cases

- Public libraries and framework APIs where defaults can provide robustness.

## When not to use / Anti-signals

- When the user base requires maximal flexibility and defaults would cause performance or semantic surprises.

## How it works (concept)

Provide sensible defaults and opinionated helpers so that following conventions yields correct behavior; expose opt-outs for edge cases.

## Example (TypeScript)

```ts
type Options = { timeout?: number; headers?: Record<string,string> };
function fetchJson(url: string, opts: Options = {}) {
  const headers = { 'Accept': 'application/json', ...(opts.headers || {}) };
  const timeout = opts.timeout ?? 5000;
  // perform fetch with defaults (omitted implementation)
}

// consumers that follow defaults get robust behaviour; opt-in changes require explicit opts
```

## Testing guidance

- Test default behavior and ensure opt-out code paths are covered.

## Trade-offs and pitfalls

- Opinionated defaults can annoy power users; provide clear escape hatches and document edge cases.

## Quick checklist ✅

- [ ] Are sensible defaults provided for common use-cases?
- [ ] Are escape hatches available and documented?
- [ ] Are defaults covered by tests?

## Related principles

- [Principle of Least Astonishment](principle-of-least-astonishment.md)
