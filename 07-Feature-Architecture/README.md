# Feature Architecture (Vertical Slice Architecture)

Organize code by business features/capabilities rather than technical layers. Each feature contains all the code it needs (domain, handlers, data access, API).

## Feature-Based Organization

```
┌───────────────────────────────────────────────┐
│               Feature: Pizza                  │
├───────────────────────────────────────────────┤
│  ├── CreatePizza/                            │
│  │   ├── Command.cs          (Request)       │
│  │   ├── Handler.cs          (Logic)         │
│  │   ├── Validator.cs        (Rules)         │
│  │   └── Controller.cs       (HTTP)          │
│  ├── GetPizza/                               │
│  │   ├── Query.cs                            │
│  │   ├── Handler.cs                          │
│  │   └── Controller.cs                       │
│  └── Shared/                                 │
│      ├── Pizza.cs            (Entity)        │
│      └── PizzaRepository.cs  (Data Access)   │
└───────────────────────────────────────────────┘
```

**Benefit:** All code for "Create Pizza" is in one folder. Easy to find, modify, and test.

---

## Why Feature Architecture?

| Aspect             | Traditional Layers                             | Feature Slices                            |
| ------------------ | ---------------------------------------------- | ----------------------------------------- |
| **Organization**   | By technical concern (Controllers/, Services/) | By business capability (Pizzas/, Orders/) |
| **Navigation**     | Jump across folders to understand one feature  | All feature code in one place             |
| **Team Ownership** | Hard to assign features to teams               | Easy: each team owns feature folders      |
| **Changes**        | Touch multiple folders for one feature change  | Change one folder                         |
| **Testing**        | Test spans multiple projects/folders           | Test folder is self-contained             |

---

## Folder Structure (Incubator)

### Option 1: Feature Folders (Recommended)

```
src/
├── Features/
│   ├── Pizzas/
│   │   ├── CreatePizza/
│   │   │   ├── CreatePizzaCommand.cs
│   │   │   ├── CreatePizzaHandler.cs
│   │   │   ├── CreatePizzaValidator.cs
│   │   │   └── PizzasController.cs (or inline in Handler)
│   │   ├── GetPizza/
│   │   │   ├── GetPizzaQuery.cs
│   │   │   ├── GetPizzaHandler.cs
│   │   │   └── (controller endpoint)
│   │   └── Shared/
│   │       ├── Pizza.cs           (Entity)
│   │       ├── IPizzaRepository.cs (Interface)
│   │       └── PizzaRepository.cs  (Implementation)
│   ├── Orders/
│   │   ├── CreateOrder/
│   │   ├── GetOrder/
│   │   └── Shared/
│   └── Customers/
│       └── ... (same pattern)
└── Common/                        # Cross-cutting concerns
    ├── Database/
    ├── Middleware/
    └── Results/
```

### Option 2: Hybrid (Features + Shared Domain)

```
src/
├── Features/
│   ├── Pizzas/
│   │   ├── CreatePizza.cs         (Command + Handler in one file)
│   │   ├── GetPizza.cs            (Query + Handler in one file)
│   │   └── PizzasController.cs
│   └── Orders/
│       └── ... (same)
├── Domain/                        # Shared entities only
│   ├── Pizza.cs
│   └── Order.cs
└── Infrastructure/                # Shared data access
    └── Repositories/
        ├── PizzaRepository.cs
        └── OrderRepository.cs
```

---

## Key Benefits

| Benefit                | Explanation                                               |
| ---------------------- | --------------------------------------------------------- |
| **Cohesion**           | Everything for one feature in one place                   |
| **Discoverability**    | New developers find "Create Pizza" code immediately       |
| **Team Ownership**     | Teams can own entire features (Orders team, Pizzas team)  |
| **Reduced Coupling**   | Features are isolated; changes don't ripple               |
| **Easier Testing**     | Test folder contains all feature code + tests             |
| **Faster Development** | No jumping between Controllers/, Services/, Repositories/ |

---

## Comparison: Layers vs. Features

**Traditional Clean Architecture (Horizontal Layers):**
```
To add "Create Pizza":
1. Add Pizza.cs in Domain/Entities/
2. Add CreatePizzaCommand.cs in Application/Commands/
3. Add CreatePizzaHandler.cs in Application/Handlers/
4. Add PizzaRepository.cs in Infrastructure/Repositories/
5. Add PizzaController.cs in API/Controllers/

Result: 5 folders touched
```

**Feature Architecture (Vertical Slices):**
```
To add "Create Pizza":
1. Add Features/Pizzas/CreatePizza/ folder
2. Add Command.cs, Handler.cs, Validator.cs in that folder

Result: 1 folder touched
```

---

## When to Use Which

| Use Feature Architecture When:         | Use Layered Architecture When:         |
| -------------------------------------- | -------------------------------------- |
| ✅ Domain is complex with many features | ✅ Small project (< 5 features)         |
| ✅ Multiple teams work on codebase      | ✅ Strict separation of concerns needed |
| ✅ Features evolve independently        | ✅ Domain logic is highly shared        |
| ✅ Onboarding new developers often      | ✅ Architectural purity is priority     |

**Incubator Approach:** Hybrid—Features folder for handlers, Shared Domain/Infrastructure for common code.

---

## Code Examples by Language

- **C#**: https://github.com/entelect-incubator/Design-Patterns/tree/main/Feature-Architecture/csharp
- **Java**: https://github.com/entelect-incubator/Design-Patterns/tree/main/Feature-Architecture/java
- **Python**: https://github.com/entelect-incubator/Design-Patterns/tree/main/Feature-Architecture/python
- **TypeScript**: https://github.com/entelect-incubator/Design-Patterns/tree/main/Feature-Architecture/typescript

---

## Incubator Implementation

All Incubators use feature-based organization from Phase 1:

- **Phase 1**: Features/Pizzas with in-memory repository
- **Phase 2**: Add CQRS handlers per feature (CreatePizza/, GetPizza/)
- **Phase 3**: ORM integration within feature folders
- **Phase 4+**: Advanced features with events and transactions

**Actual Structure in Incubators:**
```
Application/
  Features/
    Pizzas/
      Commands/
        CreatePizza.cs
      Queries/
        GetPizza.cs
      PizzaMapper.cs
Domain/
  Pizza.cs           # Shared entity
Infrastructure/
  PizzaRepository.cs # Shared data access
```

This hybrid approach gives feature cohesion while keeping shared domain logic centralized.

---

**[Full Feature Architecture Repository](https://github.com/entelect-incubator/Design-Patterns/tree/main/Feature-Architecture)**

**[Back to Design Patterns Hub](../README.md)**
