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

### Clean API

A clean approach separates concerns. The `ReportGenerator` class provides a single, high-level method, hiding the implementation details.

```ts
// Internal helpers are not exported or are private
const fetchJson = (url: string): Promise<any> => { /* ... */ };
const parseData = (data: any): string[] => { /* ... */ };
const formatAsCsv = (lines: string[]): string => { /* ... */ };

// The class has a single, clear responsibility
class CleanReportGenerator {
  // One clear, high-level method
  async generate(url: string): Promise<string> {
    const data = await fetchJson(url);
    const lines = parseData(data);
    return formatAsCsv(lines);
  }
}
```

## Related Principles

- **[Encapsulation](encapsulation.md)**: Polluting an API often involves breaking encapsulation by exposing internal details.
- **[Single Responsibility Principle](https://en.wikipedia.org/wiki/Single-responsibility_principle)**: API pollution is a direct symptom of a class or module having too many responsibilities.
- **[Principle of Least Astonishment](principle-of-least-astonishment.md)**: A clean, minimal API is predictable, whereas a polluted one is confusing.
- **[Pit of Success](pit-of-success.md)**: A well-designed, unpolluted API makes it easy to do the right thing and hard to do the wrong thing.
