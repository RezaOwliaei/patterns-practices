# API Pollution

## Summary

API pollution is an anti-pattern where a module's public interface is cluttered with an excessive number of methods, properties, and types that are not cohesive or essential to its primary purpose. This enlarges the attack surface, increases the cognitive load on consumers, and makes the API difficult to use, understand, and maintain.

## Key Symptoms

- **Overly Large Interfaces**: A class or module has a vast number of public methods, many of which are for internal-like operations.
- **"Util" or "Helper" Dumping Grounds**: Generic modules like `StringUtil` or `DateHelper` that accumulate unrelated static methods.
- **Configuration Overload**: Objects that require a huge number of configuration options or flags to be instantiated or used.
- **Breaking Encapsulation**: Exposing internal state or methods that should be private, forcing consumers to understand the inner workings of the module.

## How to Avoid

1.  **Define a Single, Clear Responsibility**: Ensure every class and module has one primary job (Single Responsibility Principle).
2.  **Keep Interfaces Small and Cohesive**: A minimal, focused interface is easier to understand and use. Only expose what is essential for the module's primary function.
3.  **Favor Composition Over Inheritance**: Instead of creating "god objects" that do everything, compose smaller, focused objects together.
4.  **Use Private by Default**: Start with all methods as `private` and only make them `public` if there is a clear, justified need for a consumer to call them.
5.  **Be Intentional**: Only expose what you intend for consumers to use. Start small and add to the API only when a clear need arises.

## Example

### Polluted API

Here, a `Report` class is responsible for fetching, parsing, and formatting data. The API is polluted with low-level methods that expose internal steps.

```ts
class PollutedReport {
  // Unrelated low-level methods exposed
  fetchJson(url: string): Promise<any> { /* ... */ }
  parseData(data: any): string[] { /* ... */ }
  formatAsCsv(lines: string[]): string { /* ... */ }

  // The intended "main" feature is buried
  async generateCsvReport(url: string): Promise<string> {
    const data = await this.fetchJson(url);
    const lines = this.parseData(data);
    return this.formatAsCsv(lines);
  }
}
```

## Summary

API Pollution is the tendency to expose many small, poorly-named, or redundant API elements that confuse consumers and increase maintenance burden.

## Why this matters

- Large, unfocused public surfaces lead to more bugs and harder maintenance.
- Consumers may rely on unstable details if the API is cluttered.

## When to use / Use-cases

N/A — this is an anti-pattern to avoid.

## When not to use / Anti-signals

- If you find many one-off helpers or unclear method names on public types, you likely have API pollution.

## How it works (concept)

Keep public surfaces small, name carefully, and provide focused helpers or modules rather than sprawling APIs.

## How to implement (practical steps)

1. Audit the public surface and remove or consolidate rarely-used APIs.
2. Provide clear, intention-revealing names and group related behavior together.
3. Document stability and deprecate carefully with migration guidance.

## Bad Example (TypeScript) ❌

```ts
class Utils {
  static parse(a: string) {}
  static parseDate(a: string) {}
  static parseNumber(a: string) {}
  // many helpers with overlapping responsibilities
}
```

## Good Example (TypeScript) ✅

```ts
// expose focused modules that group related behavior
export const DateUtils = { parse: (s: string) => new Date(s) };
export const NumberUtils = { parse: (s: string) => Number(s) };
```

## Testing guidance

- Use API usage telemetry (if available) or code search to find rarely used API surfaces for consolidation.

## Trade-offs and pitfalls

- Removing APIs requires a migration plan; use deprecation phases when needed.

## Quick checklist ✅

- [ ] Is the public API surface focused and documented?
- [ ] Are helpers grouped by responsibility?
- [ ] Is there a deprecation policy for cleanup?

## Related principles

- [Principle of Least Astonishment](principle-of-least-astonishment.md)

# API Pollution
