# Dependency Injection (DI)

Loose coupling through constructor/setter injection. Frameworks auto-wire dependencies, eliminating manual `new` statements.

## Overview

**DI Principle:** "Don't create your dependencies; receive them as parameters."

```csharp
// ❌ BAD: Tight coupling
public class OrderHandler {
    private PizzaRepository repo = new PizzaRepository();
}

// ✅ GOOD: Dependency injected
public class OrderHandler {
    private IRepository<Pizza> repo;
    public OrderHandler(IRepository<Pizza> repo) => this.repo = repo;
}
```

---

## DI Container

Frameworks manage dependency creation and injection:

| Framework   | Container                                 | Usage                                                      |
| ----------- | ----------------------------------------- | ---------------------------------------------------------- |
| **.NET**    | Built-in `ServiceCollection`              | `services.AddScoped<IRepository, Repository>()`            |
| **Spring**  | Spring Container                          | `@Bean`, `@Autowired`                                      |
| **FastAPI** | Pydantic + `Depends()`                    | `async def handler(repo: IRepository = Depends(get_repo))` |
| **Express** | Manual or libraries (Awilix, InversifyJS) | `container.register(...)`                                  |

---

## Scopes (Lifecycles)

| Scope         | Lifetime                 | Use Case                        |
| ------------- | ------------------------ | ------------------------------- |
| **Singleton** | Application lifetime     | Repositories, loggers           |
| **Transient** | New instance per request | Stateless services              |
| **Scoped**    | Per HTTP request         | Database sessions, transactions |

**Example (.NET):**
```csharp
services.AddSingleton<ILogger, ConsoleLogger>();
services.AddScoped<IRepository<Pizza>, PizzaRepository>();
services.AddTransient<CreatePizzaHandler>();
```

---

## Constructor Injection

The most common pattern:

```java
// Spring
@Service
public class PizzaService {
    private final IRepository<Pizza> repo;
    
    @Autowired
    public PizzaService(IRepository<Pizza> repo) {
        this.repo = repo;
    }
}
```

```python
# FastAPI
async def create_pizza(
    data: CreatePizzaSchema,
    repo: IRepository = Depends(get_repository)
) -> Result[Pizza]:
    return await repo.save(data)
```

---

## Benefits

| Benefit            | Explanation                                     |
| ------------------ | ----------------------------------------------- |
| **Testability**    | Inject mock repositories in tests               |
| **Loose Coupling** | Depend on abstractions, not concretions         |
| **Reusability**    | Services don't know about concrete dependencies |
| **Configuration**  | Change implementations via DI config, not code  |

---

## Code Examples by Language

- **C#**: https://github.com/entelect-incubator/Design-Patterns/tree/main/Dependency-Injection/csharp
- **Java**: https://github.com/entelect-incubator/Design-Patterns/tree/main/Dependency-Injection/java
- **Python**: https://github.com/entelect-incubator/Design-Patterns/tree/main/Dependency-Injection/python
- **TypeScript**: https://github.com/entelect-incubator/Design-Patterns/tree/main/Dependency-Injection/typescript

---

## Incubator Implementation

| Phase        | DI Level         | Details                                                        |
| ------------ | ---------------- | -------------------------------------------------------------- |
| **Phase 1**  | Manual factories | Hand-rolled dependency creation                                |
| **Phase 2**  | Framework DI     | Use framework container (`ServiceCollection`, Spring, FastAPI) |
| **Phase 3+** | Auto-discovery   | Scrutor scanning, classpath scanning, type hints               |

---

**[Full Dependency Injection Repository](https://github.com/entelect-incubator/Design-Patterns/tree/main/Dependency-Injection)**

**[Back to Design Patterns Hub](../README.md)**
