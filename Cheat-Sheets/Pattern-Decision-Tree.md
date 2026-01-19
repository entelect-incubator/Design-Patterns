# Pattern Decision Tree

Which design pattern should I use?

---

## 1. "I'm starting a new domain operation (e.g., create order)"

```
Is it a write operation (create, update, delete)?
├─ YES → Use CQRS Command Pattern
│        └─ Create CommandHandler
│           └─ Return Result<T>
│
└─ NO (read operation)
   └─ Use CQRS Query Pattern
      └─ Create QueryHandler
         └─ Return data directly (no Result)
```

---

## 2. "I need to persist data"

```
Phase 1?
├─ YES → Use in-memory Repository
│        └─ Store in Dictionary<ID, T>
│
Phase 2+?
└─ YES → Use ORM Repository
         └─ SQLAlchemy (Python), JPA (Java), TypeORM (Node), EF Core (.NET)
```

---

## 3. "I need to handle errors"

```
Domain error (business logic, validation)?
├─ YES → Use Result<T> Pattern
│        └─ Return Result { success, data, code, message }
│
System error (disk full, network timeout)?
└─ YES → Let exception propagate
         └─ Or wrap in outer Result for HTTP handler
```

---

## 4. "I need to decouple request from handler"

```
Framework has built-in routing?
├─ YES → Use framework routing
│        └─ Spring: @GetMapping
│           .NET: [HttpGet]
│           FastAPI: @app.get()
│           Express: app.get()
│
├─ NO → Use MediatR or custom dispatcher
│        └─ mediator.Send(command)
│
Always organize handlers:
└─ Separate command handlers from query handlers
```

---

## 5. "I need to avoid dependencies"

```
Always inject abstractions:
├─ Inject IRepository<T>, not ConcreteRepository
├─ Inject IEmailService, not GmailService
└─ Use DI container (Spring, .NET ServiceCollection, Nest modules)

Never do:
└─ new ConcreteService() inside a handler
```

---

## 6. "I'm writing tests"

```
Unit test (no database)?
├─ Mock repositories
├─ Test handler logic in isolation
└─ Negative paths first

Integration test (with database)?
├─ Use Testcontainers
├─ Spin up real Postgres
└─ Negative paths first (missing data, constraints)
```

---

## Quick Decision Matrix

| Scenario                     | Pattern                                              |
| ---------------------------- | ---------------------------------------------------- |
| Create pizza                 | Command → CommandHandler → Result<Pizza>             |
| List pizzas                  | Query → QueryHandler → List<Pizza>                   |
| Store pizza                  | Repository.Save(pizza)                               |
| Fetch pizza by ID            | Repository.GetById(id)                               |
| Handle business rule failure | Result { success: false, code: "ERROR" }             |
| Handle system error          | Throw exception (or wrap in Result at HTTP boundary) |
| Test with database           | Testcontainers                                       |
| Test without database        | Mock repository                                      |
| Route HTTP request           | Framework routing (@GetMapping, @app.get, etc.)      |
| Decouple handler             | DI: inject IRepository, IEmailService                |

---

## Anti-Patterns (What NOT to do)

| Anti-Pattern                          | Why                       | Fix                            |
| ------------------------------------- | ------------------------- | ------------------------------ |
| `new DatabaseRepository()` in handler | Tight coupling            | Inject IRepository             |
| Always return Result, even for OK     | Not RESTful               | Use HTTP status codes          |
| Exception for validation              | Slow, wrong semantics     | Result or validation framework |
| No tests                              | Can't verify behavior     | Add unit + integration tests   |
| Handler does too much                 | Hard to test, violate SRP | Split into smaller handlers    |
| Repository without interface          | Can't mock in tests       | Use IRepository<T> abstraction |

---

**[Back to Cheat Sheets](./README.md)**
