# Repository Pattern

Abstraction layer for data access. Enables in-memory testing, ORM swapping, and business-rule encapsulation.

## Overview

A **Repository** is an abstraction that:
- Hides persistence details (database, ORM, file system)
- Provides a clean interface (`IRepository<T>`)
- Enables easy testing (swap in-memory implementation)
- Encapsulates queries and domain logic

---

## Repository Interface

Minimal, focused interface (ISP):

```csharp
public interface IRepository<T>
{
    Task<T> SaveAsync(T entity);
    Task<T?> GetByIdAsync(Guid id);
    Task<IEnumerable<T>> GetAllAsync();
    Task DeleteAsync(Guid id);
}
```

**Why not all CRUD operations?**
- Clients use only what they need (ISP)
- Easy to add read-only or write-only repositories

---

## Phases & Implementations

| Phase        | Implementation       | Storage                                                         |
| ------------ | -------------------- | --------------------------------------------------------------- |
| **Phase 1**  | In-memory repository | `Dictionary<Guid, T>`                                           |
| **Phase 2**  | Repository interface | Still in-memory, ready to swap                                  |
| **Phase 3**  | ORM repository       | SQLAlchemy (Python), JPA (Java), TypeORM (Node), EF Core (.NET) |
| **Phase 4+** | Specialized repos    | Read-only, write-only, event stores                             |

---

## In-Memory Implementation (Phase 1)

```typescript
// Simple, testable
class InMemoryPizzaRepository implements IRepository<Pizza> {
  private pizzas = new Map<string, Pizza>();
  
  async save(pizza: Pizza): Promise<Pizza> {
    this.pizzas.set(pizza.id, pizza);
    return pizza;
  }
  
  async getById(id: string): Promise<Pizza | null> {
    return this.pizzas.get(id) || null;
  }
}
```

---

## ORM Implementation (Phase 3)

**Python (SQLAlchemy):**
```python
class PizzaRepository:
    def __init__(self, session: AsyncSession):
        self.session = session
    
    async def save(self, pizza: Pizza) -> Pizza:
        self.session.add(pizza)
        await self.session.flush()
        return pizza
    
    async def get_by_id(self, pizza_id: UUID) -> Pizza | None:
        return await self.session.get(Pizza, pizza_id)
```

**Java (Spring Data JPA):**
```java
@Repository
public interface PizzaRepository extends JpaRepository<Pizza, UUID> {
}
// Zero boilerplate - Spring provides implementation
```

---

## Key Benefits

| Benefit                 | Explanation                                                               |
| ----------------------- | ------------------------------------------------------------------------- |
| **Testability**         | In-memory implementation for unit tests                                   |
| **ORM Independence**    | Swap SQLAlchemy ↔ JPA without changing client code                        |
| **Query Encapsulation** | Business logic (e.g., "active pizzas") in repo, not scattered in handlers |
| **Consistency**         | All persistence goes through one interface                                |

---

## Anti-Pattern: No Abstraction

```python
# ❌ BAD: Business logic mixed with ORM
def create_order(user_id, pizzas):
    order = Order(user_id=user_id)
    session.add(order)
    session.flush()
    for pizza_id in pizzas:
        item = session.query(Pizza).get(pizza_id)  # Query in handler!
        order.items.append(item)
    session.commit()
    return order

# ✅ GOOD: Repository handles ORM
async def create_order(user_id, pizzas):
    order = await repo.create_order(user_id, pizzas)
    return Result(success=True, data=order)
```

---

## Code Examples by Language

- **C#** (EF Core): https://github.com/entelect-incubator/Design-Patterns/tree/main/Repository-Pattern/csharp
- **Java** (JPA): https://github.com/entelect-incubator/Design-Patterns/tree/main/Repository-Pattern/java
- **Python** (SQLAlchemy): https://github.com/entelect-incubator/Design-Patterns/tree/main/Repository-Pattern/python
- **TypeScript** (TypeORM): https://github.com/entelect-incubator/Design-Patterns/tree/main/Repository-Pattern/typescript

---

**[Full Repository Pattern Repository](https://github.com/entelect-incubator/Design-Patterns/tree/main/Repository-Pattern)**

**[Back to Design Patterns Hub](../README.md)**
