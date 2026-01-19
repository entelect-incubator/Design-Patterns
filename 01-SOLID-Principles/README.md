# SOLID Principles

The foundation of maintainable, scalable code. SOLID is enforced in all Incubator phases from day one.

## Overview

**S** – Single Responsibility Principle  
**O** – Open/Closed Principle  
**L** – Liskov Substitution Principle  
**I** – Interface Segregation Principle  
**D** – Dependency Inversion Principle  

---

## Detailed Guides

### 1. **Single Responsibility Principle (SRP)**
Each class/function should have one reason to change.

**[📖 Full SRP Guide](https://github.com/entelect-incubator/Design-Patterns/blob/main/SOLID/01-Single-Responsibility.md)**

---

### 2. **Open/Closed Principle (OCP)**
Open for extension, closed for modification. Use inheritance, composition, or strategy pattern.

**[📖 Full OCP Guide](https://github.com/entelect-incubator/Design-Patterns/blob/main/SOLID/02-Open-Closed.md)**

---

### 3. **Liskov Substitution Principle (LSP)**
Derived classes must be substitutable for base classes without breaking behavior.

**[📖 Full LSP Guide](https://github.com/entelect-incubator/Design-Patterns/blob/main/SOLID/03-Liskov-Substitution.md)**

---

### 4. **Interface Segregation Principle (ISP)**
Clients should not depend on interfaces they don't use. Keep interfaces minimal and focused.

**[📖 Full ISP Guide](https://github.com/entelect-incubator/Design-Patterns/blob/main/SOLID/04-Interface-Segregation.md)**

---

### 5. **Dependency Inversion Principle (DIP)**
Depend on abstractions, not concretions. High-level modules should not depend on low-level modules.

**[📖 Full DIP Guide](https://github.com/entelect-incubator/Design-Patterns/blob/main/SOLID/05-Dependency-Inversion.md)**

---

## When SOLID Matters

| Principle | Example Problem                                               | SOLID Solution                                             |
| --------- | ------------------------------------------------------------- | ---------------------------------------------------------- |
| SRP       | `UserService` handling auth, logging, email                   | Split into `AuthService`, `EmailService`, `LoggingService` |
| OCP       | Adding new payment method requires modifying switch statement | Create `IPaymentProvider` interface; add new class         |
| LSP       | Derived `Square` breaks `Rectangle` width/height invariant    | Don't inherit; use composition or separate hierarchy       |
| ISP       | `IRepository<T>` forces unwanted methods on consumers         | Split into `IReadRepository<T>` and `IWriteRepository<T>`  |
| DIP       | `OrderService` directly instantiates `DatabaseRepository`     | Inject `IRepository<Order>` via DI container               |

---

## Code Examples by Language

All SOLID principles with working examples:

- **C#**: https://github.com/entelect-incubator/Design-Patterns/tree/main/SOLID/csharp
- **Java**: https://github.com/entelect-incubator/Design-Patterns/tree/main/SOLID/java
- **Python**: https://github.com/entelect-incubator/Design-Patterns/tree/main/SOLID/python
- **TypeScript**: https://github.com/entelect-incubator/Design-Patterns/tree/main/SOLID/typescript

---

## How Incubators Apply SOLID

- **Phase 1**: Split domain → application → infrastructure layers (SRP). Use interfaces for Repository (ISP, DIP).
- **Phase 2**: Handlers depend on abstractions, not concretions (DIP). Repository interfaces are minimal (ISP).
- **Phase 3**: ORM integration respects SRP (entity models ≠ dto models). Service layer handles business logic (OCP).
- **Phase 4+**: Advanced CQRS and events follow SOLID for extensibility.

---

**[Full SOLID Repository](https://github.com/entelect-incubator/Design-Patterns/tree/main/SOLID)**

**[Back to Design Patterns Hub](../README.md)**
