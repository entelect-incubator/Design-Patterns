# Testing Patterns

Comprehensive testing strategy: unit tests for domain logic, integration tests for persistence, negative-path-first approach.

## Testing Pyramid

```
        ▲
       /│\        E2E Tests (few)
      / │ \       "User journeys" via HTTP
     /  │  \
    ┌───┼───┐
    │ Integration│     Integration Tests (medium)
    │  Tests  │     "Real database" with Testcontainers
    └───┼───┘
       /  \
      /    \      Unit Tests (many)
     /      \    "Fast, isolated" with mocks
    ┴────────┴
```

---

## Unit Tests

Test domain logic in isolation.

```csharp
[Fact]
public void CreatePizza_WithEmptyName_ReturnsFail()
{
    // Arrange
    var handler = new CreatePizzaHandler();
    
    // Act
    var result = handler.Create("", 10.99m);
    
    // Assert
    Assert.False(result.Success);
    Assert.Equal("INVALID_NAME", result.Code);
}
```

**Tools:**
- **C#**: xUnit
- **Java**: JUnit 5
- **Python**: pytest
- **TypeScript**: Jest

---

## Integration Tests

Test with real database (Testcontainers).

```python
@pytest.mark.asyncio
async def test_create_pizza_persists_to_db():
    # Arrange
    async with async_session_maker() as session:
        repo = PizzaRepository(session)
    
    # Act
    pizza = await repo.save("Pepperoni", 12.99)
    found = await repo.get_by_id(pizza.id)
    
    # Assert
    assert found is not None
    assert found.name == "Pepperoni"
```

**Tools:**
- **Java**: Testcontainers + JUnit 5
- **Node**: Testcontainers + Jest
- **Python**: pytest + Docker API / pytest-docker
- **.NET**: Testcontainers for .NET + xUnit

---

## Negative-Path First

Test failures before happy paths:

```typescript
describe("PizzaRepository", () => {
    // ❌ Negative paths first
    it("should return null for non-existent pizza", async () => {
        const found = await repo.getById(uuid());
        expect(found).toBeNull();
    });
    
    it("should reject pizza with empty name", async () => {
        const result = await repo.save("", 10.99);
        expect(result).toFail();
    });
    
    it("should reject pizza with negative price", async () => {
        const result = await repo.save("Valid", -5);
        expect(result).toFail();
    });
    
    // ✅ Happy path last
    it("should save and retrieve pizza", async () => {
        const saved = await repo.save("Margherita", 8.99);
        const found = await repo.getById(saved.id);
        expect(found.name).toBe("Margherita");
    });
});
```

---

## Testcontainers

Spin up real database in Docker for tests:

```java
@Testcontainers
@SpringBootTest
class PizzaRepositoryTests {
    
    @Container
    static PostgreSQLContainer<?> postgres = 
        new PostgreSQLContainer<>("postgres:14");
    
    @Test
    void testSaveAndFind() {
        Pizza pizza = new Pizza("Pepperoni", BigDecimal.valueOf(12.99));
        Pizza saved = repository.save(pizza);
        
        Optional<Pizza> found = repository.findById(saved.getId());
        assertTrue(found.isPresent());
    }
}
```

---

## Test Checklist (Incubator)

Every phase includes:

- [ ] **Unit tests** for domain logic (handlers, entities)
- [ ] **Integration tests** for repositories (with Testcontainers)
- [ ] **Negative paths** tested (missing data, invalid input, constraints)
- [ ] **Happy path** tested (save → retrieve → verify)
- [ ] **No untested code** (aim for > 80% coverage in domain)
- [ ] **Tests run in CI** (GitHub Actions)

---

## Code Examples by Language

- **C#**: https://github.com/entelect-incubator/Design-Patterns/tree/main/Testing-Patterns/csharp
- **Java**: https://github.com/entelect-incubator/Design-Patterns/tree/main/Testing-Patterns/java
- **Python**: https://github.com/entelect-incubator/Design-Patterns/tree/main/Testing-Patterns/python
- **TypeScript**: https://github.com/entelect-incubator/Design-Patterns/tree/main/Testing-Patterns/typescript

---

**[Full Testing Patterns Repository](https://github.com/entelect-incubator/Design-Patterns/tree/main/Testing-Patterns)**

**[Back to Design Patterns Hub](../README.md)**
