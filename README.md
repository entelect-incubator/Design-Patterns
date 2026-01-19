# Design Patterns Learning Hub

A comprehensive guide to design patterns, SOLID principles, clean code, and architectural patterns essential for building production-grade applications.

**📚 Full Documentation:** https://github.com/entelect-incubator/Design-Patterns

> This local folder provides quick navigation and references to each pattern. For detailed explanations, code examples, and language-specific implementations, refer to the GitHub repository.

---

## Core Pattern Categories

### 1. [SOLID Principles](./01-SOLID-Principles/README.md)
Foundation for designing flexible, maintainable code. All Incubators enforce SOLID from Phase 1.

- **S** – Single Responsibility Principle (SRP)
- **O** – Open/Closed Principle (OCP)
- **L** – Liskov Substitution Principle (LSP)
- **I** – Interface Segregation Principle (ISP)
- **D** – Dependency Inversion Principle (DIP)

**When used:** Every phase. Enforced in code review.

**[📖 Full SOLID Guide](https://github.com/entelect-incubator/Design-Patterns/tree/main/SOLID)**

---

### 2. [Result Pattern](./02-Result-Pattern/README.md)
Explicit error handling without exceptions. Surfaces domain errors and validation failures safely.

**When used:** Phase 2+. All operations return `Result<T>` for domain-level logic.

**[📖 Full Result Pattern Guide](https://github.com/entelect-incubator/Design-Patterns/tree/main/Result-Pattern)**

---

### 3. [CQRS Pattern](./03-CQRS-Pattern/README.md)
Separates command (write) and query (read) operations for clarity and scalability.

**When used:** Phase 2+. Commands → Handlers + Result; Queries → Handlers + data.

**[📖 Full CQRS Guide](https://github.com/entelect-incubator/Design-Patterns/tree/main/CQRS)**

---

### 4. [Dispatcher / Mediator](./04-Dispatcher-Mediator/README.md)
Routes commands/queries to appropriate handlers. Simplifies routing and handler discovery.

**When used:** Phase 1+. Framework-native (Spring Dispatcher, .NET MediatR, Express routing) or custom.

**[📖 Full Dispatcher/Mediator Guide](https://github.com/entelect-incubator/Design-Patterns/tree/main/Dispatcher-Mediator)**

---

### 5. [Repository Pattern](./05-Repository-Pattern/README.md)
Abstraction layer for data access. Enables in-memory testing and ORM swapping without code changes.

**When used:** Phase 1 (in-memory); Phase 3+ (ORM integration).

**[📖 Full Repository Pattern Guide](https://github.com/entelect-incubator/Design-Patterns/tree/main/Repository-Pattern)**

---

### 6. [Dependency Injection](./06-Dependency-Injection/README.md)
Loose coupling via constructor/setter injection. Frameworks (Spring, .NET, Nest) handle auto-wiring.

**When used:** Phase 1+. FromServices (.NET), @Inject (Spring), Depends (FastAPI).

**[📖 Full DI Guide](https://github.com/entelect-incubator/Design-Patterns/tree/main/Dependency-Injection)**

---

### 7. [Feature Architecture](./07-Feature-Architecture/README.md)
Organize by business features (vertical slices) rather than technical layers. Each feature contains all its code (domain, handlers, data access, API).

**When used:** Phase 1+. Features/ folder with CQRS handlers grouped by business capability.

**[📖 Full Feature Architecture Guide](https://github.com/entelect-incubator/Design-Patterns/tree/main/Feature-Architecture)**

---

### 8. [Clean Code Principles](./08-Clean-Code-Principles/README.md)
Naming, formatting, function length, testing, error handling, and maintainability best practices.

**When used:** Every phase. Enforced in code review.

**[📖 Full Clean Code Guide](https://github.com/entelect-incubator/Design-Patterns/tree/main/Clean-Code)**

---

### 9. [Testing Patterns](./09-Testing-Patterns/README.md)
Unit tests, integration tests, negative-path testing, and Testcontainers for real database validation.

**When used:** Phase 1+. Negative paths first.

**[📖 Full Testing Patterns Guide](https://github.com/entelect-incubator/Design-Patterns/tree/main/Testing-Patterns)**

---

## Quick References

### [Cheat Sheets](./Cheat-Sheets/README.md)

Fast lookup guides for busy developers:

- [SOLID Quick Reference](./Cheat-Sheets/SOLID-Quick-Ref.md) – 1-page SOLID summary
- [Pattern Decision Tree](./Cheat-Sheets/Pattern-Decision-Tree.md) – When to use which pattern
- [Language Comparison](./Cheat-Sheets/Language-Comparison.md) – Same pattern across C#, Java, Python, TypeScript

---

## Phase Progression

Patterns are introduced progressively across phases:

| Phase        | Core Patterns                                   | Focus                                            |
| ------------ | ----------------------------------------------- | ------------------------------------------------ |
| **Phase 1**  | SOLID, Result, Dispatcher, Clean Code           | Foundational architecture with in-memory storage |
| **Phase 2**  | CQRS, Dependency Injection, Repository (sync)   | Command/Query separation, validated handlers     |
| **Phase 3**  | ORM Integration, Real Repository, DI containers | Database persistence with type-safe queries      |
| **Phase 4**  | Transactions, Validation, Advanced CQRS         | Multi-step operations, business rule validation  |
| **Phase 5**  | Testing, Integration Tests, Testcontainers      | Comprehensive test coverage, CI/CD ready         |
| **Phase 6+** | Event-Driven Architecture, Background Jobs      | Clean code + feature slices at scale             |

**[📖 Detailed Phase Guide](https://github.com/entelect-incubator/Design-Patterns/blob/main/Phase-Progression.md)**

---

## How to Use This Hub

1. **Quick Lookup:** Browse folders above for pattern names and overview
2. **Deep Learning:** Follow GitHub links for detailed explanations, diagrams, and code examples
3. **Code Examples:** Check language-specific folders (csharp/, java/, python/, typescript/) in GitHub repo
4. **Incubator Links:** Each Incubator references patterns by phase:
   - [Node Incubator](../Node/README.md) – JavaScript/TypeScript patterns
   - [Python Incubator](../Python/README.md) – Python patterns
   - [Java Incubator](../Java/README.md) – Java patterns
   - [.NET Incubator](../.NET/README.md) – C# patterns

---

## Contributing

Found an issue or want to add an example? Open an issue or PR at:
**https://github.com/entelect-incubator/Design-Patterns**

---

## Key Resources

- **SOLID Principles**: https://github.com/entelect-incubator/Design-Patterns/tree/main/SOLID
- **CQRS Pattern**: https://github.com/entelect-incubator/Design-Patterns/tree/main/CQRS
- **Result Pattern**: https://github.com/entelect-incubator/Design-Patterns/tree/main/Result-Pattern
- **Feature Architecture**: https://github.com/entelect-incubator/Design-Patterns/tree/main/Feature-Architecture
- **Testing Patterns**: https://github.com/entelect-incubator/Design-Patterns/tree/main/Testing-Patterns

---

**Last Updated:** January 2026
**Related:** [Incubator Overview](../README.md) | [Intro Guide](../Intro/README.md)