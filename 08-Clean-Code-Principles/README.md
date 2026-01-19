# Clean Code Principles

Guidelines for writing readable, maintainable code that other developers (and your future self) can understand.

## Key Principles

### 1. **Naming**
- ✅ `GetActivePizzas()` vs ❌ `GetPizzas2()`
- ✅ `IsValid()` vs ❌ `Check()`
- ✅ `pizzaRepository` vs ❌ `repo`, `r`

**Rule:** A name should reveal intent in a single glance.

---

### 2. **Function Length**
- ✅ Functions under 20 lines
- ✅ One responsibility per function
- ❌ 100-line functions doing multiple things

---

### 3. **Error Handling**
- ✅ Return `Result<T>` for domain errors
- ✅ Throw exceptions for system failures
- ❌ Silent failures (no error, no result)
- ❌ Returning null without documentation

---

### 4. **Comments**
- ✅ Explain *why*, not *what* (code explains what)
- ✅ Keep comments near the code they describe
- ❌ Outdated comments that contradict code

```csharp
// ✅ GOOD: Explains why
// We use exponential backoff to avoid overwhelming the API
// during thundering herd scenarios
await Task.Delay(exponentialBackoff);

// ❌ BAD: Explains what (code is already clear)
// Increment counter
count++;
```

---

### 5. **Testing**
- ✅ Unit test domain logic
- ✅ Integration test repositories + database
- ✅ Negative paths first (failures before happy path)
- ❌ No tests
- ❌ Tests that don't fail when code breaks

---

### 6. **KISS (Keep It Simple, Stupid)**
- ✅ Simple if/else chain vs. complex nested logic
- ✅ One reason to change per class (SRP)
- ❌ Over-engineering for hypothetical future features

---

### 7. **DRY (Don't Repeat Yourself)**
- ✅ Extract common logic to functions/classes
- ✅ Reuse validation rules
- ❌ Copy-paste code across files

---

### 8. **Formatting & Consistency**
- ✅ Auto-formatter (Prettier, Roslyn, Black, gofmt)
- ✅ Consistent indentation, spacing, line length
- ❌ Mixed styles (2 spaces, then 4 spaces)

---

## Code Examples by Language

- **C#**: https://github.com/entelect-incubator/Design-Patterns/tree/main/Clean-Code/csharp
- **Java**: https://github.com/entelect-incubator/Design-Patterns/tree/main/Clean-Code/java
- **Python**: https://github.com/entelect-incubator/Design-Patterns/tree/main/Clean-Code/python
- **TypeScript**: https://github.com/entelect-incubator/Design-Patterns/tree/main/Clean-Code/typescript

---

## Incubator Checklist

Every phase enforces:

- [ ] **Naming:** Variables, functions, classes reveal intent
- [ ] **Length:** Functions < 20 lines, classes < 200 lines
- [ ] **Testing:** Negative paths covered, no untested code
- [ ] **Comments:** Only where intent is unclear
- [ ] **Formatting:** Linter and formatter run without warnings
- [ ] **DRY:** No duplicate logic
- [ ] **SOLID:** Classes have single responsibility

---

## Tools by Language

| Language       | Formatter          | Linter        | Type Checker        |
| -------------- | ------------------ | ------------- | ------------------- |
| **C#**         | dotnet format      | StyleCop      | Built-in            |
| **Java**       | Google Java Format | Checkstyle    | Built-in            |
| **Python**     | Black              | Pylint/Flake8 | mypy                |
| **TypeScript** | Prettier           | ESLint        | TypeScript compiler |

---

**[Full Clean Code Repository](https://github.com/entelect-incubator/Design-Patterns/tree/main/Clean-Code)**

**[Back to Design Patterns Hub](../README.md)**
