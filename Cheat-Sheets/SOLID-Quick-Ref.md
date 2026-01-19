# SOLID Quick Reference

One-page summary of SOLID principles with examples.

---

## **S – Single Responsibility Principle (SRP)**

**One reason to change.**

| ❌ BAD                                          | ✅ GOOD                                                                  |
| ---------------------------------------------- | ----------------------------------------------------------------------- |
| `UserService` handles auth, email, logging     | `AuthService`, `EmailService`, `Logger`                                 |
| `Repository` handles queries, caching, logging | `Repository` (queries), `Cache` (caching), `Logger`                     |
| `Handler` validates, persists, emails          | `Handler` (validates), `Repository` (persists), `EmailService` (emails) |

**How to fix:** If a class has "and" in its description, split it.

---

## **O – Open/Closed Principle (OCP)**

**Open for extension, closed for modification.**

| ❌ BAD                                                                    | ✅ GOOD                                                             |
| ------------------------------------------------------------------------ | ------------------------------------------------------------------ |
| `if (paymentType == "CARD")` ... `else if (paymentType == "PAYPAL")` ... | `IPaymentProvider` interface; new `CardProvider`, `PayPalProvider` |
| Add new rule → modify switch statement                                   | Add new rule → implement `IRule` interface                         |

**How to fix:** Use inheritance, composition, or strategy pattern to extend without modifying.

---

## **L – Liskov Substitution Principle (LSP)**

**Derived classes = base class without breaking behavior.**

| ❌ BAD                                                              | ✅ GOOD                                                             |
| ------------------------------------------------------------------ | ------------------------------------------------------------------ |
| `Square extends Rectangle` but breaks width/height invariant       | Use composition: `Shape` interface, `Rectangle`, `Square` separate |
| `InputStream.read()` can return -1 in subclass (violates contract) | All implementations return -1 for EOF                              |

**How to fix:** If derived class violates base class contract, don't inherit; use composition.

---

## **I – Interface Segregation Principle (ISP)**

**Many specific interfaces > one general interface.**

| ❌ BAD                                              | ✅ GOOD                                                                      |
| -------------------------------------------------- | --------------------------------------------------------------------------- |
| `IRepository<T>` with Create, Read, Update, Delete | `IReadRepository<T>` (Read), `IWriteRepository<T>` (Create, Update, Delete) |
| `IService` with 20 methods; client uses 2          | `IAuthService`, `INotificationService` (each focused)                       |

**How to fix:** Clients should only depend on what they use.

---

## **D – Dependency Inversion Principle (DIP)**

**Depend on abstractions, not concretions.**

| ❌ BAD                                                               | ✅ GOOD                                                         |
| ------------------------------------------------------------------- | -------------------------------------------------------------- |
| `OrderService` instantiates `DatabaseRepository` directly           | `OrderService` receives `IRepository<Order>` in constructor    |
| High-level `OrderService` depends on low-level `PostgresRepository` | High-level `OrderService` depends on abstraction `IRepository` |

**How to fix:** Inject dependencies; use DI containers.

---

## SOLID Checklist

- [ ] Each class has one reason to change (SRP)
- [ ] Add features without modifying existing classes (OCP)
- [ ] Subtypes are safe substitutes for base types (LSP)
- [ ] Classes depend only on methods they use (ISP)
- [ ] High-level modules depend on abstractions (DIP)

---

**[Full SOLID Guide](../01-SOLID-Principles/README.md)**

**[Back to Cheat Sheets](./README.md)**
