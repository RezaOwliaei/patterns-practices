# Principle of Least Astonishment

Summary
- The Principle of Least Astonishment (POLA) means software should behave in ways users (or other developers) reasonably expect.

How to apply
- Choose intuitive defaults, consistent naming, and predictable side-effects.
- Prefer explicit over implicit when ambiguity would surprise callers.

TypeScript examples
```ts
// surprising API (mutates global state silently)
function addToCache(key: string, value: any) { globalCache[key] = value; }

// less surprising: return a new cache or clearly document mutation
function withAddedCache(cache: Record<string, any>, key: string, value: any) {
  return { ...cache, [key]: value };
}
```

Benefits
- Fewer bugs, easier onboarding, better API adoption.

Trade-offs
- Sometimes ergonomics compete with explicitness — prefer consistency and good docs.
