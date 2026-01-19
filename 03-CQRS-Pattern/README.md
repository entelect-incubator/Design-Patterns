# CQRS Pattern

**Command Query Responsibility Segregation** – Separate command (write) operations from query (read) operations for clarity, scalability, and explicit intent.

## Overview

### Commands (Write)
- Change system state (create, update, delete)
- Return `Result<T>` with created/updated entity or error
- Validated, transactional, audit-logged
- Example: `CreatePizzaCommand { name, price } → Result<Pizza>`

### Queries (Read)
- Fetch data without side effects
- Return typed data directly (no Result wrapper)
- Can use read-optimized models (projections)
- Example: `GetPizzaByIdQuery { id } → Pizza`

---

## Key Benefits

| Benefit                 | Explanation                                                                  |
| ----------------------- | ---------------------------------------------------------------------------- |
| **Explicit Intent**     | Code clearly shows reads vs. writes                                          |
| **Independent Scaling** | Read handlers can be optimized separately                                    |
| **Testing**             | Easy to test commands (check side effects) and queries (check return values) |
| **Async Messaging**     | Commands can queue for async processing; queries are immediate               |
| **Caching**             | Cache queries without invalidating command paths                             |

---

## Basic Implementation (Phase 2)

Separate command handlers from query handlers:

```
CreatePizza (Command)
├── Handler validates input
├── Applies business logic (check stock, calculate price)
├── Persists to repository
└── Returns Result<Pizza>

GetPizza (Query)
├── Handler fetches from repository
└── Returns Pizza (no Result wrapper)
```

---

## Advanced Implementation (Phase 4+)

- Event sourcing (commands generate events)
- Event handlers update read models (projections)
- Saga pattern for multi-step workflows
- Async messaging (command bus, event bus)

**[📖 Event Sourcing Deep-Dive](https://github.com/entelect-incubator/Design-Patterns/tree/main/CQRS/Event-Sourcing)**

---

## Code Examples by Language

- **C#**: https://github.com/entelect-incubator/Design-Patterns/tree/main/CQRS/csharp
- **Java**: https://github.com/entelect-incubator/Design-Patterns/tree/main/CQRS/java
- **Python**: https://github.com/entelect-incubator/Design-Patterns/tree/main/CQRS/python
- **TypeScript**: https://github.com/entelect-incubator/Design-Patterns/tree/main/CQRS/typescript

---

## Incubator Implementation

| Phase        | CQRS Level        | How It Works                                              |
| ------------ | ----------------- | --------------------------------------------------------- |
| **Phase 1**  | Basic separation  | Routes call handlers; Result pattern for commands         |
| **Phase 2**  | Explicit handlers | `CreatePizzaHandler`, `GetPizzaHandler` classes           |
| **Phase 3**  | ORM integration   | Handlers use real repository (not in-memory)              |
| **Phase 4**  | Transactions      | Multi-step commands (create order + items in transaction) |
| **Phase 5+** | Event sourcing    | Commands generate events; read models updated async       |

---

## Dispatcher/Mediator Role

Framework routes commands to handlers:

```
POST /pizzas (CreatePizzaCommand)
  ↓
Dispatcher.Send(CreatePizzaCommand)
  ↓
CreatePizzaHandler.Handle()
  ↓
Result<Pizza>
  ↓
HTTP 201 Created
```

---

**[Full CQRS Repository](https://github.com/entelect-incubator/Design-Patterns/tree/main/CQRS)**

**[Back to Design Patterns Hub](../README.md)**
