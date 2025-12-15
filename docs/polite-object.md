# Polite Object (Idiom)

Summary
- A "Polite Object" is an idiom for designing objects that behave well in an OO system: they present clear, intention-revealing APIs, avoid surprising side-effects, and cooperate with callers instead of exposing internals.

Practices
- Keep methods small and focused.
- Return intention-bearing types rather than raw internals.
- Avoid exposing mutable internal structures directly.

TypeScript example (bad)
```ts
class Session { public data: any = {}; }
// external code mutates session.data arbitrarily
```

TypeScript example (polite)
```ts
class Session {
  private data: Record<string, unknown> = {};
  get<T>(key: string): T | undefined { return this.data[key] as T | undefined; }
  set(key: string, value: unknown) { this.data[key] = value; }
  clear() { this.data = {}; }
}

// API speaks intent and keeps internal structure private
```

Benefits
- Easier reasoning, safer composition, and fewer accidental invariants violated by consumers.
