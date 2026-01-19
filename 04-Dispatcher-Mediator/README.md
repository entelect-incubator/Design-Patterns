# Dispatcher / Mediator Pattern

Routes commands/queries to appropriate handlers. Decouples request from handler logic.

## Overview

A **Dispatcher** or **Mediator** is a central hub that:
1. Receives a command/query
2. Finds the appropriate handler
3. Invokes the handler
4. Returns the result

---

## Framework Implementations

| Framework   | Dispatcher Name     | How It Works                                          |
| ----------- | ------------------- | ----------------------------------------------------- |
| **.NET**    | MediatR library     | `mediator.Send(command)` → finds `IRequestHandler<T>` |
| **Spring**  | DispatcherServlet   | Routes HTTP requests to `@Controller` handlers        |
| **FastAPI** | Route decorators    | `@app.post()` → handler function                      |
| **Express** | Router + middleware | `app.post()` → middleware chain                       |

---

## Framework-Native vs. Custom

### ✅ Use Framework-Native (Recommended)

```csharp
// .NET MediatR
mediator.Send(new CreatePizzaCommand { Name = "Pepperoni", Price = 15.99 })
```

```python
# FastAPI (framework-native routing)
@app.post("/pizzas")
async def create_pizza(data: CreatePizzaSchema) -> PizzaModel:
    return await handler.create(data)
```

### ❌ Custom Dispatcher (Only if needed)

```typescript
// Only if framework doesn't provide routing
class SimpleDispatcher {
  private handlers = new Map<string, Function>();
  
  register(commandName: string, handler: Function) {
    this.handlers.set(commandName, handler);
  }
  
  async dispatch(command: any) {
    const handler = this.handlers.get(command.constructor.name);
    return await handler(command);
  }
}
```

---

## Handler Discovery

**Automatic (MediatR, Spring):**
- Framework scans for `IRequestHandler<T>` or `@Repository` annotations
- No manual registration needed

**Manual (Express, FastAPI):**
- Define routes explicitly
- Clearer for simple apps

---

## Key Benefits

| Benefit           | Explanation                                       |
| ----------------- | ------------------------------------------------- |
| **Decoupling**    | Handler logic isolated from routing               |
| **Testability**   | Easy to mock handlers                             |
| **Extensibility** | Add handlers without modifying dispatcher         |
| **Middleware**    | Logging, validation, auth applied globally        |
| **Performance**   | No handler discovery overhead (if pre-registered) |

---

## Code Examples by Language

- **C#** (MediatR): https://github.com/entelect-incubator/Design-Patterns/tree/main/Dispatcher-Mediator/csharp
- **Java** (Spring): https://github.com/entelect-incubator/Design-Patterns/tree/main/Dispatcher-Mediator/java
- **Python** (FastAPI): https://github.com/entelect-incubator/Design-Patterns/tree/main/Dispatcher-Mediator/python
- **TypeScript** (Express): https://github.com/entelect-incubator/Design-Patterns/tree/main/Dispatcher-Mediator/typescript

---

## Incubator Implementation

| Phase        | Dispatcher               | Details                                         |
| ------------ | ------------------------ | ----------------------------------------------- |
| **Phase 1**  | Framework-native routing | Basic route → handler                           |
| **Phase 2**  | Explicit handler classes | `CreatePizzaHandler`, `GetPizzaHandler` with DI |
| **Phase 3+** | Handler auto-discovery   | Scrutor (.NET), Classpath scanning (Java), etc. |

---

**[Full Dispatcher/Mediator Repository](https://github.com/entelect-incubator/Design-Patterns/tree/main/Dispatcher-Mediator)**

**[Back to Design Patterns Hub](../README.md)**
