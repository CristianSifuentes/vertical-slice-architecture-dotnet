
# 🎓 Master Guide: Vertical Slice Architecture (VSA) in .NET

## 📚 Table of Contents
1. [What Is Vertical Slice Architecture?](#1-what-is-vertical-slice-architecture)
2. [Core Principle](#core-principle)
3. [Key Concepts and Building Blocks](#2-key-concepts-and-building-blocks)
4. [A Slice Typically Includes](#a-slice-typically-includes)
5. [Structure Example in .NET](#structure-example-in-net)
6. [Comparison with Other Architectures](#3-comparison-with-other-architectures)
7. [Advantages](#4-advantages-of-vertical-slice-architecture)
8. [Implementation Patterns](#5-implementation-patterns)
9. [MediatR Integration](#mediatr-integration)
10. [Best Practices](#6-best-practices)
11. [Anti-Patterns to Avoid](#7-anti-patterns-to-avoid)
12. [When to Use VSA](#8-when-to-use-vertical-slice-architecture)
13. [Caution With](#caution-with)
14. [Real-World Example](#9-real-world-example-getactivity-slice)
15. [Advanced .NET Practices](#10-advanced-net-practices-with-vsa)
16. [Final Tips](#final-tips-to-master-vertical-slice-architecture)
17. [Key Features](#1-key-features-of-vertical-slice-architecture)
18. [Advantages Recap](#2-advantages)
19. [Disadvantages](#3-disadvantages)
20. [Highlights](#4-highlights-of-vertical-slice-architecture)
21. [GitHub Repositories](#5-github-repositories-to-study)
22. [Books](#6-books--reading-material)
23. [Articles](#7-articles--advanced-technical-resources)
24. [Tools and Libraries](#8-tools-and-libraries)
25. [Project Structure](#9-project-structure-for-mastery)
26. [Becoming a Master](#10-becoming-a-master-what-you-need-to-practice)
27. [Next Steps](#next-step-want-a-custom-template)

---

## 1. What Is Vertical Slice Architecture?
Vertical Slice Architecture organizes code by **feature**, not by layers. Each slice is a complete use case (read or write), encapsulating all necessary logic from request to response.

---

## 🎯 Core Principle
> “Minimize coupling between slices, maximize cohesion within a slice.”

---

## 2. Key Concepts and Building Blocks

- Feature-first structure
- Each slice represents one command/query
- Natural support for CQRS
- Internal decision on persistence, validation, etc.

---

## A Slice Typically Includes:

- API Endpoint
- Request DTO
- Handler (business logic)
- Domain model (if needed)
- Validation
- Persistence (EF Core, Dapper, etc.)
- Response DTO

---

## Structure Example in .NET

```plaintext
📁 Features
├── 📁 Orders
│   ├── 📁 CreateOrder
│   │   ├── CreateOrder.Command.cs
│   │   ├── CreateOrder.Handler.cs
│   │   ├── CreateOrder.Endpoint.cs
│   │   ├── CreateOrder.Validator.cs
│   │   └── CreateOrder.Response.cs
│   ├── 📁 GetOrder
│   │   ├── GetOrder.Query.cs
│   │   ├── GetOrder.Handler.cs
│   │   └── GetOrder.Response.cs
```

---

## 3. Comparison with Other Architectures

| Feature                  | Clean Architecture | Vertical Slice Architecture |
|--------------------------|--------------------|------------------------------|
| Structure                | Layered            | Feature-based (sliced)       |
| Cohesion per Feature     | Low                | High                         |
| Abstraction              | Required           | Optional                     |
| CQRS Compatibility       | Possible           | Natural                      |
| Testability              | Moderate           | High                         |

---

## 4. Advantages of Vertical Slice Architecture

- ✅ High cohesion
- ✅ Modular growth
- ✅ Natural fit with CQRS
- ✅ Simple to onboard new devs
- ✅ Reduces side effects
- ✅ Minimizes unnecessary abstractions

---

## 5. Implementation Patterns

### REPR Pattern (Request → Endpoint → Response)
Helps enforce separation and clarity in slice boundaries.

### Example:

- `CreateOrder.Command.cs`
- `CreateOrder.Handler.cs`
- `CreateOrder.Endpoint.cs`
- `CreateOrder.Response.cs`

---

## ⚙️ MediatR Integration

```csharp
public record CreateOrderCommand(...) : IRequest<OrderResponse>;

public class CreateOrderHandler : IRequestHandler<CreateOrderCommand, OrderResponse>
{
    public async Task<OrderResponse> Handle(CreateOrderCommand request, CancellationToken ct)
    {
        // use-case logic here
    }
}
```

---

## 6. Best Practices

- Organize by feature (feature folders)
- Use FluentValidation per slice
- Avoid premature abstraction
- Keep handlers focused (1 use case = 1 handler)
- Add integration tests per slice

---

## 7. Anti-Patterns to Avoid

- Too much shared code between slices
- Overuse of abstractions like services/repositories
- One handler doing multiple things
- Mixing infrastructure concerns inside domain logic

---

## 8. When to Use Vertical Slice Architecture?

- ✅ Modular monoliths
- ✅ Feature-based development
- ✅ CQRS projects
- ✅ REST APIs
- ✅ Microservice candidates

---

## 🔴 Caution With:

- Complex, deeply interconnected domains
- Teams unfamiliar with DDD or refactoring
- Domains requiring strict consistency and coordination

---

## 9. Real-World Example: GetActivity Slice

```plaintext
[GET] /api/activities/{id}
→ GetActivityQuery.cs
→ GetActivityHandler.cs
→ ActivityRepository (Dapper or EF)
→ ActivityResponse.cs
```

---

## 10. Advanced .NET Practices with VSA

- MediatR pipelines (for logging, auth, validation)
- Use FluentValidation per command/query
- Integrate with EF Core or Dapper based on need
- Write integration tests per slice

---

## 🧠 Final Tips to Master Vertical Slice Architecture

- Start simple (transaction script style)
- Refactor to domain logic if necessary
- Don’t abstract prematurely
- Avoid shared layers unless unavoidable
- Group everything by feature, not by layer

---

## 1. Key Features of Vertical Slice Architecture

- Feature-first
- Minimal abstraction
- High cohesion per use case
- Per-slice infrastructure choices
- Modular and scalable

---

## 2. Advantages

(See section 4 above)

---

## 3. Disadvantages

- Risk of duplication
- Difficult cross-feature coordination
- Requires discipline in structure
- Refactoring skills essential

---

## 4. Highlights of Vertical Slice Architecture

- Each slice is a vertical cut through the stack
- Use case-oriented
- Can use CQRS, MediatR, or FastEndpoints
- Doesn’t require full DDD
- Can be implemented incrementally

---

## 5. GitHub Repositories to Study

- https://github.com/jbogard/ContosoUniversityDotNetCore
- https://github.com/ardalis/CleanArchitecture
- https://github.com/jasontaylordev/CleanArchitecture
- https://github.com/pmbanugo/vertical-slice-architecture-dotnet
- https://github.com/kgrzybek/modular-monolith-with-ddd
- https://github.com/dotnet-architecture/eShopOnWeb

---

## 6. Books & Reading Material

- *Vertical Slice Architecture* – Jimmy Bogard (blog series)
- *Refactoring* – Martin Fowler
- *Patterns of Enterprise Architecture* – Fowler
- *Domain-Driven Design* – Eric Evans
- *Clean Architecture* – Uncle Bob
- *Architecting Modern Web Apps with ASP.NET Core* – Microsoft eBook

---

## 7. Articles & Advanced Technical Resources

- [Jimmy Bogard – Vertical Slice Architecture](https://jimmybogard.com/vertical-slice-architecture/)
- [MediatR + VSA](https://www.codingame.com/playgrounds/51579/vertical-slice-architecture-in-asp-net-core)
- [Khalid Abuhakmeh on VSA](https://khalidabuhakmeh.com/vertical-slice-architecture-in-aspnet-core)

---

## 8. Tools and Libraries

| Tool              | Purpose                              |
|-------------------|--------------------------------------|
| MediatR           | CQRS messaging pattern               |
| FluentValidation  | Input validation per slice           |
| FastEndpoints     | Feature-first .NET API framework     |
| EF Core / Dapper  | Data access per slice (your choice)  |

---

## 9. Project Structure for Mastery

```plaintext
📁 Features
├── 📁 Users
│   ├── 📁 CreateUser
│   ├── 📁 GetUser
│   └── ...
📁 Infrastructure
📁 Domain
📁 Common
📄 Program.cs
```

---

## 10. Becoming a Master: What You Need to Practice

- Build CRUD use cases using MediatR
- Use CQRS with validation
- Mix EF Core and Dapper
- Apply REPR pattern
- Write per-slice integration tests
- Learn pipeline behaviors in MediatR

---

## 🧭 Next Step: Want a Custom Template?

Let me know if you want a working .NET project template using:
- ✅ MediatR
- ✅ CQRS
- ✅ VSA structure
- ✅ FluentValidation
- ✅ EF Core + Dapper

I'll generate it for you!
