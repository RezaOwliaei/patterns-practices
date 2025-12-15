# Pit of Success (Idiom)

Summary
- The "Pit of Success" is a design idiom that makes the correct way easy and the wrong way hard. The goal is to bias developers toward safe, maintainable choices.

Applications
- Frameworks, libraries, and APIs can be designed with sensible defaults and opinionated helpers.

TypeScript example (helpful defaults)
```ts
// small HTTP helper with sensible defaults
type Options = { timeout?: number; headers?: Record<string,string> };
function fetchJson(url: string, opts: Options = {}) {
  const headers = { 'Accept': 'application/json', ...(opts.headers || {}) };
  const timeout = opts.timeout ?? 5000;
  // perform fetch with defaults (omitted implementation)
}

// consumers that follow defaults get robust behaviour; opt-in changes require explicit opts
```

Benefits
- Reduces class of bugs, improves API ergonomics, and encourages consistent usage.

Trade-offs
- Opinionated APIs sometimes frustrate power users; expose escape hatches carefully.
