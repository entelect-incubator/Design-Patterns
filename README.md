# Design Patterns Learning Hub

A comprehensive guide to design patterns, SOLID principles, clean code, and architectural patterns essential for building production-grade applications.

**📚 Full Documentation:** https://github.com/entelect-incubator/Design-Patterns

> This local folder provides quick navigation and references to each pattern. For detailed explanations, code examples, and language-specific implementations, refer to the GitHub repository.

---

## Why These Patterns Matter

You're three months into your first real project. The feature you built last week needs a change. You open the code and your heart sinks — it's everywhere. The logic you thought was isolated touches seven different files. Changing it means risking breakage in places you didn't even know existed. Your teammate mentions "we should have used the Repository pattern here," but you don't know what that means. You spend two days making what should have been a 30-minute change.

**This is the moment you realize patterns aren't academic theory — they're survival skills.**

### The Real Story: From Chaos to Clarity

When you understand these patterns from the foundation up, something shifts in how you think about code:

**Before patterns**, you see a problem and write code that solves it right now. It works. You ship it. Then requirements change. The database needs to swap from Postgres to MongoDB. Suddenly, SQL queries are hardcoded in 47 controllers. You spend a week refactoring, introducing bugs, and missing deadlines.

**After understanding Repository pattern**, you instinctively put data access behind an interface. When the database changes, you swap one implementation. 47 controllers don't even know it happened. The change takes an afternoon.

**Before SOLID principles**, you create a `UserService` class that handles authentication, email notifications, logging, database access, and validation. It's 2,000 lines long. Every time someone touches it, something breaks. Tests are impossible because you can't mock the email service without also mocking the database.

**After understanding Single Responsibility**, you see that `UserService` should do one thing. Authentication becomes `AuthenticationService`. Email becomes `NotificationService`. Each class is 200 lines, focused, testable. When email breaks, you know exactly where to look.

### Why Foundational Knowledge Changes Everything

Here's what separates developers who know patterns from those who just know syntax:

**Scenario 1: The 3 AM Production Bug**

Your API is throwing 500 errors. Users can't log in. You're on call.

- **Without Result Pattern**: Exceptions bubble up from 12 layers deep. Stack traces point to infrastructure code. You have no idea which business rule actually failed. You add try-catch blocks everywhere, guessing at the problem.

- **With Result Pattern**: Every operation returns `Result<T>`. You check the logs. The error message says exactly which validation failed and why. You fix the business logic in 10 minutes and go back to sleep.

**Scenario 2: Adding a New Feature**

Product wants "pizza recommendations" based on order history.

- **Without Feature Architecture**: You start adding code. Recommendation logic goes in... Controllers? Services? A new layer? You ask three teammates and get three answers. The code spreads across folders. Six months later, no one remembers where the recommendation logic lives.

- **With Feature Architecture**: You create `Features/Recommendations/`. Everything lives in one place: the query, handler, tests, DTOs. A junior developer finds it in 30 seconds. When recommendations need to change, you change one folder.

**Scenario 3: Debugging a Teammate's Code**

You inherit a project. The previous developer left the company. There's a bug in the order processing.

- **Without CQRS**: Business logic is mixed with database queries, validation, email sending, and logging. It's in one 500-line method. You can't test it. You can't understand it. You rewrite it from scratch.

- **With CQRS**: Commands are separated from queries. Each handler does one thing. You read `CreateOrderCommand`, see the flow, spot the bug in 15 minutes. The structure itself tells you what the code does.

### The Foundation Mindset

Understanding these patterns fundamentally changes how you approach problems:

1. **You see boundaries** where others see "just code" — data access, business logic, and API layers are separate concerns
2. **You think in abstractions** — interfaces over implementations, enabling testability and flexibility
3. **You anticipate change** — code organized by feature withstands requirement shifts better than code organized by technology
4. **You communicate clearly** — when you say "Repository pattern," your team instantly understands the structure, dependencies, and trade-offs

### This Isn't About Being "Smart" — It's About Being Prepared

These patterns emerged from decades of developers encountering the same problems:

- "How do I change this without breaking everything?" → **SOLID Principles**
- "How do I test this without spinning up a database?" → **Repository Pattern**
- "How do I handle errors without try-catch hell?" → **Result Pattern**
- "How do I organize code so teams don't collide?" → **Feature Architecture**
- "How do I add logging without touching 100 files?" → **Dispatcher/Mediator**

**You're not learning patterns to show off.** You're learning them so when you face these problems — and you will — you recognize them instantly and know exactly what to do.

### The Real Career Impact

Six months from now, you'll be in a design discussion. Someone suggests an approach. Because you understand Repository pattern, you'll see the tight coupling to the database. You'll suggest an interface. The team avoids a three-week refactor down the line.

A year from now, you'll interview at a company that uses vertical slice architecture. Instead of being confused, you'll recognize it instantly as Feature Architecture. You'll talk intelligently about cohesion, boundaries, and team ownership. You get the offer.

**This is why patterns matter.** Not because they make code "elegant." But because they make you the developer who:

- Fixes bugs in minutes, not days
- Adds features without fear
- Writes code that teammates understand six months later
- Anticipates problems before they become fires
- Grows from junior to senior not by memorizing syntax, but by recognizing patterns in chaos

Master these foundations, and you'll never look at code the same way again.

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