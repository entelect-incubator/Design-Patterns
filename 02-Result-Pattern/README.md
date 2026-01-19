# Result Pattern

Explicit error handling for domain-level operations. Returns success/failure with typed data or error details.

## Overview

Instead of throwing exceptions or returning null, wrap operation outcomes in a **Result** type:

```
Result<T> = {
  success: boolean
  data: T | null
  code: string (e.g., "OK", "NOT_FOUND", "VALIDATION_ERROR")
  message: string (e.g., "Pizza not found")
}
```

---

## Key Benefits

| Benefit                | Explanation                                        |
| ---------------------- | -------------------------------------------------- |
| **Explicit Errors**    | Caller knows what can fail without reading code    |
| **Type Safety**        | Compiler checks error handling                     |
| **No Silent Failures** | Can't forget to handle errors (with proper typing) |
| **Readable Flow**      | Success/failure paths are clear in code            |
| **Testable**           | Easy to test both success and failure paths        |

---

## When to Use

✅ **DO use Result Pattern for:**
- Domain-level operations (create order, validate pizza)
- Business logic errors (insufficient stock, invalid coupon)
- HTTP handlers (catches validation/not-found cases)
- Repository saves/updates

❌ **DON'T use Result Pattern for:**
- Input validation (use framework validators: Pydantic, Spring Validation)
- System errors (disk full, network timeout) – let them propagate
- Flow control in happy path (return early if Result is error; don't nest)

---

## Result vs. Exceptions

| Aspect              | Result Pattern            | Exceptions               |
| ------------------- | ------------------------- | ------------------------ |
| **Domain Errors**   | ✅ Result                  | ❌ Exceptions             |
| **Validation**      | ❌ Result                  | ✅ Exceptions (framework) |
| **System Failures** | ❌ Result                  | ✅ Exceptions             |
| **HTTP 404/400**    | ✅ Result                  | ❌ Exceptions             |
| **Performance**     | ✅ Result (no stack trace) | ❌ Exceptions (expensive) |
| **Readability**     | ✅ Explicit                | ❌ Hidden in try/catch    |

---

## Code Examples by Language

- **C#**: https://github.com/entelect-incubator/Design-Patterns/tree/main/Result-Pattern/csharp
- **Java**: https://github.com/entelect-incubator/Design-Patterns/tree/main/Result-Pattern/java
- **Python**: https://github.com/entelect-incubator/Design-Patterns/tree/main/Result-Pattern/python
- **TypeScript**: https://github.com/entelect-incubator/Design-Patterns/tree/main/Result-Pattern/typescript

---

## Incubator Implementation

**Phase 2+:** All handlers return `Result<T>` for domain operations.

```python
# Python example
@dataclass(frozen=True)
class Result(Generic[T]):
    success: bool
    data: T | None = None
    code: str = "OK"
    message: str = ""

# Handler uses Result for business logic
async def create_pizza(name: str, price: float) -> Result[Pizza]:
    if name.strip() == "":
        return Result(success=False, code="INVALID_NAME", message="Name required")
    if price <= 0:
        return Result(success=False, code="INVALID_PRICE", message="Price must be positive")
    pizza = await repo.save(name, price)
    return Result(success=True, data=pizza)
```

**Integration with HTTP:**
- Result.success=true → HTTP 201 (created), 200 (ok)
- Result.success=false → HTTP 400 (validation), 404 (not found), 409 (conflict)

---

## Immutability

Use **immutable Result** (frozen dataclass in Python, record in C#, sealed class in Java) to prevent accidental mutation in handlers.

---

**[Full Result Pattern Repository](https://github.com/entelect-incubator/Design-Patterns/tree/main/Result-Pattern)**

**[Back to Design Patterns Hub](../README.md)**
