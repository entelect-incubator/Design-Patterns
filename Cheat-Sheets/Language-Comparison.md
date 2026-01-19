# Language Comparison: Same Pattern, Different Syntax

See how each language implements the same patterns idiomatically.

---

## Pattern 1: Dependency Injection

### C# (.NET)
```csharp
// Register
services.AddScoped<IRepository<Pizza>, PizzaRepository>();
services.AddScoped<CreatePizzaHandler>();

// Inject
public class CreatePizzaHandler {
    private IRepository<Pizza> _repo;
    public CreatePizzaHandler(IRepository<Pizza> repo) => _repo = repo;
}
```

### Java (Spring)
```java
@Repository
public class PizzaRepository implements IRepository<Pizza> { }

@Service
public class CreatePizzaHandler {
    @Autowired
    private IRepository<Pizza> repo;
}
```

### Python (FastAPI)
```python
async def create_pizza(
    data: CreatePizzaSchema,
    repo: IRepository = Depends(get_repository)
) -> Result[Pizza]:
    return await repo.save(data)
```

### TypeScript (Express/NestJS)
```typescript
// NestJS
@Injectable()
export class CreatePizzaHandler {
    constructor(private repo: IRepository<Pizza>) {}
}

// Express (manual)
const repo = new PizzaRepository();
const handler = new CreatePizzaHandler(repo);
```

---

## Pattern 2: Repository

### C# (EF Core)
```csharp
public class PizzaRepository : IRepository<Pizza> {
    private DbContext _context;
    
    public async Task<Pizza> SaveAsync(Pizza pizza) {
        _context.Pizzas.Add(pizza);
        await _context.SaveChangesAsync();
        return pizza;
    }
    
    public async Task<Pizza?> GetByIdAsync(Guid id) =>
        await _context.Pizzas.FindAsync(id);
}
```

### Java (JPA)
```java
@Repository
public interface PizzaRepository extends JpaRepository<Pizza, UUID> {
    // Zero boilerplate - Spring provides everything
}

// Usage
Optional<Pizza> pizza = pizzaRepository.findById(id);
Pizza saved = pizzaRepository.save(pizza);
```

### Python (SQLAlchemy)
```python
class PizzaRepository(IRepository[Pizza]):
    def __init__(self, session: AsyncSession):
        self.session = session
    
    async def save(self, pizza: Pizza) -> Pizza:
        self.session.add(pizza)
        await self.session.flush()
        return pizza
    
    async def get_by_id(self, id: UUID) -> Pizza | None:
        return await self.session.get(Pizza, id)
```

### TypeScript (TypeORM)
```typescript
@Entity("pizzas")
export class Pizza {
    @PrimaryGeneratedColumn("uuid")
    id: string;
    
    @Column()
    name: string;
}

const repo = dataSource.getRepository(Pizza);
const pizza = await repo.save(new Pizza("Margherita"));
const found = await repo.findOneBy({ id });
```

---

## Pattern 3: Result Pattern

### C# (Record)
```csharp
public record Result<T>(
    bool Success,
    T? Data = null,
    string Code = "OK",
    string Message = ""
);
```

### Java (Sealed Class)
```java
public sealed interface Result<T> {
    record Success<T>(T data) implements Result<T> {}
    record Failure<T>(String code, String message) implements Result<T> {}
}
```

### Python (Frozen Dataclass)
```python
@dataclass(frozen=True)
class Result(Generic[T]):
    success: bool
    data: T | None = None
    code: str = "OK"
    message: str = ""
```

### TypeScript (Type Union)
```typescript
type Result<T> = 
    | { success: true; data: T }
    | { success: false; code: string; message: string };
```

---

## Pattern 4: CQRS Handler

### C# (MediatR)
```csharp
public class CreatePizzaCommand : IRequest<Result<Pizza>> {
    public string Name { get; set; }
    public decimal Price { get; set; }
}

public class CreatePizzaHandler : IRequestHandler<CreatePizzaCommand, Result<Pizza>> {
    public async Task<Result<Pizza>> Handle(CreatePizzaCommand cmd) {
        if (string.IsNullOrEmpty(cmd.Name))
            return new(Success: false, Code: "INVALID_NAME");
        return new(Success: true, Data: new Pizza(...));
    }
}
```

### Java (Spring Service)
```java
@Service
public class CreatePizzaHandler {
    @Transactional
    public Result<Pizza> handle(CreatePizzaCommand cmd) {
        if (cmd.name().isBlank())
            return Result.fail("INVALID_NAME", "Name required");
        return Result.ok(pizzaRepository.save(new Pizza(...)));
    }
}
```

### Python (FastAPI Handler)
```python
async def create_pizza_handler(
    data: CreatePizzaSchema,
    repo: IRepository = Depends(get_repository)
) -> Result[Pizza]:
    if not data.name.strip():
        return Result(success=False, code="INVALID_NAME")
    pizza = await repo.save(data.name, data.price)
    return Result(success=True, data=pizza)
```

### TypeScript (Express Handler)
```typescript
export async function createPizzaHandler(req: Request): Promise<Result<Pizza>> {
    const { name, price } = req.body;
    if (!name?.trim()) {
        return { success: false, code: "INVALID_NAME" };
    }
    const pizza = await repo.save(name, price);
    return { success: true, data: pizza };
}
```

---

## Pattern 5: Testing (Negative Path)

### C# (xUnit)
```csharp
[Fact]
public async Task CreatePizza_WithEmptyName_ReturnsFail() {
    var handler = new CreatePizzaHandler(repo);
    var result = await handler.Handle(new CreatePizzaCommand { Name = "" });
    Assert.False(result.Success);
    Assert.Equal("INVALID_NAME", result.Code);
}
```

### Java (JUnit 5 + Testcontainers)
```java
@Test
void createPizza_withEmptyName_returnsFail() {
    Result<Pizza> result = handler.handle(new CreatePizzaCommand("", 10.0));
    assertFalse(result.success());
    assertEquals("INVALID_NAME", result.code());
}
```

### Python (pytest)
```python
@pytest.mark.asyncio
async def test_create_pizza_with_empty_name_returns_fail():
    result = await handler.create_pizza("", 10.0)
    assert result.success is False
    assert result.code == "INVALID_NAME"
```

### TypeScript (Jest)
```typescript
it("should reject pizza with empty name", async () => {
    const result = await handler({ name: "", price: 10.0 });
    expect(result.success).toBe(false);
    expect(result.code).toBe("INVALID_NAME");
});
```

---

## Summary: Idioms by Language

| Language       | DI Container      | ORM        | Async             | Immutability     | Testing |
| -------------- | ----------------- | ---------- | ----------------- | ---------------- | ------- |
| **C#**         | Built-in          | EF Core    | async/await       | record, sealed   | xUnit   |
| **Java**       | Spring            | JPA        | CompletableFuture | sealed interface | JUnit 5 |
| **Python**     | FastAPI Depends() | SQLAlchemy | async/await       | frozen dataclass | pytest  |
| **TypeScript** | Manual/NestJS     | TypeORM    | async/await       | readonly, const  | Jest    |

---

**[Back to Cheat Sheets](./README.md)**
