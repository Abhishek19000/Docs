# ASP.NET Core Interview Book — From Scratch to Interview-Ready

> Written for a developer with **~2 years of experience** who has an interview in **3 days**.
> Every concept is explained **in simple words first**, then with an **easy example**, then with a **complex, real-world example**.
> All the main code comes from a **real, compiling, tested sample app** (`samples/ShopApi`, .NET 10, 17 passing tests).

---

## How this book is organised

Every chapter follows the same pattern:

| Section | What you get |
|---|---|
| **Why interviewers ask this** | The reason the topic matters |
| **Simple words + analogy** | Understand it like you would explain to a friend |
| **Easy example** | The smallest code that shows the idea |
| **Complex example** | How it looks in real production code |
| ⚠️ **Interview trap** | Common wrong answers that cost candidates the job |
| 🎯 **Say this in the interview** | A short, confident model answer |
| **Try it yourself** | Exercises with hidden answers (click to expand) |
| **Quick revision** | One-liners to read 10 minutes before the interview |

---

## What is expected from a 2-YOE .NET developer?

| Area | Junior (0-1 yr) | **You (2 yrs)** — expected | Senior (5+ yrs) |
|---|---|---|---|
| C# | Syntax, OOP | async/await correctly, LINQ, generics, records, IQueryable vs IEnumerable | Memory, Span<T>, perf tuning |
| ASP.NET Core basics | Make a controller | **Pipeline, middleware order, DI lifetimes, configuration/options** | Hosting internals |
| Web API | GET/POST | **Status codes, validation, ProblemDetails, versioning, pagination** | API governance |
| EF Core | Basic CRUD | **Tracking, N+1, Include vs projection, migrations, transactions, concurrency, stored procs** | Query tuning at scale |
| Security | [Authorize] | **JWT, claims, roles vs policies, CORS, 401 vs 403** | OAuth/OIDC design |
| Quality | Some tests | **Unit tests with mocks + integration tests (WebApplicationFactory)** | Test strategy |
| Design | — | **SOLID, repository/UoW debate, clean architecture basics** | System design |
| Ops | — | **Logging, exception handling, health checks, caching, deployment basics** | Observability strategy |

🎯 At 2 YOE the interviewer mostly checks: *"Do you understand **why** things work, not only **how** to type them?"*
So for every topic, be ready to answer **What is it? Why do we need it? When NOT to use it? What problem did you face with it?**

---

## The 3-day plan

### Day 1 — Foundations (≈ 8 hours)
| Time | Chapter | Goal |
|---|---|---|
| 1.5 h | [1. C# essentials](01-csharp-essentials.md) | async/await, LINQ, IEnumerable vs IQueryable |
| 1 h | [2. Fundamentals](02-fundamentals.md) | Explain Program.cs line by line |
| 1.5 h | [3. Middleware](03-middleware.md) | Draw the pipeline, know the order |
| 1.5 h | [4. Dependency Injection](04-dependency-injection.md) | Lifetimes + captive dependency |
| 1 h | [5. Configuration & Logging](05-configuration-options-logging.md) | IOptions variants, structured logs |
| 1 h | [0. Setup](00-setup.md) + run the sample | Run the tests, call the API |

### Day 2 — Building APIs (≈ 8-9 hours)
| Time | Chapter | Goal |
|---|---|---|
| 1.5 h | [6. Web API](06-web-api-controllers.md) | Routing, binding, validation, status codes |
| 0.5 h | [7. Minimal APIs](07-minimal-apis.md) | TypedResults, route groups |
| 1 h | [8. Filters](08-filters.md) | Filter pipeline, filters vs middleware |
| 2.5 h | [9. EF Core](09-ef-core.md) | **Most asked!** N+1, tracking, migrations, stored procs |
| 1.5 h | [10. Auth](10-auth.md) | JWT end-to-end, policies |
| 1 h | [17. Capstone](17-capstone.md) | Walk through the whole sample app |

### Day 3 — Production topics + practice (≈ 8 hours)
| Time | Chapter | Goal |
|---|---|---|
| 1 h | [11. Errors, logging, health](11-error-handling-logging-health.md) | IExceptionHandler, ProblemDetails |
| 1 h | [12. Performance](12-performance-caching-async.md) | Caching, HttpClientFactory, BackgroundService |
| 1 h | [13. Testing](13-testing.md) | Moq + WebApplicationFactory |
| 1 h | [14. Security & deployment](14-security-deployment.md) | OWASP, publish, Docker |
| 1 h | [15. Architecture](15-architecture-patterns.md) | SOLID, clean architecture |
| 0.5 h | [16. MVC/Razor](16-mvc-razor-quick-tour.md) | Only if the job mentions MVC |
| 1.5 h | [18. Theory Q&A](18-theory-qa.md) | Read answers **out loud** |
| 1 h | [19. Mock & cheat sheet](19-mock-cheatsheet.md) | Do the mock with a timer |

**Interview morning:** read only the *Quick revision* sections + the cheat sheet in chapter 19.

---

## The one picture to remember

```text
  HTTP request
      │
      ▼
 ┌──────────── Kestrel (web server) ────────────┐
 │  Middleware pipeline (order matters!)         │
 │  ExceptionHandler → HTTPS → Routing → CORS →  │
 │  Authentication → Authorization → RateLimit → │
 │  your custom middleware → Endpoint            │
 │                              │                │
 │          ┌───────────────────▼─────────────┐  │
 │          │ Controller (MVC filters around) │  │
 │          │  or Minimal API endpoint        │  │
 │          └───────────────┬─────────────────┘  │
 │                          ▼                    │
 │           Services (DI)  →  EF Core  →  DB    │
 └───────────────────────────────────────────────┘
      │
      ▼
  HTTP response (JSON / ProblemDetails)
```

If you can explain this picture **from left to right**, slowly and with examples, you can already handle half of the interview.

---

## Companion book
For SQL, joins, window functions, indexes and **stored procedures**, use your companion book:
`Documents\SQL-Interview-Book\SQL-Interview-Book-ALL-IN-ONE.md`. Chapter 9 of this book shows how to **call those stored procedures from EF Core**.

Good luck — you've got this! 💪

---

## Table of Contents

- 0. Setup & the Sample App
- Chapter 1. C# Essentials for ASP.NET Core Interviews
- Chapter 2. ASP.NET Core Fundamentals
- Chapter 3. Middleware and the Request Pipeline
- Chapter 4. Dependency Injection
- Chapter 5. Configuration, Options and Logging
- 6. Web API: Controllers, Routing, Model Binding, Validation
- 7. Minimal APIs
- 8. Filters
- 9. Entity Framework Core
- 10. Authentication, Authorization, JWT, OAuth & CORS
- 11. Error Handling, Logging & Health Checks
- 12. Performance: Async, Caching, Rate Limiting, HttpClient & Background Work
- 13. Testing ASP.NET Core Applications
- 14. Security (OWASP) & Deployment
- 15. Architecture & Design Patterns
- 16. MVC, Razor Pages & Blazor — Quick Tour
- 17. Capstone: The Shop API End-to-End
- 18. Theory Q&A (75 questions)
- 19. Mock Interview & Cheat Sheet

---

# 0. Setup & the Sample App

> **Why this chapter?** Reading is not enough. Interviewers can tell when someone has *really run* code.
> In 20 minutes you will have a real ASP.NET Core API running on your laptop, with tests.

---

## 0.1 What you need

| Tool | Why | Check |
|---|---|---|
| .NET SDK 8, 9 or 10 (book uses **10**) | Build & run | `dotnet --list-sdks` |
| VS Code (C# Dev Kit) or Visual Studio 2022+/2026 | Editing & debugging | — |
| A terminal (PowerShell) | Run commands | — |
| Optional: SQL Server LocalDB | Stored-procedure examples in Ch. 9 | `sqllocaldb info` |

> Everything in this book runs **only on your own machine** (`localhost`, a local SQLite file, in-memory test servers). No cloud, no shared databases.

---

## 0.2 The sample app: `samples/ShopApi`

```text
ASPNETCore-Interview-Book/
├── book.toml / src/            ← this book
└── samples/
    ├── ShopSamples.slnx        ← solution (new .slnx format, .NET 9+ SDK)
    ├── ShopApi/                ← the Web API
    │   ├── Program.cs          ← DI registrations + middleware pipeline
    │   ├── Models/Entities.cs  ← Category, Product, Order, OrderItem
    │   ├── Data/AppDbContext.cs← EF Core (SQLite), seed data, soft-delete filter
    │   ├── Dtos/ProductDtos.cs ← request/response records + validation attributes
    │   ├── Services/ProductService.cs   ← business logic (IProductService)
    │   ├── Controllers/ProductsController.cs ← CRUD controller
    │   ├── Controllers/AuthController.cs     ← issues demo JWTs
    │   ├── Endpoints/CategoryEndpoints.cs    ← Minimal API route group
    │   ├── Middleware/RequestTimingMiddleware.cs
    │   ├── Errors/GlobalExceptionHandler.cs  ← IExceptionHandler → ProblemDetails
    │   ├── Filters/LogActionFilter.cs        ← global MVC action filter
    │   ├── Options/JwtOptions.cs             ← options pattern + validation
    │   ├── Background/PendingOrderCleanupService.cs ← BackgroundService
    │   └── Clients/ExchangeRateClient.cs     ← typed HttpClient
    └── ShopApi.Tests/          ← xUnit: unit tests (Moq, SQLite) + integration tests
```

Every chapter points to the file that demonstrates its topic. **Chapter 17** walks through the whole app end-to-end.

---

## 0.3 Run it

```bash
cd samples
dotnet build ShopSamples.slnx
dotnet test ShopSamples.slnx          # expect: Passed! - Failed: 0, Passed: 17
cd ShopApi
dotnet run --urls http://localhost:5199 --environment Development
```

> **If `dotnet build/test` fails with strange MSBuild errors** like *"The target VSTest does not exist"* or *"NuGet.Build.Tasks... could not be loaded"*: your terminal has Visual Studio MSBuild variables set (common in big company dev shells). Open a **plain** terminal, or clear them for this session:
> ```powershell
> 'MSBUILDEXTENSIONSPATH','MSBUILDEXTENSIONSPATH32','MSBUILDFRAMEWORKTOOLSPATH','MSBUILDLOCATION','VSMSBUILDDIR' |
>   % { Remove-Item "env:$_" -ErrorAction SilentlyContinue }
> ```

### Try the endpoints (PowerShell)

```powershell
$base = "http://localhost:5199"

Invoke-RestMethod "$base/api/products?page=1&pageSize=2"     # 200 + paged JSON
Invoke-WebRequest "$base/api/products/999" -SkipHttpErrorCheck # 404 ProblemDetails
Invoke-RestMethod "$base/api/categories"                      # Minimal API
Invoke-RestMethod "$base/health"                              # "Healthy"
Invoke-RestMethod "$base/openapi/v1.json"                     # OpenAPI document (Development only)

# Get a token (demo users: admin/admin123, user/user123)
$token = (Invoke-RestMethod -Method Post "$base/api/auth/token" -ContentType 'application/json' `
          -Body '{"userName":"admin","password":"admin123"}').access_token

# Create a product (needs a token)
Invoke-RestMethod -Method Post "$base/api/products" -Headers @{ Authorization = "Bearer $token" } `
  -ContentType 'application/json' -Body '{"name":"Keyboard","price":30,"stock":15,"categoryId":1}'
```

What we observed when we ran it (verified output):

```text
/api/products?pageSize=2 -> 200  {"items":[{"id":1,"name":"Laptop","price":800.0,"stock":10,"category":"Electronics"},{"id":2,"name":"Mouse",...}],...}
/api/products/999        -> 404  application/problem+json
/api/categories          -> 200  [{"id":1,"name":"Electronics","productCount":2},{"id":2,"name":"Books","productCount":1}]
/health                  -> 200  Healthy
POST invalid product     -> 400  {"title":"One or more validation errors occurred.","status":400,
                                  "errors":{"Name":["The Name field is required.", ...],"Price":["The field Price must be between 0.01 and 1000000."]}}
Every response also had headers:  X-Correlation-Id: <guid>   X-Elapsed-Ms: <n>
```

---

## 0.4 How the sample was created (commands you should know)

```bash
dotnet new sln -n ShopSamples                       # .slnx in .NET 10 SDK
dotnet new webapi --use-controllers -n ShopApi -f net10.0
dotnet new xunit -n ShopApi.Tests -f net10.0
dotnet sln add ShopApi ShopApi.Tests
dotnet add ShopApi.Tests reference ShopApi

dotnet add ShopApi package Microsoft.EntityFrameworkCore.Sqlite
dotnet add ShopApi package Microsoft.AspNetCore.Authentication.JwtBearer
dotnet add ShopApi.Tests package Microsoft.AspNetCore.Mvc.Testing
dotnet add ShopApi.Tests package Moq
```

| Command | Meaning |
|---|---|
| `dotnet new <template>` | Create project from template (`webapi`, `web` = empty, `mvc`, `razor`, `console`, `xunit`) |
| `dotnet restore` | Download NuGet packages (build does it automatically) |
| `dotnet build` | Compile |
| `dotnet run` | Build + run |
| `dotnet watch` | Run + hot reload on file change |
| `dotnet test` | Run tests |
| `dotnet publish -c Release` | Produce deployable output |
| `dotnet ef migrations add X` | EF Core migrations (needs `dotnet tool install -g dotnet-ef`) |

---

## Try it yourself

1. Run the tests. How many pass?
2. Call `GET /api/products?page=2&pageSize=2`. What is `totalCount`?
3. Call `DELETE /api/products/1` with the **user** token. What status code and why?
4. Remove the `Authorization` header from the POST call. What status code?

<details><summary>Answer</summary>

1. 17.
2. `totalCount` = 3 (seed data has 3 products); page 2 contains 1 item ("C# in Depth").
3. **403 Forbidden** — you are authenticated (valid token) but the `AdminOnly` policy requires role `Admin`.
4. **401 Unauthorized** — no/invalid credentials. (401 = "who are you?", 403 = "I know you, but no".)
</details>

## Quick revision
- The sample is a real .NET 10 Web API: controllers + minimal APIs + EF Core (SQLite) + JWT + tests.
- `dotnet new webapi --use-controllers` creates a controller-based API; without the flag you get minimal APIs.
- `dotnet test` runs unit + integration tests; integration tests host the app in memory.
- Demo users: `admin/admin123` (Admin), `user/user123` (User).
- OpenAPI JSON is at `/openapi/v1.json` (Development only).

---

# Chapter 1. C# Essentials for ASP.NET Core Interviews

> **Why interviewers ask this:** ASP.NET Core is "just C#". Before anyone asks you about middleware or DI, they want to know whether you *really* understand the language: how memory works (value vs reference), why `async` code deadlocks, why a LINQ query hits the database twice, and why `throw ex;` is a code-review red flag. A shaky answer here makes everything else sound memorised.

All console demos in this chapter were compiled and run on .NET 10 with top-level statements. You can paste each one into `Program.cs` of a `dotnet new console` project and get the exact output shown.

---

## 1.1 Value types vs reference types

### In simple words

- A **value type** variable *holds the data itself*. Copying it copies the data. (`int`, `double`, `bool`, `decimal`, `DateTime`, `Guid`, `enum`, every `struct`.)
- A **reference type** variable *holds an address* (a reference) pointing to an object on the heap. Copying it copies the address — both variables now point to the **same** object. (`class`, `string`, arrays, `record` (class), delegates, `object`.)

**Analogy:** A value type is a *photocopy* of a document — scribble on your copy and mine is untouched. A reference type is a *Google Docs link* — I send you the link, you edit, I see the edit.

### Easy example (verified)

```csharp
// ---- Value type: the VALUE is copied ----
int a = 10;
int b = a;          // b gets its own copy of 10
b = 20;
Console.WriteLine($"a={a}, b={b}");               // a=10, b=20

// ---- Reference type: the REFERENCE (address) is copied ----
var p1 = new PersonClass { Name = "Asha" };
var p2 = p1;        // p2 points to the SAME object
p2.Name = "Ravi";
Console.WriteLine($"p1.Name={p1.Name}");           // p1.Name=Ravi

// ---- Struct is a value type: whole object copied ----
var s1 = new PointStruct { X = 1 };
var s2 = s1;        // independent copy
s2.X = 99;
Console.WriteLine($"s1.X={s1.X}, s2.X={s2.X}");    // s1.X=1, s2.X=99

// ---- Passing to methods ----
int n = 5;
AddOne(n);
Console.WriteLine($"after AddOne: n={n}");        // 5  (copy was changed)
AddOneRef(ref n);
Console.WriteLine($"after AddOneRef: n={n}");     // 6  (original changed)

Rename(p1);
Console.WriteLine($"after Rename: {p1.Name}");    // Kiran (same object)
Replace(p1);
Console.WriteLine($"after Replace: {p1.Name}");   // Kiran (only local copy of reference replaced)

static void AddOne(int x) => x++;
static void AddOneRef(ref int x) => x++;
static void Rename(PersonClass p) => p.Name = "Kiran";
static void Replace(PersonClass p) => p = new PersonClass { Name = "Nobody" };

public class PersonClass { public string Name { get; set; } = ""; }
public struct PointStruct { public int X; }
```

Output:

```text
a=10, b=20
p1.Name=Ravi
s1.X=1, s2.X=99
after AddOne: n=5
after AddOneRef: n=6
after Rename: Kiran
after Replace: Kiran
```

The last two lines are the classic trick question: **C# passes everything by value by default — including references.** `Rename` gets a *copy of the address*, so it can change the object. `Replace` assigns a new object to its *local copy* of the address, which the caller never sees. Only `ref`/`out` pass the variable itself.

### Stack vs heap (how it works internally — briefly)

| | Value type | Reference type |
|---|---|---|
| Where the data lives | Inline: on the stack (locals) or inside the containing object | Always on the managed heap |
| What a variable holds | The data | A reference (pointer) to the data |
| Default value | All-zero (`0`, `false`, `default(DateTime)`) | `null` |
| Assignment copies | The whole data | Only the reference |
| Garbage collected? | No (freed when the stack frame/containing object goes) | Yes, by the GC |
| Can be `null`? | Only as `Nullable<T>` (`int?`) | Yes |

⚠️ **Interview trap:** "Value types live on the stack, reference types on the heap." Half true. A value type that is a *field of a class* lives on the heap inside that object. Also boxed values and values captured by lambdas/`async` methods end up on the heap. Say "value types are stored *inline* where they're declared".

🎯 **Say this in the interview:** "Value types hold their data directly and are copied on assignment; reference types hold a reference to a heap object, so copies share the same object. Everything in C# is passed by value by default — for reference types that value is the reference itself — unless you use `ref`, `out` or `in`."

---

## 1.2 `class` vs `struct` vs `record`

### In simple words

- **class** – a normal object with identity. Two `Customer` objects with the same data are still *two different customers*.
- **struct** – a small value type. Copied around; good for tiny, immutable data (`Point`, `Money`, `DateTime`).
- **record** (C# 9+) – a class (or `record struct`, C# 10+) designed for **data**. The compiler generates value-based equality, a nice `ToString()`, `Deconstruct`, and `with`-expressions. Perfect for DTOs, API requests/responses, events.

**Analogy:** A `class` is a *person* (two people named "Asha" are different people). A `record` is a *banknote value* (two ₹10 notes are "equal" for all practical purposes).

### Verified example: equality

```csharp
// class: reference equality by default
var c1 = new MoneyClass(10, "INR");
var c2 = new MoneyClass(10, "INR");
Console.WriteLine($"class  ==     : {c1 == c2}");        // False (different objects)

// record: value equality generated by the compiler
var r1 = new Money(10, "INR");
var r2 = new Money(10, "INR");
Console.WriteLine($"record ==     : {r1 == r2}");        // True
Console.WriteLine($"record ToString: {r1}");             // Money { Amount = 10, Currency = INR }

// non-destructive mutation with 'with'
var r3 = r1 with { Amount = 25 };
Console.WriteLine($"r1={r1.Amount}, r3={r3.Amount}");    // r1=10, r3=25

// string: == is overloaded to compare CONTENT
string x = "hello";
string y = new string(['h', 'e', 'l', 'l', 'o']);
Console.WriteLine($"string ==     : {x == y}");                    // True
Console.WriteLine($"same object?  : {ReferenceEquals(x, y)}");     // False

// boxing: int -> object allocates a box on the heap
object o1 = 5;
object o2 = 5;
Console.WriteLine($"boxed ==      : {o1 == o2}");        // False (two different boxes!)
Console.WriteLine($"boxed Equals  : {o1.Equals(o2)}");   // True  (Int32.Equals compares values)

public class MoneyClass(decimal amount, string currency)
{
    public decimal Amount { get; } = amount;
    public string Currency { get; } = currency;
}

public record Money(decimal Amount, string Currency);
```

Output:

```text
class  ==     : False
record ==     : True
record ToString: Money { Amount = 10, Currency = INR }
r1=10, r3=25
string ==     : True
same object?  : False
boxed ==      : False
boxed Equals  : True
```

### Comparison table

| Feature | `class` | `struct` | `record` (class) | `record struct` |
|---|---|---|---|---|
| Kind | Reference | Value | Reference | Value |
| Default equality | Reference (same object?) | Field-by-field (via reflection — slow, override it) | Value (compiler-generated) | Value (compiler-generated) |
| Inheritance | Yes | No | Yes (records from records) | No |
| `with` expression | No | Yes (C# 10+) | Yes | Yes |
| Nice `ToString()` | No | No | Yes | Yes |
| Typical use | Services, entities, controllers | Tiny values (≤16 bytes guideline) | DTOs, requests, responses, messages | Small value-like data |

### Real-world use in ASP.NET Core

```csharp
// Request/response DTOs: records are concise and immutable-by-default (positional properties are init-only)
public record CreateProductRequest(string Name, decimal Price, int Stock, int CategoryId);
public record ProductDto(int Id, string Name, decimal Price, int Stock, string Category);

// EF Core entity: a class, because it has identity and is tracked/mutated
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; } = "";
    public decimal Price { get; set; }
}
```

The sample app (samples/ShopApi) does exactly this: `ProductDto` is a record built in a LINQ `Select`, while `Product` is a mutable EF entity class.

⚠️ **Interview trap:** "Records are immutable." Not exactly — positional properties are `init`-only, but you can still declare `{ get; set; }` properties or mutable collections inside a record. Records are *designed* for immutability, not forced into it.

⚠️ **Interview trap:** Don't use records as EF Core entities. EF tracks entities by identity; value equality and `with` copies confuse change tracking.

🎯 **Say this in the interview:** "I use classes for things with identity and behaviour — services, entities. Records for data carriers like DTOs because I get value equality, `with` and a readable `ToString` for free. Structs only for tiny immutable values where avoiding heap allocations matters."

---

## 1.3 Interfaces vs abstract classes

### In simple words

- **Interface** = a *contract*: "anything that implements me **can do** these things". No state (no instance fields). A class can implement **many** interfaces.
- **Abstract class** = a *partially built base*: "everything that **is a** me shares this code and state". It can have fields, constructors, and implemented methods. A class can inherit **only one** class.

**Analogy:** An interface is a *job description* ("must be able to drive"). An abstract class is a *franchise kit* — the kitchen, recipes and brand already exist; you just fill in a few local details.

### Verified example

```csharp
// Interface = a CONTRACT ("can do"). Abstract class = a partial BASE ("is a").
INotifier[] notifiers = [new EmailNotifier(), new SmsNotifier()];
foreach (INotifier n in notifiers)
{
    n.Send("Order #42 shipped");
}

PaymentProcessor processor = new CardPaymentProcessor();
processor.Process(499.00m);

public interface INotifier
{
    void Send(string message);

    // Default interface method (C# 8+): optional shared behaviour
    string Channel => GetType().Name.Replace("Notifier", "");
}

public class EmailNotifier : INotifier
{
    public void Send(string message) => Console.WriteLine($"[{((INotifier)this).Channel}] {message}");
}

public class SmsNotifier : INotifier
{
    public void Send(string message) => Console.WriteLine($"[SMS] {message[..Math.Min(10, message.Length)]}...");
}

public abstract class PaymentProcessor
{
    // Template method: the fixed algorithm lives in the base class
    public void Process(decimal amount)
    {
        Console.WriteLine($"Validating {amount:0.00}");
        Charge(amount);                       // the step each child must provide
        Console.WriteLine("Writing audit log");
    }

    protected abstract void Charge(decimal amount);
}

public class CardPaymentProcessor : PaymentProcessor
{
    protected override void Charge(decimal amount) => Console.WriteLine($"Charging card {amount:0.00}");
}
```

Output:

```text
[Email] Order #42 shipped
[SMS] Order #42 ...
Validating 499.00
Charging card 499.00
Writing audit log
```

| | Interface | Abstract class |
|---|---|---|
| Multiple inheritance | A class can implement many | Only one base class |
| Fields / state | No instance fields | Yes |
| Constructors | No | Yes |
| Method bodies | Only default interface methods (C# 8+) and static members | Yes |
| Access modifiers on members | Public by default | Any |
| Relationship | "can do" (capability) | "is a" (family) |
| In ASP.NET Core | **Everything registered in DI** (`IProductService`, `ILogger<T>`, `IMiddleware`) | Framework base types: `ControllerBase`, `BackgroundService`, `DbContext` |

**Why DI loves interfaces:** controllers depend on `IProductService`, not `ProductService`. In tests you swap in a fake. In production you can swap implementations without touching the consumer.

⚠️ **Interview trap:** "Interfaces can't have any implementation." Since C# 8 they can have **default interface methods**, and since C# 11 **static abstract members** (used by generic math). But they still can't hold instance state.

🎯 **Say this in the interview:** "An interface defines a capability contract and a class can implement many; an abstract class shares state and code across a family and you can inherit only one. In ASP.NET Core I put interfaces on services so they're injectable and mockable, and use abstract base classes when there's a real shared algorithm, like `BackgroundService.ExecuteAsync`."

---

## 1.4 Generics

### In simple words

Generics let you write code **once** with a placeholder type `T`, and the compiler creates a type-safe version for each real type. `List<int>` and `List<string>` share code but can't mix up items, and value types are not boxed.

**Analogy:** A generic is a *cookie cutter*: same shape, any dough.

### Verified example — easy and real-world

```csharp
// ---- Easy: a generic method ----
Console.WriteLine(Max(3, 7));             // 7
Console.WriteLine(Max("apple", "mango")); // mango

// ---- Real-world: a generic in-memory repository with constraints ----
var repo = new InMemoryRepository<Product>();
repo.Add(new Product { Id = 1, Name = "Keyboard" });
repo.Add(new Product { Id = 2, Name = "Mouse" });
Console.WriteLine(repo.GetById(2)?.Name);  // Mouse
Console.WriteLine(repo.GetById(9)?.Name ?? "not found");

static T Max<T>(T a, T b) where T : IComparable<T> => a.CompareTo(b) >= 0 ? a : b;

public interface IEntity { int Id { get; } }

public class Product : IEntity
{
    public int Id { get; init; }
    public string Name { get; init; } = "";
}

public interface IRepository<T> where T : class, IEntity
{
    void Add(T item);
    T? GetById(int id);
}

public class InMemoryRepository<T> : IRepository<T> where T : class, IEntity
{
    private readonly Dictionary<int, T> _items = [];
    public void Add(T item) => _items[item.Id] = item;
    public T? GetById(int id) => _items.GetValueOrDefault(id);
}
```

Output:

```text
7
mango
Mouse
not found
```

### Constraints cheat-sheet

| Constraint | Meaning |
|---|---|
| `where T : class` | Reference type |
| `where T : struct` | Non-nullable value type |
| `where T : notnull` | Non-nullable (value or reference) |
| `where T : new()` | Has a public parameterless constructor |
| `where T : SomeBase` | Inherits from `SomeBase` |
| `where T : ISomething` | Implements an interface |
| `where T : unmanaged` | Struct with no references (for low-level code) |

Generics are everywhere in ASP.NET Core: `ILogger<T>`, `IOptions<T>`, `DbSet<T>`, `ActionResult<T>`, `services.AddScoped<TService, TImpl>()`, and **open-generic DI registration** like `services.AddScoped(typeof(IRepository<>), typeof(EfRepository<>))` (Chapter 4).

⚠️ **Interview trap:** "Generics are like `object` with casts." No — `List<int>` stores real `int`s with no boxing, and type errors are caught at compile time. For value types the runtime even generates specialised native code per type.

---

## 1.5 Delegates, `Func`, `Action`, lambdas, extension methods

### In simple words

- A **delegate** is a *type-safe variable that holds a method*. You can pass behaviour around like data.
- `Func<...,TResult>` — built-in delegate that **returns** a value (last type parameter = return type).
- `Action<...>` — built-in delegate that returns **void**.
- `Predicate<T>` — returns `bool` (older; `Func<T,bool>` is more common).
- A **lambda** (`x => x * 2`) is the short syntax to create a delegate (or an expression tree — see IQueryable).
- An **extension method** is a static method that *looks like* an instance method on a type you don't own.

**Analogy:** A delegate is a *phone number* for a function — you can hand it to someone and they can call it later.

### Verified example

```csharp
// Func<in..., out TResult>  -> returns a value
Func<int, int, int> add = (x, y) => x + y;
// Action<in...>             -> returns void
Action<string> log = msg => Console.WriteLine($"LOG: {msg}");
// Predicate<T>              -> returns bool (same as Func<T,bool>)
Predicate<int> isEven = n => n % 2 == 0;

log($"2 + 3 = {add(2, 3)}");             // LOG: 2 + 3 = 5
log($"Is 4 even? {isEven(4)}");          // LOG: Is 4 even? True

// A custom delegate type
Discount festive = price => price * 0.9m;
Console.WriteLine(festive(1000m));       // 900.0

// Passing behaviour into a method (this is what LINQ does)
int[] numbers = [1, 2, 3, 4, 5, 6];
Console.WriteLine(string.Join(",", Filter(numbers, isEven.Invoke)));   // 2,4,6

// Closure: the lambda "captures" the variable, not its value
int counter = 0;
Action increment = () => counter++;
increment(); increment();
Console.WriteLine($"counter = {counter}");  // counter = 2

// Extension methods
string email = "  Asha@Example.COM ";
Console.WriteLine(email.NormalizeEmail());  // asha@example.com
Console.WriteLine("hello world".ToTitleCase()); // Hello World

static IEnumerable<int> Filter(IEnumerable<int> source, Func<int, bool> rule)
{
    foreach (int item in source)
        if (rule(item)) yield return item;
}

public delegate decimal Discount(decimal price);

public static class StringExtensions
{
    public static string NormalizeEmail(this string value) => value.Trim().ToLowerInvariant();

    public static string ToTitleCase(this string value) =>
        string.Join(' ', value.Split(' ', StringSplitOptions.RemoveEmptyEntries)
                              .Select(w => char.ToUpperInvariant(w[0]) + w[1..]));
}
```

Output:

```text
LOG: 2 + 3 = 5
LOG: Is 4 even? True
900.0
2,4,6
counter = 2
asha@example.com
Hello World
```

### Where you see these in ASP.NET Core

| Construct | ASP.NET Core example |
|---|---|
| `Func<HttpContext, RequestDelegate, Task>` | `app.Use(async (context, next) => { ... })` |
| `RequestDelegate` (a custom delegate: `Task (HttpContext)`) | The type of `next` in every middleware |
| `Action<T>` for configuration | `services.AddCors(o => ...)`, `AddJwtBearer(o => ...)` |
| `Func<IServiceProvider, T>` | Factory registration `AddSingleton<IClock>(sp => new SystemClock())` |
| Extension methods | **All** of `builder.Services.AddXxx()` and `app.UseXxx()` are extension methods — e.g. the sample's `UseRequestTiming()` |

**Rules for extension methods:** static class, static method, first parameter prefixed with `this`. The real instance method always wins if both exist.

⚠️ **Interview trap (closures):** A lambda captures the **variable**, not a snapshot of its value. That's why `counter` became 2. It also means a lambda that captures a scoped service (like a `DbContext`) and is stored in a singleton keeps that service alive past its lifetime.

🎯 **Say this in the interview:** "A delegate is a type-safe reference to a method. `Func` returns a value, `Action` returns void. Lambdas create delegates, and ASP.NET Core is built on them — middleware is literally a chain of `RequestDelegate`s, and every `AddXxx`/`UseXxx` is an extension method taking an `Action<Options>`."

---

## 1.6 `IEnumerable` vs `IQueryable` vs `List` — and deferred execution

This is **the** most important LINQ interview topic for ASP.NET developers because it decides whether filtering happens in SQL or in your server's memory.

### In simple words

- **`List<T>`** – data already **in memory**. Fully loaded. Has `Count`, indexer, `Add`.
- **`IEnumerable<T>`** – "something I can loop over". LINQ operators on it take **delegates** (`Func<T,bool>`) and run **in memory** (LINQ to Objects).
- **`IQueryable<T>`** – "a query not yet run against a *data source*". LINQ operators take **expression trees** (`Expression<Func<T,bool>>`), which a provider such as EF Core **translates into SQL**.

**Deferred execution:** `Where`, `Select`, `OrderBy`... don't run when you write them. They only build a "recipe". The recipe runs when you **enumerate**: `foreach`, `ToList()`, `ToArray()`, `Count()`, `First()`, `Any()`, `Sum()`...

**Analogy:** Writing a LINQ query is writing a *shopping list*. Nothing is bought until you go to the shop (`ToList()`). Go twice and you shop twice.

### Verified example: deferred execution and multiple enumeration

```csharp
var numbers = new List<int> { 1, 2, 3, 4, 5 };

// Building the query runs NOTHING yet
IEnumerable<int> query = numbers.Where(n =>
{
    Console.WriteLine($"  checking {n}");
    return n % 2 == 0;
});

Console.WriteLine("Query defined. Adding 6 to the list...");
numbers.Add(6);   // the query will see this, because it hasn't run yet

Console.WriteLine("First foreach:");
foreach (int n in query) Console.WriteLine($"  -> {n}");

Console.WriteLine($"Count(): {query.Count()}");   // runs the WHOLE query AGAIN

// Materialize once with ToList() to stop re-execution
List<int> evens = query.ToList();
Console.WriteLine($"Materialized: {string.Join(",", evens)}");

// IQueryable builds an EXPRESSION TREE (data), not compiled code
IQueryable<int> q = numbers.AsQueryable().Where(n => n > 3).OrderBy(n => n);
Console.WriteLine($"Expression: {q.Expression}");
```

Output:

```text
Query defined. Adding 6 to the list...
First foreach:
  checking 1
  checking 2
  -> 2
  checking 3
  checking 4
  -> 4
  checking 5
  checking 6
  -> 6
  checking 1
  checking 2
  checking 3
  checking 4
  checking 5
  checking 6
Count(): 3
  checking 1
  checking 2
  checking 3
  checking 4
  checking 5
  checking 6
Materialized: 2,4,6
Expression: System.Collections.Generic.List`1[System.Int32].Where(n => (n > 3)).OrderBy(n => n)
```

Notice three things:

1. Nothing printed "checking" until the `foreach` — **deferred**.
2. `6` was included even though it was added *after* the query was defined.
3. `Count()` and `ToList()` each re-ran the whole filter — **multiple enumeration**. With EF Core, that's **multiple database round-trips**.

The last line shows that an `IQueryable` holds an **expression tree** — a data structure describing the query. EF Core walks that tree and writes SQL.

### The real-world (EF Core) consequence

```csharp
// ✅ IQueryable: filter, sort and paging become SQL (WHERE ... ORDER BY ... OFFSET/FETCH)
IQueryable<Product> query = db.Products.AsNoTracking();
if (!string.IsNullOrWhiteSpace(search))
    query = query.Where(p => p.Name.Contains(search));     // still not executed
List<ProductDto> page = await query
    .OrderBy(p => p.Id)
    .Skip((pageNo - 1) * pageSize)
    .Take(pageSize)
    .Select(p => new ProductDto(p.Id, p.Name, p.Price, p.Stock, p.Category!.Name))
    .ToListAsync(ct);                                        // ONE SQL query runs here

// ❌ Casting to IEnumerable too early: the WHOLE table is downloaded, then filtered in C#
IEnumerable<Product> all = db.Products;                      // the static type is now IEnumerable
var cheap = all.Where(p => p.Price < 100).ToList();          // SELECT * FROM Products ... then filter in memory
```

The first block is (simplified) what `ProductService.GetPageAsync` in the sample app (samples/ShopApi) does — it keeps building an `IQueryable<Product>` and only materialises once with `ToListAsync`.

| | `List<T>` | `IEnumerable<T>` | `IQueryable<T>` |
|---|---|---|---|
| Namespace | `System.Collections.Generic` | `System.Collections.Generic` | `System.Linq` |
| Data | Already in memory | In memory (or streamed) | Remote source (DB, etc.) |
| Execution | Immediate (it *is* the data) | Deferred, in-process | Deferred, translated by a provider |
| LINQ operators take | — | `Func<T,bool>` (compiled code) | `Expression<Func<T,bool>>` (expression tree) |
| Filtering happens | — | In your app's memory | In the database (SQL) |
| Best for | Results you will reuse | In-memory pipelines, `yield` | Building DB queries |
| Inheritance | Implements `IEnumerable<T>` | Base interface | Inherits `IEnumerable<T>` |

⚠️ **Interview trap:** Returning `IQueryable<T>` from a repository/service *to a controller* leaks query building outside your data layer and the query may execute after the `DbContext` is disposed. Return materialised results (`List<T>`, DTOs) from services.

⚠️ **Interview trap:** Calling a C# method EF can't translate inside `Where` (e.g. `Where(p => MyHelper(p))`) throws "could not be translated" in EF Core 3+. EF Core no longer silently evaluates on the client (except in the final `Select`).

🎯 **Say this in the interview:** "`IEnumerable` executes LINQ in memory with delegates; `IQueryable` builds an expression tree that a provider like EF Core translates to SQL. Both are deferred, so nothing runs until I enumerate — and enumerating twice runs it twice. In services I compose on `IQueryable` and materialise once with `ToListAsync`."

---

## 1.7 LINQ operators you must know

### Verified example

```csharp
var categories = new List<Category>
{
    new(1, "Electronics"),
    new(2, "Books"),
};

var products = new List<Product>
{
    new(1, "Laptop",   65000m, 1),
    new(2, "Phone",    25000m, 1),
    new(3, "C# Book",    800m, 2),
    new(4, "Headphones", 2000m, 1),
};

// Where + Select (filter + projection)
var cheapNames = products.Where(p => p.Price < 5000).Select(p => p.Name);
Console.WriteLine($"Cheap: {string.Join(", ", cheapNames)}");

// OrderByDescending + Take
var top2 = products.OrderByDescending(p => p.Price).Take(2).Select(p => p.Name);
Console.WriteLine($"Top 2: {string.Join(", ", top2)}");

// GroupBy + aggregate
foreach (var g in products.GroupBy(p => p.CategoryId))
    Console.WriteLine($"Category {g.Key}: count={g.Count()}, total={g.Sum(p => p.Price)}");

// Join (inner join on CategoryId)
var joined = products.Join(categories,
    p => p.CategoryId,
    c => c.Id,
    (p, c) => $"{p.Name} ({c.Name})");
Console.WriteLine($"Joined: {string.Join(" | ", joined)}");

// Any / All / Contains
Console.WriteLine($"Any > 50000? {products.Any(p => p.Price > 50000)}");
Console.WriteLine($"All > 0?     {products.All(p => p.Price > 0)}");

// First vs FirstOrDefault vs Single vs SingleOrDefault
Console.WriteLine($"First electronics: {products.First(p => p.CategoryId == 1).Name}");
Console.WriteLine($"FirstOrDefault toys: {products.FirstOrDefault(p => p.CategoryId == 3)?.Name ?? "null"}");
Console.WriteLine($"Single book: {products.Single(p => p.CategoryId == 2).Name}");

Try("First on no match", () => products.First(p => p.CategoryId == 3));
Try("Single on many", () => products.Single(p => p.CategoryId == 1));
Try("SingleOrDefault on many", () => products.SingleOrDefault(p => p.CategoryId == 1));

static void Try(string label, Func<object?> action)
{
    try { action(); Console.WriteLine($"{label}: ok"); }
    catch (InvalidOperationException ex) { Console.WriteLine($"{label}: {ex.Message}"); }
}

public record Category(int Id, string Name);
public record Product(int Id, string Name, decimal Price, int CategoryId);
```

Output:

```text
Cheap: C# Book, Headphones
Top 2: Laptop, Phone
Category 1: count=3, total=92000
Category 2: count=1, total=800
Joined: Laptop (Electronics) | Phone (Electronics) | C# Book (Books) | Headphones (Electronics)
Any > 50000? True
All > 0?     True
First electronics: Laptop
FirstOrDefault toys: null
Single book: C# Book
First on no match: Sequence contains no matching element
Single on many: Sequence contains more than one matching element
SingleOrDefault on many: Sequence contains more than one matching element
```

### `First` vs `FirstOrDefault` vs `Single` vs `SingleOrDefault`

| Method | 0 matches | 1 match | 2+ matches | SQL (EF Core) | Use when |
|---|---|---|---|---|---|
| `First` | ❌ throws | returns it | returns the first | `TOP(1)` | At least one must exist |
| `FirstOrDefault` | `default` (`null`) | returns it | returns the first | `TOP(1)` | "Find if exists" — most common |
| `Single` | ❌ throws | returns it | ❌ throws | `TOP(2)` | Exactly one must exist (data integrity check) |
| `SingleOrDefault` | `default` | returns it | ❌ throws | `TOP(2)` | Zero or one (e.g. lookup by unique email) |

⚠️ **Interview trap:** "`SingleOrDefault` never throws." Wrong — it throws when there are **two or more** matches (see the output). Also, `Single` must read up to 2 rows to prove uniqueness, so it's slightly more expensive than `First`.

⚠️ **Interview trap:** `Any()` vs `Count() > 0`. `Any()` stops at the first element (SQL `EXISTS`). `Count()` counts everything. Always prefer `Any()` for "is there at least one?".

### Other operators worth knowing

| Operator | What it does | Note |
|---|---|---|
| `SelectMany` | Flattens nested collections (orders → order lines) | Like SQL `CROSS APPLY` / join |
| `GroupJoin` / `join ... into` | Left-join style grouping | In EF Core 10 there's also `LeftJoin` (.NET 10+) |
| `Distinct`, `DistinctBy` (.NET 6+) | Remove duplicates | |
| `Chunk` (.NET 6+) | Split into batches of N | Great for bulk processing |
| `ToDictionary`, `ToLookup` | Index by key | `ToDictionary` throws on duplicate keys |
| `Skip`/`Take` | Paging | Always with `OrderBy` for stable pages |
| `Aggregate` | Fold/reduce | |

🎯 **Say this in the interview:** "`First` throws when nothing matches, `FirstOrDefault` returns null; `Single` additionally throws if more than one matches, so I use it only when uniqueness is a business rule. For existence checks I use `Any()`, not `Count() > 0`."

---

## 1.8 `async`/`await` in depth

This is the topic where 2-YOE candidates are most often caught out. Read slowly.

### In simple words

An I/O operation (DB query, HTTP call, file read) spends most of its time **waiting**. `await` means: "start this, **give my thread back** to the thread pool while waiting, and continue here when the result is ready".

**Analogy:** A waiter (thread) takes your order to the kitchen (database). A *synchronous* waiter stands at the kitchen door doing nothing until the food is ready. An *async* waiter goes off and serves other tables, then comes back when the kitchen rings the bell. Same number of waiters → many more customers served. That's **scalability**, not speed.

### How it works internally (briefly)

- The compiler rewrites an `async` method into a **state machine**. Each `await` is a "pause point".
- If the awaited task isn't finished, the method **returns an incomplete `Task` to its caller** immediately and the thread goes back to the pool.
- When the I/O completes, the continuation (the rest of your method) is scheduled — in ASP.NET Core, on any thread-pool thread (there is **no `SynchronizationContext`** in ASP.NET Core).
- During a true async I/O wait, **no thread is blocked** at all ("there is no thread").

### Verified example: sequential vs `Task.WhenAll`, cancellation, exceptions

```csharp
using System.Diagnostics;

var sw = Stopwatch.StartNew();

// Sequential: each await finishes before the next starts (~300 ms total)
string user = await GetUserAsync();
string orders = await GetOrdersAsync();
string offers = await GetOffersAsync();
Console.WriteLine($"Sequential: {user}, {orders}, {offers} in ~{Round(sw.ElapsedMilliseconds)} ms");

// Concurrent: start all three, then await them together (~100 ms total)
sw.Restart();
Task<string> userTask = GetUserAsync();
Task<string> ordersTask = GetOrdersAsync();
Task<string> offersTask = GetOffersAsync();
string[] results = await Task.WhenAll(userTask, ordersTask, offersTask);
Console.WriteLine($"WhenAll:    {string.Join(", ", results)} in ~{Round(sw.ElapsedMilliseconds)} ms");

// Cancellation: give up after 50 ms
using var cts = new CancellationTokenSource(TimeSpan.FromMilliseconds(50));
try
{
    await SlowReportAsync(cts.Token);
}
catch (OperationCanceledException)
{
    Console.WriteLine("Report cancelled (timeout)");
}

// Exceptions from WhenAll: await throws the FIRST one; the task holds ALL of them
Task all = Task.WhenAll(FailAsync("A"), FailAsync("B"));
try
{
    await all;
}
catch (Exception ex)
{
    Console.WriteLine($"Caught one: {ex.GetType().Name}; all errors: {all.Exception!.InnerExceptions.Count}");
}

static async Task<string> GetUserAsync()   { await Task.Delay(100); return "user"; }
static async Task<string> GetOrdersAsync() { await Task.Delay(100); return "orders"; }
static async Task<string> GetOffersAsync() { await Task.Delay(100); return "offers"; }

static async Task SlowReportAsync(CancellationToken ct)
{
    for (int i = 0; i < 10; i++)
    {
        await Task.Delay(100, ct);   // pass the token down!
    }
}

static async Task FailAsync(string name)
{
    await Task.Delay(10);
    throw new InvalidOperationException($"{name} failed");
}

static long Round(long ms) => (long)Math.Round(ms / 100.0) * 100;
```

Output (timings are approximate):

```text
Sequential: user, orders, offers in ~300 ms
WhenAll:    user, orders, offers in ~100 ms
Report cancelled (timeout)
Caught one: InvalidOperationException; all errors: 2
```

⚠️ **Interview trap:** `Task.WhenAll` with **EF Core on the same `DbContext`** throws *"A second operation was started on this context instance before a previous operation completed"*. A `DbContext` is not thread-safe. Run DB queries sequentially, or use separate contexts (`IDbContextFactory<T>`).

### Why never `.Result` / `.Wait()` / `.GetAwaiter().GetResult()` ("sync over async")

```csharp
// ❌ Blocks a thread-pool thread while waiting for I/O
public IActionResult Get()
{
    var product = _service.GetByIdAsync(1, default).Result;   // DON'T
    return Ok(product);
}

// ✅ Async all the way
public async Task<IActionResult> Get(CancellationToken ct)
{
    var product = await _service.GetByIdAsync(1, ct);
    return Ok(product);
}
```

Two different problems:

1. **Deadlock (classic ASP.NET / WinForms / WPF).** Those have a `SynchronizationContext` that allows only one thread at a time. `.Result` blocks that thread; the continuation after `await` inside the async method wants to resume **on that same context** → both wait for each other forever.
2. **Thread-pool starvation (ASP.NET Core).** ASP.NET Core has no `SynchronizationContext`, so the classic deadlock usually doesn't happen — but each `.Result` *holds a thread hostage* doing nothing. Under load, all pool threads are blocked, new requests queue up, the pool injects new threads slowly (~1–2 per second), and latency explodes. The app looks "hung" with low CPU.

`.Result` also wraps exceptions in `AggregateException` (`GetAwaiter().GetResult()` doesn't — but still blocks).

### `async void` — the danger

```csharp
// ❌ async void: caller can't await it, and an exception CRASHES THE PROCESS
public async void SendWelcomeEmail(string to) { await _mailer.SendAsync(to); }

// ✅ async Task: awaitable, exceptions flow to the caller
public async Task SendWelcomeEmailAsync(string to, CancellationToken ct) { await _mailer.SendAsync(to, ct); }
```

| | `async Task` | `async void` |
|---|---|---|
| Can the caller `await` it? | Yes | No (fire-and-forget) |
| Exceptions | Stored in the Task, re-thrown at `await` | Raised on the thread pool → **crashes the process** in ASP.NET Core |
| Request may finish before it completes? | No (if awaited) | Yes → `ObjectDisposedException` on `DbContext`/`HttpContext` |
| Valid use | Everywhere | **Only** UI event handlers (`button_Click`) |

### `Task` vs `ValueTask`

- `Task` is a class → every async call that doesn't complete synchronously allocates one (cached for some common results).
- `ValueTask<T>` is a struct that can wrap **either** a result **or** a `Task`. If the method usually completes **synchronously** (e.g. cache hit), you avoid an allocation.

```csharp
public ValueTask<Product?> GetAsync(int id, CancellationToken ct)
{
    if (_cache.TryGetValue(id, out Product? p))
        return ValueTask.FromResult(p);                // hot path: no allocation
    return new ValueTask<Product?>(LoadAndCacheAsync(id, ct)); // cold path: wrap a Task
}
```

⚠️ **Interview trap — `ValueTask` rules:** await it **once only**; never `await` it twice, never call `.Result` before it completes, never use it with `WhenAll` directly (call `.AsTask()` first). Default to `Task`; use `ValueTask` only on measured hot paths. (`IAsyncDisposable.DisposeAsync` returns `ValueTask`, and so does EF Core's `FindAsync`.)

### `ConfigureAwait(false)`

`await task.ConfigureAwait(false)` says "I don't need to resume on the captured `SynchronizationContext`".

| Code type | Use `ConfigureAwait(false)`? |
|---|---|
| ASP.NET Core app code (controllers, services) | Not needed — there's no `SynchronizationContext`. Harmless but noise. |
| **Reusable libraries / NuGet packages** | **Yes** — your library may be called from WPF/WinForms/classic ASP.NET where it prevents deadlocks and is a bit faster. |
| UI code that touches controls after `await` | **No** — you must resume on the UI thread. |

### `CancellationToken` — pass it all the way down

ASP.NET Core gives you a token that fires when the **client disconnects** (`HttpContext.RequestAborted`). Controllers and minimal APIs bind it automatically if you add a `CancellationToken` parameter:

```csharp
app.MapGet("/products/{id:int}", async (int id, IProductService service, CancellationToken ct) =>
    await service.GetByIdAsync(id, ct) is { } dto ? Results.Ok(dto) : Results.NotFound());
```

Every method in the sample's `ProductService` takes a `CancellationToken ct` and passes it into EF Core (`ToListAsync(ct)`, `SaveChangesAsync(ct)`). If the user closes the browser tab, the SQL command is cancelled instead of burning DB time. In a `BackgroundService` the token is `stoppingToken`, signalled on app shutdown (see `PendingOrderCleanupService` in the sample).

### `Task.WhenAll` vs `Task.WhenAny`

| | `WhenAll` | `WhenAny` |
|---|---|---|
| Completes when | **All** tasks finish | The **first** task finishes |
| Returns | `Task<T[]>` with all results | `Task<Task<T>>` – the winner |
| Typical use | Call 3 independent APIs in parallel | Timeouts, "fastest replica wins" |

> (.NET 6+) For timeouts you can also use `await task.WaitAsync(TimeSpan.FromSeconds(5), ct);`. (.NET 9+) `Task.WhenEach` lets you process tasks as they finish with `await foreach`.

🎯 **Say this in the interview:** "`async`/`await` frees the thread during I/O so the server can handle more concurrent requests — it's about scalability. I never block with `.Result` or `.Wait()` because it can deadlock where there's a SynchronizationContext and causes thread-pool starvation in ASP.NET Core. I avoid `async void` except for event handlers, pass `CancellationToken` everywhere, and use `Task.WhenAll` for independent calls — but not on the same `DbContext`."

---

## 1.9 Exceptions: `throw` vs `throw ex`, filters, wrapping

### Verified example

```csharp
try { WithThrow(); }
catch (Exception ex) { Console.WriteLine($"throw;    keeps origin? {ex.StackTrace!.Contains("ParseOrder")}"); }

try { WithThrowEx(); }
catch (Exception ex) { Console.WriteLine($"throw ex; keeps origin? {ex.StackTrace!.Contains("ParseOrder")}"); }

try { Wrap(); }
catch (OrderException ex) { Console.WriteLine($"wrapped: {ex.Message} <- inner: {ex.InnerException!.GetType().Name}"); }

// Exception filter: catch only when the condition is true
try { throw new HttpRequestException("Service unavailable", null, System.Net.HttpStatusCode.ServiceUnavailable); }
catch (HttpRequestException ex) when (ex.StatusCode == System.Net.HttpStatusCode.ServiceUnavailable)
{
    Console.WriteLine("503 -> will retry");
}
finally
{
    Console.WriteLine("finally always runs");
}

static void ParseOrder() => throw new FormatException("Bad order format");

static void WithThrow()
{
    try { ParseOrder(); }
    catch (FormatException) { throw; }          // ✅ rethrow, stack trace preserved
}

static void WithThrowEx()
{
    try { ParseOrder(); }
#pragma warning disable CA2200
    catch (FormatException ex) { throw ex; }    // ❌ stack trace reset to THIS line
#pragma warning restore CA2200
}

static void Wrap()
{
    try { ParseOrder(); }
    catch (FormatException ex) { throw new OrderException("Could not import order", ex); }  // ✅ keep inner
}

public class OrderException(string message, Exception inner) : Exception(message, inner);
```

Output:

```text
throw;    keeps origin? True
throw ex; keeps origin? False
wrapped: Could not import order <- inner: FormatException
503 -> will retry
finally always runs
```

| Statement | Stack trace | When to use |
|---|---|---|
| `throw;` | **Preserved** (points to the original failing line) | Log-and-rethrow, cleanup-and-rethrow |
| `throw ex;` | **Reset** to the rethrow line — you lose where it really failed | Never (analyzer CA2200 warns) |
| `throw new XException("msg", ex);` | New exception, original kept in `InnerException` | Add context / translate to a domain exception |
| `ExceptionDispatchInfo.Capture(ex).Throw();` | Preserved, even from another place/thread | Advanced: rethrow a stored exception later |

### Best practices in ASP.NET Core

- **Don't** wrap every controller action in `try/catch`. Use one global handler (`app.UseExceptionHandler()` + `IExceptionHandler`, .NET 8+) that returns RFC 7807 `ProblemDetails`. The sample app registers `GlobalExceptionHandler` this way.
- Catch **specific** exceptions; use **exception filters** (`when`) so you don't catch-and-rethrow what you can't handle.
- Don't use exceptions for normal control flow (e.g. "product not found" → return `null`/`404`, not throw).
- `catch (Exception ex) when (ex is not OperationCanceledException)` — the pattern used in the sample's `PendingOrderCleanupService` so shutdown cancellation isn't logged as an error.

🎯 **Say this in the interview:** "`throw;` rethrows the same exception and keeps the original stack trace; `throw ex;` resets the stack trace so you lose the real origin. If I need to add context I wrap it with the original as `InnerException`. In ASP.NET Core I prefer one global exception handler returning ProblemDetails over try/catch in every action."

---

## 1.10 `IDisposable`, `using`, and `await using`

### In simple words

The GC frees **memory**, but not **unmanaged resources** — DB connections, file handles, sockets. `IDisposable.Dispose()` releases them *now*. `using` guarantees `Dispose()` is called, even if an exception is thrown (it compiles to `try/finally`).

**Analogy:** The GC is the cleaner who comes "eventually". `Dispose` is you returning the library book on time so the next person can borrow it.

### Verified example

```csharp
Console.WriteLine("-- using statement --");
using (var file = new FakeFile("a.txt"))
{
    file.Write("hello");
}   // Dispose() called here, even if an exception was thrown

Console.WriteLine("-- using declaration (C# 8) --");
ProcessFile();

Console.WriteLine("-- await using (IAsyncDisposable) --");
await using (var conn = new FakeConnection())
{
    await conn.QueryAsync();
}

static void ProcessFile()
{
    using var f = new FakeFile("b.txt");   // disposed at the end of the method
    f.Write("world");
    Console.WriteLine("end of method");
}

public sealed class FakeFile(string name) : IDisposable
{
    public void Write(string text) => Console.WriteLine($"writing '{text}' to {name}");
    public void Dispose() => Console.WriteLine($"closing {name}");
}

public sealed class FakeConnection : IAsyncDisposable
{
    public async Task QueryAsync() { await Task.Delay(10); Console.WriteLine("query done"); }

    public async ValueTask DisposeAsync()
    {
        await Task.Delay(10);   // e.g. flush buffers / send close packet
        Console.WriteLine("connection closed asynchronously");
    }
}
```

Output:

```text
-- using statement --
writing 'hello' to a.txt
closing a.txt
-- using declaration (C# 8) --
writing 'world' to b.txt
end of method
closing b.txt
-- await using (IAsyncDisposable) --
query done
connection closed asynchronously
```

### In ASP.NET Core

- **Services from DI are disposed by the container** — never call `Dispose()` on an injected `DbContext`. Scoped services are disposed at the end of the request (Chapter 4).
- When *you* create a scope or a resource, *you* dispose it: `await using AsyncServiceScope scope = scopeFactory.CreateAsyncScope();` in the sample's background service.
- `HttpClient`: **don't** `new` + dispose it per call (socket exhaustion). Use `IHttpClientFactory` / typed clients (the sample's `ExchangeRateClient`).

⚠️ **Interview trap:** "`Dispose` frees memory." No — it releases resources deterministically. Memory is still freed later by the GC. A *finalizer* (`~ClassName()`) is only a safety net for unmanaged handles; most app code never needs one.

---

## 1.11 Nullable reference types and primary constructors

### Nullable reference types (C# 8+, on by default in new projects)

With `<Nullable>enable</Nullable>`, `string` means "never null" and `string?` means "may be null". The **compiler** warns you about possible `NullReferenceException`s. It's a compile-time analysis — no runtime change.

```csharp
using System.Text;

// Nullable reference types: the compiler tracks null for you
string? middleName = GetMiddleName(1);
// Console.WriteLine(middleName.Length);   // ⚠️ warning CS8602: possible null dereference
Console.WriteLine(middleName?.Length ?? 0);   // 0
Console.WriteLine(middleName ?? "(none)");    // (none)

string name = GetMiddleName(2) ?? "N/A";
Console.WriteLine(name);                      // Kumar

// Primary constructor (C# 12) on a regular class
var svc = new GreetingService("Hello");
Console.WriteLine(svc.Greet("Asha"));         // Hello, Asha!

// String vs StringBuilder
string s = "";
for (int i = 0; i < 5; i++) s += i;           // creates a NEW string each time (5 allocations)
Console.WriteLine(s);                         // 01234

var sb = new StringBuilder();
for (int i = 0; i < 5; i++) sb.Append(i).Append(',');
sb.Length--;                                  // remove last comma
Console.WriteLine(sb.ToString());             // 0,1,2,3,4

static string? GetMiddleName(int id) => id == 2 ? "Kumar" : null;

public class GreetingService(string prefix)
{
    public string Greet(string person) => $"{prefix}, {person}!";
}
```

Output:

```text
0
(none)
Kumar
Hello, Asha!
01234
0,1,2,3,4
```

| Operator | Meaning | Example |
|---|---|---|
| `?.` | Null-conditional: call only if not null | `user?.Address?.City` |
| `??` | Null-coalescing: fallback value | `name ?? "Guest"` |
| `??=` | Assign if null | `_cache ??= new()` |
| `!` | Null-forgiving: "trust me, not null" (silences warning only) | `p.Category!.Name` |
| `is null` / `is not null` | Null check that can't be fooled by an overloaded `==` | `if (product is null) return NotFound();` |

In ASP.NET Core, NRT also affects **model binding/validation**: a non-nullable `string Name` property on a request DTO is treated as `[Required]` by default.

### Primary constructors (C# 12 / .NET 8+)

Parameters declared on the class name are in scope in the whole class body. It's now the most common way to write DI-friendly classes:

```csharp
// Before
public class ProductService : IProductService
{
    private readonly AppDbContext _db;
    private readonly ILogger<ProductService> _logger;
    public ProductService(AppDbContext db, ILogger<ProductService> logger)
    {
        _db = db;
        _logger = logger;
    }
}

// After (exactly how the sample app's ProductService is written)
public class ProductService(AppDbContext db, ILogger<ProductService> logger) : IProductService
{
    // use db and logger directly
}
```

⚠️ **Interview trap:** Primary-constructor parameters on a **class** are *captured parameters*, not `readonly` fields — you *can* reassign them inside the class. And unlike `record` primary constructors, they do **not** create public properties.

---

## 1.12 Pattern matching and switch expressions

### In simple words

Pattern matching lets you test the *shape* of a value (type, properties, ranges, list contents) and extract parts of it in one expression. `switch` expressions (C# 8+) return a value, are concise, and the compiler warns if you miss a case.

### Verified example

```csharp
Console.WriteLine(Classify(-5));
Console.WriteLine(Classify(0));
Console.WriteLine(Classify(42));
Console.WriteLine(Classify("hi"));
Console.WriteLine(Classify(null));

var order1 = new Order(1500m, "IN", IsPrime: true);
var order2 = new Order(300m, "US", IsPrime: false);
Console.WriteLine($"Shipping order1: {ShippingCost(order1)}");
Console.WriteLine($"Shipping order2: {ShippingCost(order2)}");

Console.WriteLine(Describe([]));
Console.WriteLine(Describe([7]));
Console.WriteLine(Describe([1, 2, 3, 4]));

// Type pattern with 'is'
object payload = new Order(99m, "IN", false);
if (payload is Order { Total: < 100m } small)
    Console.WriteLine($"Small order from {small.Country}");

static string Classify(object? value) => value switch
{
    null              => "null",
    int n when n < 0  => "negative int",
    0                 => "zero",
    int n             => $"positive int {n}",
    string s          => $"string of length {s.Length}",
    _                 => "something else"
};

// Property + relational patterns
static decimal ShippingCost(Order o) => o switch
{
    { IsPrime: true }                  => 0m,
    { Country: "IN", Total: >= 1000m } => 0m,
    { Country: "IN" }                  => 50m,
    { Total: > 500m }                  => 10m,
    _                                  => 25m
};

// List patterns (C# 11+)
static string Describe(int[] items) => items switch
{
    []               => "empty",
    [var only]       => $"one item: {only}",
    [var first, .., var last] => $"starts {first}, ends {last}, count {items.Length}"
};

public record Order(decimal Total, string Country, bool IsPrime);
```

Output:

```text
negative int
zero
positive int 42
string of length 2
null
Shipping order1: 0
Shipping order2: 25
empty
one item: 7
starts 1, ends 4, count 4
Small order from IN
```

| Pattern | Example | Since |
|---|---|---|
| Type | `x is string s` | C# 7 |
| Constant / null | `x is null`, `0 => ...` | C# 7 |
| Property | `{ Country: "IN" }` | C# 8 |
| Relational | `>= 1000m`, `< 0` | C# 9 |
| Logical | `>= 1 and <= 5`, `not null`, `"A" or "B"` | C# 9 |
| List | `[first, .., last]` | C# 11 |
| Discard | `_ => ...` | C# 8 |

**Order matters:** arms are evaluated top to bottom — put specific cases first (`int n when n < 0` before `int n`).

A real ASP.NET Core use — mapping exceptions to HTTP status codes in a global exception handler:

```csharp
int status = exception switch
{
    ArgumentException or ValidationException => StatusCodes.Status400BadRequest,
    KeyNotFoundException                     => StatusCodes.Status404NotFound,
    DbUpdateConcurrencyException             => StatusCodes.Status409Conflict,
    UnauthorizedAccessException              => StatusCodes.Status403Forbidden,
    _                                        => StatusCodes.Status500InternalServerError
};
```

---

## 1.13 `string` vs `StringBuilder`, `==` vs `Equals`, boxing

### `string` is immutable

Every "modification" (`+=`, `Replace`, `ToUpper`) creates a **new** string object. In a loop of 10,000 iterations that's 10,000 allocations and lots of copying (O(n²)). `StringBuilder` keeps a growable buffer and modifies it in place (see the verified demo in 1.11).

| | `string` | `StringBuilder` |
|---|---|---|
| Mutable? | No | Yes |
| Thread-safe? | Yes (because immutable) | No |
| Best for | Few concatenations, interpolation, keys | Building text in loops (CSV, emails, reports) |
| Interning | Literals are interned (shared) | — |

Rule of thumb: a handful of concatenations → `$"..."` interpolation (the compiler optimises it). Loops or unknown count → `StringBuilder` (or `string.Join`/`string.Concat` on a collection).

### `==` vs `Equals` vs `ReferenceEquals`

| Type | `==` | `.Equals()` |
|---|---|---|
| Value types (`int`, `decimal`) | Value comparison | Value comparison |
| `string` | **Content** (operator overloaded) | Content |
| Normal `class` | Reference (same object?) | Reference, unless overridden |
| `record` | Value (compiler-generated) | Value |
| Two boxed `object`s | **Reference** → `False` even for `5` and `5` | Value → `True` |

The verified demo in 1.2 shows all of these. `ReferenceEquals(a, b)` always means "same object", no matter what overloads exist.

⚠️ **Interview trap:** If you override `Equals`, you **must** override `GetHashCode` too — otherwise the object misbehaves as a `Dictionary` key or in a `HashSet`. Records do both for you.

### Boxing and unboxing

**Boxing** = wrapping a value type in an `object` on the heap (`object o = 5;`). **Unboxing** = extracting it (`int i = (int)o;`). Each box is a heap allocation and later GC work.

Where boxing sneaks in:

- Using non-generic collections (`ArrayList`, `Hashtable`) — use `List<T>`, `Dictionary<TKey,TValue>`.
- Passing a struct as `object` or as an interface (`IComparable c = 5;`).
- Logging with `params object[]` (`logger.LogInformation("{Id}", id)` boxes `id`) — which is why the `LoggerMessage` source generator exists (Chapter 5).

---

## Try it yourself

**1.** What does this print, and why?

```csharp
var list = new List<int> { 1, 2, 3 };
var q = list.Select(x => x * 10);
list.Add(4);
Console.WriteLine(q.Count());
```

<details><summary>Answer</summary>

It prints `4`. `Select` is **deferred** — it only runs when `Count()` enumerates it, and by then the list has 4 items. If you'd written `list.Select(x => x * 10).ToList()` before `Add(4)`, it would print `3`.

</details>

**2.** A colleague writes this in a controller. What are the two problems?

```csharp
public IActionResult GetAll()
{
    var items = _db.Products.ToListAsync().Result;
    return Ok(items.Where(p => p.Price > 100));
}
```

<details><summary>Answer</summary>

1. `.Result` blocks a thread-pool thread (sync-over-async) → thread-pool starvation under load (and a deadlock risk in environments with a SynchronizationContext). Make the action `async Task<IActionResult>` and `await` it, passing a `CancellationToken`.
2. It loads the **whole** `Products` table into memory and then filters in C#. Put the `Where` **before** materialising so it becomes SQL: `await _db.Products.Where(p => p.Price > 100).ToListAsync(ct)`. Bonus: use `AsNoTracking()` and project to a DTO.

</details>

**3.** You call `SingleOrDefault(u => u.Email == email)` and get `InvalidOperationException`. What happened, and what does it tell you about your data?

<details><summary>Answer</summary>

There are **two or more** users with that email. `SingleOrDefault` returns null for zero matches but throws for more than one. The fix is in the data: add a **unique index** on `Email` in the database. Don't "fix" it by switching to `FirstOrDefault` — that hides a data-integrity bug.

</details>

**4.** Why is `async void` dangerous in ASP.NET Core? Give the one place it's acceptable.

<details><summary>Answer</summary>

The caller can't await it, so the request may finish (and dispose scoped services like `DbContext`) while it's still running, and any exception it throws is raised on the thread pool and **crashes the process**. It's acceptable only for event handlers (e.g. UI `Click` events), whose signature requires `void`.

</details>

**5.** Make this run faster without changing the methods. Then explain when you must NOT do it.

```csharp
var profile = await _profileApi.GetAsync(id, ct);
var orders  = await _ordersApi.GetAsync(id, ct);
var points  = await _loyaltyApi.GetAsync(id, ct);
```

<details><summary>Answer</summary>

Start all three, then await together:

```csharp
var profileTask = _profileApi.GetAsync(id, ct);
var ordersTask  = _ordersApi.GetAsync(id, ct);
var pointsTask  = _loyaltyApi.GetAsync(id, ct);
await Task.WhenAll(profileTask, ordersTask, pointsTask);
var profile = profileTask.Result;   // safe: already completed
```

Total time ≈ the slowest call instead of the sum. Don't do this when the calls share a non-thread-safe resource — especially the **same `DbContext`** — or when one call depends on another's result.

</details>

**6.** `record Point(int X, int Y);` — what do `new Point(1,2) == new Point(1,2)` and `ReferenceEquals(new Point(1,2), new Point(1,2))` return?

<details><summary>Answer</summary>

`==` returns **True** (records have compiler-generated value equality). `ReferenceEquals` returns **False** — they are two different objects on the heap (a `record` is a class).

</details>

---

## Quick revision

- Value types hold data and are copied; reference types hold a reference — copies share one object. Everything is passed by value unless `ref`/`out`/`in`.
- `class` = identity, `struct` = tiny value, `record` = data with value equality, `with`, and nice `ToString` (great for DTOs, not EF entities).
- Interface = "can do" contract (many allowed, no state); abstract class = "is a" shared base (one only, has state).
- `Func` returns a value, `Action` returns void; lambdas create delegates; every `AddXxx`/`UseXxx` is an extension method.
- LINQ is **deferred**: nothing runs until you enumerate; enumerating twice runs twice — materialise once with `ToList()`.
- `IEnumerable` = in-memory delegates; `IQueryable` = expression tree translated to SQL. Filter **before** materialising.
- `First` throws on 0; `FirstOrDefault` returns null; `Single`/`SingleOrDefault` also throw on 2+. Use `Any()` not `Count() > 0`.
- `async`/`await` frees threads during I/O → scalability. Never `.Result`/`.Wait()` (deadlock / thread-pool starvation). No `async void` (crashes process).
- Pass `CancellationToken` everywhere; use `Task.WhenAll` for independent calls — never on the same `DbContext`.
- `ValueTask` only for hot paths that usually complete synchronously; await it once. `ConfigureAwait(false)` matters in libraries, not ASP.NET Core app code.
- `throw;` keeps the stack trace, `throw ex;` resets it; wrap with `InnerException` to add context.
- `using`/`await using` guarantee `Dispose`; DI disposes the services it creates — don't dispose injected services yourself.

---

# Chapter 2. ASP.NET Core Fundamentals

> **Why interviewers ask this:** "What is ASP.NET Core?" and "What happens in `Program.cs`?" are the opening questions of almost every .NET web interview. They quickly reveal whether you've only *used* the template or actually understand the host, the server (Kestrel), environments and the request lifecycle. A crisp, confident answer here sets the tone for the rest of the interview.

---

## 2.1 What is ASP.NET Core?

### In simple words

ASP.NET Core is Microsoft's **free, open-source, cross-platform framework for building web apps and APIs** on .NET. With it you build:

- REST/HTTP APIs (controllers or **minimal APIs**)
- Server-rendered web apps (MVC, Razor Pages)
- Interactive UI (Blazor)
- Real-time apps (SignalR), gRPC services, background workers

**Analogy:** Think of .NET as the *engine* and ASP.NET Core as the *car body built for roads (HTTP)*. Kestrel is the *wheels that touch the road*, middleware is the *assembly line every passenger (request) passes through*, and DI is the *parts warehouse*.

### Key characteristics (what to say in one breath)

| Characteristic | What it means |
|---|---|
| Cross-platform | Runs on Windows, Linux, macOS; deploy to Docker/Kubernetes, Azure App Service, IIS, Nginx |
| High performance | Kestrel + `Span<T>`/pooling; consistently among the fastest mainstream frameworks in TechEmpower benchmarks |
| Modular | You opt in to features (`AddControllers`, `AddAuthentication`...) — pay only for what you use |
| Built-in DI | Dependency injection is part of the framework, not an add-on |
| Unified | MVC and Web API are one framework (`Controller` vs `ControllerBase`) |
| Configuration & logging | Pluggable providers (JSON, env vars, Key Vault...; console, Seq, App Insights...) |
| Open source | Developed on GitHub (`dotnet/aspnetcore`) |
| Side-by-side versions | Each app carries/targets its own .NET version |

🎯 **Say this in the interview:** "ASP.NET Core is a cross-platform, open-source, high-performance framework for building web APIs and web apps on .NET. It's modular, has dependency injection, configuration and logging built in, runs on its own web server Kestrel, and processes every request through a configurable middleware pipeline."

---

## 2.2 .NET vs .NET Framework (and where "Core" went)

### A short history (useful for the "why" question)

| Year | Version | Notes |
|---|---|---|
| 2002–2019 | .NET Framework 1.0 → **4.8.x** | Windows-only, tied to IIS and `System.Web`. Still supported (it ships with Windows) but receives **no new features**. |
| 2016 | .NET Core 1.0 / ASP.NET Core 1.0 | Rewritten from scratch: cross-platform, modular, fast. |
| 2019 | .NET Core 3.1 (LTS) | Mature; Windows desktop support added. |
| 2020 | **.NET 5** | "Core" dropped from the runtime name; one unified .NET. (Skipped "4" to avoid confusion with Framework 4.x.) |
| 2021 | .NET 6 (LTS) | Minimal hosting model (`WebApplication.CreateBuilder`), minimal APIs. |
| 2023 | .NET 8 (LTS) | Keyed DI services, `IExceptionHandler`, Native AOT for APIs, Identity API endpoints. |
| 2024 | .NET 9 (STS) | Built-in OpenAPI document generation (`AddOpenApi`), `HybridCache`, `MapStaticAssets`. |
| 2025 | .NET 10 (LTS) | Current LTS. The sample app targets `net10.0`. |

The *framework* kept the name **ASP.NET Core** even after the runtime became just ".NET".

**Release cadence:** a new major version every November. **Even** numbers are **LTS** (3 years of support), **odd** numbers are **STS** (now 2 years).

### Comparison

| | .NET Framework 4.8 | .NET (Core) 5+ |
|---|---|---|
| OS | Windows only | Windows, Linux, macOS |
| Web framework | ASP.NET (MVC 5, Web API 2, Web Forms) | ASP.NET Core |
| Web server | IIS only (`System.Web`) | Kestrel (+ IIS, Nginx, Apache, YARP as reverse proxy) |
| DI | Third-party (Autofac, Unity) | Built in |
| Config | `web.config` / `app.config` (XML) | `appsettings.json`, env vars, secrets... |
| Deployment | Machine-wide install | Framework-dependent or **self-contained**, Docker-friendly |
| Performance | Good | Much better |
| Open source | Reference source only | Fully open source |
| New features | None (maintenance only) | Every year |
| Not supported | — | Web Forms, WCF *server* (use CoreWCF/gRPC), .NET Remoting, AppDomains |

⚠️ **Interview trap:** ".NET Core and .NET Framework are the same thing." No — they're different runtimes. You can't run an ASP.NET (System.Web) app on .NET 8 without migrating it. Libraries targeting **.NET Standard 2.0** can be shared between both.

---

## 2.3 Kestrel, IIS and reverse proxies

### Kestrel

**Kestrel** is ASP.NET Core's built-in, cross-platform, high-performance web server. It listens on a port, parses raw HTTP (1.1, 2, 3), creates an `HttpContext`, and hands it to your middleware pipeline. **Every** ASP.NET Core app runs on a server implementation — almost always Kestrel.

**Analogy:** Kestrel is the *receptionist* who speaks HTTP. A reverse proxy (IIS/Nginx) is the *security guard at the building entrance*.

### Can Kestrel face the internet directly?

Yes — since ASP.NET Core 2.x Kestrel is hardened and supported as an edge server. But in production you often put a **reverse proxy** in front:

```text
              Internet
                 │  HTTPS :443
        ┌────────▼─────────┐
        │  Reverse proxy   │  IIS / Nginx / Apache / YARP / Azure Front Door / K8s Ingress
        │  - TLS offload   │
        │  - load balance  │
        │  - static cache  │
        │  - many apps on  │
        │    one port 443  │
        └────────┬─────────┘
                 │  HTTP :5000 (private network)
        ┌────────▼─────────┐
        │     Kestrel      │  your ASP.NET Core app
        │  Middleware ...  │
        └──────────────────┘
```

| Why a reverse proxy? | Explanation |
|---|---|
| Port sharing | Several apps behind one public IP:443, routed by host name/path |
| TLS termination | Certificates managed in one place |
| Load balancing | Spread traffic across instances |
| Extra security | Request filtering, WAF, rate limiting, hiding internal topology |
| Existing infrastructure | Company standard is IIS on Windows / Nginx on Linux |

⚠️ When behind a proxy, the app sees the *proxy's* IP and `http` scheme. Use **Forwarded Headers middleware** (`app.UseForwardedHeaders()` reading `X-Forwarded-For` / `X-Forwarded-Proto`) so `HttpContext.Connection.RemoteIpAddress`, HTTPS redirection and generated links are correct.

### IIS hosting: in-process vs out-of-process

On Windows/IIS, the **ASP.NET Core Module (ANCM)** connects IIS to your app.

```text
In-process (default since 3.0)                   Out-of-process
┌───────────── w3wp.exe ─────────────┐           ┌── w3wp.exe ──┐  HTTP   ┌── dotnet MyApp.dll ──┐
│ IIS ─► ANCM ─► IIS HTTP Server     │           │ IIS ─► ANCM  │ ──────► │ Kestrel ─► your app  │
│        (IISHttpServer) ─► your app │           └──────────────┘ loopback└──────────────────────┘
└────────────────────────────────────┘
```

| | In-process | Out-of-process |
|---|---|---|
| Process | App runs **inside** the IIS worker process (`w3wp.exe`) | App runs in its **own** `dotnet.exe` process |
| Server | `IISHttpServer` (not Kestrel) | **Kestrel**; IIS acts as reverse proxy |
| Performance | Faster (no extra network hop) | Slightly slower (loopback HTTP hop) |
| Config | `<AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>` (default) | `OutOfProcess` |
| One app per app pool | Yes | No restriction |

🎯 **Say this in the interview:** "Kestrel is the built-in cross-platform web server; every ASP.NET Core app runs on it except IIS in-process hosting, where the app runs inside w3wp using IIS HTTP Server. In production we typically put a reverse proxy — IIS, Nginx, or an ingress — in front for TLS, load balancing and port sharing, and enable forwarded headers."

---

## 2.4 `Program.cs` and the minimal hosting model (.NET 6+)

Since .NET 6 there's no `Startup.cs` by default — everything lives in `Program.cs` with **top-level statements**. The file always has the same 4-step shape.

### Easy example (verified, compiles on .NET 10)

```csharp
var builder = WebApplication.CreateBuilder(args);   // config + logging + DI + Kestrel + environment

builder.Services.AddControllers();                  // ① register services
builder.Services.AddScoped<IGreeter, Greeter>();

var app = builder.Build();                          // ② lock the container, build the app

app.UseHttpsRedirection();                          // ③ configure the middleware pipeline
app.MapControllers();
app.MapGet("/hello/{name}", (string name, IGreeter greeter) => greeter.Greet(name));

app.Run();                                          // ④ start Kestrel and block until shutdown

public interface IGreeter { string Greet(string name); }
public class Greeter : IGreeter { public string Greet(string name) => $"Hello, {name}!"; }
```

### What `WebApplication.CreateBuilder(args)` sets up for you

| Area | What you get by default |
|---|---|
| **Content root** | Current directory (where `appsettings.json` lives) |
| **Environment** | Read from `ASPNETCORE_ENVIRONMENT` (or `DOTNET_ENVIRONMENT`); defaults to `Production` |
| **Configuration** | `appsettings.json` → `appsettings.{Environment}.json` → **user secrets** (Development only) → environment variables → command-line args (later wins; Chapter 5) |
| **Logging** | Console, Debug, EventSource (+ EventLog on Windows) providers; levels from the `Logging` config section |
| **DI container** | `builder.Services` pre-filled with framework services (logging, options, `IConfiguration`, `IWebHostEnvironment`, routing...) |
| **Scope validation** | In Development: `ValidateScopes` + `ValidateOnBuild` = true (catches captive dependencies — Chapter 4) |
| **Web server** | **Kestrel**, configured from the `Kestrel` config section; IIS integration if hosted in IIS |
| **Host defaults** | Generic Host, graceful shutdown (Ctrl+C / SIGTERM), static web assets in Development |
| **Implicit middleware** | `WebApplication` automatically adds the Developer Exception Page (in Development), `UseRouting` and `UseEndpoints` if you don't add them yourself |

### `builder.Services` vs `app` — the two phases

| | `builder` (`WebApplicationBuilder`) | `app` (`WebApplication`) |
|---|---|---|
| Phase | **Before** `Build()` — configuration time | **After** `Build()` — runtime |
| You do | Register services: `builder.Services.AddXxx()`; read/add config; configure logging & Kestrel | Add middleware: `app.UseXxx()`; map endpoints: `app.MapXxx()`; run the app |
| Key properties | `Services`, `Configuration`, `Environment`, `Logging`, `WebHost`, `Host` | `Services` (the built provider, read-only), `Configuration`, `Environment`, `Logger`, `Lifetime`, `Urls` |
| Can you add services? | Yes | **No** — the container is locked; `app.Services` is for *resolving* |

⚠️ **Interview trap:** Trying `builder.Services.AddScoped<...>()` after `builder.Build()` throws *"The service collection cannot be modified because it is read-only"*. Registration happens only on the builder.

⚠️ **Interview trap:** "`app.Run()` and `app.Run(context => ...)` are the same." No! `app.Run()` with no arguments **starts the server**. `app.Run(RequestDelegate)` adds a **terminal middleware** (Chapter 3).

### The real-world version: the sample app

The sample app (samples/ShopApi) `Program.cs` follows the same three sections with comments. Abridged:

```csharp
var builder = WebApplication.CreateBuilder(args);

// ---------------- 1. Register services (DI container) ----------------
builder.Services.AddOptions<JwtOptions>()
    .Bind(builder.Configuration.GetSection(JwtOptions.SectionName))
    .ValidateDataAnnotations()
    .ValidateOnStart();
builder.Services.AddDbContext<AppDbContext>(o =>
    o.UseSqlite(builder.Configuration.GetConnectionString("Shop")));
builder.Services.AddScoped<IProductService, ProductService>();
builder.Services.AddControllers(o => o.Filters.Add<LogActionFilter>());
builder.Services.AddProblemDetails();
builder.Services.AddExceptionHandler<GlobalExceptionHandler>();
// ... authentication, authorization, CORS, caching, rate limiting, HttpClient, health checks
builder.Services.AddHostedService<PendingOrderCleanupService>();

var app = builder.Build();

// ---------------- 2. Middleware pipeline (ORDER MATTERS) ----------------
app.UseExceptionHandler();
app.UseStatusCodePages();
if (app.Environment.IsDevelopment()) { app.MapOpenApi(); } else { app.UseHsts(); }
app.UseHttpsRedirection();
// ... correlation id, request timing, CORS, auth, rate limiter, output cache

// ---------------- 3. Endpoints ----------------
app.MapControllers().RequireRateLimiting("fixed");
app.MapCategoryEndpoints();
app.MapHealthChecks("/health");
app.MapGet("/", () => "Shop API is running");

app.Run();

// Makes Program visible to WebApplicationFactory<Program> in integration tests
public partial class Program;
```

That last line matters: with top-level statements the compiler generates an `internal` `Program` class. Declaring `public partial class Program;` lets integration tests use `WebApplicationFactory<Program>`.

### Old style: `Startup.cs` (.NET Core 3.1 / .NET 5)

You'll still meet this in existing codebases:

| Old (`Startup`) | New (minimal hosting) |
|---|---|
| `Host.CreateDefaultBuilder(args).ConfigureWebHostDefaults(w => w.UseStartup<Startup>())` | `WebApplication.CreateBuilder(args)` |
| `Startup.ConfigureServices(IServiceCollection services)` | `builder.Services.AddXxx()` |
| `Startup.Configure(IApplicationBuilder app, IWebHostEnvironment env)` | `app.UseXxx()` / `app.MapXxx()` |
| `app.UseEndpoints(e => e.MapControllers())` | `app.MapControllers()` (routing/endpoints added implicitly) |

🎯 **Say this in the interview:** "`CreateBuilder` sets up the Generic Host with configuration, logging, DI, the environment and Kestrel. Before `Build()` I register services on `builder.Services`; after `Build()` the container is locked and I compose the middleware pipeline and map endpoints on `app`; `app.Run()` starts Kestrel."

---

## 2.5 The request lifecycle

What happens between a browser hitting `https://shop.example.com/api/products/5` and getting JSON back?

```text
 Client (browser / Postman / mobile app)
   │ 1. HTTPS request
   ▼
 Reverse proxy (optional: IIS / Nginx / ingress)
   │ 2. forwards to Kestrel
   ▼
 Kestrel (web server)
   │ 3. parses HTTP, builds HttpContext (Request, Response, User, Items, RequestServices)
   │ 4. creates a DI SCOPE for this request (HttpContext.RequestServices)
   ▼
 ┌──────────────────────── Middleware pipeline ────────────────────────┐
 │ ExceptionHandler → HSTS → HttpsRedirection → StaticFiles → Routing  │
 │   → CORS → Authentication → Authorization → RateLimiter → custom    │
 └──────────────────────────────┬──────────────────────────────────────┘
                                │ 5. Routing has selected an endpoint
                                ▼
 ┌──────────────────────── Endpoint execution ─────────────────────────┐
 │ Minimal API handler  ─ or ─  MVC: filters → model binding →         │
 │                              validation → controller action          │
 │   → your service (DI) → EF Core → database                           │
 └──────────────────────────────┬──────────────────────────────────────┘
                                │ 6. result serialized (System.Text.Json)
                                ▼
   Response travels back OUT through the middleware in reverse order
   (timing, logging, exception handling can act on the way out)
                                │
                                ▼
 Kestrel writes bytes to the socket → client
   7. Request scope disposed (DbContext and other scoped/transient IDisposables)
```

Key points to mention:

1. **`HttpContext`** is the per-request object carrying everything: `Request`, `Response`, `User` (claims), `Items` (per-request bag), `RequestServices` (the request's DI scope), `RequestAborted` (cancellation token), `TraceIdentifier`.
2. **One DI scope per request** — every `Scoped` service (like `DbContext`) is created once per request and disposed at the end.
3. Middleware runs **in order on the way in** and **in reverse order on the way out** (Chapter 3).
4. **Routing** selects the endpoint early (`UseRouting`), so later middleware (CORS, auth, rate limiting) can read the endpoint's metadata (`[Authorize]`, `RequireCors`, `RequireRateLimiting`), and the endpoint actually *executes* at the end of the pipeline.

🎯 **Say this in the interview:** "Kestrel receives the request and creates an `HttpContext` and a DI scope. The request flows through middleware in registration order — exception handling, HTTPS, static files, routing, CORS, authentication, authorization — until the endpoint runs: for MVC that's filters, model binding, validation and the action. The response flows back through the middleware in reverse, and finally the scope is disposed."

---

## 2.6 Hosting environments

### In simple words

The same build runs in different places — your laptop, a test server, production. The **environment name** tells the app where it is so it can behave differently: detailed errors on your laptop, HSTS and generic errors in production.

| Environment | Typical behaviour |
|---|---|
| `Development` | Developer exception page, user secrets loaded, OpenAPI/Swagger UI, verbose logging, scope validation |
| `Staging` | Production-like config against test resources; final verification |
| `Production` (**default** if nothing is set) | Generic error responses, HSTS, minimal logging, secrets from Key Vault/env |
| Any custom name (`QA`, `UAT`) | Allowed — check with `env.IsEnvironment("QA")` |

### How it's set

- Environment variable **`ASPNETCORE_ENVIRONMENT`** (or `DOTNET_ENVIRONMENT`, lower precedence).
- Command line: `dotnet run --environment Staging`.
- In code (tests): `builder.UseEnvironment("Testing")` in `WebApplicationFactory`.
- Locally it usually comes from `Properties/launchSettings.json`.

```bash
# Linux/macOS
export ASPNETCORE_ENVIRONMENT=Staging
# PowerShell
$env:ASPNETCORE_ENVIRONMENT = "Staging"
# Docker
docker run -e ASPNETCORE_ENVIRONMENT=Staging myshop:latest
```

### Using it in code

```csharp
if (app.Environment.IsDevelopment())
{
    app.MapOpenApi();               // /openapi/v1.json only in dev
}
else
{
    app.UseHsts();
}

// Anywhere else: inject IWebHostEnvironment (web) or IHostEnvironment (any host)
public class ReportService(IWebHostEnvironment env)
{
    public string TemplatesFolder => Path.Combine(env.ContentRootPath, "Templates");
}
```

The environment also decides which JSON file layers on top of `appsettings.json`: `appsettings.Development.json`, `appsettings.Production.json`, etc.

⚠️ **Interview trap:** Forgetting to set the environment on a server means it defaults to **Production**, not Development. That's a safe default — but it surprises people when their `appsettings.Development.json` "isn't being read" on the test server.

⚠️ **Interview trap:** `launchSettings.json` is **only** used by local tooling (`dotnet run`, Visual Studio, VS Code). It is **not** deployed or read in production.

---

## 2.7 `launchSettings.json`

Lives in `Properties/launchSettings.json`. Defines **launch profiles** for local development: which URLs to use, which environment variables to set, whether to open a browser.

```json
{
  "$schema": "https://json.schemastore.org/launchsettings.json",
  "profiles": {
    "http": {
      "commandName": "Project",
      "dotnetRunMessages": true,
      "launchBrowser": false,
      "applicationUrl": "http://localhost:5080",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    },
    "https": {
      "commandName": "Project",
      "applicationUrl": "https://localhost:7080;http://localhost:5080",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```

| Field | Meaning |
|---|---|
| `commandName` | `Project` = run with Kestrel; `IISExpress` = run under IIS Express; `Docker` for containers |
| `applicationUrl` | URLs Kestrel listens on (sets `ASPNETCORE_URLS`) |
| `environmentVariables` | Env vars for this profile — typically `ASPNETCORE_ENVIRONMENT` |
| `launchBrowser` / `launchUrl` | Open a browser (e.g. at `/swagger`) |

`dotnet run --launch-profile https` picks a profile; `dotnet run` uses the first one.

**Other ways to set URLs** (in production): `ASPNETCORE_URLS=http://+:8080` env var, `--urls` command-line argument, `"Kestrel": { "Endpoints": ... }` in config, or `app.Urls.Add(...)` in code. (.NET 8+ container images listen on port **8080** by default, and `ASPNETCORE_HTTP_PORTS` / `ASPNETCORE_HTTPS_PORTS` are the simple way to change ports.)

---

## 2.8 `wwwroot` and static files

- `wwwroot` is the **web root**: files here (CSS, JS, images, a SPA's built files) can be served directly.
- Files are **not** served unless you add the middleware: `app.UseStaticFiles();` (or, .NET 9+, `app.MapStaticAssets();` which adds build-time compression and fingerprinted, cache-friendly URLs).
- Only files **inside** `wwwroot` are exposed — `appsettings.json` in the content root is never served.

```text
MyShop/
├── Program.cs
├── appsettings.json          ← content root (NOT publicly served)
└── wwwroot/                  ← web root (publicly served)
    ├── css/site.css          → GET /css/site.css
    ├── js/app.js             → GET /js/app.js
    └── images/logo.png       → GET /images/logo.png
```

```csharp
app.UseDefaultFiles();   // "/" → "/index.html" (must come BEFORE UseStaticFiles — it only rewrites the path)
app.UseStaticFiles();    // serves the file and SHORT-CIRCUITS (no auth, no MVC)
```

Static files middleware is placed **early** so `logo.png` doesn't go through authentication, routing and MVC — a big performance win. If static files must be protected, put them outside `wwwroot` and serve them from an authorized endpoint (or place `UseStaticFiles` after `UseAuthorization` with a fallback policy).

| | Content root | Web root |
|---|---|---|
| Default | App's base folder | `{content root}/wwwroot` |
| Access | `env.ContentRootPath` | `env.WebRootPath` |
| Publicly served? | No | Yes (with static files middleware) |

---

## 2.9 The Generic Host

### In simple words

The **host** is the object that owns your app's *shared infrastructure* and *lifetime*: DI container, configuration, logging, and a list of **hosted services** to start and stop. A web app is just a host whose main hosted service is the web server.

**Analogy:** The host is the *building manager*: turns on electricity (config), water (logging), hires staff (DI), opens the doors at 9 (start) and makes sure everyone leaves safely at night (graceful shutdown).

| Builder | Use for | Since |
|---|---|---|
| `WebApplication.CreateBuilder` | Web apps/APIs | .NET 6 |
| `Host.CreateApplicationBuilder` | Worker services, console apps with DI | .NET 7 |
| `Host.CreateDefaultBuilder` | Older callback-style API (still works) | .NET Core 3.0 |
| `WebHost.CreateDefaultBuilder` | **Legacy** Web Host (ASP.NET Core 2.x) | — |

The same host powers non-web apps. A verified Worker Service example — DI, config and logging, but no Kestrel:

```csharp
// A non-web app (Worker Service) on the same Generic Host: DI, config, logging - no Kestrel
var builder = Host.CreateApplicationBuilder(args);
builder.Services.AddHostedService<Heartbeat>();
IHost host = builder.Build();
host.Run();

public class Heartbeat(ILogger<Heartbeat> logger) : BackgroundService
{
    protected override async Task ExecuteAsync(CancellationToken stoppingToken)
    {
        while (!stoppingToken.IsCancellationRequested)
        {
            logger.LogInformation("Still alive at {Time}", DateTimeOffset.Now);
            await Task.Delay(TimeSpan.FromSeconds(5), stoppingToken);
        }
    }
}
```

`IHostedService` / `BackgroundService` instances are started when the host starts and stopped (their `stoppingToken` signalled) on shutdown. They are **singletons**. The sample's `PendingOrderCleanupService` is a real example: it uses a `PeriodicTimer`, creates a DI scope per run to get a `DbContext`, and catches exceptions so the loop never dies.

---

## 2.10 `IHostApplicationLifetime` — startup and graceful shutdown

### In simple words

`IHostApplicationLifetime` gives you three **events** (as `CancellationToken`s) and one action:

| Member | When |
|---|---|
| `ApplicationStarted` | Host fully started (server is listening) |
| `ApplicationStopping` | Shutdown requested (Ctrl+C, SIGTERM from Docker/K8s, IIS recycle) — **in-flight requests are still running** |
| `ApplicationStopped` | Everything has stopped — last chance for final cleanup/flush |
| `StopApplication()` | Request a shutdown from code |

### Verified example

```csharp
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

IHostApplicationLifetime lifetime = app.Lifetime;   // same as resolving IHostApplicationLifetime

lifetime.ApplicationStarted.Register(() =>
    app.Logger.LogInformation("Started in {Env} environment", app.Environment.EnvironmentName));
lifetime.ApplicationStopping.Register(() =>
    app.Logger.LogInformation("Stopping: finish in-flight work, stop taking new jobs"));
lifetime.ApplicationStopped.Register(() =>
    Console.WriteLine("Stopped: all hosted services are down"));

if (app.Environment.IsDevelopment())
{
    app.UseDeveloperExceptionPage();          // added automatically in .NET 6+, shown for clarity
}

app.MapGet("/", (IWebHostEnvironment env) => $"Hello from {env.ApplicationName} ({env.EnvironmentName})");

app.Run();
```

Running it, calling `/` once and pressing Ctrl+C prints (Kestrel's own startup lines trimmed; `MyApp` is your project name):

```text
info: MyApp[0]
      Started in Production environment
...                                          ← GET / returns "Hello from MyApp (Production)"
info: MyApp[0]
      Stopping: finish in-flight work, stop taking new jobs
Stopped: all hosted services are down
```

### Graceful shutdown — how it works

1. Kubernetes/Docker sends **SIGTERM** (or you press Ctrl+C).
2. `ApplicationStopping` fires. Kestrel stops accepting new connections.
3. In-flight requests get time to finish; hosted services' `StopAsync` is called and their `stoppingToken` is cancelled.
4. After `HostOptions.ShutdownTimeout` (**30 seconds** by default since .NET 6) the host stops waiting.
5. `ApplicationStopped` fires; the process exits.

```csharp
builder.Services.Configure<HostOptions>(o => o.ShutdownTimeout = TimeSpan.FromSeconds(60));
```

⚠️ **Interview trap:** Doing long synchronous work inside `ApplicationStopping` callbacks blocks shutdown. Keep them short, or do cleanup in a hosted service's `StopAsync`, which honours the shutdown timeout.

---

## 2.11 Minimal APIs vs Controllers (quick map)

Interviewers often ask "which would you choose?" — details come in later chapters, but here's the summary.

| | Minimal APIs (.NET 6+) | Controllers (MVC) |
|---|---|---|
| Style | Lambdas: `app.MapGet("/x", handler)` | Classes deriving from `ControllerBase` with attributes |
| Ceremony | Very low | More structure |
| Filters | Endpoint filters (`AddEndpointFilter`) | Rich MVC filter pipeline (action, resource, result, exception) |
| Model validation | Built-in from .NET 10 (`AddValidation()`); earlier: manual or libraries | Automatic with `[ApiController]` |
| Performance | Slightly faster, Native AOT friendly | Very good |
| Organisation | Group with `MapGroup` + extension methods | Controllers group naturally |
| Best for | Microservices, small/focused APIs | Large APIs, teams used to MVC, complex filters |

The sample app uses **both**: controllers for products/orders, and minimal API endpoints for categories (`app.MapCategoryEndpoints()`).

---

## Try it yourself

**1.** You deploy to a new Linux VM and your app ignores `appsettings.Development.json` — the connection string from `appsettings.json` is used. You never set any environment variable. Why?

<details><summary>Answer</summary>

`ASPNETCORE_ENVIRONMENT` isn't set, so the environment defaults to **Production**. Only `appsettings.json` and `appsettings.Production.json` are loaded. `launchSettings.json` (which set Development on your laptop) isn't used outside local tooling. Set the variable on the server (or better, provide a proper Production config).

</details>

**2.** What's wrong with this code?

```csharp
var app = builder.Build();
builder.Services.AddScoped<IEmailSender, SmtpEmailSender>();
app.MapPost("/signup", (IEmailSender sender) => sender.SendAsync("welcome"));
app.Run();
```

<details><summary>Answer</summary>

Services are registered **after** `Build()`. The service collection is read-only at that point, so `AddScoped` throws `InvalidOperationException` ("The service collection cannot be modified because it is read-only"). Move the registration above `builder.Build()`.

</details>

**3.** Explain in-process vs out-of-process IIS hosting in two sentences each. Which is the default and which is faster?

<details><summary>Answer</summary>

**In-process** (default since ASP.NET Core 3.0): the app runs inside the IIS worker process `w3wp.exe` using `IISHttpServer` instead of Kestrel; it's faster because there's no extra hop. **Out-of-process**: IIS forwards requests over loopback HTTP to a separate `dotnet` process running Kestrel; IIS acts as a reverse proxy, which adds a little latency.

</details>

**4.** Draw (in words) the request lifecycle for `GET /api/products/5` that requires a logged-in user.

<details><summary>Answer</summary>

Client → (reverse proxy) → Kestrel builds `HttpContext` and a DI scope → ExceptionHandler → HTTPS redirection → static files (no match, passes on) → routing selects `ProductsController.GetById` → CORS → **Authentication** reads the JWT and sets `HttpContext.User` → **Authorization** checks the endpoint's `[Authorize]` metadata against the user → MVC filters → model binding (`id = 5`) → action calls `IProductService` → EF Core → SQL → `ProductDto` serialized to JSON → response flows back through the middleware → scope disposed (DbContext disposed).

</details>

**5.** Your Kubernetes pods sometimes lose in-flight orders when deployments roll. Which host feature do you look at and what would you change?

<details><summary>Answer</summary>

Graceful shutdown. On SIGTERM the host fires `ApplicationStopping`, stops accepting connections, waits for in-flight requests and hosted services up to `HostOptions.ShutdownTimeout` (30 s default). Make sure background services honour `stoppingToken` and finish/persist work, increase `ShutdownTimeout` if needed, and make sure the pod's `terminationGracePeriodSeconds` is longer than the host's timeout.

</details>

**6.** Where should `logo.png` live so it can be served, and why is `UseStaticFiles` placed early in the pipeline?

<details><summary>Answer</summary>

In `wwwroot` (e.g. `wwwroot/images/logo.png` → `/images/logo.png`). `UseStaticFiles` is placed early because when it finds a file it **short-circuits** — the request doesn't go through routing, authentication, MVC etc., which saves CPU. Files outside `wwwroot` (like `appsettings.json`) are never served.

</details>

---

## Quick revision

- ASP.NET Core = cross-platform, open-source, high-performance web framework on .NET with built-in DI, config, logging and a middleware pipeline.
- .NET Framework 4.8 is Windows-only and maintenance-only; .NET 5+ is the unified, cross-platform successor. Even versions are LTS.
- Kestrel is the built-in web server; a reverse proxy (IIS/Nginx/ingress) is optional but common — use Forwarded Headers behind one.
- IIS in-process (default, faster, inside `w3wp`, `IISHttpServer`) vs out-of-process (Kestrel in a separate process, IIS proxies).
- `CreateBuilder` sets up config, logging, DI, environment and Kestrel; register services **before** `Build()`, add middleware/endpoints **after**.
- `app.Run()` starts the server; `app.Run(ctx => ...)` adds terminal middleware.
- Request: Kestrel → `HttpContext` + DI scope → middleware in order → endpoint → back out in reverse → scope disposed.
- Environment comes from `ASPNETCORE_ENVIRONMENT`; default is **Production**; it selects `appsettings.{Env}.json`.
- `launchSettings.json` is for local development only — never used in production.
- `wwwroot` is the public web root; `UseStaticFiles` short-circuits early; `MapStaticAssets` (.NET 9+) adds compression and fingerprinting.
- The Generic Host runs web apps *and* workers; `BackgroundService`s are singletons started/stopped by the host.
- `IHostApplicationLifetime`: `ApplicationStarted`, `ApplicationStopping`, `ApplicationStopped`, `StopApplication()`; shutdown timeout is 30 s by default.

---

# Chapter 3. Middleware and the Request Pipeline

> **Why interviewers ask this:** Middleware is the heart of ASP.NET Core — every request flows through it. Interviewers use it to test whether you understand *how* the framework works rather than just which attributes to add. Expect: "What is middleware?", "Use vs Run vs Map?", "Write a custom middleware", "Why must `UseAuthentication` come before `UseAuthorization`?", and "Middleware vs filters?".

---

## 3.1 What is middleware?

### In simple words

**Middleware** is a piece of code that sits in the request pipeline. Each middleware:

1. receives the `HttpContext`,
2. can do work **before** the next middleware,
3. decides whether to call the **next** middleware (or stop — *short-circuit*),
4. can do work **after** the next middleware returns (on the way back out).

All middleware together form the **request pipeline**.

**Analogy — airport security:** You (the request) pass through check-in → security → passport control → boarding gate (the endpoint). Any checkpoint can stop you ("no boarding pass" = short-circuit). On arrival (the response) you pass back through baggage claim and customs in reverse order.

**Another analogy — Russian dolls:** each middleware *wraps* the rest of the pipeline.

```text
           Request                                                  Response
              │                                                        ▲
   ┌──────────▼────────────────────────────────────────────────────────┴───────┐
   │ Middleware 1 (Exception handler)                                          │
   │   before ──┐                                                   ┌── after  │
   │   ┌────────▼───────────────────────────────────────────────────┴──────┐   │
   │   │ Middleware 2 (Authentication)                                     │   │
   │   │   before ──┐                                       ┌── after      │   │
   │   │   ┌────────▼───────────────────────────────────────┴──────────┐   │   │
   │   │   │ Middleware 3 (Authorization)                              │   │   │
   │   │   │   before ──┐                           ┌── after          │   │   │
   │   │   │   ┌────────▼───────────────────────────┴──────────────┐   │   │   │
   │   │   │   │ Endpoint (controller action / minimal API)        │   │   │   │
   │   │   │   └───────────────────────────────────────────────────┘   │   │   │
   │   │   └───────────────────────────────────────────────────────────┘   │   │
   │   └───────────────────────────────────────────────────────────────────┘   │
   └───────────────────────────────────────────────────────────────────────────┘
```

### How it works internally (briefly)

A middleware is ultimately a function `RequestDelegate → RequestDelegate`:

```csharp
public delegate Task RequestDelegate(HttpContext context);
// Middleware = Func<RequestDelegate, RequestDelegate>
```

When you call `app.Use(...)`, the builder just adds that function to a list. At `Build()` time the list is composed **in reverse**: the last middleware wraps the endpoint, the one before wraps that, and so on — producing a single `RequestDelegate` that Kestrel calls for every request. That's why the **registration order is the execution order**.

🎯 **Say this in the interview:** "Middleware are components chained into a pipeline; each gets the `HttpContext`, can act before and after calling `next`, or short-circuit by not calling it. Internally each is a `Func<RequestDelegate, RequestDelegate>` composed into one delegate, so the order of registration is the order of execution on the way in and reverse on the way out."

---

## 3.2 `Use`, `Run`, `Map`, `MapWhen`, `UseWhen`

| Method | Calls next? | Purpose | Rejoins main pipeline? |
|---|---|---|---|
| `app.Use(async (ctx, next) => ...)` | Optional (usually yes) | Add a normal middleware | — |
| `app.Run(async ctx => ...)` | **Never** (no `next`) | **Terminal** middleware — ends the pipeline | — |
| `app.Map("/path", branch => ...)` | — | Branch by **path prefix**; strips the prefix from `Request.Path` (moves it to `PathBase`) | **No** |
| `app.MapWhen(predicate, branch => ...)` | — | Branch by **any condition** (header, query, ...) | **No** |
| `app.UseWhen(predicate, branch => ...)` | — | **Conditionally add** middleware for some requests | **Yes** (unless the branch short-circuits) |
| `app.UseMiddleware<T>()` | Depends on `T` | Add a class-based middleware | — |

⚠️ **Interview trap:** Don't confuse `app.Map("/health", ...)` (**middleware branching**, old style) with `app.MapGet("/health", ...)` (**endpoint routing**, a route handled by the endpoint at the end of the pipeline). Also `app.Run()` (start the server) vs `app.Run(handler)` (terminal middleware).

### Verified example: order, short-circuiting, branching

```csharp
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

// Middleware 1: wraps everything after it
app.Use(async (context, next) =>
{
    Console.WriteLine("1. Logger: before");
    await next(context);                       // go deeper into the pipeline
    Console.WriteLine("1. Logger: after");
});

// Middleware 2: may SHORT-CIRCUIT
app.Use(async (context, next) =>
{
    if (context.Request.Query.ContainsKey("block"))
    {
        Console.WriteLine("2. Gate: blocked -> short-circuit");
        context.Response.StatusCode = StatusCodes.Status403Forbidden;
        return;                                // next() NOT called
    }
    Console.WriteLine("2. Gate: before");
    await next(context);
    Console.WriteLine("2. Gate: after");
});

// Branch: requests starting with /health go to a separate mini-pipeline
app.Map("/health", health => health.Run(async context =>
{
    Console.WriteLine("   /health branch");
    await context.Response.WriteAsync("Healthy");
}));

// Terminal middleware: never calls next
app.Run(async context =>
{
    Console.WriteLine("3. Terminal: writing response");
    await context.Response.WriteAsync("Hello from the end of the pipeline");
});

app.Run();
```

Calling `GET /`, `GET /?block=1` and `GET /health` prints (`>>>` = request sent, `<<<` = status and body received):

```text
>>> GET /
1. Logger: before
2. Gate: before
3. Terminal: writing response
2. Gate: after
1. Logger: after
<<< 200 Hello from the end of the pipeline
>>> GET /?block=1
1. Logger: before
2. Gate: blocked -> short-circuit
1. Logger: after
<<< 403 
>>> GET /health
1. Logger: before
2. Gate: before
   /health branch
2. Gate: after
1. Logger: after
<<< 200 Healthy
```

Read it carefully:

- **Request 1**: down through 1 → 2 → terminal, then back **up** 2 → 1. Classic Russian doll.
- **Request 2**: middleware 2 short-circuits. The terminal is never reached, but middleware 1's "after" code **still runs** (it's waiting on `await next`).
- **Request 3**: `/health` matches the `Map` branch, which handles it; the main terminal `app.Run` never runs. The branch **does not rejoin**.

### Verified example: `MapWhen` vs `UseWhen` + factory-based middleware

```csharp
var builder = WebApplication.CreateBuilder(args);
builder.Configuration["ApiKey"] = "dev-key-123";   // demo only: normally from appsettings/user-secrets

// IMiddleware must be REGISTERED in DI (any lifetime you like)
builder.Services.AddScoped<ApiKeyMiddleware>();

var app = builder.Build();

// MapWhen: branch on any condition; the branch does NOT rejoin the main pipeline
app.MapWhen(ctx => ctx.Request.Query.ContainsKey("ping"),
    branch => branch.Run(ctx => ctx.Response.WriteAsync("pong")));

// UseWhen: run extra middleware for some requests, then REJOIN the main pipeline
app.UseWhen(ctx => ctx.Request.Path.StartsWithSegments("/api"),
    branch => branch.UseMiddleware<ApiKeyMiddleware>());

app.MapGet("/", () => "public page");
app.MapGet("/api/orders", () => "secret orders");

app.Run();

// Factory-based middleware: created by DI PER REQUEST, so scoped dependencies are safe in the constructor
public class ApiKeyMiddleware(IConfiguration config, ILogger<ApiKeyMiddleware> logger) : IMiddleware
{
    public async Task InvokeAsync(HttpContext context, RequestDelegate next)
    {
        string? key = context.Request.Headers["X-Api-Key"];
        if (key != config["ApiKey"])
        {
            logger.LogWarning("Rejected request to {Path}: bad API key", context.Request.Path);
            context.Response.StatusCode = StatusCodes.Status401Unauthorized;
            await context.Response.WriteAsync("Missing or invalid API key");
            return;
        }
        await next(context);
    }
}
```

Output for four requests (the last one sends header `X-Api-Key: dev-key-123`):

```text
>>> GET /?ping
<<< 200 pong
>>> GET /
<<< 200 public page
>>> GET /api/orders
warn: ApiKeyMiddleware[0]
      Rejected request to /api/orders: bad API key
<<< 401 Missing or invalid API key
>>> GET /api/orders
<<< 200 secret orders
```

`UseWhen` added the API-key check **only** for `/api/*`, and after it passed, the request **rejoined** the main pipeline and reached the `/api/orders` endpoint.

---

## 3.3 Three ways to write custom middleware

### 1) Inline middleware (`app.Use`)

Quick, for small cross-cutting logic. The sample app (samples/ShopApi) uses one for a **correlation ID**:

```csharp
// Inline middleware: correlation id for tracing
app.Use(async (context, next) =>
{
    string id = context.Request.Headers["X-Correlation-Id"].FirstOrDefault() ?? Guid.NewGuid().ToString();
    context.Response.Headers["X-Correlation-Id"] = id;
    using (app.Logger.BeginScope(new Dictionary<string, object> { ["CorrelationId"] = id }))
    {
        await next(context);
    }
});
```

Every log line written during the request now carries `CorrelationId` (log scopes — Chapter 5), and the caller gets the ID back in the response header to quote in support tickets.

### 2) Convention-based class middleware (most common)

**Rules (the "convention"):**

- A **public constructor** that takes `RequestDelegate next` (plus any **singleton-safe** dependencies).
- A public method named **`Invoke` or `InvokeAsync`** that takes `HttpContext` as the first parameter and returns `Task`. Extra parameters are **resolved from DI per request** — that's where scoped services go.
- No interface needed — the framework finds the method by reflection once at startup.

The sample app's `Middleware/RequestTimingMiddleware.cs`:

```csharp
using System.Diagnostics;

namespace ShopApi.Middleware;

// Convention-based middleware: constructor takes RequestDelegate, InvokeAsync takes HttpContext.
// It is created ONCE (singleton-like) so inject scoped services into InvokeAsync, not the constructor.
public class RequestTimingMiddleware(RequestDelegate next, ILogger<RequestTimingMiddleware> logger)
{
    public async Task InvokeAsync(HttpContext context)
    {
        var sw = Stopwatch.StartNew();

        context.Response.OnStarting(() =>
        {
            context.Response.Headers["X-Elapsed-Ms"] = sw.ElapsedMilliseconds.ToString();
            return Task.CompletedTask;
        });

        await next(context);      // call the next middleware

        sw.Stop();
        logger.LogInformation("{Method} {Path} -> {StatusCode} in {Elapsed} ms",
            context.Request.Method, context.Request.Path, context.Response.StatusCode, sw.ElapsedMilliseconds);
    }
}

public static class RequestTimingMiddlewareExtensions
{
    public static IApplicationBuilder UseRequestTiming(this IApplicationBuilder app)
        => app.UseMiddleware<RequestTimingMiddleware>();
}
```

And in `Program.cs`: `app.UseRequestTiming();`. The extension method is the idiomatic way to expose middleware — exactly like the framework's `UseAuthentication()`.

Notice two techniques worth mentioning in an interview:

- It **times the whole rest of the pipeline** by measuring around `await next(context)`.
- It writes the header in **`Response.OnStarting`**, not after `next` — because by then the response may already be sent (section 3.6).

### 3) Factory-based middleware (`IMiddleware`)

Implement `IMiddleware` (`InvokeAsync(HttpContext, RequestDelegate)`), **register the class in DI**, then `app.UseMiddleware<T>()`. ASP.NET Core's `IMiddlewareFactory` **resolves it from the request's DI scope on every request** and releases it afterwards. See `ApiKeyMiddleware` in 3.2.

### Convention-based vs `IMiddleware`

| | Convention-based | Factory-based (`IMiddleware`) |
|---|---|---|
| Interface | None (duck typing by method name) | `IMiddleware` |
| Created | **Once**, at app startup (effectively a singleton) | **Per request** by `IMiddlewareFactory` from the request scope |
| Register in DI? | No | **Yes** (`AddScoped<T>`/`AddTransient<T>`/`AddSingleton<T>`) — otherwise it throws |
| Scoped deps (e.g. `DbContext`) | Only as **`InvokeAsync` parameters** | Safe in the **constructor** |
| `next` | Constructor parameter | `InvokeAsync` parameter |
| Extra constructor args via `UseMiddleware<T>(args)` | Supported | Not supported |
| Strongly typed / testable | OK | Better (clear contract) |
| Performance | Slightly faster (no per-request activation) | Tiny per-request cost |

⚠️ **Interview trap:** Injecting a scoped service (like `AppDbContext`) into the **constructor** of convention-based middleware. The middleware lives for the whole app, so it would capture the first request's `DbContext` forever — a **captive dependency**. In Development, scope validation throws *"Cannot resolve scoped service ... from root provider"*. Put it in `InvokeAsync(HttpContext context, AppDbContext db)` instead, or use `IMiddleware`.

```csharp
// ✅ Scoped dependency as an InvokeAsync parameter (resolved per request)
public class TenantMiddleware(RequestDelegate next)
{
    public async Task InvokeAsync(HttpContext context, AppDbContext db, ILogger<TenantMiddleware> logger)
    {
        string? tenant = context.Request.Headers["X-Tenant"];
        if (tenant is not null && !await db.Tenants.AnyAsync(t => t.Code == tenant, context.RequestAborted))
        {
            logger.LogWarning("Unknown tenant {Tenant}", tenant);
            context.Response.StatusCode = StatusCodes.Status400BadRequest;
            return;
        }
        await next(context);
    }
}
```

🎯 **Say this in the interview:** "Convention-based middleware is constructed once at startup, so its constructor should only take `RequestDelegate` and singletons; scoped services go in `InvokeAsync` parameters. `IMiddleware` is activated from DI per request, so it must be registered, but scoped services can go in its constructor."

---

## 3.4 Order matters — the recommended pipeline

Because the pipeline is a chain, the order you call `UseXxx` is the order they run. Microsoft's recommended order (verified, compiles on .NET 10):

```csharp
using Microsoft.AspNetCore.RateLimiting;

var builder = WebApplication.CreateBuilder(args);

builder.Services.AddProblemDetails();
builder.Services.AddCors(o => o.AddPolicy("Frontend", p => p.WithOrigins("http://localhost:4200").AllowAnyHeader().AllowAnyMethod()));
builder.Services.AddAuthentication();          // + .AddJwtBearer(...) in a real app
builder.Services.AddAuthorization();
builder.Services.AddRateLimiter(o => o.AddFixedWindowLimiter("fixed", l =>
{
    l.PermitLimit = 100;
    l.Window = TimeSpan.FromMinutes(1);
}));
builder.Services.AddControllers();

var app = builder.Build();

app.UseExceptionHandler();                     // 1. catch everything below
if (!app.Environment.IsDevelopment())
{
    app.UseHsts();                             // 2. tell browsers "HTTPS only"
}
app.UseHttpsRedirection();                     // 3. http -> https
app.UseStaticFiles();                          // 4. serve wwwroot files, short-circuit early
app.UseRouting();                              // 5. pick the endpoint (implicit if omitted)
app.UseCors("Frontend");                       // 6. after routing, before auth
app.UseAuthentication();                       // 7. who are you?
app.UseAuthorization();                        // 8. are you allowed?
app.UseRateLimiter();                          // 9. throttle (can use endpoint metadata / user)
app.Use(async (ctx, next) => await next(ctx)); // 10. your custom middleware
app.MapControllers().RequireRateLimiting("fixed");   // 11. endpoints

app.Run();
```

```text
 Request ─► ExceptionHandler ─► HSTS ─► HttpsRedirection ─► StaticFiles ─► Routing
            ─► CORS ─► Authentication ─► Authorization ─► RateLimiter ─► custom ─► Endpoint
 Response ◄─────────────────────────── (same chain in reverse) ◄───────────────────────
```

### Why each one is where it is

| # | Middleware | Why here |
|---|---|---|
| 1 | `UseExceptionHandler` | **First**, so its `try/catch` wraps *everything* after it. If it were last, exceptions in earlier middleware would be unhandled. |
| 2 | `UseHsts` | Adds `Strict-Transport-Security` header early; production only (would cache HSTS for `localhost` in your browser). |
| 3 | `UseHttpsRedirection` | Redirect before doing any real work over insecure HTTP. |
| 4 | `UseStaticFiles` | Serve CSS/JS/images and **short-circuit** before the expensive stuff (auth, MVC). Files are public. |
| 5 | `UseRouting` | Matches the URL to an endpoint and stores it on `HttpContext` so the following middleware can read its **metadata** (`[Authorize]`, CORS policy, rate-limit policy). |
| 6 | `UseCors` | Must run **after routing** (to use endpoint-specific policies) and **before auth** (browser **preflight `OPTIONS`** requests carry no token — they must be answered before authorization would reject them). |
| 7 | `UseAuthentication` | Reads cookie/JWT and sets `HttpContext.User`. |
| 8 | `UseAuthorization` | Checks `HttpContext.User` against the endpoint's policies. Needs the user (from 7) and the endpoint (from 5). |
| 9 | `UseRateLimiter` | After auth when you partition limits by user; uses endpoint metadata (`RequireRateLimiting`). |
| 10 | Custom middleware | Usually wants the user and endpoint to be known. |
| 11 | Endpoints (`MapControllers`, `MapGet`...) | Execute at the end of the pipeline. |

The sample app's order is the same idea, with a few extras: `UseStatusCodePages()` right after the exception handler (so empty 404/401 responses become ProblemDetails), the correlation-ID and timing middleware before CORS, and `UseOutputCache()` after the rate limiter. `UseRouting()` is omitted there because **`WebApplication` adds it implicitly** at the start of the pipeline when you map endpoints and don't call it yourself.

### Why authentication must come before authorization

**Authentication** = *who are you?* (reads the token and creates `HttpContext.User`).
**Authorization** = *are you allowed?* (checks that `User` against `[Authorize(Roles = "Admin")]`).

If you swap them, authorization runs while `HttpContext.User` is still an **anonymous** identity → every `[Authorize]` endpoint returns **401**, even with a valid token. You can't check a boarding pass before checking the passenger's ID.

⚠️ **Interview trap:** "I put `UseAuthorization()` but forgot `UseAuthentication()` and JWT still worked." — With the minimal hosting model, `WebApplication` adds authentication middleware **automatically** if `AddAuthentication` was called and you didn't add it yourself (.NET 7+). Still, always add both explicitly so the order is obvious.

⚠️ **Interview trap:** Placing `UseExceptionHandler()` *after* other middleware. It only catches exceptions thrown by middleware **registered after it**.

🎯 **Say this in the interview:** "Order matters because each middleware wraps the ones after it. Exception handling goes first so it catches everything; static files early so they short-circuit; routing before CORS and auth so they can read endpoint metadata; CORS before auth so preflights succeed; authentication before authorization because authorization needs `HttpContext.User`."

---

## 3.5 Short-circuiting

A middleware **short-circuits** when it writes a response and returns **without calling `next`**. Built-in examples:

| Middleware | Short-circuits when |
|---|---|
| `UseStaticFiles` | A matching file exists in `wwwroot` |
| `UseHttpsRedirection` | Request is HTTP → returns 307/308 redirect |
| `UseCors` | It's a CORS preflight `OPTIONS` request |
| `UseAuthorization` | User isn't allowed → 401/403 |
| `UseRateLimiter` | Limit exceeded → 429 (the sample sets `RejectionStatusCode = 429`) |
| `UseOutputCache` | A cached response exists |
| `MapHealthChecks` / terminal `Run` | Always (they're endpoints/terminal) |

.NET 8+ also lets an **endpoint** short-circuit routing and skip the remaining middleware entirely:

```csharp
app.MapGet("/ping", () => "pong").ShortCircuit();   // skips auth, CORS etc. — for cheap probes
app.MapShortCircuit(404, "robots.txt", "favicon.ico");
```

---

## 3.6 `Response.HasStarted` and `Response.OnStarting`

### In simple words

HTTP sends **headers first, then the body**. Once the first byte of the body is flushed to the client, the status code and headers are **on the wire** — you can't change them anymore. `Response.HasStarted` tells you whether that has happened. `Response.OnStarting(callback)` lets you run code **just before** headers are sent — the last safe moment to add headers.

**Analogy:** Once the courier has picked up the parcel, you can't change the address label. `OnStarting` is "call me just before the courier arrives".

### Verified example

```csharp
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.Use(async (context, next) =>
{
    // ✅ Register a callback that runs just BEFORE headers are sent
    context.Response.OnStarting(() =>
    {
        context.Response.Headers["X-Powered-By"] = "ShopApi";
        return Task.CompletedTask;
    });

    await next(context);

    // ❌ Too late: the body has been written, headers are already on the wire
    Console.WriteLine($"HasStarted after next(): {context.Response.HasStarted}");
    if (!context.Response.HasStarted)
    {
        context.Response.Headers["X-Too-Late"] = "never";
    }
});

app.MapGet("/", () => "hi");

app.Run();
```

Output for `GET /`:

```text
HasStarted after next(): True
```

The response contains the `X-Powered-By` header (added by `OnStarting`) but could never contain `X-Too-Late`. Setting a header after the response has started throws *"Headers are read-only, response has already started."*

Where this matters:

- **Timing headers** (the sample's `RequestTimingMiddleware` uses `OnStarting` for `X-Elapsed-Ms`).
- **Exception handling**: the built-in exception handler checks `HasStarted`; if the response already started streaming, it can't replace it with a 500 ProblemDetails — it can only log and abort the connection.
- Use `Response.OnCompleted(...)` for work **after** the response is fully sent (e.g. metrics), but it must not touch the response.

---

## 3.7 Middleware vs filters

Both handle cross-cutting concerns, but at different levels.

```text
 Request ─► [Middleware pipeline .............................................] ─► Endpoint
                                                                                  │
                                    MVC only:  Authorization filters → Resource filters
                                               → Model binding → Action filters → ACTION
                                               → Exception filters → Result filters → Result
```

| | Middleware | MVC Filters (and minimal-API endpoint filters) |
|---|---|---|
| Runs for | **Every** request (incl. static files, health checks, 404s) | Only requests that reach an MVC action (or an endpoint with the filter) |
| Knows about | `HttpContext` only | `ActionContext`: controller, action, **model-bound arguments**, `ModelState`, `IActionResult` |
| Scope | Global (or branch) | Global, per controller, per action (`[MyFilter]`) |
| Can short-circuit | Yes (skip `next`) | Yes (set `context.Result`) |
| Typical use | Exception handling, logging, CORS, auth, compression, correlation ID, rate limiting | Validation, per-action auth rules, audit of action arguments, result shaping, caching per action |
| Example in sample | `RequestTimingMiddleware`, correlation ID | `LogActionFilter` registered globally via `AddControllers(o => o.Filters.Add<LogActionFilter>())` |

Rule of thumb: **if it needs MVC concepts (action arguments, `ModelState`, the `IActionResult`), use a filter; otherwise use middleware.**

🎯 **Say this in the interview:** "Middleware runs for every request and only sees `HttpContext`; filters run inside MVC around a specific action and can see model-bound arguments, ModelState and the action result. I use middleware for app-wide concerns like logging and exception handling, and filters for action-specific logic like validation or auditing."

---

## 3.8 Built-in middleware cheat sheet

| Middleware | Registration (services → pipeline) | Purpose |
|---|---|---|
| Exception handler | `AddProblemDetails()` / `AddExceptionHandler<T>()` → `UseExceptionHandler()` | Global error handling → ProblemDetails |
| Developer exception page | automatic in Development | Detailed error page for devs |
| HSTS | → `UseHsts()` | `Strict-Transport-Security` header |
| HTTPS redirection | → `UseHttpsRedirection()` | Redirect HTTP → HTTPS |
| Static files | → `UseStaticFiles()` / `MapStaticAssets()` (.NET 9+) | Serve `wwwroot` |
| Routing | → `UseRouting()` (implicit) | Match endpoint |
| CORS | `AddCors()` → `UseCors()` | Cross-origin rules |
| Authentication | `AddAuthentication()` → `UseAuthentication()` | Set `HttpContext.User` |
| Authorization | `AddAuthorization()` → `UseAuthorization()` | Enforce policies |
| Rate limiting (.NET 7+) | `AddRateLimiter()` → `UseRateLimiter()` | 429 when over limit |
| Output caching (.NET 7+) | `AddOutputCache()` → `UseOutputCache()` | Server-side response cache |
| Response compression | `AddResponseCompression()` → `UseResponseCompression()` | gzip/brotli |
| Forwarded headers | → `UseForwardedHeaders()` | Correct IP/scheme behind a proxy (put it very early) |
| Request localization | `AddLocalization()` → `UseRequestLocalization()` | Culture per request |
| Session | `AddSession()` → `UseSession()` | Server-side session (needs a cache) |
| W3C logging / HTTP logging | `AddHttpLogging()` → `UseHttpLogging()` | Log requests/responses |

---

## Try it yourself

**1.** Predict the console output for a single `GET /`:

```csharp
app.Use(async (ctx, next) => { Console.Write("A"); await next(ctx); Console.Write("a"); });
app.Use(async (ctx, next) => { Console.Write("B"); await next(ctx); Console.Write("b"); });
app.Run(async ctx => { Console.Write("C"); await ctx.Response.WriteAsync("done"); });
app.Use(async (ctx, next) => { Console.Write("D"); await next(ctx); });
```

<details><summary>Answer</summary>

`ABCba`. Middleware runs in registration order and unwinds in reverse. `D` never runs because `app.Run(...)` is **terminal** — anything registered after it is unreachable.

</details>

**2.** Users with valid JWTs get 401 on every `[Authorize]` endpoint. You see this in `Program.cs`. What's wrong?

```csharp
app.UseAuthorization();
app.UseAuthentication();
app.MapControllers();
```

<details><summary>Answer</summary>

The order is reversed. Authorization runs before authentication has read the token, so `HttpContext.User` is anonymous and the authorization middleware rejects the request. Swap them: `UseAuthentication()` then `UseAuthorization()`.

</details>

**3.** Write a convention-based middleware that rejects requests with a body larger than 1 MB (using the `Content-Length` header) with status 413, and expose it as `app.UseMaxBodySize()`.

<details><summary>Answer</summary>

```csharp
public class MaxBodySizeMiddleware(RequestDelegate next)
{
    private const long Limit = 1_048_576;

    public async Task InvokeAsync(HttpContext context)
    {
        if (context.Request.ContentLength is > Limit)
        {
            context.Response.StatusCode = StatusCodes.Status413PayloadTooLarge;
            await context.Response.WriteAsync("Request body too large");
            return;                       // short-circuit
        }
        await next(context);
    }
}

public static class MaxBodySizeMiddlewareExtensions
{
    public static IApplicationBuilder UseMaxBodySize(this IApplicationBuilder app)
        => app.UseMiddleware<MaxBodySizeMiddleware>();
}
```

(In production you'd also configure Kestrel's `MaxRequestBodySize` or use `[RequestSizeLimit]`, because `Content-Length` can be absent with chunked encoding.)

</details>

**4.** Your convention-based middleware takes `AppDbContext` in its constructor. It works on your machine in Production mode but crashes at startup in Development. Why both behaviours, and how do you fix it?

<details><summary>Answer</summary>

Convention middleware is created **once** from the root provider, so a scoped `DbContext` in its constructor becomes a captive dependency (one context shared by every request — not thread-safe, stale data). In Development, `ValidateScopes` is on, so resolving a scoped service from the root provider throws. In Production validation is off, so it "works" while silently being wrong. Fix: move `AppDbContext` to an `InvokeAsync(HttpContext context, AppDbContext db)` parameter, or implement `IMiddleware` and register it as scoped.

</details>

**5.** When would you choose `UseWhen` over `MapWhen`?

<details><summary>Answer</summary>

`UseWhen` when you want to **add** middleware for some requests and then **continue** through the main pipeline to the normal endpoints (e.g. API-key check only for `/api/*`). `MapWhen` when the branch should **handle the request completely** and never rejoin (e.g. a special `?ping` responder or a legacy sub-app).

</details>

**6.** You add a response header after `await next(context)` and get "Headers are read-only, response has already started." Fix it.

<details><summary>Answer</summary>

Register the header in `context.Response.OnStarting(() => { context.Response.Headers["X"] = "..."; return Task.CompletedTask; })` **before** calling `next`. The callback runs just before headers are flushed. Optionally check `context.Response.HasStarted` before modifying headers after `next`.

</details>

---

## Quick revision

- Middleware = component in the pipeline that can act before/after `next` or short-circuit; internally `Func<RequestDelegate, RequestDelegate>`.
- Order of registration = order on the way in; reverse on the way out.
- `Use` (may call next), `Run(handler)` (terminal), `Map` (branch by path), `MapWhen` (branch by condition, no rejoin), `UseWhen` (conditional, rejoins).
- `app.Run()` starts the server; `app.Run(handler)` is terminal middleware; `app.Map` ≠ `app.MapGet`.
- Convention middleware: ctor(`RequestDelegate`, singletons) + `InvokeAsync(HttpContext, scoped deps...)`; created once.
- `IMiddleware`: must be registered in DI, created per request, scoped deps OK in the constructor.
- Recommended order: ExceptionHandler → HSTS → HttpsRedirection → StaticFiles → Routing → CORS → Authentication → Authorization → RateLimiter → custom → endpoints.
- Authentication sets `HttpContext.User`; authorization needs it — so authentication first.
- CORS before auth so preflight `OPTIONS` requests succeed; after routing for endpoint policies.
- Once `Response.HasStarted` is true you can't change status/headers — use `Response.OnStarting`.
- Middleware = every request, only `HttpContext`; filters = MVC actions, see arguments/ModelState/results.
- Expose custom middleware with a `UseXxx()` extension method (like the sample's `UseRequestTiming()`).

---

# Chapter 4. Dependency Injection

> **Why interviewers ask this:** DI is built into ASP.NET Core and everything — controllers, middleware, EF Core, logging, options — is wired through it. Interviewers almost always ask "What is DI?", "Explain the three lifetimes", and the killer follow-up: "What happens if you inject a scoped service into a singleton?". Getting lifetimes wrong causes real production bugs (stale data, thread-safety crashes, memory leaks), so they test it hard.

---

## 4.1 What are DI and IoC?

### In simple words

- **Dependency**: something a class needs to do its job (a `DbContext`, a logger, an email sender).
- **Inversion of Control (IoC)**: the class doesn't *create* its dependencies; *someone else* controls creation and hands them in. It's a principle.
- **Dependency Injection (DI)**: the most common way to do IoC — dependencies are **passed in** (usually through the constructor).
- **DI container** (`IServiceProvider` in .NET): the "someone else" that knows how to build every service and manages their lifetimes.

**Analogy:** A chef (your class) shouldn't grow vegetables and raise chickens. The restaurant manager (DI container) delivers the ingredients each morning. The chef just declares *"I need tomatoes and eggs"* — in the constructor. Want organic tomatoes for testing? Change the supplier, not the chef.

### Before DI (tightly coupled)

```csharp
public class OrderService
{
    private readonly SqlOrderRepository _repo = new SqlOrderRepository("Server=prod;...");  // hard-wired
    private readonly SmtpEmailSender _email = new SmtpEmailSender("smtp.company.com");      // hard-wired

    public void PlaceOrder(Order order)
    {
        _repo.Save(order);
        _email.Send(order.CustomerEmail, "Thanks for your order!");
    }
}
```

Problems: you can't unit-test it without a real database and SMTP server; changing the email provider means editing `OrderService`; connection strings are buried in code; every `OrderService` creates its own expensive objects.

### After DI (loosely coupled)

```csharp
public interface IOrderRepository { Task SaveAsync(Order order, CancellationToken ct); }
public interface IEmailSender { Task SendAsync(string to, string body, CancellationToken ct); }

public class OrderService(IOrderRepository repo, IEmailSender email)   // "I need these"
{
    public async Task PlaceOrderAsync(Order order, CancellationToken ct)
    {
        await repo.SaveAsync(order, ct);
        await email.SendAsync(order.CustomerEmail, "Thanks for your order!", ct);
    }
}

// Program.cs — the ONLY place that knows concrete types
builder.Services.AddScoped<IOrderRepository, EfOrderRepository>();
builder.Services.AddSingleton<IEmailSender, SendGridEmailSender>();
builder.Services.AddScoped<OrderService>();
```

Now a unit test passes fakes (`new OrderService(new FakeRepo(), new FakeEmail())`), and swapping SMTP for SendGrid is a one-line change in `Program.cs`.

| Benefit | Why |
|---|---|
| Testability | Inject mocks/fakes |
| Loose coupling | Depend on abstractions (the "D" in SOLID — Dependency Inversion) |
| Single place for wiring | `Program.cs` is the *composition root* |
| Lifetime management | Container creates, shares and **disposes** objects correctly |
| Configuration | Swap implementations per environment |

🎯 **Say this in the interview:** "Dependency injection means a class receives its dependencies from outside — usually via the constructor — instead of creating them. It's a form of Inversion of Control. ASP.NET Core has a built-in container: I register services with a lifetime in `Program.cs`, and the framework constructs controllers, middleware and services with their dependencies, which makes code loosely coupled and testable."

---

## 4.2 Constructor injection — how the container builds objects

When ASP.NET Core needs a `ProductsController`, it:

1. Looks at its constructor: needs `IProductService`.
2. Looks up `IProductService` → registered as `ProductService` (scoped).
3. `ProductService`'s primary constructor needs `AppDbContext` and `ILogger<ProductService>`.
4. Resolves those (recursively), builds `ProductService`, then the controller.

This is the **dependency graph**, resolved automatically. In the sample app (samples/ShopApi):

```csharp
// Program.cs
builder.Services.AddDbContext<AppDbContext>(o =>
    o.UseSqlite(builder.Configuration.GetConnectionString("Shop")));     // scoped by default
builder.Services.AddScoped<IProductService, ProductService>();

// Services/ProductService.cs — primary constructor = constructor injection
public class ProductService(AppDbContext db, ILogger<ProductService> logger) : IProductService
{
    // ...
}
```

Rules the container follows:

- If a class has multiple constructors, it picks the one with the **most parameters it can satisfy**. (Ambiguity → exception.) `[ActivatorUtilitiesConstructor]` can mark the preferred one for `ActivatorUtilities`.
- A missing registration throws *"Unable to resolve service for type 'X' while attempting to activate 'Y'"* — the most common DI error you'll see.
- Primitive parameters (`string`, `int`) can't be resolved — use the Options pattern (Chapter 5) or a factory.
- Circular dependencies (A needs B, B needs A) throw *"A circular dependency was detected"*.

---

## 4.3 The three lifetimes

### In simple words

| Lifetime | One instance per... | Analogy |
|---|---|---|
| **Transient** | **every time** it's requested | Paper cups — a new one every time you ask |
| **Scoped** | **HTTP request** (DI scope) | A hotel room key card — one per guest stay, everyone in your party shares it during the stay |
| **Singleton** | **the whole application** | The building's front door — one for everyone, forever |

### Verified demo: GUIDs per "request"

This is a plain console app (the same container ASP.NET Core uses — `Microsoft.Extensions.DependencyInjection`). Each `CreateScope()` simulates what ASP.NET Core does for each HTTP request.

```csharp
var services = new ServiceCollection();
services.AddTransient<TransientService>();
services.AddScoped<ScopedService>();
services.AddSingleton<SingletonService>();
services.AddTransient<Consumer>();   // asks for all three

using ServiceProvider root = services.BuildServiceProvider();

for (int request = 1; request <= 2; request++)
{
    // ASP.NET Core creates one scope per HTTP request - we simulate that here
    using IServiceScope scope = root.CreateScope();
    var sp = scope.ServiceProvider;

    var t = sp.GetRequiredService<TransientService>();
    var s = sp.GetRequiredService<ScopedService>();
    var g = sp.GetRequiredService<SingletonService>();
    var c = sp.GetRequiredService<Consumer>();

    Console.WriteLine($"Request {request}");
    Console.WriteLine($"  Transient : {t.Id} | inside Consumer: {c.Transient.Id}");
    Console.WriteLine($"  Scoped    : {s.Id} | inside Consumer: {c.Scoped.Id}");
    Console.WriteLine($"  Singleton : {g.Id} | inside Consumer: {c.Singleton.Id}");
}

public abstract class OperationBase
{
    public string Id { get; } = Guid.NewGuid().ToString()[..8];
}
public class TransientService : OperationBase;
public class ScopedService : OperationBase;
public class SingletonService : OperationBase;

public class Consumer(TransientService transient, ScopedService scoped, SingletonService singleton)
{
    public TransientService Transient => transient;
    public ScopedService Scoped => scoped;
    public SingletonService Singleton => singleton;
}
```

Output (your GUIDs will differ — look at **which ones match**):

```text
Request 1
  Transient : 0185b4fc | inside Consumer: 024279f9
  Scoped    : 7b2117b9 | inside Consumer: 7b2117b9
  Singleton : aea7f3e2 | inside Consumer: aea7f3e2
Request 2
  Transient : 9c394d28 | inside Consumer: bd3428bc
  Scoped    : 85e9fe2a | inside Consumer: 85e9fe2a
  Singleton : aea7f3e2 | inside Consumer: aea7f3e2
```

- **Transient**: different *every* time — even within the same request.
- **Scoped**: same *within* a request, different *between* requests.
- **Singleton**: same everywhere, forever.

### When to use which

| Lifetime | Use for | Examples | Watch out |
|---|---|---|---|
| Transient | Lightweight, stateless services | Validators, mappers, small helpers | Many allocations if heavy; disposable transients are held until the scope ends |
| Scoped | Per-request state / unit of work | **`DbContext`**, repositories, services using `DbContext`, "current user" context | Never inject into a singleton; not available outside a scope |
| Singleton | Expensive to create, shared, **thread-safe** state | Caches (`IMemoryCache`), configuration wrappers, `HttpClient` handlers (via factory), `IOptionsMonitor`, hosted services | **Must be thread-safe**; never hold scoped services; memory lives forever |

```csharp
builder.Services.AddTransient<IPriceCalculator, PriceCalculator>();
builder.Services.AddScoped<IProductService, ProductService>();
builder.Services.AddSingleton<IExchangeRateCache, ExchangeRateCache>();
```

⚠️ **Interview trap:** "Singleton is best for performance, so make everything singleton." A singleton is shared by **all concurrent requests** — any mutable field is a race condition, and anything it holds lives forever. Only make a service singleton if it's stateless or explicitly thread-safe.

⚠️ **Interview trap:** "`DbContext` should be singleton to save connections." No! `DbContext` is **not thread-safe** and tracks entities; a singleton context leaks memory, returns stale data and throws under concurrency. Connection pooling already makes per-request contexts cheap. `AddDbContext` registers it as **scoped** by default.

🎯 **Say this in the interview:** "Transient creates a new instance every time it's resolved, scoped creates one per request, and singleton one for the app's lifetime. `DbContext` is scoped; caches and stateless shared services are singletons and must be thread-safe; lightweight stateless helpers are transient. The key rule is that a service should never depend on something with a shorter lifetime."

---

## 4.4 Captive dependencies, `ValidateScopes` and `ValidateOnBuild`

### In simple words

A **captive dependency** is a longer-lived service holding on to a shorter-lived one. The classic: a **singleton** with a **scoped** `DbContext` in its constructor. The singleton is created once, so it "captures" the first request's `DbContext` and keeps using it forever — across all requests and threads.

**Analogy:** A permanent employee (singleton) borrows a visitor's badge (scoped) and keeps using it for years after the visitor has left.

| Consumer ↓ / Dependency → | Transient | Scoped | Singleton |
|---|---|---|---|
| **Transient** | ✅ | ✅ | ✅ |
| **Scoped** | ✅ | ✅ | ✅ |
| **Singleton** | ⚠️ OK technically, but the transient effectively becomes a singleton | ❌ **Captive dependency** | ✅ |

Rule: **depend only on services with the same or longer lifetime.**

### Verified demo: the container catches it

```csharp
var services = new ServiceCollection();
services.AddScoped<AppDbContextFake>();
services.AddSingleton<PriceCache>();      // ❌ singleton depends on scoped

// What ASP.NET Core does automatically in Development:
var options = new ServiceProviderOptions { ValidateScopes = true, ValidateOnBuild = true };

try
{
    using var provider = services.BuildServiceProvider(options);
}
catch (AggregateException ex)
{
    Console.WriteLine(ex.InnerExceptions[0].Message);
}

public class AppDbContextFake;
public class PriceCache(AppDbContextFake db)
{
    public AppDbContextFake Db => db;
}
```

Output:

```text
Error while validating the service descriptor 'ServiceType: PriceCache Lifetime: Singleton ImplementationType: PriceCache': Cannot consume scoped service 'AppDbContextFake' from singleton 'PriceCache'.
```

| Option | What it checks | When |
|---|---|---|
| `ValidateScopes` | Scoped services aren't resolved from the **root** provider and aren't injected into singletons | At resolve time |
| `ValidateOnBuild` | Every registration **can be constructed** (all dependencies registered, no captive scoped deps) | At `Build()` — app fails to start |

`WebApplication.CreateBuilder` turns **both on in Development only** (they cost startup time). That's why the bug "works in production" but crashes locally — the crash is a gift. You can force them on everywhere:

```csharp
builder.Host.UseDefaultServiceProvider(o =>
{
    o.ValidateScopes = true;
    o.ValidateOnBuild = true;
});
```

### The fix: `IServiceScopeFactory`

When a singleton genuinely needs a scoped service (typically a `BackgroundService` needing a `DbContext`), **create a scope yourself** for each unit of work. The sample's `Background/PendingOrderCleanupService.cs` is the textbook example:

```csharp
// BackgroundService is a SINGLETON. DbContext is SCOPED.
// So we create a scope per run with IServiceScopeFactory (avoid the captive-dependency bug).
public class PendingOrderCleanupService(IServiceScopeFactory scopeFactory, ILogger<PendingOrderCleanupService> logger)
    : BackgroundService
{
    protected override async Task ExecuteAsync(CancellationToken stoppingToken)
    {
        using var timer = new PeriodicTimer(TimeSpan.FromMinutes(5));
        do
        {
            try
            {
                await using AsyncServiceScope scope = scopeFactory.CreateAsyncScope();
                AppDbContext db = scope.ServiceProvider.GetRequiredService<AppDbContext>();

                DateTime cutoff = DateTime.UtcNow.AddDays(-1);
                int cancelled = await db.Orders
                    .Where(o => o.Status == "Pending" && o.CreatedUtc < cutoff)
                    .ExecuteUpdateAsync(s => s.SetProperty(o => o.Status, "Cancelled"), stoppingToken);

                if (cancelled > 0)
                    logger.LogInformation("Cancelled {Count} stale orders", cancelled);
            }
            catch (Exception ex) when (ex is not OperationCanceledException)
            {
                logger.LogError(ex, "Cleanup failed");   // never let the loop die
            }
        }
        while (await timer.WaitForNextTickAsync(stoppingToken));
    }
}
```

Points to call out:

- `IServiceScopeFactory` itself is a **singleton**, so it's safe to inject into a singleton.
- `CreateAsyncScope()` + `await using` (.NET 6+) disposes the scope — and the `DbContext` — asynchronously after each run. A fresh `DbContext` per run means no stale tracked entities.
- The same pattern also appears in the sample's `Program.cs` to create the database at startup: `using (IServiceScope scope = app.Services.CreateScope()) { ... }`.

Alternatives: `IDbContextFactory<T>` (`AddDbContextFactory`) when you only need a `DbContext`; or make the consumer scoped too.

🎯 **Say this in the interview:** "Injecting a scoped service into a singleton creates a captive dependency — the scoped instance lives forever, so a `DbContext` would be shared across requests and threads. ASP.NET Core detects it in Development with `ValidateScopes` and `ValidateOnBuild`. The fix is to inject `IServiceScopeFactory` and create a scope per unit of work, like in a `BackgroundService`."

---

## 4.5 Registration techniques

### Verified demo: multiple registrations, `TryAdd`, keyed, factory, open generics

```csharp
using Microsoft.Extensions.DependencyInjection.Extensions;

var services = new ServiceCollection();

// 1. Multiple registrations: last one wins for single resolve, ALL come via IEnumerable<T>
services.AddSingleton<INotifier, EmailNotifier>();
services.AddSingleton<INotifier, SmsNotifier>();

// 2. TryAdd: only registers if nothing is registered yet (library-friendly)
services.TryAddSingleton<INotifier, PushNotifier>();       // ignored

// 3. Keyed services (.NET 8+)
services.AddKeyedSingleton<IPaymentGateway, RazorpayGateway>("razorpay");
services.AddKeyedSingleton<IPaymentGateway, StripeGateway>("stripe");

// 4. Factory registration: you control construction
services.AddSingleton<IClock>(sp => new FixedClock(new DateTime(2025, 1, 1)));

// 5. Open generics: one line covers IRepository<Product>, IRepository<Order>, ...
services.AddScoped(typeof(IRepository<>), typeof(InMemoryRepository<>));

services.AddScoped<CheckoutService>();

using var provider = services.BuildServiceProvider();
using var scope = provider.CreateScope();
var sp = scope.ServiceProvider;

Console.WriteLine($"Single INotifier : {sp.GetRequiredService<INotifier>().GetType().Name}");
Console.WriteLine($"All INotifiers   : {string.Join(", ", sp.GetServices<INotifier>().Select(n => n.GetType().Name))}");
Console.WriteLine($"Keyed 'stripe'   : {sp.GetRequiredKeyedService<IPaymentGateway>("stripe").Name}");
Console.WriteLine($"Clock            : {sp.GetRequiredService<IClock>().Now:yyyy-MM-dd}");
Console.WriteLine($"Open generic     : {sp.GetRequiredService<IRepository<Product>>().GetType().Name}");
Console.WriteLine(sp.GetRequiredService<CheckoutService>().Pay(500m));

public interface INotifier;
public class EmailNotifier : INotifier;
public class SmsNotifier : INotifier;
public class PushNotifier : INotifier;

public interface IPaymentGateway { string Name { get; } }
public class RazorpayGateway : IPaymentGateway { public string Name => "Razorpay"; }
public class StripeGateway : IPaymentGateway { public string Name => "Stripe"; }

public interface IClock { DateTime Now { get; } }
public class FixedClock(DateTime now) : IClock { public DateTime Now => now; }

public record Product(int Id);
public interface IRepository<T>;
public class InMemoryRepository<T> : IRepository<T>;

public class CheckoutService(
    [FromKeyedServices("razorpay")] IPaymentGateway gateway,
    IEnumerable<INotifier> notifiers)
{
    public string Pay(decimal amount) =>
        $"Paid {amount} via {gateway.Name}, notifying {notifiers.Count()} channels";
}
```

Output:

```text
Single INotifier : SmsNotifier
All INotifiers   : EmailNotifier, SmsNotifier
Keyed 'stripe'   : Stripe
Clock            : 2025-01-01
Open generic     : InMemoryRepository`1
Paid 500 via Razorpay, notifying 2 channels
```

### Multiple implementations and `IEnumerable<T>`

- Resolving `INotifier` returns the **last** registration (`SmsNotifier`).
- Injecting `IEnumerable<INotifier>` gives **all** of them, in registration order — perfect for "notify all channels", validation rule pipelines, or plugin lists.

### `Add` vs `TryAdd` vs `TryAddEnumerable` vs `Replace`

| Method | Behaviour | Typical use |
|---|---|---|
| `AddScoped<I, T>()` | Always appends | App code |
| `TryAddScoped<I, T>()` | Adds only if **no** registration for `I` exists | Libraries providing a **default** that apps can override |
| `TryAddEnumerable(ServiceDescriptor)` | Adds only if this exact `I`+`T` pair isn't there | Libraries adding one item to a multi-implementation list without duplicates |
| `Replace(ServiceDescriptor)` | Removes the first registration of `I`, adds the new one | Tests / overriding framework services |
| `RemoveAll<I>()` | Removes all registrations | Integration tests (swap real DB for test DB) |

### Keyed services (.NET 8+)

Before .NET 8, choosing between implementations meant factories or a custom resolver. Now:

```csharp
builder.Services.AddKeyedScoped<IPaymentGateway, RazorpayGateway>("razorpay");
builder.Services.AddKeyedScoped<IPaymentGateway, StripeGateway>("stripe");

// Constructor injection
public class CheckoutService([FromKeyedServices("razorpay")] IPaymentGateway gateway) { /* ... */ }

// Minimal API
app.MapPost("/pay/stripe", ([FromKeyedServices("stripe")] IPaymentGateway gateway) => gateway.Name);

// Runtime choice (key from the request)
app.MapPost("/pay/{provider}", (string provider, IServiceProvider sp) =>
    sp.GetRequiredKeyedService<IPaymentGateway>(provider).Name);
```

### Factory registration

Use a lambda when construction needs logic, configuration values, or a choice at runtime:

```csharp
builder.Services.AddSingleton<IBlobStore>(sp =>
{
    var env = sp.GetRequiredService<IHostEnvironment>();
    return env.IsDevelopment()
        ? new LocalDiskBlobStore("./blobs")
        : new AzureBlobStore(sp.GetRequiredService<IConfiguration>()["Storage:Connection"]!);
});
```

Note: with a factory, **the container still disposes** the returned object (unlike passing a ready-made instance — see 4.6).

### Open generics

`services.AddScoped(typeof(IRepository<>), typeof(EfRepository<>))` registers the generic pattern once; asking for `IRepository<Order>` builds `EfRepository<Order>`. `ILogger<T>` and `IOptions<T>` are registered exactly this way by the framework.

---

## 4.6 Disposal rules

### Verified demo

```csharp
var services = new ServiceCollection();
services.AddScoped<ScopedResource>();
services.AddTransient<TransientResource>();
services.AddSingleton(new ExternallyCreated());   // YOU created it -> container will NOT dispose it

using (var root = services.BuildServiceProvider())
{
    using (var scope = root.CreateScope())
    {
        scope.ServiceProvider.GetRequiredService<ScopedResource>();
        scope.ServiceProvider.GetRequiredService<TransientResource>();
        scope.ServiceProvider.GetRequiredService<ExternallyCreated>();
        Console.WriteLine("end of scope (request)");
    }
    Console.WriteLine("end of app");
}

public class ScopedResource : IDisposable
{
    public void Dispose() => Console.WriteLine("ScopedResource disposed");
}
public class TransientResource : IDisposable
{
    public void Dispose() => Console.WriteLine("TransientResource disposed");
}
public class ExternallyCreated : IDisposable
{
    public void Dispose() => Console.WriteLine("ExternallyCreated disposed");
}
```

Output:

```text
end of scope (request)
TransientResource disposed
ScopedResource disposed
end of app
```

| Rule | Detail |
|---|---|
| The container disposes what **it** creates | `IDisposable`/`IAsyncDisposable` services are disposed when their scope (or the root, for singletons) is disposed |
| Order | Reverse order of creation |
| Scoped & transient from a request scope | Disposed **at the end of the request** |
| Transient disposables resolved from the **root** | Held until the **app** shuts down → memory leak if resolved repeatedly. Avoid resolving transients from `app.Services` in a loop |
| Instances you pass in (`AddSingleton(new X())`) | **Never** disposed by the container — you own them (notice `ExternallyCreated` never printed) |
| Your code | Never call `Dispose()` on an injected service — you don't own it |

---

## 4.7 The Service Locator anti-pattern

### In simple words

**Service Locator** = instead of declaring dependencies in the constructor, the class asks the container for them at runtime (`IServiceProvider.GetService<T>()` sprinkled through methods).

```csharp
// ❌ Service locator: hidden dependencies
public class InvoiceService(IServiceProvider sp)
{
    public async Task SendAsync(int id)
    {
        var db = sp.GetRequiredService<AppDbContext>();        // what does this class need? Read every line to know.
        var mailer = sp.GetRequiredService<IEmailSender>();
        // ...
    }
}

// ✅ Explicit dependencies
public class InvoiceService(AppDbContext db, IEmailSender mailer) { /* ... */ }
```

| Why it's bad | |
|---|---|
| Hidden dependencies | The constructor lies — you can't see what the class needs |
| Runtime failures | Missing registrations fail late, not at startup (`ValidateOnBuild` can't check them) |
| Harder to test | Tests must build a whole container instead of passing fakes |
| Lifetime bugs | Easy to resolve scoped services from the root provider |

**Legitimate exceptions** (infrastructure code, not business code): creating scopes in singletons/background services (`IServiceScopeFactory`), factory lambdas (`sp => ...`), choosing a keyed service by a runtime key, and framework plumbing like `HttpContext.RequestServices` inside middleware or filters.

⚠️ **Interview trap:** Calling `builder.Services.BuildServiceProvider()` inside `Program.cs` to resolve something "early". It creates a **second container** with its own singletons (the analyzer warns: *ASP0000*). Use the Options pattern, a factory lambda, or `app.Services` after `Build()` instead.

---

## 4.8 Injection points in ASP.NET Core

| Where | How | Notes |
|---|---|---|
| Controllers | Constructor | Controllers are created per request (transient-like) |
| Controller **actions** | `[FromServices] IMyService svc` parameter | Inject into one action only — handy for rarely-used services |
| Minimal API handlers | Parameter: `(IProductService svc) => ...` | Inferred from DI automatically for registered types; `[FromServices]` optional |
| Convention middleware | Constructor (singletons only) **and** `InvokeAsync` parameters (scoped OK) | Created once |
| `IMiddleware` | Constructor | Created per request |
| Filters | `[ServiceFilter(typeof(F))]` (filter registered in DI) or `[TypeFilter(typeof(F))]` (not registered; dependencies resolved) | Global: `o.Filters.Add<F>()` — as in the sample's `LogActionFilter` |
| Razor views | `@inject IMyService Svc` | |
| Hosted services | Constructor (singletons) + `IServiceScopeFactory` for scoped | |
| Anywhere with `HttpContext` | `context.RequestServices.GetRequiredService<T>()` | Request scope; service-locator style, use sparingly |

### Verified demo: per-request scoping in a real web app

```csharp
using Microsoft.AspNetCore.Mvc;

var builder = WebApplication.CreateBuilder(args);
builder.Services.AddScoped<RequestInfo>();          // one per request
builder.Services.AddSingleton<VisitCounter>();      // one for the whole app
builder.Services.AddControllers();

var app = builder.Build();

app.UseMiddleware<StampMiddleware>();
app.MapControllers();

// Minimal API: services are injected straight into the handler parameters
app.MapGet("/whoami", (RequestInfo info, VisitCounter counter) =>
    $"request {info.Id}, visit #{counter.Current}");

app.Run();

public class RequestInfo
{
    public string Id { get; } = Guid.NewGuid().ToString()[..8];
}

public class VisitCounter
{
    private int _count;
    public int Increment() => Interlocked.Increment(ref _count);
    public int Current => Volatile.Read(ref _count);
}

// Convention middleware is created ONCE -> only singletons in the constructor.
// Scoped services go into InvokeAsync parameters (resolved per request).
public class StampMiddleware(RequestDelegate next, VisitCounter counter)
{
    public async Task InvokeAsync(HttpContext context, RequestInfo info)
    {
        counter.Increment();
        context.Response.Headers["X-Request-Id"] = info.Id;
        await next(context);
    }
}

[ApiController]
[Route("api/report")]
public class ReportController(RequestInfo info) : ControllerBase
{
    // [FromServices]: inject into ONE action only (handy for rarely-used, expensive services)
    [HttpGet]
    public string Get([FromServices] VisitCounter counter) => $"report for {info.Id}, visits so far {counter.Current}";
}
```

Calling `/whoami` twice and then `/api/report`:

```text
>>> GET /whoami
<<< 200 request d3be0b12, visit #1
>>> GET /whoami
<<< 200 request 932c72d4, visit #2
>>> GET /api/report
<<< 200 report for 0d51dc67, visits so far 3
```

Within one request, the middleware and the endpoint/controller get the **same** `RequestInfo` (the `X-Request-Id` header equals the ID in the body); every request gets a new one. The singleton `VisitCounter` keeps counting across requests — and is thread-safe thanks to `Interlocked`.

---

## 4.9 Built-in container vs third-party containers

The built-in container covers 95% of needs. Third-party containers (Autofac, Lamar, DryIoc, Scrutor for scanning) add:

| Feature | Built-in | Autofac / others |
|---|---|---|
| Constructor injection, 3 lifetimes, open generics | ✅ | ✅ |
| Keyed services | ✅ (.NET 8+) | ✅ |
| Assembly scanning / convention registration | ❌ (Scrutor adds it) | ✅ |
| Property injection | ❌ | ✅ |
| Decorators / interceptors | ❌ (Scrutor `Decorate`) | ✅ |
| Child containers, named lifetimes | ❌ | ✅ |

Plug in with `builder.Host.UseServiceProviderFactory(new AutofacServiceProviderFactory());`.

---

## Try it yourself

**1.** You register `builder.Services.AddSingleton<ReportGenerator>()` and `ReportGenerator`'s constructor takes `AppDbContext`. What happens in Development, what happens in Production, and how do you fix it?

<details><summary>Answer</summary>

**Development:** `ValidateOnBuild`/`ValidateScopes` are on → startup fails with "Cannot consume scoped service 'AppDbContext' from singleton 'ReportGenerator'". **Production:** validation is off → it starts, but one `DbContext` is captured forever and shared across concurrent requests → stale data, memory growth, and "A second operation was started on this context" exceptions. **Fix:** make `ReportGenerator` scoped, or inject `IServiceScopeFactory` (or `IDbContextFactory<AppDbContext>`) and create a scope/context per operation.

</details>

**2.** Two classes are registered as `INotifier`. A service injects `INotifier`. Which one does it get? How can it get both?

<details><summary>Answer</summary>

It gets the **last registered** one. To get all of them, inject `IEnumerable<INotifier>` — the container returns every registration in order. With .NET 8+ you could also use keyed services to pick a specific one.

</details>

**3.** Why is `services.TryAddSingleton<IClock, SystemClock>()` preferred over `AddSingleton` inside a reusable library's `AddMyLibrary()` extension method?

<details><summary>Answer</summary>

`TryAdd` only registers if the app hasn't already registered `IClock`. This lets the consuming app provide its own implementation (e.g. a fake clock in tests) **before or after** calling `AddMyLibrary()` without being silently overridden, and calling `AddMyLibrary()` twice doesn't create duplicates.

</details>

**4.** Rewrite this to avoid the service locator, and say what you gain.

```csharp
public class OrdersController(IServiceProvider sp) : ControllerBase
{
    [HttpGet("{id}")]
    public async Task<IActionResult> Get(int id)
        => Ok(await sp.GetRequiredService<IOrderService>().GetAsync(id));
}
```

<details><summary>Answer</summary>

```csharp
public class OrdersController(IOrderService orders) : ControllerBase
{
    [HttpGet("{id}")]
    public async Task<IActionResult> Get(int id, CancellationToken ct)
        => Ok(await orders.GetAsync(id, ct));
}
```

Gains: the dependency is visible in the constructor, a unit test can pass a fake `IOrderService` directly, and `ValidateOnBuild` can verify the registration at startup instead of failing at request time.

</details>

**5.** A teammate resolves a transient `IDisposable` service from `app.Services` inside a loop that runs every second in a hosted service. Memory keeps growing. Why?

<details><summary>Answer</summary>

`app.Services` is the **root** provider. Disposable transients resolved from the root are tracked by the root so they can be disposed at app shutdown — so every instance stays referenced until the app stops → leak. Fix: create a scope per iteration (`using var scope = scopeFactory.CreateScope();`) and resolve from `scope.ServiceProvider`, so instances are disposed at the end of each iteration.

</details>

**6.** Which lifetime would you choose for: (a) `AppDbContext`, (b) a class that wraps `IMemoryCache` for product prices, (c) a stateless `PriceCalculator`, (d) a `CurrentUser` class populated from `HttpContext.User`?

<details><summary>Answer</summary>

(a) **Scoped** (one unit of work per request, not thread-safe). (b) **Singleton** (the cache is shared and thread-safe; `IMemoryCache` itself is a singleton). (c) **Transient** or **singleton** — stateless, so either works; transient is the safe default, singleton avoids allocations. (d) **Scoped** — it represents data of the current request.

</details>

---

## Quick revision

- DI = dependencies are passed in (constructor), not created inside; it's a form of IoC; `Program.cs` is the composition root.
- Transient = new every resolve; Scoped = one per HTTP request; Singleton = one per app.
- `DbContext` is scoped (not thread-safe); singletons must be thread-safe.
- Never depend on a shorter lifetime — scoped inside singleton = **captive dependency**.
- `ValidateScopes` + `ValidateOnBuild` are on in Development only; they catch captive deps and missing registrations at startup.
- Need scoped work in a singleton/`BackgroundService`? Inject `IServiceScopeFactory` and `CreateAsyncScope()` per unit of work.
- Multiple registrations: single resolve gets the **last**; `IEnumerable<T>` gets **all**.
- `TryAdd*` registers only if absent (library defaults); `Replace`/`RemoveAll` are for overrides and tests.
- Keyed services (.NET 8+): `AddKeyedScoped<I,T>("key")` + `[FromKeyedServices("key")]`.
- Factory registration `sp => new X(...)` for runtime decisions; open generics with `typeof(IRepo<>)`.
- The container disposes what it creates (end of scope / app); never dispose injected services; it does **not** dispose instances you passed in.
- Service locator hides dependencies — avoid in business code; `[FromServices]` injects into a single action.

---

# Chapter 5. Configuration, Options and Logging

> **Why interviewers ask this:** Every real app needs settings that change per environment, secrets that must never be committed, and logs you can actually search at 2 a.m. during an outage. Interviewers ask about configuration precedence, `IOptions` vs `IOptionsSnapshot` vs `IOptionsMonitor`, where secrets go, and — a favourite trick — why `logger.LogInformation($"Order {id}")` is wrong.

---

## 5.1 Configuration basics

### In simple words

**Configuration** in ASP.NET Core is one big **key–value dictionary** built by stacking several **providers** (JSON files, environment variables, command line, user secrets, Azure Key Vault...). Keys are hierarchical, separated by `:` — e.g. `Jwt:Issuer`, `ConnectionStrings:Shop`, `Logging:LogLevel:Default`.

**Analogy:** Transparent sheets on an overhead projector. The bottom sheet is `appsettings.json`. You lay `appsettings.Production.json` on top, then environment variables, then command-line args. Where a later sheet writes something, it **covers** what's below; everywhere else you still see the lower sheets.

The sample app's `appsettings.json`:

```json
{
  "ConnectionStrings": {
    "Shop": "Data Source=shop.db"
  },
  "Jwt": {
    "Issuer": "ShopApi",
    "Audience": "ShopApiClients",
    "Key": "DEMO-ONLY-change-me-this-key-must-be-32+-bytes!",
    "ExpiryMinutes": 60
  },
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning",
      "Microsoft.EntityFrameworkCore.Database.Command": "Warning"
    }
  },
  "AllowedHosts": "*"
}
```

It flattens to keys like `ConnectionStrings:Shop`, `Jwt:Issuer`, `Jwt:ExpiryMinutes`, `Logging:LogLevel:Default`. Arrays become `Admins:0`, `Admins:1`... Keys are **case-insensitive**.

> ⚠️ The sample keeps a demo JWT key in `appsettings.json` so it runs out of the box. In a real app that key would live in user secrets (dev) and Key Vault / environment variables (prod) — see 5.5.

---

## 5.2 Providers and precedence

`WebApplication.CreateBuilder` adds these providers, **lowest to highest priority** (later wins):

| # | Provider | Notes |
|---|---|---|
| 1 | `appsettings.json` | Base settings, committed to Git (no secrets!) |
| 2 | `appsettings.{Environment}.json` | e.g. `appsettings.Development.json`; overrides the base |
| 3 | **User secrets** | **Development only**, stored outside the project folder |
| 4 | **Environment variables** | Containers, App Service, Kubernetes |
| 5 | **Command-line arguments** | `--Shop:Discount=20` — highest priority |

(Host-level settings such as `ASPNETCORE_ENVIRONMENT` and `ASPNETCORE_URLS` come from `ASPNETCORE_`/`DOTNET_`-prefixed env vars and are applied first, because they decide *which* environment file to load.) Providers you add yourself — Azure Key Vault, Azure App Configuration, a database — go **after** these by default, so they win.

### Verified demo: watch an override climb the stack

```csharp
var builder = WebApplication.CreateBuilder(args);

// Which providers did CreateBuilder add, and in what order? (later = higher priority)
foreach (IConfigurationProvider provider in ((IConfigurationRoot)builder.Configuration).Providers)
{
    Console.WriteLine($"- {provider}");
}

// Read values three ways
decimal discount = builder.Configuration.GetValue<decimal>("Shop:Discount");           // single value, typed
IConfigurationSection shop = builder.Configuration.GetSection("Shop");                   // a sub-tree
ShopSettings settings = shop.Get<ShopSettings>() ?? new ShopSettings();                  // bind to a class

Console.WriteLine($"Discount        = {discount}");
Console.WriteLine($"Shop:Name       = {shop["Name"]}");
Console.WriteLine($"Bound settings  = {settings.Name}, {settings.Discount}%, admins: {string.Join("/", settings.Admins)}");
Console.WriteLine($"ConnectionString= {builder.Configuration.GetConnectionString("Shop")}");

public class ShopSettings
{
    public string Name { get; set; } = "";
    public decimal Discount { get; set; }
    public string[] Admins { get; set; } = [];
}
```

With these two files next to the app:

```json
// appsettings.json
{
  "ConnectionStrings": { "Shop": "Data Source=shop.db" },
  "Shop": {
    "Name": "Demo Shop",
    "Discount": 5,
    "Admins": [ "asha", "ravi" ]
  }
}
```

```json
// appsettings.Development.json
{
  "Shop": { "Discount": 10 }
}
```

Running it four ways (provider list shown once, for the last run in Development with a command-line argument; `MyApp` is the project name):

```text
- MemoryConfigurationProvider
- EnvironmentVariablesConfigurationProvider Prefix: 'ASPNETCORE_'
- MemoryConfigurationProvider
- EnvironmentVariablesConfigurationProvider Prefix: 'DOTNET_'
- CommandLineConfigurationProvider
- JsonConfigurationProvider for 'appsettings.json' (Optional)
- JsonConfigurationProvider for 'appsettings.Development.json' (Optional)
- JsonConfigurationProvider for 'MyApp.settings.json' (Optional)
- JsonConfigurationProvider for 'MyApp.settings.Development.json' (Optional)
- EnvironmentVariablesConfigurationProvider
- CommandLineConfigurationProvider
- Microsoft.Extensions.Configuration.ChainedConfigurationProvider
```

(User secrets aren't listed because this demo project has no `UserSecretsId`; once you run `dotnet user-secrets init` a `JsonConfigurationProvider for 'secrets.json'` appears right after the environment JSON file. The optional `MyApp.settings.json` entries appear on recent .NET 10 runtimes; older versions don't list them.)

| Run | Command | `Discount` printed | Who won |
|---|---|---|---|
| 1 | `dotnet MyApp.dll` (Production) | `5` | `appsettings.json` |
| 2 | `ASPNETCORE_ENVIRONMENT=Development` | `10` | `appsettings.Development.json` |
| 3 | + env var `Shop__Discount=15` | `15` | Environment variable |
| 4 | + `--Shop:Discount=20` | `20` | Command line |

`Name` and `Admins` stayed `Demo Shop` / `asha/ravi` in every run: **overrides are per key**, not per file.

### Environment variables and the double underscore `__`

`:` isn't allowed in environment variable names on many platforms (Linux shells, some CI systems), so use **`__` (double underscore)** — it's translated to `:`.

```bash
# Linux / Docker / Kubernetes
export ConnectionStrings__Shop="Server=prod-sql;Database=Shop;..."
export Jwt__ExpiryMinutes=30
export Shop__Admins__0=asha          # arrays use the index as a key
```

```yaml
# Kubernetes deployment
env:
  - name: ASPNETCORE_ENVIRONMENT
    value: Production
  - name: Jwt__Key
    valueFrom:
      secretKeyRef: { name: shop-secrets, key: jwt-key }
```

In Azure App Service, application settings become environment variables; you can type `Jwt:Key` there (Windows) or `Jwt__Key` (Linux).

⚠️ **Interview trap:** "Environment variables override everything." Almost — **command-line arguments** are higher. And providers you add *after* `CreateBuilder` (e.g. Key Vault) override env vars too.

🎯 **Say this in the interview:** "Configuration is a layered key-value store. By default the order is appsettings.json, then appsettings.{Environment}.json, then user secrets in Development, then environment variables, then command-line args — the last provider that sets a key wins, per key. In env vars I use double underscore instead of colon for nesting."

---

## 5.3 Reading configuration: indexer, `GetValue`, `GetSection`, `Bind`/`Get`

| API | Returns | Example | Notes |
|---|---|---|---|
| `config["Jwt:Issuer"]` | `string?` | `"ShopApi"` | Always string; `null` if missing |
| `config.GetValue<int>("Jwt:ExpiryMinutes", 60)` | Typed value | `60` | Converts; default if missing |
| `config.GetConnectionString("Shop")` | `string?` | | Shortcut for `ConnectionStrings:Shop` |
| `config.GetSection("Jwt")` | `IConfigurationSection` | | Sub-tree; never null (check `.Exists()`) |
| `section.Get<JwtOptions>()` | New `T` or `null` | | Binds a POCO |
| `section.Bind(existingObj)` | void | | Fills an existing object |
| `config.GetRequiredSection("Jwt")` | Section or **throws** | | Fail fast if missing (.NET 6+) |

The sample uses `Get<T>()` once in `Program.cs` because it needs the JWT values *while registering* authentication:

```csharp
JwtOptions jwt = builder.Configuration.GetSection(JwtOptions.SectionName).Get<JwtOptions>() ?? new JwtOptions();
```

Everywhere else, **don't inject `IConfiguration` and read magic strings** — use the Options pattern.

⚠️ **Interview trap:** Injecting `IConfiguration` into services and calling `config["Some:Key"]` everywhere. It works, but you get stringly-typed keys, no validation, no IntelliSense, and painful tests. Use strongly-typed options.

---

## 5.4 The Options pattern

### In simple words

Bind a configuration **section** to a **strongly-typed class** once, validate it, and inject it wherever needed as `IOptions<T>` (or its siblings).

**Analogy:** Instead of every employee rummaging through the filing cabinet (`IConfiguration`) for "the third sheet in folder J", HR hands each one a neat, checked **ID card** (`JwtOptions`) with exactly the fields they need.

### The sample app's options class and registration

```csharp
using System.ComponentModel.DataAnnotations;

namespace ShopApi.Options;

public class JwtOptions
{
    public const string SectionName = "Jwt";

    [Required] public string Issuer { get; set; } = "";
    [Required] public string Audience { get; set; } = "";

    // HS256 needs at least 32 bytes. In real apps keep this in Key Vault / user-secrets.
    [Required, MinLength(32)] public string Key { get; set; } = "";

    [Range(1, 1440)] public int ExpiryMinutes { get; set; } = 60;
}
```

```csharp
// Program.cs — Options pattern + validation at startup (app fails fast if config is wrong)
builder.Services.AddOptions<JwtOptions>()
    .Bind(builder.Configuration.GetSection(JwtOptions.SectionName))
    .ValidateDataAnnotations()
    .ValidateOnStart();
```

```csharp
// Consuming it (e.g. a token service)
public class TokenService(IOptions<JwtOptions> options)
{
    private readonly JwtOptions _jwt = options.Value;
    public DateTime Expiry() => DateTime.UtcNow.AddMinutes(_jwt.ExpiryMinutes);
}
```

Shorter alternative without validation: `builder.Services.Configure<JwtOptions>(builder.Configuration.GetSection("Jwt"));`.

Options classes rules: public parameterless constructor, public read-write properties (or `init` with .NET 8+ binder), and a `const string SectionName` is a nice convention.

### Validation: fail fast at startup (verified)

Without `ValidateOnStart`, validation happens **the first time someone reads `.Value`** — maybe hours after deployment, on a customer's request. With it, a broken config **stops the app from starting**, which deployment health checks catch immediately.

```csharp
using System.ComponentModel.DataAnnotations;
using Microsoft.Extensions.Options;

var builder = WebApplication.CreateBuilder(args);

// Simulate a BAD appsettings.json: key too short, audience missing
builder.Configuration.AddInMemoryCollection(new Dictionary<string, string?>
{
    ["Jwt:Issuer"] = "ShopApi",
    ["Jwt:Key"] = "too-short",
    ["Jwt:ExpiryMinutes"] = "600",
});

builder.Services.AddOptions<JwtOptions>()
    .Bind(builder.Configuration.GetSection(JwtOptions.SectionName))
    .ValidateDataAnnotations()                                   // [Required], [MinLength], [Range]...
    .Validate(o => o.ExpiryMinutes <= 120, "Tokens must not live longer than 2 hours")  // custom rule
    .ValidateOnStart();                                          // fail at startup, not on first use

var app = builder.Build();
app.MapGet("/", (IOptions<JwtOptions> jwt) => jwt.Value.Issuer);

try
{
    await app.StartAsync();       // app.Run() would crash the same way
}
catch (OptionsValidationException ex)
{
    foreach (string failure in ex.Failures)
        Console.WriteLine($"Config error: {failure}");
}

public class JwtOptions
{
    public const string SectionName = "Jwt";

    [Required] public string Issuer { get; set; } = "";
    [Required] public string Audience { get; set; } = "";
    [Required, MinLength(32)] public string Key { get; set; } = "";
    [Range(1, 1440)] public int ExpiryMinutes { get; set; } = 60;
}
```

Output (after the host's own "Hosting failed to start" error log):

```text
Config error: DataAnnotation validation failed for 'JwtOptions' members: 'Audience' with the error: 'The Audience field is required.'.
Config error: DataAnnotation validation failed for 'JwtOptions' members: 'Key' with the error: 'The field Key must be a string or array type with a minimum length of '32'.'.
Config error: Tokens must not live longer than 2 hours
```

All three problems are reported at once — Data Annotations **and** the custom `.Validate(...)` rule. For complex rules implement `IValidateOptions<T>`; for AOT/performance (.NET 8+) there's a source-generated validator: `[OptionsValidator] public partial class JwtOptionsValidator : IValidateOptions<JwtOptions>;`.

### `IOptions<T>` vs `IOptionsSnapshot<T>` vs `IOptionsMonitor<T>`

| | `IOptions<T>` | `IOptionsSnapshot<T>` | `IOptionsMonitor<T>` |
|---|---|---|---|
| DI lifetime | Singleton | **Scoped** | Singleton |
| Value computed | **Once** (first access) for the app's life | Once **per request** (scope) | Always current: `CurrentValue` |
| Sees changes in `appsettings.json` at runtime (reloadOnChange)? | ❌ No | ✅ On the **next request** | ✅ Immediately |
| Change notification | No | No | ✅ `OnChange(listener)` |
| Named options (`Get("name")`) | No | ✅ | ✅ |
| Inject into a singleton? | ✅ | ❌ (scoped — captive dependency!) | ✅ |
| Cost | Cheapest | Recomputed per request | Cached; recomputed on change |
| Use when | Settings that never change at runtime (most) | Per-request consistency + reload | Singletons/background services needing live values |

```csharp
using Microsoft.Extensions.Options;

var builder = WebApplication.CreateBuilder(args);
builder.Services.Configure<FeatureFlags>(builder.Configuration.GetSection("Features"));
builder.Services.AddSingleton<SingletonReader>();
var app = builder.Build();

// IOptions<T>        : singleton, value read ONCE, never changes
// IOptionsSnapshot<T>: scoped, re-read once PER REQUEST (can't inject into singletons)
// IOptionsMonitor<T> : singleton, always CurrentValue + OnChange callback
app.MapGet("/flags", (IOptions<FeatureFlags> o, IOptionsSnapshot<FeatureFlags> s, IOptionsMonitor<FeatureFlags> m) =>
    new { options = o.Value.NewCheckout, snapshot = s.Value.NewCheckout, monitor = m.CurrentValue.NewCheckout });

app.Run();

public class FeatureFlags
{
    public bool NewCheckout { get; set; }
}

public sealed class SingletonReader : IDisposable
{
    private readonly IOptionsMonitor<FeatureFlags> _monitor;
    private readonly IDisposable? _subscription;

    public SingletonReader(IOptionsMonitor<FeatureFlags> monitor, ILogger<SingletonReader> logger)
    {
        _monitor = monitor;
        _subscription = monitor.OnChange(f => logger.LogInformation("NewCheckout changed to {Value}", f.NewCheckout));
    }

    public bool IsNewCheckoutOn => _monitor.CurrentValue.NewCheckout;   // always fresh

    public void Dispose() => _subscription?.Dispose();
}
```

If you edit `"Features": { "NewCheckout": true }` in `appsettings.json` while the app runs (JSON files are loaded with `reloadOnChange: true` by default), `/flags` returns `options=false` (stuck at the startup value), `snapshot=true` and `monitor=true`, and `SingletonReader` logs the change.

**Named options** — the same class bound to different sections:

```csharp
builder.Services.Configure<SmtpOptions>("Marketing", builder.Configuration.GetSection("Smtp:Marketing"));
builder.Services.Configure<SmtpOptions>("Alerts",    builder.Configuration.GetSection("Smtp:Alerts"));

public class Mailer(IOptionsMonitor<SmtpOptions> smtp)
{
    public string AlertsHost => smtp.Get("Alerts").Host;
}
```

⚠️ **Interview trap:** Injecting `IOptionsSnapshot<T>` into a singleton (or a `BackgroundService`). It's scoped → captive dependency, and scope validation throws in Development. Use `IOptionsMonitor<T>`.

⚠️ **Interview trap:** Caching `options.Value` in a field of a **scoped** service and expecting reloads — fine with `IOptionsSnapshot` (new per request), but with `IOptions<T>` the value never changes no matter what.

🎯 **Say this in the interview:** "`IOptions<T>` is a singleton read once, so it never sees changes. `IOptionsSnapshot<T>` is scoped and recomputed per request, so it picks up reloads on the next request but can't be used in singletons. `IOptionsMonitor<T>` is a singleton that always returns the current value and supports change callbacks. I bind with `AddOptions<T>().Bind(...).ValidateDataAnnotations().ValidateOnStart()` so bad config fails at startup."

---

## 5.5 Secrets: never commit them

### In simple words

Connection strings with passwords, API keys, JWT signing keys and client secrets must **never** be in Git — not even in a private repo (history is forever, laptops get stolen, repos get forked).

| Environment | Where secrets live |
|---|---|
| Local development | **User Secrets** (`dotnet user-secrets`) |
| CI/CD | Pipeline secret variables / secure files |
| Production (Azure) | **Azure Key Vault** (+ **Managed Identity** so there's no secret to access the secrets), or App Service settings |
| Kubernetes | Kubernetes Secrets (ideally synced from Key Vault via CSI driver) |
| Docker | Environment variables / Docker secrets |

### User Secrets (Development only)

```bash
dotnet user-secrets init                                    # adds <UserSecretsId> to the .csproj
dotnet user-secrets set "Jwt:Key" "a-much-longer-local-development-signing-key-123"
dotnet user-secrets set "ConnectionStrings:Shop" "Server=localhost;Database=Shop;User Id=sa;Password=..."
dotnet user-secrets list
```

- Stored **outside** the project folder: `%APPDATA%\Microsoft\UserSecrets\<id>\secrets.json` (Windows) or `~/.microsoft/usersecrets/<id>/secrets.json` (Linux/macOS) — so they can't be committed by accident.
- **Not encrypted** — it's a convenience for dev, not a vault.
- Loaded automatically by `CreateBuilder` **only when the environment is Development**.

### Azure Key Vault (production)

```csharp
// dotnet add package Azure.Extensions.AspNetCore.Configuration.Secrets
// dotnet add package Azure.Identity
if (builder.Environment.IsProduction())
{
    builder.Configuration.AddAzureKeyVault(
        new Uri($"https://{builder.Configuration["KeyVaultName"]}.vault.azure.net/"),
        new DefaultAzureCredential());      // Managed Identity in Azure, your dev login locally
}
```

Key Vault secret names can't contain `:`, so use `--`: a secret named `Jwt--Key` maps to `Jwt:Key`. Because it's added last, Key Vault values override everything else.

⚠️ **Interview trap:** "I put secrets in `appsettings.Production.json` and don't commit that file." Fragile — one `git add .` and it's leaked; and the file sits in plain text on every server. Use env vars/Key Vault, and add secret scanning to CI.

🎯 **Say this in the interview:** "Secrets never go in source control. Locally I use user-secrets, which live outside the repo and only load in Development. In production I use Azure Key Vault with Managed Identity — or environment variables / Kubernetes secrets — and because those providers load later, they override the JSON files."

---

## 5.6 Logging with `ILogger<T>`

### In simple words

ASP.NET Core has a built-in logging abstraction: you write to `ILogger<T>`; **providers** decide where it goes (console, debug window, Event Log, Application Insights, Seq, Elasticsearch, files via Serilog...). `T` becomes the **category** (the full class name), used for filtering.

```csharp
public class ProductService(AppDbContext db, ILogger<ProductService> logger) : IProductService
{
    // ...
    logger.LogInformation("Created product {ProductId} {ProductName}", product.Id, product.Name);
}
```

That's a real line from the sample's `ProductService.CreateAsync`.

### Log levels

| Level | Value | Use for | Example |
|---|---|---|---|
| `Trace` | 0 | Very detailed, may contain sensitive data; never in prod | Method entry/exit, payloads |
| `Debug` | 1 | Developer diagnostics | Cache hit/miss, computed values |
| `Information` | 2 | Normal app flow / business events | "Order 42 placed", request completed |
| `Warning` | 3 | Unexpected but handled; app continues | Retry succeeded, rejected API key, slow query |
| `Error` | 4 | Operation failed (current request) | Exception while saving an order |
| `Critical` | 5 | App/system-wide failure; needs immediate attention | DB unreachable, disk full, startup failure |
| `None` | 6 | Disable logging for a category | |

Configured per category in `appsettings.json` (the most specific prefix wins):

```json
"Logging": {
  "LogLevel": {
    "Default": "Information",
    "Microsoft.AspNetCore": "Warning",
    "Microsoft.EntityFrameworkCore.Database.Command": "Warning"
  }
}
```

The sample sets EF Core's SQL command logging to `Warning`; change it to `Information` locally to **see the SQL EF generates** — a great debugging trick (and interview talking point). The minimum level is inclusive: `Warning` logs Warning, Error and Critical.

---

## 5.7 Structured logging: message templates, NOT string interpolation

### In simple words

A **message template** has **named placeholders**: `"Order {OrderId} placed for {Total}"`. The logger keeps the **template** *and* each **value as a separate property**. Log stores (App Insights, Seq, Elastic, Splunk) index those properties, so you can query `OrderId = 42` or `Total > 1000` instead of grepping text.

**Analogy:** Interpolation gives the librarian a *photo of a page*. A template gives them a *filled-in form* — they can file it, sort it and search by any field.

### Verified example: what a structured log really contains

```csharp
using System.Text.Json;

using ILoggerFactory factory = LoggerFactory.Create(b => b
    .AddJsonConsole(o =>
    {
        o.IncludeScopes = true;
        o.JsonWriterOptions = new JsonWriterOptions { Indented = true };
    }));

ILogger logger = factory.CreateLogger("Checkout");

int orderId = 42;
decimal total = 1499.50m;

using (logger.BeginScope("CorrelationId {CorrelationId}", "abc-123"))
{
    // ✅ Message template: OrderId and Total become SEPARATE searchable fields
    logger.LogInformation("Order {OrderId} placed for {Total}", orderId, total);
}
```

Output:

```json
{
  "EventId": 0,
  "LogLevel": "Information",
  "Category": "Checkout",
  "Message": "Order 42 placed for 1499.50",
  "State": {
    "OrderId": 42,
    "Total": 1499.50,
    "{OriginalFormat}": "Order {OrderId} placed for {Total}"
  },
  "Scopes": [
    {
      "Message": "CorrelationId abc-123",
      "CorrelationId": "abc-123",
      "{OriginalFormat}": "CorrelationId {CorrelationId}"
    }
  ]
}
```

`OrderId` and `Total` are real, typed fields. `{OriginalFormat}` is identical for every "order placed" event, so you can group/count them.

### Why NOT string interpolation

```csharp
logger.LogInformation($"Order {orderId} placed for {total}");      // ❌
logger.LogInformation("Order {OrderId} placed for {Total}", orderId, total);   // ✅
```

| | Interpolation `$"..."` | Message template |
|---|---|---|
| Stored as | One flat string: `"Order 42 placed for 1499.50"` | Template + properties `OrderId=42`, `Total=1499.50` |
| Search by field | ❌ text search only | ✅ `OrderId == 42` |
| Group "same kind" of events | ❌ every message is a different string | ✅ same `{OriginalFormat}` |
| Cost when level is disabled | ❌ String is **always built** (allocations) even if Debug is off | ✅ Formatting is skipped if the level is disabled |
| Analyzer | CA2254 warns: "template should be a static expression" | ✅ |

Placeholders are matched **by position**, not by name — `"{OrderId} {Total}"` takes the first and second arguments. Use PascalCase names, and keep them consistent across the codebase (`{OrderId}` everywhere, not `{id}` here and `{orderId}` there). Use `{@Order}` (Serilog) to destructure an object, and never log secrets or full personal data.

Logging exceptions: pass the exception **as the first argument**, not in the message:

```csharp
catch (Exception ex)
{
    logger.LogError(ex, "Failed to place order {OrderId}", orderId);   // ✅ full stack trace captured
}
```

⚠️ **Interview trap:** "Interpolation and templates produce the same log line, so they're equivalent." The *rendered text* is the same — but interpolation throws away the structure and always pays the formatting cost.

🎯 **Say this in the interview:** "I always use message templates with named placeholders instead of string interpolation. The logger keeps each value as a separate property, so log tools can filter and aggregate on `OrderId`, all events share the same template, and if the level is disabled no string is built. Interpolation flattens everything into text and allocates every time."

---

## 5.8 Log scopes

A **scope** attaches properties to **every** log written inside a block — perfect for correlation IDs, order IDs, tenant IDs. You saw `Scopes` in the JSON above. The sample app's correlation-ID middleware opens a scope for the whole request:

```csharp
app.Use(async (context, next) =>
{
    string id = context.Request.Headers["X-Correlation-Id"].FirstOrDefault() ?? Guid.NewGuid().ToString();
    context.Response.Headers["X-Correlation-Id"] = id;
    using (app.Logger.BeginScope(new Dictionary<string, object> { ["CorrelationId"] = id }))
    {
        await next(context);
    }
});
```

Every log from controllers, `ProductService`, EF Core etc. during that request now carries `CorrelationId` — so you can pull up *all* logs for one failing request. (The console provider only prints scopes with `IncludeScopes = true`; structured sinks store them as properties. ASP.NET Core also adds its own scope with `RequestId`, `RequestPath`, `SpanId`/`TraceId`.)

---

## 5.9 Serilog (and other third-party loggers)

The built-in providers are fine for console/Debug/App Insights. Most teams add **Serilog** for rich sinks (files with rolling, Seq, Elasticsearch, Splunk), enrichers (machine name, thread, user) and config-driven setup:

```csharp
// dotnet add package Serilog.AspNetCore
builder.Host.UseSerilog((ctx, lc) => lc
    .ReadFrom.Configuration(ctx.Configuration)     // levels/sinks from "Serilog" section
    .Enrich.FromLogContext()
    .WriteTo.Console()
    .WriteTo.File("logs/shop-.log", rollingInterval: RollingInterval.Day));

var app = builder.Build();
app.UseSerilogRequestLogging();   // one tidy log line per request instead of many
```

Your code still uses `ILogger<T>` — Serilog plugs in behind the abstraction, so you can swap it without touching services. Other options: NLog, and **OpenTelemetry** (`builder.Logging.AddOpenTelemetry()`) for logs + traces + metrics to any OTLP backend.

---

## 5.10 High-performance logging: the `LoggerMessage` source generator (.NET 6+)

### Why

`logger.LogInformation("...{Id}", id)` is convenient but each call: parses/looks up the template, allocates a `params object[]`, and **boxes** value types (`int`, `decimal`). On hot paths (thousands of logs/second) that's measurable GC pressure.

The `[LoggerMessage]` attribute makes the compiler **generate** a strongly-typed, pre-parsed logging method: no boxing, no array, and an `IsEnabled` check built in.

### Verified example

```csharp
using ILoggerFactory factory = LoggerFactory.Create(b => b.AddSimpleConsole(o => o.SingleLine = true));
var logger = factory.CreateLogger<PaymentService>();
var service = new PaymentService(logger);
service.Charge(42, 1499.50m);
service.Fail(43, new TimeoutException("Gateway timed out"));

public partial class PaymentService(ILogger<PaymentService> logger)
{
    public void Charge(int orderId, decimal amount)
    {
        LogCharging(orderId, amount);      // strongly-typed, no boxing, no params array
    }

    public void Fail(int orderId, Exception ex) => LogChargeFailed(ex, orderId);

    // The source generator writes the body of these methods at compile time (.NET 6+)
    [LoggerMessage(EventId = 1001, Level = LogLevel.Information, Message = "Charging order {OrderId} amount {Amount}")]
    private partial void LogCharging(int orderId, decimal amount);

    [LoggerMessage(EventId = 1002, Level = LogLevel.Error, Message = "Charge failed for order {OrderId}")]
    private partial void LogChargeFailed(Exception ex, int orderId);
}
```

Output:

```text
info: PaymentService[1001] Charging order 42 amount 1499.50
fail: PaymentService[1002] Charge failed for order 43 System.TimeoutException: Gateway timed out
```

Notes:

- The class must be `partial`; the methods are `partial` with no body. With a primary constructor, the generator finds the `ILogger` parameter automatically (.NET 8+); otherwise use a static method with `ILogger` as the first parameter.
- An `Exception` parameter is treated as the exception, not a placeholder.
- **Event IDs** make it easy to alert on a specific event ("page me when EventId 1002 spikes").
- Bonus: placeholder/parameter mismatches become **compile-time errors**.

| | `LogInformation(...)` extension | `[LoggerMessage]` source generator |
|---|---|---|
| Boxing of value types | Yes | No |
| `params object[]` allocation | Yes | No |
| Template parsing | Cached at runtime | At compile time |
| Template/argument mismatch | Runtime bug | Compile error |
| Effort | Lowest | A few extra lines |
| Use for | Most code | Hot paths, libraries, high-volume services |

🎯 **Say this in the interview:** "For hot paths I use the `LoggerMessage` source generator: I declare a partial method with the template and level, and the compiler generates a strongly-typed implementation that avoids boxing, array allocations and runtime parsing, and checks placeholders at compile time."

---

## Try it yourself

**1.** `appsettings.json` has `"Jwt": { "ExpiryMinutes": 60 }`, `appsettings.Production.json` has `45`, the container sets env var `Jwt__ExpiryMinutes=30`, and the app is started with `--Jwt:ExpiryMinutes=15`. Environment is Production. What value does the app see? What if you remove the command-line argument?

<details><summary>Answer</summary>

`15` — command-line arguments have the highest priority among the default providers. Without it, `30` from the environment variable (env vars beat both JSON files). The double underscore `__` maps to `:`.

</details>

**2.** A `BackgroundService` needs a feature flag that ops can flip in `appsettings.json` without restarting. Which options interface do you inject and why not the others?

<details><summary>Answer</summary>

`IOptionsMonitor<T>` — it's a singleton (safe to inject into a hosted service, which is a singleton) and `CurrentValue` always reflects the reloaded file; you can also subscribe with `OnChange`. `IOptions<T>` never changes after first read. `IOptionsSnapshot<T>` is scoped → captive dependency in a singleton (throws in Development).

</details>

**3.** Rewrite these log lines correctly and explain the benefit:

```csharp
_logger.LogInformation("User " + userId + " logged in from " + ip);
_logger.LogError($"Payment failed: {ex.Message}");
```

<details><summary>Answer</summary>

```csharp
_logger.LogInformation("User {UserId} logged in from {IpAddress}", userId, ip);
_logger.LogError(ex, "Payment failed for order {OrderId}", orderId);
```

The first keeps `UserId` and `IpAddress` as searchable properties and avoids building a string when Information is disabled. The second passes the exception object so the full stack trace and exception type are logged (not just the message), and adds useful context.

</details>

**4.** Someone pushed `"Jwt": { "Key": "" }` to production config. With the sample's registration, what happens, and why is that good?

<details><summary>Answer</summary>

The sample uses `.ValidateDataAnnotations().ValidateOnStart()`, and `Key` has `[Required, MinLength(32)]`. The host throws `OptionsValidationException` during startup, so the app never starts serving traffic. That's good: the deployment/health check fails immediately with a clear message ("The Key field is required"), instead of every login failing later at runtime.

</details>

**5.** Where should a developer put the production SQL password for local testing, and where should production read it from? Name the commands/services.

<details><summary>Answer</summary>

Locally: user secrets — `dotnet user-secrets init` then `dotnet user-secrets set "ConnectionStrings:Shop" "..."`; stored outside the repo and loaded only in Development. (Better still: don't use the *production* password locally at all.) Production: Azure Key Vault (`builder.Configuration.AddAzureKeyVault(uri, new DefaultAzureCredential())` with Managed Identity; secret named `ConnectionStrings--Shop`), or environment variables / Kubernetes secrets set by the platform.

</details>

**6.** You need every log line of a request to include the tenant ID from the `X-Tenant` header. How?

<details><summary>Answer</summary>

Write a middleware that opens a logging scope around `next`:

```csharp
app.Use(async (context, next) =>
{
    string tenant = context.Request.Headers["X-Tenant"].FirstOrDefault() ?? "unknown";
    using (app.Logger.BeginScope(new Dictionary<string, object> { ["TenantId"] = tenant }))
    {
        await next(context);
    }
});
```

All logs written during the request (by any `ILogger<T>`) carry `TenantId`. Enable `IncludeScopes` for the console, or use a structured sink (App Insights, Seq, Serilog with `Enrich.FromLogContext()`).

</details>

---

## Quick revision

- Configuration = layered key-value providers; keys use `:`; later providers win **per key**.
- Default order: `appsettings.json` → `appsettings.{Env}.json` → user secrets (Dev only) → env vars → command line.
- In env vars use `__` for `:` (`ConnectionStrings__Shop`); Key Vault uses `--`.
- `GetValue<T>`, `GetSection`, `Get<T>`/`Bind`, `GetConnectionString` — but prefer the Options pattern over raw `IConfiguration`.
- `AddOptions<T>().Bind(...).ValidateDataAnnotations().ValidateOnStart()` = strongly typed + fail fast at startup.
- `IOptions` = singleton, never changes; `IOptionsSnapshot` = scoped, per-request reload; `IOptionsMonitor` = singleton, live `CurrentValue` + `OnChange`.
- Never inject `IOptionsSnapshot` into singletons.
- Never commit secrets: user-secrets locally (not encrypted, Dev only), Key Vault + Managed Identity / env vars in production.
- `ILogger<T>`: category = `T`; levels Trace < Debug < Information < Warning < Error < Critical; filter per category in config.
- Use message templates (`"{OrderId}"`), not `$"..."` — structured properties, grouping, no cost when disabled.
- Pass exceptions as the first argument: `LogError(ex, "...")`; use `BeginScope` for correlation/tenant IDs.
- `[LoggerMessage]` source generator = zero-boxing, compile-time-checked logging for hot paths; Serilog/OpenTelemetry plug in behind `ILogger`.

---

# 6. Web API: Controllers, Routing, Model Binding, Validation

> **Why interviewers ask this**
> Almost every .NET backend job is "build and maintain REST APIs". In 10 minutes of questions the interviewer can see if you *really* built APIs: do you pick the right **status code**, do you know **where a parameter comes from**, do you **validate** input, do you avoid exposing **entities**, and do you know what `[ApiController]` silently does for you?

---

## 6.1 REST in simple words

**Simple words:** REST is a *style* of designing web APIs where everything is a **resource** (a "thing": product, order, customer), each resource has an **address** (URL), and you use standard **HTTP verbs** to say *what you want to do* with it.

**Analogy — a library:**
- The **books** are the resources.
- Each book has a **shelf number** (the URL): `/books/42`.
- You don't invent new words to talk to the librarian. You use a small, fixed set of verbs: *show me* (GET), *add this* (POST), *replace this* (PUT), *fix one page* (PATCH), *throw away* (DELETE).

### Resources and URLs

| Good (nouns, plural, hierarchy) | Bad (verbs in URL, RPC style) |
|---|---|
| `GET /api/products` | `GET /api/getAllProducts` |
| `GET /api/products/5` | `GET /api/product?id=5&action=get` |
| `POST /api/products` | `POST /api/createProduct` |
| `GET /api/customers/7/orders` | `GET /api/getOrdersOfCustomer/7` |
| `POST /api/orders/12/cancel` *(action that is not CRUD — acceptable)* | `GET /api/cancelOrder/12` *(GET must not change data!)* |

### HTTP verbs, safety and idempotency

- **Safe** = the request does **not change** server data (read-only).
- **Idempotent** = sending the **same request 1 time or 10 times** leaves the server in the **same state**.

| Verb | Typical use | Safe? | Idempotent? | Typical success code | Has body? |
|---|---|---|---|---|---|
| GET | Read a resource / list | ✅ | ✅ | 200 | No |
| HEAD | Like GET but headers only | ✅ | ✅ | 200 | No |
| OPTIONS | What can I do here? (CORS preflight) | ✅ | ✅ | 204/200 | No |
| POST | Create / run a command | ❌ | ❌ | 201 (create), 200/202 | Yes |
| PUT | **Replace** whole resource | ❌ | ✅ | 204 or 200 | Yes |
| PATCH | **Partially** update | ❌ | ❌ *(not guaranteed)* | 204 or 200 | Yes |
| DELETE | Remove | ❌ | ✅ | 204 | Usually no |

**Easy example of idempotency:**
- `PUT /api/products/5 { "price": 100 }` 10 times → price is 100. Same state. ✅ idempotent.
- `POST /api/orders { ... }` 10 times → **10 orders**. ❌ not idempotent.
- `DELETE /api/products/5` 10 times → product is gone (first call 204, later calls 404 — the *response* differs but the *server state* is the same). ✅ idempotent.

**Complex/real-world:** a mobile app on a bad network sends "Pay ₹500" (POST), times out, and **retries**. Without protection the customer pays twice. The fix is an **Idempotency-Key** header: the server remembers the key and returns the first response again. We build exactly this as a resource filter in [Chapter 8](08-filters.md).

⚠️ **Interview trap:** "DELETE is not idempotent because the second call returns 404." — Wrong. Idempotency is about the **server state**, not the response code.

⚠️ **Interview trap:** "PATCH is idempotent." — Not guaranteed. `PATCH { "op": "increment stock by 1" }` gives a different result each time.

🎯 **Say this in the interview:** "Safe means read-only, idempotent means repeating the request gives the same server state. GET, PUT and DELETE are idempotent; POST is not, so for payments I add an Idempotency-Key so client retries don't create duplicates."

---

## 6.2 Status codes you must know

**Simple words:** the status code is the **one-number summary** of what happened. `2xx` = OK, `4xx` = **client** did something wrong, `5xx` = **server** did something wrong.

| Code | Name | When to return it | ASP.NET Core helper |
|---|---|---|---|
| **200** | OK | Successful GET, or PUT/POST that returns data | `Ok(dto)` |
| **201** | Created | POST created a resource. Must include **`Location`** header | `CreatedAtAction(...)`, `CreatedAtRoute(...)`, `Created(uri, dto)` |
| **204** | No Content | Success, nothing to return (PUT/DELETE) | `NoContent()` |
| **400** | Bad Request | Malformed JSON, failed validation | `BadRequest()`, `ValidationProblem()` (automatic with `[ApiController]`) |
| **401** | Unauthorized | **Not authenticated** — no/invalid/expired token. "Who are you?" | `Unauthorized()` / `[Authorize]` |
| **403** | Forbidden | **Authenticated but not allowed**. "I know you, but no." | `Forbid()` / policy failure |
| **404** | Not Found | Resource with that id doesn't exist | `NotFound()` |
| **409** | Conflict | State conflict: duplicate email, concurrency conflict, order already shipped | `Conflict()` |
| **422** | Unprocessable Entity | JSON is valid but **business rule** fails (some teams use 400 for everything) | `UnprocessableEntity()` |
| **429** | Too Many Requests | Rate limit hit (see `AddRateLimiter` in the sample) | rate limiter middleware |
| **500** | Internal Server Error | Unhandled exception — **a bug on our side** | exception handler (Chapter 11) |

Others worth knowing: **202 Accepted** (job queued, check later), **304 Not Modified** (caching/ETag), **405 Method Not Allowed**, **406 Not Acceptable** (content negotiation), **415 Unsupported Media Type** (wrong `Content-Type`), **503 Service Unavailable**.

⚠️ **Interview trap — 401 vs 403:** 401 means "I don't know who you are" (log in again). 403 means "I know who you are, but you are not allowed". A normal user calling an admin-only endpoint with a valid token gets **403**, not 401.

⚠️ **Interview trap — returning 200 with `{ "success": false }`.** Clients, proxies, monitoring and retries all rely on the status code. Errors must use 4xx/5xx.

⚠️ **Interview trap — returning 500 for validation errors.** If the client sent bad data, it's a **4xx**. 500 means *we* have a bug.

🎯 **Say this in the interview:** "I return 201 with a Location header for creates, 204 for updates and deletes, 400 for validation, 401 vs 403 for authentication vs authorization, 404 for missing resources, 409 for conflicts like duplicates or concurrency, and 500 only for unexpected bugs — all errors as ProblemDetails."

---

## 6.3 `ControllerBase` vs `Controller`

**Simple words:** `Controller` = `ControllerBase` + **view** support (Razor pages/HTML). For a JSON API you don't need views.

| | `ControllerBase` | `Controller` |
|---|---|---|
| Namespace | `Microsoft.AspNetCore.Mvc` | same |
| Used for | **Web APIs** (JSON) | MVC apps (HTML views) **and** APIs |
| Helpers | `Ok`, `NotFound`, `CreatedAtAction`, `Problem`, `ValidationProblem`... | everything in ControllerBase **+** `View()`, `PartialView()`, `ViewBag`, `ViewData`, `TempData`, `Json()` |
| Registration | `AddControllers()` | `AddControllersWithViews()` |

**Easy example** (`samples/ShopApi` style):

```csharp
[ApiController]
[Route("api/[controller]")]                 // [controller] = "Hello" (class name minus "Controller")
public class HelloController : ControllerBase
{
    // GET api/hello
    [HttpGet]
    public string Get() => "Hello from a controller!";

    // GET api/hello/Abhi   ->  {"message":"Hello, Abhi"}
    [HttpGet("{name}")]
    public IActionResult Greet(string name) => Ok(new { message = $"Hello, {name}" });
}
```

And in `Program.cs`:

```csharp
builder.Services.AddControllers();   // registers MVC services for APIs (no views)
// ...
app.MapControllers();                // maps attribute-routed controllers as endpoints
```

🎯 **Say this in the interview:** "For APIs I inherit from ControllerBase because Controller only adds view support like View() and ViewBag, which a JSON API never uses."

---

## 6.4 What `[ApiController]` does for you

**Simple words:** `[ApiController]` is a "**smart defaults**" switch. Put it on a controller (or the whole assembly) and ASP.NET Core adds API-friendly behaviour automatically.

| Behaviour | Without `[ApiController]` | With `[ApiController]` |
|---|---|---|
| **Automatic 400** | Action runs even if model is invalid. You must check `ModelState.IsValid` yourself | Invalid model → **400 ValidationProblemDetails** returned automatically, action never runs |
| **Binding source inference** | Complex types bind from **form/query** by default; you need `[FromBody]` | Complex types → **body**, simple types → route/query, `IFormFile` → form, registered services → DI |
| **Attribute routing** | Optional (conventional routing allowed) | **Required** — conventional routes don't reach it |
| **ProblemDetails for errors** | `NotFound()` returns an empty 404 | Error status codes (≥ 400) from `NotFound()`, `BadRequest()` etc. get an **RFC 7807 ProblemDetails** body |
| **Multipart/form-data inference** | — | `IFormFile` parameters are inferred as `[FromForm]` |

**Easy example — the automatic 400 (actual output from our verification run):**

```csharp
public record PingRequest([Required] string Name);

[ApiController]
[Route("api/ping")]
public class PingController : ControllerBase
{
    [HttpPost]
    public IActionResult Post(PingRequest r) => Ok(r);   // never reached if Name is missing
}
```

```http
POST /api/ping
Content-Type: application/json

{}
```

```http
HTTP/1.1 400 Bad Request
Content-Type: application/problem+json; charset=utf-8

{
  "type": "https://tools.ietf.org/html/rfc9110#section-15.5.1",
  "title": "One or more validation errors occurred.",
  "status": 400,
  "errors": { "Name": ["The Name field is required."] },
  "traceId": "00-81b884ec9eeb0c3eb0ac2c02f0a64be3-a8a52f26cf87cef7-00"
}
```

You can turn the automatic 400 off (rarely needed), or customise its shape:

```csharp
builder.Services.AddControllers()
    .ConfigureApiBehaviorOptions(o =>
    {
        // o.SuppressModelStateInvalidFilter = true;   // disable automatic 400
        o.InvalidModelStateResponseFactory = ctx =>
            new UnprocessableEntityObjectResult(new ValidationProblemDetails(ctx.ModelState)); // 422 instead
    });
```

You can also apply it to **every controller in the project** with one line (any .cs file):

```csharp
[assembly: Microsoft.AspNetCore.Mvc.ApiController]
```

⚠️ **Interview trap:** "I always write `if (!ModelState.IsValid) return BadRequest(ModelState);` in every action." — With `[ApiController]` that line is dead code. Say you know `[ApiController]` does it, and mention `SuppressModelStateInvalidFilter` if you need custom handling.

🎯 **Say this in the interview:** "`[ApiController]` gives automatic 400 ValidationProblem responses, infers binding sources so complex types come from the body, forces attribute routing, and returns ProblemDetails for error status codes."

---

## 6.5 Routing — how a URL finds your action

**Simple words:** routing is the **post office** of your app. It reads the address on the envelope (`GET /api/products/5`) and delivers it to exactly one method (`ProductsController.GetById(5)`).

### Conventional vs attribute routing

| | Conventional routing | Attribute routing |
|---|---|---|
| Where defined | One central pattern in `Program.cs` | On each controller/action with `[Route]`, `[HttpGet("...")]` |
| Example | `{controller=Home}/{action=Index}/{id?}` | `[Route("api/products")]` + `[HttpGet("{id:int}")]` |
| Used by | MVC websites (views) | **Web APIs** (required with `[ApiController]`) |
| Pros | Less typing, consistent URLs | Full control, REST-friendly URLs, visible next to the code |

```csharp
// Conventional (MVC apps)
app.MapControllerRoute(
    name: "default",
    pattern: "{controller=Home}/{action=Index}/{id?}");
// /Products/Details/5  ->  ProductsController.Details(5)

// Attribute (APIs)
app.MapControllers();
```

### Route templates, tokens and constraints

```csharp
[ApiController]
[Route("api/[controller]")]          // token: [controller] -> "orders"  (also [action], [area])
public class OrdersController : ControllerBase
{
    [HttpGet]                                    // GET api/orders
    public IActionResult List() => Ok();

    [HttpGet("{id:int:min(1)}")]                 // GET api/orders/5   (0 or "abc" -> 404)
    public IActionResult ById(int id) => Ok($"order #{id}");

    [HttpGet("{orderNumber:regex(^ORD-\\d{{5}}$)}")]   // GET api/orders/ORD-12345
    public IActionResult ByNumber(string orderNumber) => Ok($"order {orderNumber}");

    [HttpGet("{year:int:length(4)}/{month:range(1,12)}")]  // GET api/orders/2025/03
    public IActionResult ByMonth(int year, int month) => Ok($"{year}-{month:D2}");

    [HttpGet("~/api/latest-order")]              // "~/" ignores the controller prefix
    public IActionResult Latest() => Ok("latest");
}
```

(Verified: `/api/orders/ORD-12345` → 200, `/api/orders/0` → 404 because of `min(1)`.)

| Constraint | Example | Matches |
|---|---|---|
| `int`, `long`, `guid`, `bool`, `datetime`, `decimal` | `{id:int}` | type check |
| `min(n)`, `max(n)`, `range(a,b)` | `{page:min(1)}` | numeric range |
| `length(n)`, `minlength(n)`, `maxlength(n)` | `{code:length(5)}` | string length |
| `alpha` | `{name:alpha}` | letters only |
| `regex(...)` | `{sku:regex(^[A-Z]{{3}}$)}` | pattern (escape `{` `}` as `{{` `}}`) |
| `?` optional, `=x` default, `*` catch-all | `{id?}`, `{page=1}`, `{*path}` | |

⚠️ **Interview trap:** "Route constraints are for validation." — No! A failed constraint returns **404** (route not matched), not 400. Use constraints to **disambiguate routes** (`{id:int}` vs `{slug}`), and use **validation** for input rules.

⚠️ **Interview trap — ambiguous routes:** two actions with `[HttpGet("{id}")]` and `[HttpGet("{name}")]` → `AmbiguousMatchException` at runtime. Fix with constraints (`{id:int}`) or different templates.

**Route names & link generation:** `[HttpGet("{id:int}", Name = "GetProductById")]` gives the route a name you can use in `CreatedAtRoute` or `Url.Link(...)`.

🎯 **Say this in the interview:** "APIs use attribute routing — `[ApiController]` requires it. I use `[Route("api/[controller]")]` on the class, verb attributes with templates on actions, and constraints like `{id:int}` to disambiguate routes; a failed constraint gives 404, so real validation stays in the model."

---

## 6.6 Model binding — where do parameters come from?

**Simple words:** model binding is the **form-filler**. It reads pieces of the HTTP request (URL, query string, headers, body, form) and fills your method parameters, converting text to `int`, `DateTime`, objects, etc.

**Analogy:** a courier receives a parcel. The address is on the **label** (route), a few notes on **sticky notes** (query string), the delivery instructions on the **envelope** (headers), and the real stuff is **inside the box** (body). Model binding knows where to look for each.

| Attribute | Reads from | Example request part | Typical type |
|---|---|---|---|
| `[FromRoute]` | URL path segment | `/api/products/**5**` | `int id` |
| `[FromQuery]` | Query string | `?**page=2&pageSize=10**` | simple types, or a class of filters |
| `[FromBody]` | Request body (JSON) | `{ "name": "Pen" }` | DTO / record |
| `[FromHeader]` | HTTP header | `X-Correlation-Id: abc` | `string` |
| `[FromForm]` | Form fields (`multipart/form-data` / urlencoded) | file uploads, HTML forms | `IFormFile`, `string` |
| `[FromServices]` | **DI container**, not the request | — | any registered service |
| *(special)* | `CancellationToken`, `HttpContext`, `IFormFile`, `ClaimsPrincipal` | — | bound automatically |

### Inference rules with `[ApiController]` (no attribute written)

1. **Complex type** (class/record) → `[FromBody]`
2. Name matches a **route parameter** → `[FromRoute]`
3. `IFormFile` / `IFormFileCollection` → `[FromForm]`
4. Type **registered in DI** → `[FromServices]` (.NET 7+)
5. Anything else (simple types) → `[FromQuery]`

### Easy example — every source in one action (verified)

```csharp
public record AddNoteRequest(string Text);

[ApiController]
[Route("api/customers/{customerId:int}/notes")]
public class NotesController : ControllerBase
{
    [HttpPost]
    public IActionResult Add(
        [FromRoute] int customerId,                                        // /customers/5/notes
        [FromQuery] bool notify,                                           // ?notify=true
        [FromHeader(Name = "X-Correlation-Id")] string? correlationId,     // header
        [FromBody] AddNoteRequest request,                                 // JSON body
        [FromServices] IClock clock)                                       // DI
    {
        return Ok(new { customerId, notify, correlationId, request.Text, at = clock.UtcNow });
    }
}
```

```http
POST /api/customers/5/notes?notify=true
X-Correlation-Id: abc
Content-Type: application/json

{ "text": "hi" }
```
→ `{"customerId":5,"notify":true,"correlationId":"abc","text":"hi","at":"2026-11-25T10:45:54.98Z"}`

### Rules people forget

- **Only ONE `[FromBody]` parameter per action.** The body is a stream that can be read only once. Need two objects? Wrap them in one DTO.
- **Complex type from the query string needs `[FromQuery]`** explicitly (otherwise `[ApiController]` assumes body): `Get([FromQuery] ProductQuery q)`.
- `[FromBody]` JSON property names are **case-insensitive** by default (`name` = `Name`).
- Missing/invalid JSON → automatic 400 (the `errors` contains a `"$"` key with the JSON parse error).
- **Unknown JSON properties are silently ignored** (important for overposting — see 6.11).
- Wrong `Content-Type` (e.g. `text/plain` for JSON) → **415 Unsupported Media Type**.

⚠️ **Interview trap:** "I can put `[FromBody]` on two parameters." — No, only one. Make a wrapper DTO.

⚠️ **Interview trap:** "A GET can have a body." — Technically HTTP allows it, but proxies/caches/browsers may drop it and ASP.NET Core doesn't bind it by default. Use query parameters for GET.

🎯 **Say this in the interview:** "Model binding fills action parameters from route, query, headers, body, form or DI. With `[ApiController]` complex types come from the body and simple types from route or query. Only one parameter can be `[FromBody]` because the body stream is read once."

---

## 6.7 Validation

**Simple words:** validation is the **security guard at the door** — check the input *before* it reaches your business logic.

**Layers of validation (from cheap to expensive):**

| Layer | Example | Tool |
|---|---|---|
| Shape/format | Required, length, range, email format | **DataAnnotations** |
| Rule across fields | `ValidTo > ValidFrom` | **IValidatableObject** or FluentValidation |
| Reusable custom rule | "date must be in the future" | **custom `ValidationAttribute`** |
| Needs the database | "email already registered", "category exists" | service layer → 409/400 |

### Easy example — DataAnnotations (from the sample app `Dtos/ProductDtos.cs`)

```csharp
public record CreateProductRequest(
    [Required, StringLength(200, MinimumLength = 2)] string Name,
    [Range(0.01, 1_000_000)] decimal Price,
    [Range(0, int.MaxValue)] int Stock,
    [Range(1, int.MaxValue)] int CategoryId);
```

Common attributes:

| Attribute | Purpose |
|---|---|
| `[Required]` | not null / not empty string |
| `[StringLength(max, MinimumLength = n)]`, `[MaxLength]`, `[MinLength]` | length |
| `[Range(min, max)]` | numeric/date range |
| `[EmailAddress]`, `[Phone]`, `[Url]`, `[CreditCard]` | formats |
| `[RegularExpression("...")]` | pattern |
| `[Compare("Password")]` | two fields equal (confirm password) |
| `[AllowedValues(...)]`, `[DeniedValues(...)]`, `[Length(min,max)]`, `[Base64String]` | **.NET 8+** additions |

⚠️ **Interview trap — `[Required]` on a non-nullable `int`:** an `int` is never null; a missing value becomes `0` and `[Required]` passes. Use `int?` with `[Required]`, or a `[Range(1, ...)]` as the sample does for `CategoryId`.

### Custom `ValidationAttribute` (reusable rule)

```csharp
public class FutureDateAttribute : ValidationAttribute
{
    public FutureDateAttribute() : base("{0} must be a date in the future.") { }

    protected override ValidationResult? IsValid(object? value, ValidationContext context)
    {
        if (value is null) return ValidationResult.Success;   // let [Required] handle null
        return value is DateTime d && d > DateTime.UtcNow
            ? ValidationResult.Success
            : new ValidationResult(FormatErrorMessage(context.DisplayName), [context.MemberName!]);
    }
}
```

### `IValidatableObject` (rules across several properties)

```csharp
public class CreateCouponRequest : IValidatableObject
{
    [Required, StringLength(20, MinimumLength = 3)]
    [RegularExpression("^[A-Z0-9]+$", ErrorMessage = "Code must be UPPERCASE letters/digits.")]
    public string Code { get; set; } = "";

    [Range(1, 90)]
    public int DiscountPercent { get; set; }

    [Required, FutureDate] public DateTime? ValidFrom { get; set; }
    [Required, FutureDate] public DateTime? ValidTo { get; set; }

    public IEnumerable<ValidationResult> Validate(ValidationContext validationContext)
    {
        if (ValidFrom >= ValidTo)
            yield return new ValidationResult(
                "ValidTo must be after ValidFrom.",
                [nameof(ValidFrom), nameof(ValidTo)]);
    }
}
```

### Complex example — adding your own error after a DB check

```csharp
[ApiController]
[Route("api/coupons")]
public class CouponsController : ControllerBase
{
    [HttpPost]
    public IActionResult Create(CreateCouponRequest request)
    {
        // Reaching here means DataAnnotations + IValidatableObject passed.
        if (request.Code == "TAKEN")                     // imagine: await _db.Coupons.AnyAsync(...)
        {
            ModelState.AddModelError(nameof(request.Code), "This code already exists.");
            return ValidationProblem(ModelState);        // 400, same JSON shape as automatic errors
        }
        return Created($"/api/coupons/{request.Code}", request);
    }
}
```

**What we observed when verifying this:** sending `code: "abc"`, `discountPercent: 95` **and** `validFrom > validTo` returned only the `Code` and `DiscountPercent` errors — the `ValidTo must be after ValidFrom` error did **not** appear:

```json
"errors": {
  "Code": ["Code must be UPPERCASE letters/digits."],
  "DiscountPercent": ["The field DiscountPercent must be between 1 and 90."]
}
```

⚠️ **Interview trap:** `IValidatableObject.Validate` runs **only after all property attributes pass**. So the user may fix two errors, resubmit, and *then* see a third. If you want all errors at once, use FluentValidation.

⚠️ **Interview trap — records:** for **positional records**, put the attributes on the constructor parameters (as the sample does). For MVC that's the supported place; `[property: Required]` on a positional record makes MVC throw at request time (verified: *"Record type 'PropRecord' has validation metadata defined on property 'Name' that will be ignored..."* → 500). (Minimal-API/`Validator.TryValidateObject` code that reads *properties* needs `[property: ...]` — see Chapter 7.)

### FluentValidation (very common in real projects)

A popular NuGet package (`FluentValidation.DependencyInjectionExtensions`) that keeps rules in a separate class — easier to test, supports async/DB rules, and gives all errors at once.

```csharp
public record CreateOrderRequest(string CustomerEmail, List<OrderLine> Lines);
public record OrderLine(int ProductId, int Quantity);

public class CreateOrderValidator : AbstractValidator<CreateOrderRequest>
{
    public CreateOrderValidator()
    {
        RuleFor(x => x.CustomerEmail).NotEmpty().EmailAddress();
        RuleFor(x => x.Lines).NotEmpty().WithMessage("Order must have at least one line.");
        RuleForEach(x => x.Lines).ChildRules(line =>
        {
            line.RuleFor(l => l.ProductId).GreaterThan(0);
            line.RuleFor(l => l.Quantity).InclusiveBetween(1, 50);
        });
    }
}

// Program.cs
builder.Services.AddValidatorsFromAssemblyContaining<CreateOrderValidator>();

// In an endpoint / action: inject IValidator<CreateOrderRequest>
FluentValidation.Results.ValidationResult result = await validator.ValidateAsync(req);
if (!result.IsValid)
    return TypedResults.ValidationProblem(result.ToDictionary());   // same 400 shape
```

| | DataAnnotations | FluentValidation |
|---|---|---|
| Where rules live | Attributes on the DTO | Separate validator class |
| Cross-field rules | `IValidatableObject` (runs last) | Natural (`When`, `Must`, `GreaterThan(x => x.Other)`) |
| Async / DB checks | Awkward | `MustAsync` (but keep heavy DB checks in services) |
| Automatic with `[ApiController]` | ✅ built-in | Call it manually or via a filter (the old auto-validation package is deprecated) |
| Package | built-in | NuGet |

🎯 **Say this in the interview:** "Simple shape rules go in DataAnnotations and `[ApiController]` returns 400 automatically. For cross-field or more complex rules I use IValidatableObject or FluentValidation, and anything that needs the database like 'email already exists' lives in the service and returns 409 or a ValidationProblem."

---

## 6.8 Return types: `T` vs `IActionResult` vs `ActionResult<T>`

**Simple words:** the return type decides **what you are allowed to return** and **how much Swagger/OpenAPI can learn** from your signature.

```csharp
// 1) Specific type: always 200 (or 204 if null!). Can't return NotFound.
[HttpGet("a/{id:int}")]
public ProductDto? GetA(int id) => _products.FirstOrDefault(p => p.Id == id);

// 2) IActionResult: any status, but OpenAPI can't see the body type -> need [ProducesResponseType]
[HttpGet("b/{id:int}")]
[ProducesResponseType<ProductDto>(StatusCodes.Status200OK)]
[ProducesResponseType(StatusCodes.Status404NotFound)]
public IActionResult GetB(int id)
{
    ProductDto? p = _products.FirstOrDefault(p => p.Id == id);
    return p is null ? NotFound() : Ok(p);
}

// 3) ActionResult<T>: best of both. Return T directly (implicit 200) or any ActionResult.
[HttpGet("c/{id:int}")]
public ActionResult<ProductDto> GetC(int id)
{
    ProductDto? p = _products.FirstOrDefault(p => p.Id == id);
    if (p is null) return NotFound();
    return p;                       // implicit conversion -> 200 OK
}
```

| | `T` (e.g. `ProductDto`) | `IActionResult` | `ActionResult<T>` |
|---|---|---|---|
| Return other status codes | ❌ | ✅ | ✅ |
| OpenAPI knows the 200 body type | ✅ | ❌ (needs `[ProducesResponseType]`) | ✅ |
| Return `T` directly | ✅ | ❌ (must wrap in `Ok(...)`) | ✅ |
| Typical use | trivial endpoints | actions with no body (`NoContent`) or many types | **most GET/POST actions** |

⚠️ **Interview trap:** `ActionResult<T>` doesn't work with **interfaces**: `ActionResult<IEnumerable<T>>` can't implicitly convert from `IEnumerable<T>`. Return `.ToList()` wrapped in `Ok(...)` or use `ActionResult<List<T>>`.

⚠️ **Interview trap:** returning `null` from a `T` action gives **204 No Content**, not 404.

### Helper methods cheat sheet

| Method | Status |
|---|---|
| `Ok(obj)` | 200 |
| `Created(uri, obj)` / `CreatedAtAction(action, routeValues, obj)` / `CreatedAtRoute(name, routeValues, obj)` | 201 + Location |
| `Accepted()` | 202 |
| `NoContent()` | 204 |
| `BadRequest()` / `ValidationProblem(ModelState)` | 400 |
| `Unauthorized()` / `Forbid()` | 401 / 403 |
| `NotFound()` | 404 |
| `Conflict()` | 409 |
| `UnprocessableEntity()` | 422 |
| `Problem(detail, statusCode: 500)` | any, as ProblemDetails |
| `StatusCode(418)` | anything |

### `CreatedAtAction` vs `CreatedAtRoute`

Both return **201 Created** with a **`Location`** header pointing at the new resource.

```csharp
// CreatedAtAction: by ACTION name (+ optional controller name)
return CreatedAtAction(nameof(GetById), new { id = created.Id }, created);

// CreatedAtRoute: by ROUTE NAME - used in the sample app
[HttpGet("{id:int}", Name = nameof(GetById))]
public async Task<ActionResult<ProductDto>> GetById(int id, CancellationToken ct) { ... }

return CreatedAtRoute(nameof(GetById), new { id = created.Id }, created);
```

```http
HTTP/1.1 201 Created
Location: https://localhost:5001/api/products/4
Content-Type: application/json

{ "id": 4, "name": "Pen", "price": 1.5, "stock": 100, "category": "Books" }
```

⚠️ **Interview trap — the "Async" suffix:** MVC **removes `Async`** from action names by default (`SuppressAsyncSuffixInActionNames = true`). `CreatedAtAction(nameof(GetByIdAsync), ...)` therefore fails at runtime with *"No route matches the supplied values"* (verified: the POST itself succeeds in the action, then the response becomes a **500** while writing the Location header). Fix: name actions without `Async`, or use `CreatedAtRoute` with an explicit route name (one reason the sample uses `CreatedAtRoute`).

---

## 6.9 The sample app's controller, line by line

From the sample app (`samples/ShopApi/Controllers/ProductsController.cs`):

```csharp
[ApiController]
[Route("api/[controller]")]
public class ProductsController(IProductService service) : ControllerBase   // primary ctor DI (C# 12)
{
    // GET api/products?page=1&pageSize=10&search=lap
    [HttpGet]
    public async Task<ActionResult<PagedResult<ProductDto>>> GetAll(
        [FromQuery] int page = 1,
        [FromQuery] int pageSize = 10,
        [FromQuery] string? search = null,
        CancellationToken ct = default)
        => Ok(await service.GetPageAsync(page, pageSize, search, ct));

    // GET api/products/5
    [HttpGet("{id:int}", Name = nameof(GetById))]
    [ProducesResponseType(StatusCodes.Status200OK)]
    [ProducesResponseType(StatusCodes.Status404NotFound)]
    public async Task<ActionResult<ProductDto>> GetById(int id, CancellationToken ct)
    {
        ProductDto? product = await service.GetByIdAsync(id, ct);
        return product is null ? NotFound() : Ok(product);
    }

    // POST api/products   (body validated automatically by [ApiController] -> 400)
    [HttpPost]
    [Authorize]
    [ProducesResponseType(StatusCodes.Status201Created)]
    [ProducesResponseType(StatusCodes.Status400BadRequest)]
    public async Task<ActionResult<ProductDto>> Create(CreateProductRequest request, CancellationToken ct)
    {
        ProductDto created = await service.CreateAsync(request, ct);
        return CreatedAtRoute(nameof(GetById), new { id = created.Id }, created);
    }

    // PUT api/products/5
    [HttpPut("{id:int}")]
    [Authorize]
    public async Task<IActionResult> Update(int id, UpdateProductRequest request, CancellationToken ct)
        => await service.UpdateAsync(id, request, ct) ? NoContent() : NotFound();

    // DELETE api/products/5
    [HttpDelete("{id:int}")]
    [Authorize(Policy = "AdminOnly")]
    public async Task<IActionResult> Delete(int id, CancellationToken ct)
        => await service.DeleteAsync(id, ct) ? NoContent() : NotFound();
}
```

What to point out in an interview:

1. **Thin controller** — no EF code here; it talks to `IProductService` (testable with Moq, see `ShopApi.Tests/UnitTests.cs`).
2. **DTOs in, DTOs out** (`CreateProductRequest`, `ProductDto`) — never the `Product` entity.
3. **Correct status codes**: 200 / 201 + Location / 204 / 404, and 400 automatically.
4. **`CancellationToken`** passed all the way down to EF Core.
5. **Security per action**: `[Authorize]` for write, `AdminOnly` policy for delete (Chapter 10).
6. **`{id:int}` constraint** on every id route.

---

## 6.10 Pagination, filtering and sorting

**Simple words:** never return "all 2 million rows". Return **one page** plus enough info to get the next page.

**Analogy:** Google shows 10 results and "Page 1 2 3 … Next", not every web page on Earth.

**Easy example:** the sample app's `GET api/products?page=2&pageSize=10&search=lap` — the service clamps `pageSize` to 1-100 and returns a `PagedResult<T>`:

```csharp
public record PagedResult<T>(IReadOnlyList<T> Items, int Page, int PageSize, int TotalCount);
```

**Complex example — filter + sort + page with a query object (verified):**

```csharp
public class ProductQuery
{
    public int Page { get; set; } = 1;

    [Range(1, 100)]
    public int PageSize { get; set; } = 20;

    public string? Search { get; set; }
    public decimal? MinPrice { get; set; }
    public decimal? MaxPrice { get; set; }

    [RegularExpression("^(name|price|-name|-price)$")]   // whitelist! never sort by raw user input
    public string? Sort { get; set; }
}

public record PagedResult<T>(IReadOnlyList<T> Items, int Page, int PageSize, int TotalCount)
{
    public int TotalPages => (int)Math.Ceiling(TotalCount / (double)PageSize);
    public bool HasNext => Page < TotalPages;
}

[ApiController]
[Route("api/catalog")]
public class CatalogController(AppDbContext db) : ControllerBase
{
    // GET api/catalog?page=2&pageSize=10&search=lap&minPrice=50&sort=-price
    [HttpGet]
    public async Task<ActionResult<PagedResult<ProductDto>>> Get([FromQuery] ProductQuery q, CancellationToken ct)
    {
        IQueryable<Product> query = db.Products.AsNoTracking();     // nothing executed yet

        if (!string.IsNullOrWhiteSpace(q.Search)) query = query.Where(p => p.Name.Contains(q.Search));
        if (q.MinPrice is not null) query = query.Where(p => p.Price >= q.MinPrice);
        if (q.MaxPrice is not null) query = query.Where(p => p.Price <= q.MaxPrice);

        query = q.Sort switch
        {
            "name"   => query.OrderBy(p => p.Name),
            "-name"  => query.OrderByDescending(p => p.Name),
            "price"  => query.OrderBy(p => p.Price),
            "-price" => query.OrderByDescending(p => p.Price),
            _        => query.OrderBy(p => p.Id)          // ALWAYS a stable default order
        };

        int total = await query.CountAsync(ct);
        int page = Math.Max(q.Page, 1);
        List<ProductDto> items = await query
            .Skip((page - 1) * q.PageSize).Take(q.PageSize)
            .Select(p => new ProductDto(p.Id, p.Name, p.Price))
            .ToListAsync(ct);

        Response.Headers["X-Total-Count"] = total.ToString();    // optional, some clients like it
        return new PagedResult<ProductDto>(items, page, q.PageSize, total);
    }
}
```

Verified responses (in-memory version of the same code):
- `?page=2&pageSize=3&sort=-price&minPrice=100` → `{"items":[{"id":52,...},{"id":51,...},{"id":50,...}],"page":2,"pageSize":3,"totalCount":46,"totalPages":16,"hasNext":true}`
- `?sort=drop` → **400** `"Sort": ["The field Sort must match the regular expression ..."]`

| Style | How | Pros | Cons |
|---|---|---|---|
| **Offset** (`page`, `pageSize` → `Skip/Take`) | `OFFSET 1000 ROWS FETCH NEXT 20` | Simple, jump to page N | Slow on deep pages; rows shift if data changes |
| **Keyset / cursor** (`?after=lastId`) | `WHERE Id > @lastId ORDER BY Id` | Fast at any depth, stable | No "jump to page 50" |

⚠️ **Interview trap:** `Skip/Take` **without `OrderBy`** — SQL doesn't guarantee row order, so pages can repeat or skip rows (EF Core also warns about it). Always order, and order by something **unique** (add `ThenBy(p => p.Id)`).

⚠️ **Interview trap:** no upper limit on `pageSize` → someone sends `pageSize=1000000`. Clamp it (sample: `Math.Clamp(pageSize, 1, 100)`).

🎯 **Say this in the interview:** "I accept page, pageSize, filters and a whitelisted sort in a query object, build an IQueryable so the database does the filtering, always OrderBy before Skip/Take, clamp pageSize, and return items plus total count. For very large tables I switch to keyset pagination."

---

## 6.11 DTOs vs entities — and overposting

**Simple words:**
- An **entity** is your **database row** shape (EF Core class).
- A **DTO** (Data Transfer Object) is the **API contract** — exactly what the client may send or see.

**Analogy:** your bank's internal customer record has a credit score, risk flags, internal notes. The bank app shows you only name, balance, transactions. The app screen = DTO, internal record = entity.

### Overposting (a.k.a. mass assignment) — the security bug

```csharp
public class User           // ENTITY
{
    public int Id { get; set; }
    public string Name { get; set; } = "";
    public string Email { get; set; } = "";
    public bool IsAdmin { get; set; }          // sensitive!
    public decimal CreditLimit { get; set; }   // sensitive!
}

// ❌ BAD: binds the entity directly
[HttpPut("{id:int}")]
public async Task<IActionResult> UpdateBad(int id, User user)
{
    db.Users.Update(user);                  // copies EVERY property, including IsAdmin
    await db.SaveChangesAsync();
    return NoContent();
}
```

Attacker sends: `{ "id": 1, "name": "Hacker", "email": "h@x.com", "isAdmin": true }` → now an admin. 😱

```csharp
// ✅ GOOD: a DTO with ONLY the editable fields
public record UpdateProfileRequest(
    [Required, StringLength(100)] string Name,
    [Required, EmailAddress] string Email);

[HttpPut("{id:int}")]
public async Task<IActionResult> Update(int id, UpdateProfileRequest request)
{
    User? user = await db.Users.FindAsync(id);
    if (user is null) return NotFound();
    user.Name = request.Name;                // explicit mapping
    user.Email = request.Email;
    await db.SaveChangesAsync();
    return NoContent();
}
```

(Verified: sending `"isAdmin": true` to the DTO version returns 204 and the extra property is simply ignored.)

| Reason to use DTOs | Explanation |
|---|---|
| **Security** | Prevents overposting; hides sensitive columns (password hash, internal flags) |
| **Stable contract** | You can rename a DB column without breaking mobile apps |
| **No cycles** | Entities with navigation properties (`Order.Customer.Orders...`) cause JSON cycles / huge payloads |
| **Performance** | `Select` into a DTO fetches only needed columns (Chapter 9) |
| **Different shapes** | `CreateXRequest`, `UpdateXRequest`, `XDto`, `XListItemDto` can differ |

Mapping options: **manual** (clear, fast, recommended in interviews), **Mapster/AutoMapper** (less code; AutoMapper is now commercially licensed for newer versions — mention you'd check licensing), or **EF `Select` projection** straight into the DTO.

⚠️ **Interview trap:** "Using `[Bind]` / `[JsonIgnore]` on the entity is enough." — It's fragile: the next developer adds a property and forgets. DTOs are the standard answer.

🎯 **Say this in the interview:** "I never expose EF entities from controllers. Separate request and response DTOs prevent overposting, hide sensitive fields, avoid serialization cycles and keep the API contract stable when the database changes."

---

## 6.12 API versioning

**Simple words:** once mobile apps use your API, you can't just change a response shape. Versioning lets **v1 clients keep working** while **v2** gets the new shape.

**Analogy:** a phone charger standard. New phones use USB-C, but the shop still sells micro-USB cables for old phones.

**When do you need a new version?** Only for **breaking changes**: removing/renaming a field, changing a type, changing behaviour. Adding an optional field is *not* breaking.

| Strategy | Example | Pros | Cons |
|---|---|---|---|
| **URL segment** | `/api/v2/reports` | Most visible, easy to test in a browser, cache-friendly | URL changes |
| **Query string** | `/api/reports?api-version=2.0` | Easy, URL path unchanged | Easy to forget |
| **Header** | `X-Api-Version: 2.0` | Clean URLs | Hidden, harder to test |
| **Media type** | `Accept: application/json;v=2` | "Pure REST" | Complex |

Package: **`Asp.Versioning.Mvc`** (for controllers) / **`Asp.Versioning.Http`** (minimal APIs) — the successor of `Microsoft.AspNetCore.Mvc.Versioning`.

```csharp
// Program.cs
builder.Services.AddApiVersioning(o =>
{
    o.DefaultApiVersion = new ApiVersion(1, 0);
    o.AssumeDefaultVersionWhenUnspecified = true;   // old clients without a version -> 1.0
    o.ReportApiVersions = true;                     // adds "api-supported-versions" header
    o.ApiVersionReader = ApiVersionReader.Combine(
        new UrlSegmentApiVersionReader(),
        new HeaderApiVersionReader("X-Api-Version"),
        new QueryStringApiVersionReader("api-version"));
}).AddMvc();

// Controller
[ApiController]
[ApiVersion("1.0")]
[ApiVersion("2.0")]
[Route("api/v{version:apiVersion}/reports")]
public class ReportsController : ControllerBase
{
    [HttpGet, MapToApiVersion("1.0")]
    public IActionResult GetV1() => Ok(new { total = 100 });

    [HttpGet, MapToApiVersion("2.0")]
    public IActionResult GetV2() => Ok(new { total = 100, currency = "INR", generatedUtc = DateTime.UtcNow });
}
```

Verified: `GET /api/v1/reports` → `{"total":100}`; `GET /api/v2/reports` → the v2 body with header `api-supported-versions: 1.0, 2.0`; `GET /api/v3/reports` → 404.

Also mark old versions with `[ApiVersion("1.0", Deprecated = true)]` so clients see `api-deprecated-versions` in responses.

🎯 **Say this in the interview:** "I version only for breaking changes, usually with a URL segment like /api/v2 using the Asp.Versioning package, keep v1 running, report supported and deprecated versions in headers, and give clients a sunset date."

---

## 6.13 Content negotiation

**Simple words:** the client says *"I'd like JSON (or XML)"* in the **`Accept`** header; the server picks a matching **output formatter**. The server says what it actually sent in **`Content-Type`**.

**Analogy:** a restaurant menu in English and Hindi. You ask for Hindi; if they don't have it, they either give you English (default) or say "sorry, not available" (406).

```csharp
builder.Services.AddControllers(o =>
{
    o.ReturnHttpNotAcceptable = true;     // unknown Accept -> 406 instead of silently sending JSON
})
.AddXmlSerializerFormatters();            // adds XML input + output formatters
```

| Request header | Result |
|---|---|
| `Accept: application/json` (or none, or `*/*`) | JSON (System.Text.Json, camelCase) |
| `Accept: application/xml` | XML (if formatter added) |
| `Accept: text/csv` with `ReturnHttpNotAcceptable = true` | **406 Not Acceptable** |
| `Content-Type: text/plain` on a JSON body | **415 Unsupported Media Type** |

What we saw while verifying: asking for XML for a **positional record** DTO also produced **406**, because `XmlSerializer` needs a public parameterless constructor. Use classes (or `AddXmlDataContractSerializerFormatters`) for XML DTOs.

Customise JSON globally:

```csharp
builder.Services.AddControllers().AddJsonOptions(o =>
{
    o.JsonSerializerOptions.Converters.Add(new JsonStringEnumConverter());   // enums as "Shipped" not 2
    o.JsonSerializerOptions.DefaultIgnoreCondition = JsonIgnoreCondition.WhenWritingNull;
});
```

---

## 6.14 File upload with `IFormFile`

**Simple words:** files come as `multipart/form-data`. ASP.NET Core gives you an `IFormFile` (name, size, content type, stream).

**Easy example:**

```csharp
[HttpPost("avatar")]
public async Task<IActionResult> UploadAvatar(IFormFile file)
{
    await using var stream = System.IO.File.Create(Path.Combine("uploads", Guid.NewGuid() + ".png"));
    await file.CopyToAsync(stream);
    return Ok();
}
```

**Complex/safe example (verified compile):**

```csharp
[ApiController]
[Route("api/files")]
public class FilesController(IWebHostEnvironment env) : ControllerBase
{
    private static readonly string[] Allowed = [".jpg", ".jpeg", ".png", ".pdf"];
    private const long MaxBytes = 5 * 1024 * 1024;   // 5 MB

    [HttpPost]
    [RequestSizeLimit(MaxBytes)]                        // Kestrel rejects bigger bodies early
    public async Task<IActionResult> Upload(IFormFile file, [FromForm] string? description, CancellationToken ct)
    {
        if (file.Length == 0) return BadRequest("Empty file.");
        if (file.Length > MaxBytes) return BadRequest("File too large.");

        string ext = Path.GetExtension(file.FileName).ToLowerInvariant();
        if (!Allowed.Contains(ext)) return BadRequest($"Extension {ext} not allowed.");

        // NEVER trust file.FileName for the path (../../web.config). Generate our own name.
        string safeName = $"{Guid.NewGuid()}{ext}";
        string folder = Path.Combine(env.ContentRootPath, "uploads");   // outside wwwroot!
        Directory.CreateDirectory(folder);

        await using FileStream fs = System.IO.File.Create(Path.Combine(folder, safeName));
        await file.CopyToAsync(fs, ct);

        return Ok(new { safeName, file.Length, description });
    }
}
```

Checklist: size limit, extension whitelist (and ideally check the file's "magic bytes"), generated file name, store outside `wwwroot` or in **Blob Storage**, virus scan for public uploads. For very large files use **streaming** (`MultipartReader`) instead of buffering `IFormFile`. Default request body limit in Kestrel is ~**30 MB**.

⚠️ **Interview trap:** `IFormFile` + `[FromBody]` in the same action — impossible; a multipart request has no JSON body. Send extra fields as `[FromForm]`.

---

## 6.15 `CancellationToken` in actions

**Simple words:** if the user closes the browser tab or the mobile app times out, why keep running a 10-second SQL query? Add a `CancellationToken` parameter — ASP.NET Core binds it to `HttpContext.RequestAborted` and **signals it when the client disconnects**.

```csharp
[HttpGet]
public async Task<IActionResult> Get(CancellationToken ct)
{
    try
    {
        await Task.Delay(TimeSpan.FromSeconds(10), ct);   // pass ct to EVERY async call (EF, HttpClient...)
        return Ok("done");
    }
    catch (OperationCanceledException)
    {
        logger.LogInformation("Client went away, stopped work early.");
        return new EmptyResult();                         // nobody is listening anyway
    }
}
```

The sample app passes `ct` from every action → service → `ToListAsync(ct)` / `SaveChangesAsync(ct)`.

⚠️ **Interview trap:** cancelling **writes** half-way. For a "place order" call, think twice — you may prefer to finish the transaction (`SaveChangesAsync(CancellationToken.None)`) rather than leave partial work. Reads are always safe to cancel.

---

## 6.16 OpenAPI (Swagger)

**Simple words:** OpenAPI is a **machine-readable description** of your API (endpoints, parameters, schemas, status codes) — a JSON file. Tools turn it into docs UI (Swagger UI, Scalar), client SDKs, and tests.

| | **Microsoft.AspNetCore.OpenApi** (.NET 9+) | **Swashbuckle.AspNetCore** |
|---|---|---|
| Who | Microsoft, built-in | Community (was in templates up to .NET 8) |
| Setup | `AddOpenApi()` + `MapOpenApi()` | `AddSwaggerGen()` + `UseSwagger()` + `UseSwaggerUI()` |
| Doc URL | `/openapi/v1.json` | `/swagger/v1/swagger.json` |
| UI | none built-in — add Scalar or Swagger UI separately | Swagger UI included |
| AOT friendly | ✅ | ❌ |
| .NET 10 | OpenAPI **3.1** by default, YAML output supported | still usable |

The sample app (.NET 10):

```csharp
builder.Services.AddOpenApi();

if (app.Environment.IsDevelopment())
{
    app.MapOpenApi();          // GET /openapi/v1.json
}
```

Help the generator with metadata:

```csharp
[ProducesResponseType<ProductDto>(StatusCodes.Status200OK)]   // generic form is .NET 7+
[ProducesResponseType(StatusCodes.Status404NotFound)]
[EndpointSummary("Get one product")]                            // .NET 9+
[Tags("Products")]
```

and enable XML comments (`<GenerateDocumentationFile>true</GenerateDocumentationFile>`) — .NET 10's generator picks them up.

⚠️ **Interview trap:** "Swagger is part of ASP.NET Core." — Swashbuckle was only *in the template* up to .NET 8. Since .NET 9 the template uses `Microsoft.AspNetCore.OpenApi`; Swagger UI is optional.

🎯 **Say this in the interview:** "On .NET 9+ I use the built-in Microsoft.AspNetCore.OpenApi with AddOpenApi and MapOpenApi, expose it only in development or behind auth, and enrich it with ProducesResponseType or typed results so clients can generate SDKs."

---

## 6.17 A real gotcha we hit: `[Produces("application/json")]` breaks ProblemDetails

While verifying the sample app, the controller originally had `[Produces("application/json")]` on it ("our API only returns JSON — let's say so"). Validation errors then came back like this (actual captured output):

```http
HTTP/1.1 400 Bad Request
Content-Type: application/json; charset=utf-8          <-- WRONG

{"type":"https://tools.ietf.org/html/rfc9110#section-15.5.1","title":"One or more validation errors occurred.","status":400,"errors":{"Name":["The Name field is required."]}, ...}
```

The identical controller **without** `[Produces]`:

```http
HTTP/1.1 400 Bad Request
Content-Type: application/problem+json; charset=utf-8  <-- correct (RFC 7807/9457)
```

**Why:** `[Produces]` is a *result filter* that **forces** the content type for **every** response of that controller — including the ProblemDetails that `[ApiController]` creates. Clients (and tests) that check for `application/problem+json` to detect errors then break.

**Fix:** we **removed** `[Produces]` from the controller. JSON is already the default. If you need it for OpenAPI documentation only, use `[ProducesResponseType(..., "application/json")]` on specific responses instead.

⚠️ **Interview trap:** "`[Produces]` is just documentation." — No, it **changes runtime behaviour** (it's a filter). `[ProducesResponseType]` is the documentation-only attribute.

---

## Try it yourself

1. You have `[HttpGet("{id}")] GetById(int id)` and `[HttpGet("{slug}")] GetBySlug(string slug)` in the same controller. What happens and how do you fix it?

<details><summary>Answer</summary>

Both templates match `/api/products/5`, so routing throws `AmbiguousMatchException` (500). Fix: add a constraint — `[HttpGet("{id:int}")]` — so numbers go to `GetById` and everything else to `GetBySlug`; or use a different template like `[HttpGet("by-slug/{slug}")]`.
</details>

2. Write an action `POST api/orders` that takes `CreateOrderRequest` from the body and returns **201** with a Location header pointing at `GET api/orders/{id}`.

<details><summary>Answer</summary>

```csharp
[HttpGet("{id:int}", Name = "GetOrderById")]
public async Task<ActionResult<OrderDto>> GetById(int id, CancellationToken ct) =>
    await service.GetAsync(id, ct) is { } o ? o : NotFound();

[HttpPost]
public async Task<ActionResult<OrderDto>> Create(CreateOrderRequest request, CancellationToken ct)
{
    OrderDto created = await service.CreateAsync(request, ct);
    return CreatedAtRoute("GetOrderById", new { id = created.Id }, created);
}
```
`[ApiController]` infers `[FromBody]` and returns 400 automatically if invalid.
</details>

3. A user with role `User` calls `DELETE api/products/1` (policy `AdminOnly`) with a valid token. Which status code? What if the token has expired?

<details><summary>Answer</summary>

Valid token but wrong role → **403 Forbidden** (authenticated, not authorized). Expired token → **401 Unauthorized** (authentication failed).
</details>

4. Why is this dangerous, and how do you fix it?
```csharp
[HttpPut("{id}")]
public async Task<IActionResult> Update(int id, Customer customer)
{ db.Update(customer); await db.SaveChangesAsync(); return NoContent(); }
```

<details><summary>Answer</summary>

It binds the **entity** → **overposting**: the client can set any column (e.g. `IsVip`, `CreditLimit`, even another customer's `Id`). `db.Update` also marks *all* columns modified. Fix: accept an `UpdateCustomerRequest` DTO with only editable fields, load the entity by `id`, copy the allowed fields, save; return 404 if not found. Add `{id:int}` constraint.
</details>

5. A client sends `?pageSize=50000`. What should your API do and where?

<details><summary>Answer</summary>

Never trust it. Either validate (`[Range(1,100)]` on the query object → automatic 400) or clamp silently (`Math.Clamp(pageSize, 1, 100)` as in the sample's `ProductService`). Also always `OrderBy` before `Skip/Take`.
</details>

6. Your teammate added `[Produces("application/json")]` to every controller. Integration tests that check `Content-Type: application/problem+json` on 400 responses started failing. Explain.

<details><summary>Answer</summary>

`[Produces]` is a result filter that forces the response content type for all results, including ProblemDetails created by `[ApiController]`, so errors are sent as `application/json` instead of `application/problem+json`. Remove it (JSON is the default) and use `[ProducesResponseType]` for documentation.
</details>

---

## Quick revision

- REST = resources (nouns) + HTTP verbs; GET/PUT/DELETE idempotent, POST not; GET must never change data.
- 201 + Location for create, 204 for update/delete, 400 validation, **401 = who are you, 403 = not allowed**, 404, 409 conflict, 422 business rule, 429 rate limit, 500 = our bug.
- APIs inherit **`ControllerBase`**; `Controller` only adds views.
- `[ApiController]` = automatic 400 ValidationProblem + binding inference + attribute routing required + ProblemDetails for errors.
- Route constraints (`{id:int}`) disambiguate routes; failure = **404**, not 400.
- Binding sources: Route, Query, Body, Header, Form, Services; **only one `[FromBody]`**; complex query objects need `[FromQuery]`.
- Validation: DataAnnotations → custom `ValidationAttribute` → `IValidatableObject` (runs **after** attributes pass) → FluentValidation; DB checks in services.
- Prefer **`ActionResult<T>`**: any status code + OpenAPI knows the type; doesn't work with interfaces like `IEnumerable<T>`.
- `CreatedAtAction` vs `CreatedAtRoute`; beware the stripped **`Async`** suffix.
- Paging: query object, whitelist sort, **OrderBy before Skip/Take**, clamp pageSize, return total.
- **DTOs, never entities** → prevents overposting, cycles, leaks.
- Versioning with `Asp.Versioning` (URL/query/header); version only for breaking changes.
- `ReturnHttpNotAcceptable = true` → 406; wrong Content-Type → 415.
- Pass `CancellationToken` everywhere; .NET 9+ uses `AddOpenApi`/`MapOpenApi`; **`[Produces]` changes runtime content type — don't put it on controllers.**

---

# 7. Minimal APIs

> **Why interviewers ask this**
> Since .NET 6, Microsoft's templates push **minimal APIs**, and new projects (microservices, serverless, AOT) increasingly use them. Interviewers want to know if you understand **how they differ from controllers**, how parameters are bound without attributes, why `TypedResults` exist, and how you **organise** them so `Program.cs` doesn't become a 2,000-line monster.

---

## 7.1 What are minimal APIs?

**Simple words:** a minimal API is an HTTP endpoint written as **one line that maps a URL + verb to a function** (usually a lambda). No controller class, no base class, no attributes required.

**Analogy:** a controller is a **full restaurant** (reception, waiters, menu card, kitchen). A minimal API is a **food truck**: one window, you order, you get food. Less ceremony, same food.

**Easy example** — a complete working app (verified):

```csharp
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.MapGet("/hello", () => "Hello, minimal API!");
app.MapGet("/hello/{name}", (string name) => $"Hello, {name}!");

app.Run();
```

`GET /hello/Abhi` → `Hello, Abhi!` (plain text, 200).

### The Map* methods

| Method | HTTP verb |
|---|---|
| `MapGet` | GET |
| `MapPost` | POST |
| `MapPut` | PUT |
| `MapPatch` | PATCH |
| `MapDelete` | DELETE |
| `MapMethods("/x", ["HEAD", "OPTIONS"], ...)` | any list of verbs |
| `MapGroup("/prefix")` | groups endpoints under a common prefix + shared config |
| `MapFallback(...)` | runs when nothing else matches |

Each `Map*` returns a **`RouteHandlerBuilder`**, so you can chain configuration: `.WithName()`, `.WithTags()`, `.RequireAuthorization()`, `.AddEndpointFilter()`, `.CacheOutput()`, `.RequireRateLimiting()`, `.Produces<T>()`, `.DisableValidation()` ...

### What the handler can return

| Return value | What the client gets |
|---|---|
| `string` | `200`, `text/plain` |
| any object / record / `T` | `200`, JSON |
| `IResult` (`Results.Ok(...)`, `TypedResults.NotFound()`) | whatever status/body the result describes |
| `Task<T>` / `ValueTask<T>` | same as above, awaited |

---

## 7.2 Parameter binding — no attributes needed

**Simple words:** minimal APIs look at each lambda parameter and **guess** where it comes from, using its **name and type**. You *can* add `[FromQuery]`, `[FromHeader]` etc. when the guess isn't what you want.

**Inference rules (in order):**

| Parameter | Bound from |
|---|---|
| Has an explicit attribute (`[FromRoute]`, `[FromQuery]`, `[FromHeader]`, `[FromBody]`, `[FromForm]`, `[FromServices]`, `[FromKeyedServices]`) | that source |
| Special types: `HttpContext`, `HttpRequest`, `HttpResponse`, `CancellationToken`, `ClaimsPrincipal`, `IFormFile`, `Stream`, `PipeReader` | built-in |
| Type has `BindAsync` / `TryParse` method | custom binding |
| Name matches a **route** parameter | route |
| **Simple type** (`int`, `string`, `Guid`, `DateTime`, enum...) | **query string** |
| Type **registered in DI** | **services** |
| Anything else (complex type) | **JSON body** (only on POST/PUT/PATCH/DELETE) |

**Complex example — every source at once (verified):**

```csharp
app.MapGet("/api/binding/{id:int}", (
    int id,                                           // route  (name matches {id})
    string? sort,                                     // query  (?sort=...)
    [FromHeader(Name = "X-Tenant")] string? tenant,   // header (must be explicit)
    TodoStore store,                                  // service (registered in DI)
    HttpContext http,                                 // special type
    CancellationToken ct) =>                          // special type = RequestAborted
    new { id, sort, tenant, count = store.All.Count, path = http.Request.Path.Value });
```

`GET /api/binding/7?sort=name` with header `X-Tenant: acme` →
`{"id":7,"sort":"name","tenant":"acme","count":2,"path":"/api/binding/7"}`

### `[AsParameters]` — many query parameters in one object (.NET 7+)

```csharp
public record SearchArgs(int Page = 1, int PageSize = 10, string? Q = null);

app.MapGet("/api/search", ([AsParameters] SearchArgs args) => args);
// GET /api/search?page=3&q=milk  ->  {"page":3,"pageSize":10,"q":"milk"}
```

Without `[AsParameters]`, `SearchArgs` (a complex type) would be treated as the **JSON body** — and a GET has no body, so you'd get an error.

⚠️ **Interview trap:** "Optional vs required." A non-nullable parameter (`int page`, `string q`) is **required** — if it's missing you get a **400**. Make it optional with `int? page`, `string? q`, or a default value (`int page = 1`).

⚠️ **Interview trap:** headers are **never** inferred — you must write `[FromHeader]`.

🎯 **Say this in the interview:** "Minimal APIs infer binding from name and type: route names go to the route, simple types to the query, DI-registered types to services, special types like HttpContext and CancellationToken are injected, and complex types come from the JSON body. For a group of query parameters I use `[AsParameters]`."

---

## 7.3 `Results` vs `TypedResults` and union return types

**Simple words:** both create an `IResult` (200, 404, 201...). The difference is the **return type the compiler sees**:

- `Results.Ok(x)` returns **`IResult`** — the compiler (and OpenAPI) only knows "some result".
- `TypedResults.Ok(x)` returns **`Ok<T>`** — a *concrete* type that says "200 with a `T` body".

When a handler can return different results, you declare them in a **union type**: `Results<Ok<Todo>, NotFound>` — "this returns either 200 with a Todo, or 404".

**Easy example:**

```csharp
// Results -> IResult (OpenAPI sees only "200 OK", no schema, no 404)
app.MapGet("/results/{id:int}", IResult (int id) =>
    id > 0 ? Results.Ok(new { id }) : Results.NotFound());

// TypedResults + union -> OpenAPI sees 200 (with Todo schema) AND 404
app.MapGet("/api/todos/{id:int}", Results<Ok<Todo>, NotFound> (int id, TodoStore store) =>
    store.Find(id) is { } todo ? TypedResults.Ok(todo) : TypedResults.NotFound());
```

**What we actually saw in the generated `/openapi/v1.json`:**

```json
// "/results/{id}"  (Results / IResult)
"responses": { "200": { "description": "OK" } }

// "/api/todos/{id}"  (TypedResults + Results<Ok<Todo>, NotFound>)
"responses": {
  "200": { "description": "OK",
           "content": { "application/json": { "schema": { "$ref": "#/components/schemas/Todo" } } } },
  "404": { "description": "Not Found" }
}
```

| | `Results.X()` | `TypedResults.X()` |
|---|---|---|
| Return type | `IResult` | concrete (`Ok<T>`, `NotFound`, `Created<T>`...) |
| OpenAPI metadata | ❌ must add `.Produces<T>(200).Produces(404)` manually | ✅ automatic |
| Unit testing | must cast/reflect to check | `Assert.IsType<Ok<Todo>>(result)` and read `.Value` |
| Multiple return types | just works (all are `IResult`) | declare a union `Results<A, B, C>` (up to 6 types) |

**Why it helps testing** — the handler is a plain static method you can call directly:

```csharp
public static Results<Ok<Todo>, NotFound> GetById(int id, TodoStore store) =>
    store.Find(id) is { } todo ? TypedResults.Ok(todo) : TypedResults.NotFound();

// xUnit
var result = TodoEndpoints.GetById(99, new TodoStore());
Assert.IsType<NotFound>(result.Result);
```

From the sample app (`samples/ShopApi/Endpoints/CategoryEndpoints.cs`):

```csharp
group.MapGet("/{id:int}", async Task<Results<Ok<CategoryDto>, NotFound>> (int id, AppDbContext db, CancellationToken ct) =>
{
    CategoryDto? c = await db.Categories
        .Where(x => x.Id == id)
        .Select(x => new CategoryDto(x.Id, x.Name, x.Products.Count))
        .FirstOrDefaultAsync(ct);
    return c is null ? TypedResults.NotFound() : TypedResults.Ok(c);
});
```

⚠️ **Interview trap:** with a union return type in a **lambda**, you must write the return type **before the parameter list** (`async Task<Results<...>> (int id, ...) => ...`), otherwise the compiler can't infer it from two different branches.

🎯 **Say this in the interview:** "I prefer TypedResults with a `Results<Ok<T>, NotFound>` union: the compiler checks I only return declared results, OpenAPI documents every status code automatically, and unit tests can assert on concrete result types."

---

## 7.4 Route groups — `MapGroup`

**Simple words:** a route group is a **folder** for endpoints: shared **URL prefix** + shared **settings** (auth, tags, filters, rate limits, CORS).

**Easy example:**

```csharp
var todos = app.MapGroup("/api/todos");
todos.MapGet("/", ...);          // GET  /api/todos
todos.MapGet("/{id:int}", ...);  // GET  /api/todos/5
todos.MapPost("/", ...);         // POST /api/todos
```

**Complex example — nested groups with shared policies:**

```csharp
RouteGroupBuilder api = app.MapGroup("/api")
    .RequireRateLimiting("fixed");                 // applies to everything inside

RouteGroupBuilder admin = api.MapGroup("/admin")
    .RequireAuthorization("AdminOnly")             // only admins
    .WithTags("Admin");

admin.MapGet("/stats", () => TypedResults.Ok(new { users = 42 }));     // GET /api/admin/stats
admin.MapDelete("/cache", () => TypedResults.NoContent());             // DELETE /api/admin/cache

RouteGroupBuilder publicApi = api.MapGroup("/public").AllowAnonymous();
publicApi.MapGet("/ping", () => "pong");                               // GET /api/public/ping
```

🎯 **Say this in the interview:** "MapGroup gives a shared prefix and shared conventions like RequireAuthorization, tags, filters and rate limiting, so I don't repeat them on every endpoint. Groups can be nested."

---

## 7.5 Organising endpoints in extension methods

**Simple words:** don't put 50 endpoints in `Program.cs`. One **static class per feature** with an extension method `MapXxxEndpoints()`, just like the sample app.

**Sample app** (`samples/ShopApi/Endpoints/CategoryEndpoints.cs`, abridged) and `Program.cs`:

```csharp
public static class CategoryEndpoints
{
    public static RouteGroupBuilder MapCategoryEndpoints(this IEndpointRouteBuilder app)
    {
        RouteGroupBuilder group = app.MapGroup("/api/categories").WithTags("Categories");

        group.MapGet("/", async (AppDbContext db, CancellationToken ct) =>
            await db.Categories
                .Select(c => new CategoryDto(c.Id, c.Name, c.Products.Count))
                .ToListAsync(ct))
            .CacheOutput(p => p.Expire(TimeSpan.FromSeconds(30)));

        // GET /{id} shown in 7.3 ...

        group.MapPost("/", async Task<Results<Created<CategoryDto>, ValidationProblem>> (
            CreateCategoryRequest req, AppDbContext db, CancellationToken ct) =>
        {
            if (string.IsNullOrWhiteSpace(req.Name))
                return TypedResults.ValidationProblem(new Dictionary<string, string[]>
                {
                    ["Name"] = ["Name is required."]
                });

            var category = new Category { Name = req.Name.Trim() };
            db.Categories.Add(category);
            await db.SaveChangesAsync(ct);
            return TypedResults.Created($"/api/categories/{category.Id}", new CategoryDto(category.Id, category.Name, 0));
        }).RequireAuthorization("AdminOnly");

        return group;
    }
}

// Program.cs
app.MapCategoryEndpoints();
```

**Complex example — named handler methods instead of big lambdas (verified):** easier to read, test, and debug.

```csharp
public static class TodoEndpoints
{
    public static IEndpointRouteBuilder MapTodoEndpoints(this IEndpointRouteBuilder app)
    {
        RouteGroupBuilder todos = app.MapGroup("/api/todos")
            .WithTags("Todos")
            .AddEndpointFilter<TimingFilter>();            // filter for the whole group (7.6)

        todos.MapGet("/", GetAll);
        todos.MapGet("/{id:int}", GetById).WithName("GetTodoById");
        todos.MapPost("/", Create)
             .AddEndpointFilter<ValidationFilter<CreateTodo>>();
        todos.MapDelete("/{id:int}", Delete)
             .RequireAuthorization();

        return app;
    }

    public static Ok<IReadOnlyList<Todo>> GetAll(TodoStore store) => TypedResults.Ok(store.All);

    public static Results<Ok<Todo>, NotFound> GetById(int id, TodoStore store) =>
        store.Find(id) is { } todo ? TypedResults.Ok(todo) : TypedResults.NotFound();

    public static CreatedAtRoute<Todo> Create(CreateTodo req, TodoStore store)
    {
        Todo todo = store.Add(req.Title);
        return TypedResults.CreatedAtRoute(todo, "GetTodoById", new { id = todo.Id });
    }

    public static Results<NoContent, NotFound> Delete(int id, TodoStore store) =>
        store.Remove(id) ? TypedResults.NoContent() : TypedResults.NotFound();
}
```

Verified: `POST /api/todos {"title":"Buy milk"}` → `201 Created` with `Location: http://localhost:5289/api/todos/2`. `DELETE /api/todos/1` without login → `401`.

> Bigger projects sometimes use the **Carter** library or a "vertical slice" structure where each feature folder has its endpoint + handler + validator. The idea is the same: **one feature, one file/folder**.

---

## 7.6 Endpoint filters (`IEndpointFilter`)

**Simple words:** an endpoint filter is code that runs **just before and after a minimal API handler** — like an MVC action filter, but for minimal APIs (.NET 7+). It can **inspect the arguments**, **short-circuit** (return early), or **change the result**.

**Analogy:** the **security check before boarding** — it sees your boarding pass (the arguments), can stop you, or let you through to the plane (the handler).

```text
request ─► [filter 1 before] ─► [filter 2 before] ─► HANDLER
response ◄─ [filter 1 after] ◄── [filter 2 after] ◄──┘
```

Filters run in the order they were added (first added = outermost).

**Easy example — inline lambda filter (verified):**

```csharp
app.MapGet("/api/lambda-filter", () => "inside handler")
   .AddEndpointFilter(async (ctx, next) =>
   {
       if (!ctx.HttpContext.Request.Headers.ContainsKey("X-Api-Key"))
           return TypedResults.Unauthorized();       // short-circuit: handler never runs
       return await next(ctx);                       // continue
   });
```

**Class-based filter with DI — timing:**

```csharp
public class TimingFilter(ILogger<TimingFilter> logger) : IEndpointFilter
{
    public async ValueTask<object?> InvokeAsync(EndpointFilterInvocationContext context, EndpointFilterDelegate next)
    {
        long start = Stopwatch.GetTimestamp();
        object? result = await next(context);
        logger.LogInformation("{Path} took {Ms} ms",
            context.HttpContext.Request.Path, Stopwatch.GetElapsedTime(start).TotalMilliseconds);
        return result;
    }
}
```

`AddEndpointFilter<TimingFilter>()` creates the filter via DI (constructor injection works; it doesn't even need to be registered).

**Complex example — a reusable generic validation filter (verified):**

```csharp
public record CreateTodo(
    [property: Required, StringLength(100, MinimumLength = 3)] string Title);   // attributes on the PROPERTY

public class ValidationFilter<T> : IEndpointFilter where T : class
{
    public async ValueTask<object?> InvokeAsync(EndpointFilterInvocationContext context, EndpointFilterDelegate next)
    {
        T? arg = context.Arguments.OfType<T>().FirstOrDefault();   // find the DTO among the handler args
        if (arg is null)
            return TypedResults.BadRequest("Request body is missing.");

        var results = new List<ValidationResult>();
        if (!Validator.TryValidateObject(arg, new ValidationContext(arg), results, validateAllProperties: true))
        {
            Dictionary<string, string[]> errors = results
                .SelectMany(r => r.MemberNames.DefaultIfEmpty(""), (r, m) => new { m, r.ErrorMessage })
                .GroupBy(x => x.m)
                .ToDictionary(g => g.Key, g => g.Select(x => x.ErrorMessage ?? "Invalid").ToArray());
            return TypedResults.ValidationProblem(errors);   // 400, handler never runs
        }

        return await next(context);
    }
}

// usage
todos.MapPost("/", Create).AddEndpointFilter<ValidationFilter<CreateTodo>>();
```

`POST /api/todos {"title":"ab"}` →

```json
HTTP/1.1 400 Bad Request
Content-Type: application/problem+json

{"type":"https://tools.ietf.org/html/rfc9110#section-15.5.1","title":"One or more validation errors occurred.","status":400,
 "errors":{"Title":["The field Title must be a string with a minimum length of 3 and a maximum length of 100."]}}
```

Note the `[property: ...]` target: `Validator.TryValidateObject` reads attributes from **properties**, and positional-record attributes go to the constructor **parameter** by default. (For MVC controllers it's the opposite — see Chapter 6.)

### .NET 10: built-in validation for minimal APIs

Up to .NET 9, minimal APIs had **no automatic validation** (that's why everyone wrote filters like the one above or used FluentValidation). **.NET 10** adds it:

```csharp
builder.Services.AddValidation();       // .NET 10+

public record CreateNote([Required, StringLength(50, MinimumLength = 3)] string Title, [Range(1, 5)] int Priority);

app.MapPost("/api/notes", (CreateNote n) => TypedResults.Ok(n));                         // validated
app.MapPost("/api/notes-raw", (CreateNote n) => TypedResults.Ok(n)).DisableValidation(); // opt out
```

Verified: `{"title":"ab","priority":9}` → **400** with errors for both `Title` and `Priority` on the first endpoint; **200** on the one with `.DisableValidation()`. It uses a source generator and works with records' parameter attributes.

### `EndpointFilterInvocationContext` — what's inside

| Member | Use |
|---|---|
| `HttpContext` | request, user, headers, services |
| `Arguments` | the handler's arguments (after binding) — inspect or modify |
| `GetArgument<T>(index)` | typed access by position |

⚠️ **Interview trap:** "Can I use MVC `IActionFilter` on minimal APIs?" — No. MVC filters run only inside MVC (controllers/Razor Pages). Minimal APIs use **`IEndpointFilter`**. (Endpoint filters *can't* be used on controllers either — use action filters there. See [Chapter 8](08-filters.md).)

🎯 **Say this in the interview:** "Endpoint filters are the minimal-API equivalent of action filters: they run around the handler, can read the bound arguments, short-circuit by returning a result, or post-process the result. I use them for validation, logging and API keys, applied per endpoint or to a whole MapGroup."

---

## 7.7 Security: `RequireAuthorization` and friends

```csharp
app.MapGet("/me", (ClaimsPrincipal user) => user.Identity!.Name)
   .RequireAuthorization();                        // any authenticated user

app.MapDelete("/api/products/{id:int}", DeleteProduct)
   .RequireAuthorization("AdminOnly");             // named policy (sample app)

app.MapGet("/health-lite", () => "ok")
   .AllowAnonymous();                              // override a group's RequireAuthorization

app.MapPost("/api/orders", CreateOrder)
   .RequireRateLimiting("fixed")                   // sample app's limiter
   .RequireCors("Frontend");
```

The middleware (`UseAuthentication`, `UseAuthorization`) is the same as for controllers. Attributes like `[Authorize]` also work on **named handler methods** or on the lambda itself: `app.MapGet("/x", [Authorize] () => ...)`.

---

## 7.8 Minimal APIs vs controllers

| Topic | Controllers | Minimal APIs |
|---|---|---|
| Style | Class per resource, methods = actions | `Map*` + lambda/method |
| Boilerplate | More (class, attributes, base class) | Very little |
| Routing | Attribute routing (`[Route]`, `[HttpGet]`) | In the `Map*` call; `MapGroup` for prefixes |
| Binding | `[ApiController]` inference | Inference by name/type, `[AsParameters]` |
| Validation | Automatic with `[ApiController]` | Manual / filters up to .NET 9; **`AddValidation()` in .NET 10** |
| Filters | MVC filters (auth, resource, action, exception, result) | Endpoint filters only |
| Model state, `ModelState.AddModelError` | ✅ | ❌ (return `ValidationProblem` yourself) |
| Content negotiation (XML etc.) | ✅ formatters | ❌ JSON only (custom `IResult` possible) |
| OpenAPI | `[ProducesResponseType]` | `TypedResults` (automatic) |
| Performance | Fast | Slightly faster, less allocation |
| **Native AOT** | ❌ not supported | ✅ supported (with `CreateSlimBuilder` + source-generated JSON) |
| Best for | Large apps, teams used to MVC, need MVC features | Microservices, small/medium APIs, AOT, new projects |

**When to choose which?**

- Choose **controllers** when: big existing MVC codebase, you rely on MVC filters / model binders / formatters / `ModelState`, or team convention.
- Choose **minimal APIs** when: new service, small focused APIs, you want maximum performance or **Native AOT** (fast startup, small containers, serverless).
- **You can mix both** in one app — the sample app does exactly that (`ProductsController` + `CategoryEndpoints`).

### AOT note

**Native AOT** compiles your app ahead-of-time to native code: very fast startup and small memory — great for containers/serverless. Requirements: minimal APIs (MVC isn't supported), no runtime reflection-heavy libraries, and **source-generated JSON**:

```csharp
var builder = WebApplication.CreateSlimBuilder(args);
builder.Services.ConfigureHttpJsonOptions(o =>
    o.SerializerOptions.TypeInfoResolverChain.Insert(0, AppJsonContext.Default));

[JsonSerializable(typeof(Todo[]))]
internal partial class AppJsonContext : JsonSerializerContext { }
```

and `<PublishAot>true</PublishAot>` in the `.csproj` (template: `dotnet new webapiaot`). EF Core's AOT support is still limited (precompiled queries are experimental) — say so if asked.

⚠️ **Interview trap:** "Minimal APIs are only for demos / can't do real apps." — Wrong since .NET 7: groups, filters, typed results, OpenAPI, auth, rate limiting, and in .NET 10 validation. Large production systems use them.

⚠️ **Interview trap:** "Minimal APIs are much faster, so always use them." — The difference is small for most apps; your database calls dominate. Choose for **maintainability** and **features**, not micro-benchmarks.

🎯 **Say this in the interview:** "Minimal APIs are lighter and support Native AOT; controllers give the full MVC feature set like filters, ModelState and content negotiation. For a new microservice I'd pick minimal APIs organised with MapGroup and extension methods; for an existing MVC-heavy codebase I'd stay with controllers. Both can live in the same app."

---

## Try it yourself

1. Write a minimal API `GET /api/products/{id}` that returns 200 with a `ProductDto` or 404, using a union return type.

<details><summary>Answer</summary>

```csharp
app.MapGet("/api/products/{id:int}", async Task<Results<Ok<ProductDto>, NotFound>> (int id, AppDbContext db, CancellationToken ct) =>
{
    ProductDto? p = await db.Products.AsNoTracking()
        .Where(x => x.Id == id)
        .Select(x => new ProductDto(x.Id, x.Name, x.Price, x.Stock, x.Category!.Name))
        .FirstOrDefaultAsync(ct);
    return p is null ? TypedResults.NotFound() : TypedResults.Ok(p);
});
```
</details>

2. In `app.MapGet("/api/orders", (OrderFilter filter) => ...)`, `OrderFilter` is a class with `Status` and `FromDate`. Calling `GET /api/orders?status=Paid` fails. Why, and what's the fix?

<details><summary>Answer</summary>

A complex type is inferred as the **JSON body**, and GET has no body. Use `([AsParameters] OrderFilter filter)` so each property binds from the query string (or bind simple parameters individually).
</details>

3. Put all `/api/admin/*` endpoints behind the `AdminOnly` policy and a timing filter without repeating code.

<details><summary>Answer</summary>

```csharp
var admin = app.MapGroup("/api/admin")
    .RequireAuthorization("AdminOnly")
    .AddEndpointFilter<TimingFilter>()
    .WithTags("Admin");
admin.MapGet("/stats", GetStats);
admin.MapPost("/reindex", Reindex);
```
</details>

4. Write an endpoint filter that rejects requests whose `X-Tenant-Id` header is missing with a 400 ProblemDetails.

<details><summary>Answer</summary>

```csharp
public class RequireTenantFilter : IEndpointFilter
{
    public async ValueTask<object?> InvokeAsync(EndpointFilterInvocationContext ctx, EndpointFilterDelegate next)
    {
        if (!ctx.HttpContext.Request.Headers.TryGetValue("X-Tenant-Id", out var t) || string.IsNullOrWhiteSpace(t))
            return TypedResults.Problem(statusCode: 400, title: "X-Tenant-Id header is required.");
        return await next(ctx);
    }
}
// .AddEndpointFilter<RequireTenantFilter>();
```
</details>

5. Why does OpenAPI show only "200 OK" for `IResult (int id) => id > 0 ? Results.Ok(x) : Results.NotFound()`? Give two fixes.

<details><summary>Answer</summary>

`Results.*` returns `IResult`, so no type information exists at compile time. Fix 1: use `TypedResults` with a `Results<Ok<T>, NotFound>` return type. Fix 2: keep `Results` but add metadata: `.Produces<T>(200).Produces(404)`.
</details>

6. Your team wants Native AOT for a new service. Controllers or minimal APIs? What else changes?

<details><summary>Answer</summary>

Minimal APIs (MVC/controllers aren't AOT-compatible). Use `WebApplication.CreateSlimBuilder`, `<PublishAot>true</PublishAot>`, source-generated `JsonSerializerContext`, avoid reflection-heavy libraries, and check that dependencies (e.g. EF Core) support AOT — consider Dapper/AOT-friendly data access.
</details>

---

## Quick revision

- Minimal API = `app.MapGet/MapPost/...(route, handler)`; no controller class.
- Binding inference: route name → route; simple type → query; DI type → service; special types injected; complex type → body. Headers need `[FromHeader]`.
- `[AsParameters]` binds an object's properties from route/query/header.
- Non-nullable parameters are required → missing = 400.
- `Results.*` → `IResult` (no metadata); **`TypedResults.*` → concrete types** → automatic OpenAPI + easy unit tests.
- `Results<Ok<T>, NotFound>` union return type (declare it before the lambda's parameter list).
- `MapGroup` = shared prefix + shared auth/filters/tags; groups can nest.
- Organise endpoints with `MapXxxEndpoints()` extension methods (sample: `MapCategoryEndpoints`).
- `IEndpointFilter` / `AddEndpointFilter` = minimal-API filter; short-circuit by returning a result.
- **.NET 10: `AddValidation()`** adds automatic DataAnnotations validation (opt out with `.DisableValidation()`).
- `RequireAuthorization("Policy")`, `AllowAnonymous()`, `RequireRateLimiting`, `CacheOutput`.
- Controllers = full MVC features; minimal APIs = lighter + **Native AOT**; both can coexist.

---

# 8. Filters

> **Why interviewers ask this:** Filters are the "MVC-only middleware" of ASP.NET Core. Interviewers use them to check whether you understand the request pipeline *inside* MVC, how cross-cutting concerns (logging, validation, error mapping, caching, auth checks) are implemented without copy-paste, and whether you know the difference between a filter and a middleware. Questions about `ServiceFilter` vs `TypeFilter` also test your understanding of Dependency Injection.

All code in this chapter was compiled and run against .NET 10. The full verified file is reproduced piece-by-piece below, and the sample app (samples/ShopApi) uses a global action filter (`Filters/LogActionFilter.cs`).

---

## 8.1 What is a filter? (simple words)

A **filter** is a piece of code that runs **before and/or after a specific stage** of MVC processing a request for a controller action.

🏢 **Analogy – an office building:**

- **Middleware** = the security guard at the **main gate**. Every visitor passes through, whether they go to HR, Finance or the cafeteria. The guard knows nothing about *which room* or *which form* you are bringing.
- **Filters** = the receptionist **inside a specific department**. The receptionist knows *which officer* you will meet (the action), *which form* you filled in (the bound model), and can inspect the officer's reply before it goes out (the result).

So filters run **only for MVC endpoints (controllers / Razor Pages)** and have **rich context**: action name, arguments, `ModelState`, the `IActionResult` that will be returned, etc.

Typical uses:

| Cross-cutting concern | Typical filter type |
|---|---|
| Custom API-key / "is this tenant allowed" check | Authorization filter |
| Response caching, idempotency, short-circuit before model binding | Resource filter |
| Logging, timing, validating `ModelState`, changing action arguments | Action filter |
| Turning exceptions from actions into nice HTTP responses | Exception filter |
| Adding headers, wrapping/formatting results | Result filter |

🎯 **Say this in the interview:** "Filters are MVC's own pipeline around action execution. Middleware wraps the whole app; filters wrap a controller action and get MVC context like the action descriptor, bound arguments, ModelState and the IActionResult."

---

## 8.2 The filter pipeline

When a request has been routed to a controller action, MVC runs the **filter pipeline**:

```text
Request ─► [Middleware ... UseRouting ... UseAuthorization ...] ─► MVC endpoint
                                                                     │
  ┌──────────────────────────────────────────────────────────────────┘
  ▼
┌──────────────────────────┐
│ 1. Authorization filters │  "Are you allowed?"          (can short-circuit)
└────────────┬─────────────┘
             ▼
┌──────────────────────────┐
│ 2. Resource filters      │  before ──┐                  (can short-circuit:
└────────────┬─────────────┘           │                   caching, idempotency)
             ▼                         │
     ( Model binding + validation )    │
             ▼                         │
┌──────────────────────────┐           │
│ 3. Action filters        │  before   │                  (can short-circuit)
│      ►► ACTION RUNS ◄◄   │           │
│    Action filters        │  after    │
└────────────┬─────────────┘           │
             ▼   (if the action / action filters threw)
┌──────────────────────────┐           │
│ 4. Exception filters     │  turn exception into result │
└────────────┬─────────────┘           │
             ▼                         │
┌──────────────────────────┐           │
│ 5. Result filters        │  before   │                  (around IActionResult
│   ►► RESULT EXECUTES ◄◄  │  (body written)              execution)
│    Result filters        │  after    │
└────────────┬─────────────┘           │
             ▼                         │
     Resource filters  after ◄─────────┘
             ▼
Response ◄── back through middleware
```

Key points:

1. **Authorization** runs first – nothing else should run for an unauthorized user.
2. **Resource** filters run **before model binding** – that's why they are perfect for caching/idempotency: you can return a cached response without even reading the body.
3. **Action** filters run **after model binding** – they can see and change `context.ActionArguments` and `ModelState`.
4. **Exception** filters only handle exceptions thrown by **action filters, the action, or model binding**… not by result execution or middleware.
5. **Result** filters wrap the execution of the `IActionResult` (when JSON is written). They only run if an action (or action filter) produced a result – see 8.8.

There are also two **filter *categories* (not stages)** you may hear about:

- **Endpoint filters** (`IEndpointFilter`) – for **minimal APIs** (Chapter 7). Not part of this MVC pipeline.
- **Page filters** (`IPageFilter`) – Razor Pages only.

⚠️ **Interview trap:** "Exception filters catch all exceptions in the app." ❌ They only catch exceptions from the MVC action pipeline (model binding, action filters, the action). Exceptions in middleware, in result execution (serialization), or in minimal API endpoints are **not** seen. For global error handling use `UseExceptionHandler` + `IExceptionHandler` / ProblemDetails (Chapter 11).

---

## 8.3 Sync vs async filter interfaces

Every stage has a **sync** and an **async** interface:

| Stage | Sync interface | Async interface | Attribute base class |
|---|---|---|---|
| Authorization | `IAuthorizationFilter` | `IAsyncAuthorizationFilter` | – (implement interface + `Attribute`) |
| Resource | `IResourceFilter` | `IAsyncResourceFilter` | – |
| Action | `IActionFilter` | `IAsyncActionFilter` | `ActionFilterAttribute` (implements both + result) |
| Exception | `IExceptionFilter` | `IAsyncExceptionFilter` | `ExceptionFilterAttribute` |
| Result | `IResultFilter` | `IAsyncResultFilter` | `ResultFilterAttribute` |
| Result (always) | `IAlwaysRunResultFilter` | `IAsyncAlwaysRunResultFilter` | – |

**Sync** filters have two methods: `OnXxxExecuting` (before) and `OnXxxExecuted` (after).

**Async** filters have **one** method that receives a `next` delegate – the same "before / `await next()` / after" shape as middleware:

```csharp
// Sync: two methods
public class SyncLog : IActionFilter
{
    public void OnActionExecuting(ActionExecutingContext context) { /* before */ }
    public void OnActionExecuted(ActionExecutedContext context)  { /* after  */ }
}

// Async: one method, call next() exactly once
public class AsyncLog : IAsyncActionFilter
{
    public async Task OnActionExecutionAsync(ActionExecutingContext context, ActionExecutionDelegate next)
    {
        // before
        ActionExecutedContext executed = await next();
        // after (executed.Exception, executed.Result available here)
    }
}
```

Rules:

- Use **async** when you do I/O (DB, cache, HTTP call) – never block with `.Result`.
- **Implement only one** of sync/async. If a class implements both, the framework calls **only the async one**.
- In an async filter, if you want to short-circuit, **set `context.Result` and do NOT call `next()`**. Calling `next()` after setting a result is a bug.

⚠️ **Interview trap:** "I implemented both `IActionFilter` and `IAsyncActionFilter` so it works either way." ❌ Only the async method runs; the sync methods are silently ignored.

---

## 8.4 Each filter type with examples

### 8.4.1 Action filter – the most common one

**When:** logging, timing, auditing, checking/modifying action arguments, validating `ModelState` (if you don't use `[ApiController]`).

**Easy example – the sample app's global logging filter** (`samples/ShopApi/Filters/LogActionFilter.cs`):

```csharp
using Microsoft.AspNetCore.Mvc.Filters;

namespace ShopApi.Filters;

// Async action filter: runs code before and after every controller action.
public class LogActionFilter(ILogger<LogActionFilter> logger) : IAsyncActionFilter
{
    public async Task OnActionExecutionAsync(ActionExecutingContext context, ActionExecutionDelegate next)
    {
        string action = context.ActionDescriptor.DisplayName ?? "unknown";
        logger.LogDebug("Before {Action}", action);

        ActionExecutedContext executed = await next();   // runs the action (and inner filters)

        if (executed.Exception is null)
            logger.LogDebug("After {Action}", action);
    }
}
```

Registered globally in the sample's `Program.cs`:

```csharp
builder.Services.AddControllers(o => o.Filters.Add<LogActionFilter>());
```

**Complex example – timing filter that adds a response header:**

```csharp
using System.Diagnostics;
using Microsoft.AspNetCore.Mvc.Filters;

public class TimingFilter(ILogger<TimingFilter> logger) : IAsyncActionFilter
{
    public async Task OnActionExecutionAsync(ActionExecutingContext context, ActionExecutionDelegate next)
    {
        long start = Stopwatch.GetTimestamp();

        ActionExecutedContext executed = await next();        // run the action

        double ms = Stopwatch.GetElapsedTime(start).TotalMilliseconds;
        context.HttpContext.Response.Headers["X-Elapsed-Ms"] = ms.ToString("F1");
        logger.LogInformation("{Action} took {Ms:F1} ms (exception: {HasEx})",
            context.ActionDescriptor.DisplayName, ms, executed.Exception is not null);
    }
}
```

Why can we still set a header *after* `next()`? Because in an **action** filter, "after" means "after the action returned an `IActionResult`" – the result (JSON body) has **not been written yet**. That happens later in the result stage. Verified: the response contained a header like `X-Elapsed-Ms: 0.3`.

> Note: this measures only the action, not model binding or JSON serialization. To time the whole request, use middleware or OpenTelemetry.

**Validation action filter (short-circuit):**

```csharp
public class ValidateModelAttribute : ActionFilterAttribute
{
    public override void OnActionExecuting(ActionExecutingContext context)
    {
        if (!context.ModelState.IsValid)
            context.Result = new BadRequestObjectResult(new ValidationProblemDetails(context.ModelState));
        // setting Result = SHORT-CIRCUIT: the action does not run
    }
}
```

With `[ApiController]` you don't need this – the framework already adds an equivalent filter (`ModelStateInvalidFilter`) that returns 400 ValidationProblem (Chapter 6). This filter is useful in MVC controllers **without** `[ApiController]`, or when you disabled the automatic behaviour.

### 8.4.2 Authorization filter

**When:** a custom check that must run **before everything else** in MVC – e.g. a simple API key for a partner endpoint.

```csharp
public class ApiKeyAuthFilter(IConfiguration config) : IAuthorizationFilter
{
    public const string HeaderName = "X-Api-Key";

    public void OnAuthorization(AuthorizationFilterContext context)
    {
        string? expected = config["ApiKey"];
        if (!context.HttpContext.Request.Headers.TryGetValue(HeaderName, out var provided)
            || string.IsNullOrEmpty(expected)
            || provided != expected)
        {
            context.Result = new UnauthorizedResult();   // 401, pipeline stops here
        }
    }
}
```

Usage (it needs `IConfiguration` from DI, so we use `ServiceFilter` – see 8.5):

```csharp
builder.Services.AddScoped<ApiKeyAuthFilter>();

[HttpGet("secret")]
[ServiceFilter(typeof(ApiKeyAuthFilter))]
public IActionResult Secret() => Ok("top secret");
```

Verified: without header → `401` (with `[ApiController]` the empty 401 becomes a `application/problem+json` body); with `X-Api-Key: secret-123` → `200 "top secret"`.

⚠️ **Interview trap:** "I write my own authorization filters for roles/policies." ❌ For real authN/authZ use the built-in system: `[Authorize(Policy = "...")]`, policies, requirements and handlers (Chapter 10). Custom authorization filters are for niche checks; they also don't plug into policies, `IAuthorizationService`, or minimal APIs. Also, use a constant-time comparison (`CryptographicOperations.FixedTimeEquals`) for real secrets.

### 8.4.3 Resource filter

**When:** you want to run code **before model binding** and possibly **skip everything** – caching, idempotency, rejecting oversized requests, disabling form value providers for streaming uploads.

**Complex example – idempotency filter.** Payment APIs often accept an `Idempotency-Key` header: if the client retries the same POST (network glitch), the server must **not** charge twice – it replays the first response.

```csharp
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Mvc.Filters;
using Microsoft.Extensions.Caching.Memory;

public class IdempotencyFilter(IMemoryCache cache, ILogger<IdempotencyFilter> logger) : IAsyncResourceFilter
{
    private const string Header = "Idempotency-Key";

    public async Task OnResourceExecutionAsync(ResourceExecutingContext context, ResourceExecutionDelegate next)
    {
        if (!context.HttpContext.Request.Headers.TryGetValue(Header, out var key) || string.IsNullOrWhiteSpace(key))
        {
            context.Result = new BadRequestObjectResult($"Header '{Header}' is required.");
            return;
        }

        string cacheKey = $"idem:{context.HttpContext.Request.Path}:{key}";
        if (cache.TryGetValue(cacheKey, out ObjectResult? previous))
        {
            logger.LogInformation("Replaying response for idempotency key {Key}", key.ToString());
            context.Result = previous;          // short-circuit BEFORE model binding & action
            return;
        }

        ResourceExecutedContext executed = await next();

        if (executed.Result is ObjectResult { StatusCode: >= 200 and < 300 } ok)
            cache.Set(cacheKey, ok, TimeSpan.FromHours(24));
    }
}
```

```csharp
builder.Services.AddMemoryCache();
builder.Services.AddScoped<IdempotencyFilter>();

[HttpPost("payments")]
[ServiceFilter<IdempotencyFilter>]          // generic form, C# 11+/.NET 7+
public IActionResult Pay([FromBody] decimal amount)
    => Ok(new { paymentId = Guid.NewGuid(), amount });
```

Verified behaviour:

| Request | Result |
|---|---|
| POST without `Idempotency-Key` | `400 Header 'Idempotency-Key' is required.` |
| POST with key `abc` (1st time) | `200 {"paymentId":"7f2c…","amount":100}` |
| POST with key `abc` (2nd time) | `200` with the **same** `paymentId` – action not executed |

Production notes: use a **distributed** cache (Redis) so all servers share keys, include the user id in the key, and handle two concurrent requests with the same key (lock / "in progress" marker).

### 8.4.4 Exception filter

**When:** map *domain* exceptions thrown by actions to HTTP responses for a specific controller/action set.

```csharp
public class NotFoundException(string message) : Exception(message);

public class NotFoundExceptionFilter : IExceptionFilter
{
    public void OnException(ExceptionContext context)
    {
        if (context.Exception is NotFoundException nf)
        {
            context.Result = new NotFoundObjectResult(new ProblemDetails
            {
                Status = StatusCodes.Status404NotFound,
                Title = "Resource not found",
                Detail = nf.Message
            });
            context.ExceptionHandled = true;   // stop the exception from bubbling up
        }
    }
}
```

```csharp
[HttpGet("missing/{id:int}")]
[TypeFilter<NotFoundExceptionFilter>]
public IActionResult Missing(int id) => throw new NotFoundException($"Product {id} was not found.");
```

Verified: `GET /api/filters/missing/5` → `404` with `{"title":"Resource not found","status":404,"detail":"Product 5 was not found."}`.

Rules:

- Set `context.Result` **and** `context.ExceptionHandled = true` to "swallow" it. If you don't set `ExceptionHandled`, the exception keeps propagating to middleware.
- Exceptions you don't recognise → leave them alone; let the global exception handler log them and return 500.

| | Exception filter | Exception handler middleware (`UseExceptionHandler` / `IExceptionHandler`) |
|---|---|---|
| Scope | MVC actions only | Whole app (middleware, minimal APIs, MVC) |
| Catches errors in result serialization | ❌ | ✅ |
| Knows action/controller context | ✅ | ❌ (only `HttpContext`) |
| Recommended for global error handling | ❌ | ✅ |

🎯 **Say this in the interview:** "For global error handling I use `UseExceptionHandler` with `IExceptionHandler` and ProblemDetails because it covers the whole pipeline. Exception filters are fine for mapping domain exceptions in a specific MVC area, but they don't see middleware or serialization errors."

### 8.4.5 Result filter

**When:** do something right before / after the `IActionResult` is executed – add headers, wrap the response object in an envelope, change formatting.

```csharp
public class AddApiVersionHeaderFilter : IResultFilter
{
    public void OnResultExecuting(ResultExecutingContext context)
        => context.HttpContext.Response.Headers["X-Api-Version"] = "1.0";   // safe: body not written yet

    public void OnResultExecuted(ResultExecutedContext context)
    {
        // Too late to change headers here - the response has already started.
    }
}
```

**Complex example – wrapping all successful object results in an envelope:**

```csharp
public class EnvelopeResultFilter : IResultFilter
{
    public void OnResultExecuting(ResultExecutingContext context)
    {
        if (context.Result is ObjectResult { Value: not ProblemDetails } obj
            && obj.StatusCode is null or (>= 200 and < 300))
        {
            obj.Value = new { data = obj.Value, serverTime = DateTime.UtcNow };
        }
    }
    public void OnResultExecuted(ResultExecutedContext context) { }
}
```

Verified: `GET /api/env/3` returned `{"data":{"id":3},"serverTime":"2026-…Z"}`, while a `400` from a short-circuiting filter was left unwrapped (its status code is not 2xx).

⚠️ **Interview trap:** "I'll add the header in `OnResultExecuted`." ❌ By then the body is already being written and headers are sent – you get an exception or the header is silently lost. Modify headers in `OnResultExecuting` (or before `await next()` in the async version). You can check `context.HttpContext.Response.HasStarted`.

**`IAlwaysRunResultFilter`:** normal result filters are **skipped** when an authorization / resource filter short-circuits or an exception filter produced the result (see 8.7). If you need code that runs for **every** result (e.g. add a correlation header even to 401/404 problem responses), implement `IAlwaysRunResultFilter` / `IAsyncAlwaysRunResultFilter`.

---

## 8.5 Applying filters: attribute vs ServiceFilter vs TypeFilter (and DI)

There are **three ways** to attach a filter to a controller/action, plus global registration.

### 1) Filter **is** an attribute

```csharp
public class ValidateModelAttribute : ActionFilterAttribute { ... }

[ValidateModel]
public IActionResult Create(ProductDto dto) => ...
```

- ✅ Simplest, can take **constant** constructor arguments (`[Trace("Action")]`).
- ❌ **No constructor DI**: attributes are created by the runtime from metadata, so you can't inject `ILogger<T>` or `DbContext` in the constructor. (Workaround: `context.HttpContext.RequestServices.GetRequiredService<T>()` – service locator, avoid.)

### 2) `[ServiceFilter(typeof(T))]` – filter resolved **from DI**

```csharp
builder.Services.AddScoped<ApiKeyAuthFilter>();      // MUST register!

[ServiceFilter(typeof(ApiKeyAuthFilter))]            // or [ServiceFilter<ApiKeyAuthFilter>] (.NET 7+)
public IActionResult Secret() => Ok("top secret");
```

- ✅ Full constructor DI; **you control the lifetime** (Scoped/Singleton/Transient).
- ❌ Forgetting to register → `InvalidOperationException: No service for type 'ApiKeyAuthFilter' has been registered` at request time.

### 3) `[TypeFilter(typeof(T))]` – created with `ActivatorUtilities`

```csharp
public class RequireHeaderFilter(string headerName, ILogger<RequireHeaderFilter> logger) : IActionFilter
{
    public void OnActionExecuting(ActionExecutingContext context)
    {
        if (!context.HttpContext.Request.Headers.ContainsKey(headerName))
        {
            logger.LogWarning("Missing header {Header}", headerName);
            context.Result = new BadRequestObjectResult($"Header '{headerName}' is required.");
        }
    }
    public void OnActionExecuted(ActionExecutedContext context) { }
}

[HttpGet("tenant")]
[TypeFilter(typeof(RequireHeaderFilter), Arguments = ["X-Tenant-Id"])]
public IActionResult Tenant() => Ok("tenant ok");
```

- ✅ **No registration needed**; constructor dependencies come from DI, **and** you can pass extra non-DI arguments (`Arguments`).
- ❌ Arguments are not type-checked at compile time; a new instance per request by default (`IsReusable = false`).

Verified: `GET /api/filters/tenant` without header → `400 "Header 'X-Tenant-Id' is required."`; with header → `200 "tenant ok"`.

### 4) Global registration

```csharp
builder.Services.AddControllers(options =>
{
    options.Filters.Add<LogActionFilter>();          // by type: created via DI/ActivatorUtilities per request
    options.Filters.Add(new ValidateModelAttribute()); // by instance: SINGLE instance reused (must be thread-safe)
    options.Filters.AddService<TimingFilter>();      // resolved from DI (must be registered)
});
```

### Comparison table

| | Attribute filter | `ServiceFilter` | `TypeFilter` | `IFilterFactory` (advanced) |
|---|---|---|---|---|
| Constructor DI | ❌ | ✅ | ✅ | ✅ (you create it) |
| Must register in DI | – | ✅ yes | ❌ no | – |
| Extra constant arguments | ✅ | ❌ | ✅ `Arguments` | ✅ |
| Lifetime control | one instance per attribute | your DI registration | new per request | your code |
| Generic form (.NET 7+) | – | `[ServiceFilter<T>]` | `[TypeFilter<T>]` | – |

**Complex trick – attribute + DI with `IFilterFactory`:** gives you a nice attribute *and* DI.

```csharp
public class AuditAttribute(string area) : Attribute, IFilterFactory
{
    public bool IsReusable => false;

    public IFilterMetadata CreateInstance(IServiceProvider services)
        => ActivatorUtilities.CreateInstance<RequireHeaderFilter>(services, area);
}
```

(This is exactly how `ServiceFilterAttribute` and `TypeFilterAttribute` are implemented internally.)

⚠️ **Interview trap – captive dependency:** registering a filter as **Singleton** (or adding it as an instance globally) that depends on a **Scoped** service like `DbContext`. The singleton captures one `DbContext` forever → threading bugs and stale data. Register filters that use scoped services as **Scoped** (or use `TypeFilter`).

🎯 **Say this in the interview:** "Attribute filters can't use constructor injection. If the filter needs services I use `ServiceFilter`, which resolves it from DI so it must be registered, or `TypeFilter`, which creates it with `ActivatorUtilities` without registration and lets me pass extra arguments."

---

## 8.6 Scope and order

Filters can be applied at three **scopes**:

| Scope | How |
|---|---|
| Global | `AddControllers(o => o.Filters.Add<...>())` |
| Controller | attribute on the controller class |
| Action | attribute on the action method |

**Default order = "Russian doll" from outside to inside:** Global → Controller → Action on the way **in**, and the reverse on the way **out**.

Verified with a tracing filter applied at all three levels:

```csharp
public class TraceAttribute(string name) : ActionFilterAttribute
{
    public override void OnActionExecuting(ActionExecutingContext context) => Trace.Steps.Add($"{name} before");
    public override void OnActionExecuted(ActionExecutedContext context) => Trace.Steps.Add($"{name} after");
}

// Program.cs: o.Filters.Add<GlobalTraceFilter>();

[Trace("Controller")]
public class FiltersDemoController : ControllerBase
{
    [HttpGet("order"), Trace("Action")]
    public IActionResult OrderDemo() { Trace.Steps.Add("ACTION RUNS"); return Ok(); }
}
```

Actual output:

```text
Global before
Controller before
Action before
ACTION RUNS
Action after
Controller after
Global after
```

**Changing the order with `Order`:** filters implementing `IOrderedFilter` (all built-in attribute bases do) have an `Order` property, default `0`. **Lower Order runs first (outermost)**; scope only breaks ties.

```csharp
[HttpGet("order-first")]
[Trace("Action(Order=-1)", Order = -1)]
public IActionResult OrderFirst() { ... }
```

Actual output:

```text
Action(Order=-1) before
Global before
Controller before
ACTION RUNS
Controller after
Global after
Action(Order=-1) after
```

Important: `Order` only sorts filters **within the same stage**. An action filter with `Order = -1000` still runs *after* every authorization and resource filter.

⚠️ **Interview trap:** "Action-level filters always run first because they're the most specific." ❌ By default the **global** filter is the outermost; action-level is innermost. Use `Order` to change it.

---

## 8.7 Short-circuiting

**Short-circuit** = stop the pipeline early and send a response without running the action.

How: **set `context.Result`** in a "before" method (`OnAuthorization`, `OnResourceExecuting`, `OnActionExecuting`, `OnResultExecuting` → use `context.Cancel = true` for result filters). In async filters, set `context.Result` and **don't call `next()`**.

```csharp
public void OnActionExecuting(ActionExecutingContext context)
{
    if (context.ActionArguments.TryGetValue("id", out var v) && v is int id && id <= 0)
        context.Result = new BadRequestObjectResult("id must be positive");   // action will NOT run
}
```

What still runs after a short-circuit?

| Short-circuited by | Action runs? | Action filters? | Result filters run? | Always-run result filters? |
|---|---|---|---|---|
| Authorization filter | ❌ | ❌ | ❌ | ✅ |
| Resource filter | ❌ | ❌ | ❌ | ✅ |
| Action filter | ❌ | outer ones see "after" | ✅ | ✅ |
| Exception filter (handled) | (threw) | – | ❌ | ✅ |

The last row was **verified**: for `GET /api/filters/missing/5` with both `NotFoundExceptionFilter` and `AddApiVersionHeaderFilter`, the response was a 404 ProblemDetails **without** `X-Api-Version` – the result filter didn't run – while `X-Elapsed-Ms` (action filter, outer) was present.

---

## 8.8 Changing action arguments and reading results

Action filters can **read and modify** arguments before the action runs, and inspect the result afterwards:

```csharp
public class TrimStringsFilter : IActionFilter
{
    public void OnActionExecuting(ActionExecutingContext context)
    {
        foreach (var key in context.ActionArguments.Keys.ToList())
            if (context.ActionArguments[key] is string s)
                context.ActionArguments[key] = s.Trim();
    }

    public void OnActionExecuted(ActionExecutedContext context)
    {
        if (context.Result is ObjectResult { Value: null })
            context.Result = new NotFoundResult();       // turn "Ok(null)" into 404
    }
}
```

In `OnActionExecuted` you can also inspect `context.Exception` and set `context.ExceptionHandled = true` – but prefer exception filters / handler middleware for that.

---

## 8.9 Filters vs middleware

| | Middleware | MVC filters |
|---|---|---|
| Runs for | **every** request (static files, minimal APIs, MVC, health checks) | only MVC controller actions / Razor Pages |
| Registered | `app.UseXxx()` in `Program.cs`; order = code order | globally / controller / action; order = stage + scope + `Order` |
| Knows about | `HttpContext` (and the endpoint after `UseRouting`) | action descriptor, `ActionArguments`, `ModelState`, `IActionResult` |
| Runs before model binding? | ✅ always | Authorization/Resource ✅, Action ❌ |
| Can be applied to one action only | awkward (check endpoint metadata) | ✅ attribute |
| Typical use | exception handling, HTTPS, CORS, auth, compression, rate limiting, request logging | validation, per-action logging/auditing, domain exception mapping, result shaping |

**Rule of thumb:** if it's about **HTTP in general** → middleware. If it needs **MVC knowledge** (model, action, result) or applies to **some actions only** → filter.

🎯 **Say this in the interview:** "Middleware is app-wide and only knows HttpContext. Filters run inside MVC after routing, can be applied per controller or action, and see MVC context like ModelState and the action result. I use middleware for global concerns like error handling, and filters for MVC-specific concerns like validation or auditing."

---

## 8.10 Endpoint filters (minimal APIs) – quick reminder

MVC filters **don't run for minimal APIs**. Minimal APIs have their own single type: `IEndpointFilter` (.NET 7+), covered in Chapter 7.

```csharp
public class RequireTenantFilter : IEndpointFilter
{
    public async ValueTask<object?> InvokeAsync(EndpointFilterInvocationContext ctx, EndpointFilterDelegate next)
    {
        if (!ctx.HttpContext.Request.Headers.TryGetValue("X-Tenant-Id", out var t) || string.IsNullOrWhiteSpace(t))
            return TypedResults.Problem(statusCode: 400, title: "X-Tenant-Id header is required.");
        return await next(ctx);     // like await next() in an async MVC filter
    }
}

app.MapGroup("/api/public").AddEndpointFilter<RequireTenantFilter>();
```

| | MVC filters | Endpoint filters |
|---|---|---|
| Works with | controllers, Razor Pages | minimal APIs (and route groups) |
| Stages | 5 (auth, resource, action, exception, result) | 1 (wraps handler; runs after binding) |
| Access to arguments | `context.ActionArguments` (dictionary by name) | `ctx.GetArgument<T>(index)` / `ctx.Arguments` |
| Apply | attributes / global options | `.AddEndpointFilter<T>()` on endpoint or group |

---

## 8.11 Putting it together – a realistic controller

```csharp
[ApiController]
[Route("api/filters")]
[ServiceFilter<TimingFilter>]                       // controller-level: time every action
public class FiltersDemoController : ControllerBase
{
    [HttpPost("payments")]
    [ServiceFilter<IdempotencyFilter>]              // resource filter: replay duplicates
    public IActionResult Pay([FromBody] decimal amount)
        => Ok(new { paymentId = Guid.NewGuid(), amount });

    [HttpGet("secret")]
    [ServiceFilter(typeof(ApiKeyAuthFilter))]        // authorization filter: X-Api-Key
    public IActionResult Secret() => Ok("top secret");

    [HttpGet("tenant")]
    [TypeFilter(typeof(RequireHeaderFilter), Arguments = ["X-Tenant-Id"])]
    public IActionResult Tenant() => Ok("tenant ok");

    [HttpGet("missing/{id:int}")]
    [TypeFilter<NotFoundExceptionFilter>]           // exception -> 404 ProblemDetails
    [TypeFilter<AddApiVersionHeaderFilter>]         // result filter
    public IActionResult Missing(int id) => throw new NotFoundException($"Product {id} was not found.");
}
```

`Program.cs` registrations used for the verified run:

```csharp
builder.Services.AddMemoryCache();
builder.Services.AddScoped<TimingFilter>();
builder.Services.AddScoped<IdempotencyFilter>();
builder.Services.AddScoped<ApiKeyAuthFilter>();
builder.Services.AddControllers(o => o.Filters.Add<GlobalTraceFilter>());
// appsettings: "ApiKey": "secret-123"
```

For `POST /api/filters/payments` the order is: global/controller *authorization* (none) → `IdempotencyFilter` (resource) → model binding of `amount` → `GlobalTraceFilter` + `TimingFilter` (action) → `Pay` → result execution → `IdempotencyFilter` "after" caches the result.

---

## 8.12 Common interview questions (quick answers)

| Question | Short answer |
|---|---|
| Name the filter types in order | Authorization → Resource → (model binding) → Action → Exception → Result |
| Which filter runs before model binding? | Authorization and Resource |
| Where do you cache a full response? | Resource filter (or Output Caching middleware, .NET 7+) |
| How do you short-circuit? | Set `context.Result` (don't call `next()` in async) |
| How to use DI in a filter? | `ServiceFilter` (registered) or `TypeFilter` (not registered) or global `Filters.Add<T>()` |
| Do MVC filters run for minimal APIs? | No – use `IEndpointFilter` |
| How to change filter order? | `Order` property (lower = first), otherwise Global → Controller → Action |
| Exception filter vs `UseExceptionHandler`? | Filter = MVC-only, has action context; middleware = global, recommended |
| Where to add a response header? | Before result executes (`OnResultExecuting` or action filter after `next()`) |
| What does `[ApiController]` add? | `ModelStateInvalidFilter` (auto 400), plus binding inference etc. |

---

## Try it yourself

**1.** You need to log the request body *size* and reject requests whose `Content-Length` is above 1 MB **before** the JSON is deserialized, only for one controller. Which filter type?

<details><summary>Answer</summary>

A **resource filter** (`IResourceFilter` / `IAsyncResourceFilter`) applied to the controller – it runs before model binding, so you can set `context.Result = new StatusCodeResult(413)` and the body is never read. (For the whole app, Kestrel's `MaxRequestBodySize` or the `[RequestSizeLimit]` attribute is simpler.)
</details>

**2.** This filter throws `InvalidOperationException: No service for type 'AuditFilter' has been registered`. Why, and give two fixes.

```csharp
[ServiceFilter(typeof(AuditFilter))]
public IActionResult Delete(int id) => NoContent();
```

<details><summary>Answer</summary>

`ServiceFilter` resolves the filter from the DI container, so it must be registered. Fix 1: `builder.Services.AddScoped<AuditFilter>();`. Fix 2: use `[TypeFilter(typeof(AuditFilter))]`, which creates it with `ActivatorUtilities` (dependencies still from DI) without registration.
</details>

**3.** Predict the output order. Global filter `G`, controller attribute `C`, action attribute `A` with `Order = -5`.

<details><summary>Answer</summary>

```text
A before, G before, C before, ACTION, C after, G after, A after
```

Lower `Order` runs first (outermost); `G` and `C` both have Order 0, so the scope (global before controller) breaks the tie.
</details>

**4.** A developer adds `X-Request-Id` in `OnResultExecuted` of a result filter but the header never appears (or an exception "Headers are read-only, response has already started" is thrown). Fix it.

<details><summary>Answer</summary>

Headers must be set **before** the response body starts. Move the code to `OnResultExecuting` (or in an `IAsyncResultFilter`, before `await next()`), or use `Response.OnStarting(...)`. If it must appear even on short-circuited / exception responses, implement `IAlwaysRunResultFilter` or use middleware.
</details>

**5.** Write an async action filter that rejects the request with 400 if any `int` action argument named `id` is ≤ 0, otherwise runs the action.

<details><summary>Answer</summary>

```csharp
public class PositiveIdFilter : IAsyncActionFilter
{
    public async Task OnActionExecutionAsync(ActionExecutingContext context, ActionExecutionDelegate next)
    {
        if (context.ActionArguments.TryGetValue("id", out var v) && v is int id && id <= 0)
        {
            context.Result = new BadRequestObjectResult("id must be positive");
            return;                       // do NOT call next()
        }
        await next();
    }
}
```
</details>

**6.** Your team has a minimal API endpoint and a controller action; both should return 404 ProblemDetails when `NotFoundException` is thrown. Where should this logic live?

<details><summary>Answer</summary>

In **global exception handling middleware** (`app.UseExceptionHandler()` + an `IExceptionHandler` that maps `NotFoundException` to 404 ProblemDetails, Chapter 11). An MVC exception filter would not catch exceptions from the minimal API endpoint.
</details>

---

## Quick revision

- Filters = MVC's inner pipeline around a controller action; middleware = app-wide pipeline.
- Order of stages: **Authorization → Resource → model binding → Action → Exception → Result**.
- Resource filters run before model binding → caching, idempotency, size limits.
- Action filters see `ActionArguments` and `ModelState`; `[ApiController]` adds an automatic ModelState 400 filter.
- Short-circuit by setting `context.Result`; in async filters don't call `next()`.
- Sync = `OnXxxExecuting/OnXxxExecuted`; async = one method with `next`; implement only one (async wins).
- Attribute filters have no constructor DI → use `ServiceFilter` (must register) or `TypeFilter` (no registration, can pass `Arguments`).
- Default order: Global → Controller → Action in, reverse out; lower `Order` runs first.
- Exception filters only see MVC action exceptions; set `ExceptionHandled = true`; prefer `UseExceptionHandler` globally.
- Result filters don't run after authorization/resource short-circuits or handled exceptions – use `IAlwaysRunResultFilter`.
- Set headers before the response starts (`OnResultExecuting`), never in `OnResultExecuted`.
- Minimal APIs use `IEndpointFilter` – MVC filters do not apply to them.

---

# 9. Entity Framework Core

> **Why interviewers ask this:** Almost every ASP.NET Core job uses a database, and EF Core is the default way to talk to it. Interviewers use EF questions to find out whether you *understand what SQL your code produces*. The classic filters are: "What is the N+1 problem?", "IQueryable vs IEnumerable?", "What does AsNoTracking do?", "How do you handle concurrency?", "How do you call a stored procedure?" and "Is DbContext thread-safe?". A 2-YOE developer is expected to answer all of these confidently.

All EF code in this chapter was **compiled and run** with EF Core 10 against SQLite in-memory, with SQL logging turned on – the SQL you see below is the **actual SQL EF generated**. Parts marked **(SQL Server)** were compile-checked only (no SQL Server was available), and the `dotnet ef` CLI commands are shown but were not executed. The sample app (samples/ShopApi) uses EF Core with SQLite: see `Data/AppDbContext.cs` and `Services/ProductService.cs`.

---

## 9.1 What is an ORM? What is EF Core?

**Simple words:** your C# code thinks in **objects** (`Product`, `Order` with a `List<OrderItem>`), the database thinks in **tables and rows**. An **ORM (Object-Relational Mapper)** is a translator between the two worlds.

🌍 **Analogy:** You speak English (C#), the database speaks only SQL. EF Core is a **professional interpreter**: you say "give me all products cheaper than 50, sorted by name" in LINQ, the interpreter says `SELECT ... WHERE Price < 50 ORDER BY Name` to the database, and translates the answer back into `List<Product>`. Like any interpreter, it is great 95% of the time – but you must check what it actually said when something is slow.

EF Core gives you:

| Feature | What it means |
|---|---|
| LINQ → SQL translation | write `db.Products.Where(p => p.Price < 50)` instead of SQL strings |
| Materialization | rows → C# objects automatically |
| Change tracking | it remembers what you loaded and generates `UPDATE` only for changed columns |
| Unit of Work | `SaveChanges()` sends all inserts/updates/deletes in one transaction |
| Migrations | evolve DB schema from C# model, versioned in source control |
| Providers | SQL Server, PostgreSQL, SQLite, MySQL, Cosmos DB, In-Memory (tests) |

**Easy example – end to end:**

```csharp
public class Product
{
    public int Id { get; set; }                 // "Id" => primary key by convention
    public string Name { get; set; } = "";
    public decimal Price { get; set; }
}

public class ShopContext(DbContextOptions<ShopContext> options) : DbContext(options)
{
    public DbSet<Product> Products => Set<Product>();
}

// Program.cs
builder.Services.AddDbContext<ShopContext>(o =>
    o.UseSqlServer(builder.Configuration.GetConnectionString("Shop")));

// anywhere (e.g. controller/service) via DI
List<Product> cheap = await db.Products
    .Where(p => p.Price < 50)
    .OrderBy(p => p.Name)
    .ToListAsync();
```

🎯 **Say this in the interview:** "EF Core is Microsoft's ORM. A DbContext represents a session with the database and a unit of work; DbSets represent tables. I write LINQ against DbSets, EF translates it to SQL, materializes the results, tracks changes and SaveChanges persists them in a transaction."

---

## 9.2 DbContext and DbSet

| Type | Think of it as | Responsibilities |
|---|---|---|
| `DbContext` | a **shopping session / unit of work** | DB connection management, change tracker, `SaveChanges`, model configuration (`OnModelCreating`) |
| `DbSet<T>` | a **table** you can query | `IQueryable<T>` for LINQ, `Add`, `Remove`, `Find` |

### DbContext is Scoped and NOT thread-safe

`AddDbContext` registers the context as **Scoped** = **one instance per HTTP request**. That is exactly right because:

- A DbContext is **cheap to create** and meant to be **short-lived** (one unit of work).
- It is **not thread-safe**: you must not run two operations on the same instance at the same time.
- Its change tracker grows with everything you load – a long-lived (singleton) context becomes a memory leak with stale data.

From the sample app's `Program.cs`:

```csharp
// EF Core (DbContext is SCOPED = one per HTTP request)
builder.Services.AddDbContext<AppDbContext>(o =>
    o.UseSqlite(builder.Configuration.GetConnectionString("Shop")));
```

**The classic thread-safety bug:**

```csharp
// ❌ BAD – two queries on the SAME context in parallel
var productsTask = db.Products.ToListAsync();
var ordersTask   = db.Orders.ToListAsync();
await Task.WhenAll(productsTask, ordersTask);
// InvalidOperationException: A second operation was started on this context instance
// before a previous operation completed...
// (SQL Server; SQLite's async methods actually run synchronously, so the bug may stay hidden in local tests)

// ✅ GOOD – await one after the other (simplest)
var products = await db.Products.ToListAsync();
var orders   = await db.Orders.ToListAsync();

// ✅ GOOD – truly parallel: a separate context per task via IDbContextFactory
builder.Services.AddDbContextFactory<ShopContext>(o => o.UseSqlServer(cs));

public class ReportService(IDbContextFactory<ShopContext> factory)
{
    public async Task<(int, int)> CountsAsync()
    {
        async Task<int> Count<T>(Func<ShopContext, IQueryable<T>> q)
        {
            await using var db = await factory.CreateDbContextAsync();
            return await q(db).CountAsync();
        }
        var t1 = Count(db => db.Products);
        var t2 = Count(db => db.Orders);
        return (await t1, await t2);
    }
}
```

Use `IDbContextFactory<T>` also in **Blazor Server**, **background services** (`BackgroundService` is a singleton – create a scope or use the factory) and anywhere without a natural request scope.

⚠️ **Interview trap:** "I'll inject DbContext into a singleton service / `BackgroundService`." ❌ That's a **captive dependency** – one context for the whole app lifetime, shared across threads. Use `IServiceScopeFactory.CreateScope()` or `IDbContextFactory<T>`.

⚠️ **Interview trap:** "DbContext should be disposed after every query / wrapped in `using` in controllers." ❌ When injected via DI the container disposes it at the end of the request. You only `using`/`await using` contexts **you create yourself** (factory, `new`).

---

## 9.3 Code-first vs database-first

| | Code-first | Database-first |
|---|---|---|
| Source of truth | C# classes + `DbContext` | the existing database |
| Schema changes | **migrations** generated from model changes | DBA changes DB → you re-scaffold |
| Tooling | `dotnet ef migrations add` | `dotnet ef dbcontext scaffold "<conn>" Microsoft.EntityFrameworkCore.SqlServer` |
| Best for | new projects, team owns the DB | legacy/shared DBs, DBA-owned schema (e.g. the ShopDB from the companion SQL book) |

**Database-first scaffolding** (CLI, not executed here):

```bash
dotnet ef dbcontext scaffold "Server=.;Database=ShopDB;Trusted_Connection=True;TrustServerCertificate=True" \
    Microsoft.EntityFrameworkCore.SqlServer \
    --output-dir Entities --context-dir Data --context ShopDbContext \
    --table Customers --table Products --table Orders --table OrderItems --table Categories
```

This generates entity classes and a `ShopDbContext` with Fluent API configuration. Generated classes are `partial` – put your additions in separate partial files so re-scaffolding doesn't overwrite them.

---

## 9.4 Migrations

**Simple words:** a migration is a **versioned C# script** that says "how to go from schema version N to N+1" (`Up`) and back (`Down`). Like Git commits, but for your database schema.

The commands (need the `dotnet-ef` tool and the `Microsoft.EntityFrameworkCore.Design` package; **CLI commands, shown for reference, not executed in this book's verification**):

```bash
dotnet tool install --global dotnet-ef               # once

dotnet ef migrations add InitialCreate               # create a migration from model changes
dotnet ef migrations add AddProductStock
dotnet ef database update                            # apply all pending migrations
dotnet ef database update AddProductStock            # migrate up/down to a specific migration
dotnet ef migrations remove                          # delete the LAST migration (if not applied)
dotnet ef migrations list                            # show migrations and which are applied
dotnet ef migrations script --idempotent -o migrate.sql   # SQL script for production (safe to re-run)
dotnet ef migrations bundle                          # self-contained exe that applies migrations (CI/CD)
```

| Command | Purpose |
|---|---|
| `migrations add <Name>` | compares current model with the **model snapshot** and generates `Up`/`Down` code |
| `database update` | applies pending migrations; records them in `__EFMigrationsHistory` |
| `migrations remove` | removes the last unapplied migration and reverts the snapshot |
| `migrations script --idempotent` | generates SQL you can review and give to a DBA |
| `migrations bundle` | builds an executable for deployment pipelines |

A generated migration looks like this:

```csharp
public partial class AddProductStock : Migration
{
    protected override void Up(MigrationBuilder migrationBuilder)
    {
        migrationBuilder.AddColumn<int>(
            name: "Stock", table: "Products", type: "int", nullable: false, defaultValue: 0);
    }

    protected override void Down(MigrationBuilder migrationBuilder)
    {
        migrationBuilder.DropColumn(name: "Stock", table: "Products");
    }
}
```

Applying at startup (fine for dev/demos; for production prefer scripts/bundles controlled by the pipeline):

```csharp
using (var scope = app.Services.CreateScope())
{
    var db = scope.ServiceProvider.GetRequiredService<ShopContext>();
    await db.Database.MigrateAsync();      // applies pending migrations
}
```

| `EnsureCreated()` | `Migrate()` |
|---|---|
| Creates DB + schema from the current model if DB doesn't exist | Applies migrations incrementally |
| **No** migration history; can't evolve schema later | Keeps `__EFMigrationsHistory` |
| Good for tests, demos (the sample app uses it with SQLite) | Real applications |

⚠️ **Interview trap:** mixing `EnsureCreated()` and migrations. A DB created by `EnsureCreated` has no history table, so `Migrate()` later fails trying to create tables that already exist.

⚠️ **Interview trap:** "Migrations rename columns automatically." EF sees a rename as **drop + add** (data loss!) unless it can detect it; always **review** the generated migration and use `RenameColumn` when needed.

---

## 9.5 Configuring the model: conventions, data annotations, Fluent API

EF builds a **model** from three sources, in increasing priority:

1. **Conventions** – `Id`/`<Type>Id` is the key, `CategoryId` + `Category` navigation is a FK, `string` → `nvarchar(max)` (SQL Server), non-nullable types → `NOT NULL`.
2. **Data annotations** – attributes on the class: `[Key]`, `[Required]`, `[MaxLength(200)]`, `[Column("product_name")]`, `[Table]`, `[Timestamp]`, `[Precision(10,2)]`.
3. **Fluent API** – code in `OnModelCreating` – **wins** over annotations.

```csharp
// Data annotations
public class Category
{
    public int Id { get; set; }
    [Required, MaxLength(100)]
    public string Name { get; set; } = "";
}

// Equivalent Fluent API (from the sample app's AppDbContext)
b.Entity<Category>(e =>
{
    e.Property(c => c.Name).HasMaxLength(100).IsRequired();
    e.HasIndex(c => c.Name).IsUnique();       // annotation equivalent: [Index(nameof(Name), IsUnique = true)] on the class (EF Core 5+)
});
```

| | Data annotations | Fluent API |
|---|---|---|
| Where | on entity properties | `OnModelCreating` / `IEntityTypeConfiguration<T>` |
| Power | basic (keys, length, required, column names) | **everything**: composite keys, indexes, relationships, delete behavior, query filters, value conversions, owned types, table splitting |
| Entities stay "clean" (no EF references) | ❌ | ✅ |
| Also used by MVC validation | ✅ (`[Required]`, `[MaxLength]`) | ❌ |
| Priority | lower | **higher** |

**Organizing Fluent config** – one class per entity, picked up automatically (verified):

```csharp
public class ProductConfiguration : IEntityTypeConfiguration<Product>
{
    public void Configure(EntityTypeBuilder<Product> e)
    {
        e.ToTable("Products");
        e.HasKey(p => p.Id);
        e.Property(p => p.Name).HasMaxLength(200).IsRequired();
        e.Property(p => p.Price).HasPrecision(10, 2);
        e.Property(p => p.Version).IsConcurrencyToken();
        e.HasIndex(p => p.Name);

        e.HasOne(p => p.Category)
         .WithMany(c => c.Products)
         .HasForeignKey(p => p.CategoryId)
         .OnDelete(DeleteBehavior.Restrict);

        e.HasMany(p => p.Tags)
         .WithMany(t => t.Products)
         .UsingEntity(j => j.ToTable("ProductTags"));

        e.HasQueryFilter(p => !p.IsDeleted);
    }
}

public class ShopContext(DbContextOptions<ShopContext> options) : DbContext(options)
{
    public DbSet<Product> Products => Set<Product>();
    // ...
    protected override void OnModelCreating(ModelBuilder b)
    {
        b.ApplyConfigurationsFromAssembly(typeof(ShopContext).Assembly);
    }
}
```

⚠️ **Interview trap:** `decimal` without precision. On SQL Server EF defaults to `decimal(18,2)` and **logs a warning**; always configure money columns explicitly with `HasPrecision` / `[Precision]`.

🎯 **Say this in the interview:** "I prefer the Fluent API in separate `IEntityTypeConfiguration` classes because it can configure everything and keeps entities free of persistence attributes. Annotations are fine for simple things and double as validation, but Fluent API wins if both are used."

---

## 9.6 Relationships

The entities used for the rest of the chapter (verified model):

```csharp
public class Category
{
    public int Id { get; set; }
    public string Name { get; set; } = "";
    public virtual List<Product> Products { get; set; } = [];       // 1 category -> many products
}

public class Product
{
    public int Id { get; set; }
    public string Name { get; set; } = "";
    public decimal Price { get; set; }
    public int Stock { get; set; }
    public int CategoryId { get; set; }                             // FK
    public virtual Category? Category { get; set; }                 // reference navigation
    public bool IsDeleted { get; set; }                             // soft delete
    public Guid Version { get; set; } = Guid.NewGuid();             // concurrency token
    public virtual List<Tag> Tags { get; set; } = [];               // many-to-many
}

public class Tag
{
    public int Id { get; set; }
    public string Name { get; set; } = "";
    public virtual List<Product> Products { get; set; } = [];
}

public class Customer
{
    public int Id { get; set; }
    public string Name { get; set; } = "";
    public string Email { get; set; } = "";
    public virtual CustomerProfile? Profile { get; set; }           // 1-1
    public virtual List<Order> Orders { get; set; } = [];           // 1-many
}

public class CustomerProfile
{
    public int Id { get; set; }
    public string Address { get; set; } = "";
    public int CustomerId { get; set; }                             // FK lives on the dependent
    public virtual Customer? Customer { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public virtual Customer? Customer { get; set; }
    public DateTime OrderDate { get; set; }
    public string Status { get; set; } = "Pending";
    public virtual List<OrderItem> Items { get; set; } = [];
}

public class OrderItem
{
    public int Id { get; set; }
    public int OrderId { get; set; }
    public int ProductId { get; set; }
    public virtual Product? Product { get; set; }
    public int Quantity { get; set; }
    public decimal UnitPrice { get; set; }
}
```

(`virtual` is only needed for lazy-loading proxies – see 9.8. Without lazy loading you can drop it.)

### One-to-many (most common)

**Analogy:** one mother, many children. Each child row stores its mother's id (`CategoryId`).

```csharp
b.Entity<Order>(e =>
{
    e.HasOne(o => o.Customer).WithMany(c => c.Orders).HasForeignKey(o => o.CustomerId);
    e.HasMany(o => o.Items).WithOne().HasForeignKey(i => i.OrderId)
     .OnDelete(DeleteBehavior.Cascade);        // delete order => delete its items
});
```

### One-to-one

**Analogy:** a person and their passport. The **dependent** (`CustomerProfile`) holds the FK; you must tell EF which side that is:

```csharp
b.Entity<Customer>()
 .HasOne(c => c.Profile)
 .WithOne(p => p.Customer)
 .HasForeignKey<CustomerProfile>(p => p.CustomerId);   // generic arg = dependent type
```

EF creates a **unique index** on `CustomerProfile.CustomerId`, guaranteeing at most one profile per customer.

### Many-to-many

**Analogy:** students and courses. Needs a **join table**. Since EF Core 5 you don't need a join entity class:

```csharp
e.HasMany(p => p.Tags)
 .WithMany(t => t.Products)
 .UsingEntity(j => j.ToTable("ProductTags"));   // join table with ProductsId, TagsId
```

Verified query `db.Products.Select(p => new { p.Name, Tags = p.Tags.Select(t => t.Name).ToList() })`:

```sql
SELECT "p"."Name", "p"."Id", "s"."Name", "s"."ProductsId", "s"."TagsId", "s"."Id"
FROM "Products" AS "p"
LEFT JOIN (
    SELECT "t"."Name", "p0"."ProductsId", "p0"."TagsId", "t"."Id"
    FROM "ProductTags" AS "p0"
    INNER JOIN "Tags" AS "t" ON "p0"."TagsId" = "t"."Id"
) AS "s" ON "p"."Id" = "s"."ProductsId"
ORDER BY "p"."Id", "s"."ProductsId", "s"."TagsId"
```

If the join table needs **extra columns** (e.g. `OrderItems` has `Quantity` and `UnitPrice` – a many-to-many between Orders and Products *with payload*), model it as a **real entity** with two one-to-many relationships – exactly what `OrderItem` is.

### Delete behaviors

| `DeleteBehavior` | What happens to children when the parent is deleted |
|---|---|
| `Cascade` | children deleted too (default for **required** FKs) |
| `Restrict` / `NoAction` | delete fails if children exist (sample app: can't delete a Category with Products) |
| `SetNull` | child FK set to NULL (FK must be nullable; default for optional FKs) |
| `ClientCascade` | EF deletes tracked children; DB doesn't cascade |

⚠️ **Interview trap:** SQL Server error *"may cause cycles or multiple cascade paths"* when two cascade paths reach the same table. Fix with `OnDelete(DeleteBehavior.Restrict)` / `NoAction` on one of the relationships.

---

## 9.7 Change tracking and entity states

**Simple words:** when you load entities with a tracking query, the DbContext keeps a **snapshot of the original values**. On `SaveChanges()` it compares current vs original values (**DetectChanges**) and generates SQL only for what changed.

🏨 **Analogy:** a hotel receptionist who photographs your room at check-in. At check-out, they compare the room with the photo and only charge for what's different.

| State | Meaning | What `SaveChanges` does |
|---|---|---|
| `Detached` | context doesn't know this object | nothing |
| `Unchanged` | loaded, no changes | nothing |
| `Added` | new, `Add()` called | `INSERT` |
| `Modified` | some property changed | `UPDATE` (changed columns only) |
| `Deleted` | `Remove()` called | `DELETE` |

**Verified demo:**

```csharp
Product p = await db.Products.FirstAsync(x => x.Name == "Mouse");
Console.WriteLine(db.Entry(p).State);                       // Unchanged

p.Price = 25;
Console.WriteLine(db.Entry(p).State);                       // Modified
var price = db.Entry(p).Property(x => x.Price);
Console.WriteLine($"{price.IsModified}, original={price.OriginalValue}");  // True, original=20.0

var n = new Product { Name = "Keyboard", Price = 30, CategoryId = 1 };
db.Products.Add(n);
Console.WriteLine(db.Entry(n).State);                       // Added

var ghost = new Product { Name = "Ghost" };
Console.WriteLine(db.Entry(ghost).State);                   // Detached

db.Products.Remove(p);
Console.WriteLine(db.Entry(p).State);                       // Deleted

// (undo the delete for the demo)
p.Price = 20;
db.Entry(p).State = EntityState.Unchanged;

await db.SaveChangesAsync();
Console.WriteLine($"{db.Entry(n).State}, Id={n.Id}");      // Unchanged, Id=4
```

The only SQL sent by that `SaveChanges` – the insert, with the generated key read back:

```sql
INSERT INTO "Products" ("CategoryId", "IsDeleted", "Name", "Price", "Stock", "Version")
VALUES (@p0, @p1, @p2, @p3, @p4, @p5)
RETURNING "Id";
```

(On SQL Server you'd see `OUTPUT INSERTED.[Id]` instead of `RETURNING`.)

### Updating: tracked vs `Update()` on a detached object

**Good – load, change, save → only changed columns are updated** (verified):

```csharp
Product? existing = await db.Products.FindAsync(dto.Id);
db.Entry(existing!).CurrentValues.SetValues(new { dto.Name, dto.Price });   // copy matching props
await db.SaveChangesAsync();
```

```sql
UPDATE "Products" SET "Name" = @p0, "Price" = @p1
WHERE "Id" = @p2 AND "Version" = @p3
RETURNING 1;
```

**`Update()` on a detached object → ALL columns are updated** (verified):

```csharp
var detached = new Product { Id = 4, Name = "Keyboard Pro", Price = 36, Stock = 5, CategoryId = 1, Version = v };
db.Products.Update(detached);       // marks EVERY property as Modified
await db.SaveChangesAsync();
```

```sql
UPDATE "Products" SET "CategoryId" = @p0, "IsDeleted" = @p1, "Name" = @p2, "Price" = @p3, "Stock" = @p4, "Version" = @p5
WHERE "Id" = @p6 AND "Version" = @p7
RETURNING 1;
```

⚠️ **Interview trap:** `Update(entityBuiltFromRequestBody)` silently overwrites columns the client didn't send (e.g. `IsDeleted` becomes `false`, `Stock` becomes `0`). This is the EF side of the **overposting** problem from Chapter 6. Load the entity, map only allowed fields from a DTO, then save.

### AsNoTracking

For **read-only** queries (most GET endpoints) the snapshot is wasted work and memory. `AsNoTracking()` skips it:

```csharp
var untracked = await db.Products.AsNoTracking().FirstAsync(x => x.Name == "Laptop");
Console.WriteLine(db.Entry(untracked).State);    // Detached  (verified)
```

From the sample app's `ProductService`:

```csharp
IQueryable<Product> query = db.Products.AsNoTracking();
```

| | Tracking (default) | `AsNoTracking()` | `AsNoTrackingWithIdentityResolution()` |
|---|---|---|---|
| Snapshot kept | ✅ | ❌ | ❌ |
| Changes saved on `SaveChanges` | ✅ | ❌ | ❌ |
| Same row → same object instance | ✅ | ❌ (duplicates possible) | ✅ |
| Speed / memory | slower | **fastest** | middle |
| Use for | load-modify-save | read-only APIs, reports | read-only with Includes where identity matters |

You can also make no-tracking the default for a context: `o.UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking)`, then opt in with `.AsTracking()`.

Note: queries that **project** into DTOs (`Select(p => new ProductDto(...))`) are never tracked anyway – only entity instances are tracked.

🎯 **Say this in the interview:** "By default EF tracks every entity it loads so SaveChanges can detect changes. For read-only queries I use AsNoTracking or project to DTOs, which avoids the snapshot overhead. For updates I load the tracked entity and change only allowed properties, so EF updates only the changed columns."

---

## 9.8 Loading related data: eager, explicit, lazy – and the N+1 problem

When you load `Order`, its `Customer` and `Items` are **not** loaded automatically. Three ways to load them:

| Strategy | How | SQL | When |
|---|---|---|---|
| **Eager** | `Include` / `ThenInclude` | in the same query (JOIN) or split queries | you know upfront you need the related data |
| **Explicit** | `db.Entry(x).Collection(...).LoadAsync()` | separate query, when *you* ask | conditional loading |
| **Lazy** | `UseLazyLoadingProxies()` + `virtual` navigations | separate query **automatically** on first access | rarely in web APIs – N+1 danger |
| **Projection** | `Select(...)` into DTO | only needed columns, JOINs/subqueries | **best default for APIs** |

### The N+1 problem

**Simple words:** you run **1** query to get a list of N items, then **N** more queries – one per item – to get related data. 100 orders → 101 round-trips.

🛒 **Analogy:** going to the supermarket once to buy the shopping list, then going back **once for every item** on it, instead of buying everything in one trip.

**BAD #1 – lazy loading (verified: 6 orders for 3 customers → 4 SQL commands; with 1,000 customers it would be 1,001):**

```csharp
// options.UseLazyLoadingProxies();  navigations are virtual
List<Order> orders = await db.Orders.ToListAsync();     // 1 query
foreach (Order o in orders)
    Console.WriteLine(o.Customer!.Name);                 // +1 query per DISTINCT customer, silently!
```

**BAD #2 – the manual version (verified: 4 SQL commands):**

```csharp
var customers = await db.Customers.ToListAsync();        // 1 query
foreach (var c in customers)
    c.OrderCount = await db.Orders.CountAsync(o => o.CustomerId == c.Id);  // N queries
```

**GOOD #1 – eager loading with `Include` (verified: 1 command):**

```csharp
var orders = await db.Orders.Include(o => o.Customer).ToListAsync();
```

```sql
SELECT "o"."Id", "o"."CustomerId", "o"."OrderDate", "o"."Status", "c"."Id", "c"."Email", "c"."Name"
FROM "Orders" AS "o"
INNER JOIN "Customers" AS "c" ON "o"."CustomerId" = "c"."Id"
```

**GOOD #2 – projection (verified: 1 command, only the needed columns, count done by the DB):**

```csharp
var rows = await db.Orders
    .Select(o => new { o.Id, CustomerName = o.Customer!.Name, ItemCount = o.Items.Count })
    .ToListAsync();
// { Id = 1, CustomerName = Customer1, ItemCount = 2 }, { Id = 2, CustomerName = Customer1, ItemCount = 2 }, ...
```

```sql
SELECT "o"."Id", "c"."Name" AS "CustomerName", (
    SELECT COUNT(*)
    FROM "OrderItems" AS "o0"
    WHERE "o"."Id" = "o0"."OrderId") AS "ItemCount"
FROM "Orders" AS "o"
INNER JOIN "Customers" AS "c" ON "o"."CustomerId" = "c"."Id"
```

Notice: we used `o.Customer!.Name` in the projection **without any `Include`** – inside `Select`, EF creates the JOIN itself. `Include` is only needed when you return **entities**.

### Eager loading deeper: `ThenInclude`

```csharp
var customers = await db.Customers
    .Include(c => c.Orders)
        .ThenInclude(o => o.Items)
            .ThenInclude(i => i.Product)
    .ToListAsync();
```

Verified: **1** big SQL statement with nested `LEFT JOIN`s and `ORDER BY "c"."Id", "s0"."Id", "s0"."Id0"` (EF needs the ordering to stitch rows back into object graphs). Filtered includes are allowed too (EF Core 5+):

```csharp
db.Customers.Include(c => c.Orders.Where(o => o.Status == "Pending").OrderByDescending(o => o.OrderDate).Take(5))
```

### Explicit loading

```csharp
Customer c = await db.Customers.FirstAsync();
await db.Entry(c).Collection(x => x.Orders).LoadAsync();     // load collection
await db.Entry(c).Reference(x => x.Profile).LoadAsync();     // load single navigation
int big = await db.Entry(c).Collection(x => x.Orders)
                  .Query().CountAsync(o => o.Items.Count > 1);   // query without loading
// verified: orders=2, profile=Street 1, big=2  — 4 SQL commands
```

### Lazy loading – how and why to be careful

```csharp
// dotnet add package Microsoft.EntityFrameworkCore.Proxies
builder.Services.AddDbContext<ShopContext>(o => o.UseSqlServer(cs).UseLazyLoadingProxies());
// all navigation properties must be virtual; entities must not be sealed
```

Problems in web APIs: hidden N+1 queries, queries firing **during JSON serialization** (possibly after the context is disposed → exception), and serializer cycles (`Order.Customer.Orders[0].Customer...`).

⚠️ **Interview trap:** "Lazy loading is the default in EF Core." ❌ In **EF6** (old .NET Framework) it was on by default; in **EF Core** it is **off** and requires the Proxies package (or `ILazyLoader`) and `virtual` navigations.

🎯 **Say this in the interview:** "N+1 is when one query loads a list and then each item triggers another query for related data – usually via lazy loading or a query inside a loop. I fix it with eager loading (`Include`) or, better for APIs, projection with `Select` into a DTO so EF generates a single query with only the needed columns. I verify by logging the SQL."

---

## 9.9 Projection with Select into DTOs

Returning entities from an API has problems: overposting, exposing internal columns (`IsDeleted`, `Version`), cycles, and loading columns you don't need. **Project** straight into DTOs:

```csharp
// from the sample app's ProductService
public Task<ProductDto?> GetByIdAsync(int id, CancellationToken ct) =>
    db.Products.AsNoTracking()
        .Where(p => p.Id == id)
        .Select(p => new ProductDto(p.Id, p.Name, p.Price, p.Stock, p.Category!.Name))
        .FirstOrDefaultAsync(ct);
```

Benefits: one SQL query, only selected columns, JOIN generated automatically, no tracking, no cycles. (`AsNoTracking()` here is harmless but not required, because DTOs are never tracked.)

⚠️ **Interview trap:** calling a **C# method** EF can't translate inside the projection *in the middle* of the query:

```csharp
// Final Select: EF evaluates FormatPrice on the client – OK (client evaluation allowed in the LAST Select)
db.Products.Select(p => new { p.Name, Price = FormatPrice(p.Price) })

// In Where/OrderBy: EF CAN'T translate it -> InvalidOperationException "could not be translated"
db.Products.Where(p => FormatPrice(p.Price).StartsWith("$1"))
```

EF Core 3.0+ throws instead of silently loading the whole table (EF Core 2.x did silent client evaluation).

---

## 9.10 IQueryable vs IEnumerable – the "filter in memory" pitfall

| | `IQueryable<T>` | `IEnumerable<T>` |
|---|---|---|
| What it holds | an **expression tree** (a description of the query) | an in-memory sequence / iterator |
| `Where` runs | **in the database** (translated to SQL) | **in your app's memory** (C# delegate) |
| Namespace of `Where` | `System.Linq.Queryable` | `System.Linq.Enumerable` |
| Executes when | enumerated (`ToListAsync`, `foreach`, `First`, `Count`) | already in memory / enumerated |

🍕 **Analogy:** `IQueryable` = phoning the pizza shop and **ordering exactly one pizza**. `IEnumerable` after `AsEnumerable()`/`ToList()` = asking them to **deliver the whole menu**, then picking one pizza at home and throwing the rest away.

**Verified:**

```csharp
// ✅ GOOD – filter becomes SQL
IQueryable<Product> q = db.Products;
var good = await q.Where(p => p.Price > 100).ToListAsync();

// ❌ BAD – AsEnumerable() switches to LINQ-to-Objects: ALL rows fetched, filtered in C#
IEnumerable<Product> e = db.Products.AsEnumerable();
var bad = e.Where(p => p.Price > 100).ToList();
```

SQL for the good one (note EF also added the global soft-delete filter):

```sql
SELECT "p"."Id", "p"."CategoryId", "p"."IsDeleted", "p"."Name", "p"."Price", "p"."Stock", "p"."Version"
FROM "Products" AS "p"
WHERE NOT ("p"."IsDeleted") AND ef_compare("p"."Price", '100.0') > 0
```

SQL for the bad one – **no price filter at all**:

```sql
SELECT "p"."Id", "p"."CategoryId", "p"."IsDeleted", "p"."Name", "p"."Price", "p"."Stock", "p"."Version"
FROM "Products" AS "p"
WHERE NOT ("p"."IsDeleted")
```

(`ef_compare` appears because SQLite has no native decimal type; on SQL Server it is simply `WHERE [p].[Price] > 100.0`.)

The same bug in a "repository" disguise:

```csharp
// ❌ returns IEnumerable – every caller's Where/Skip/Take runs in memory
public IEnumerable<Product> GetAll() => db.Products;

// ❌ ToList() too early – loads everything, then filters
var expensive = db.Products.ToList().Where(p => p.Price > 100);

// ✅ keep it IQueryable until the end, or accept filter parameters
public Task<List<Product>> GetExpensiveAsync(decimal min, CancellationToken ct)
    => db.Products.Where(p => p.Price > min).ToListAsync(ct);
```

**Building queries dynamically** – the power of `IQueryable` (from the sample app):

```csharp
IQueryable<Product> query = db.Products.AsNoTracking();
if (!string.IsNullOrWhiteSpace(search))
    query = query.Where(p => p.Name.Contains(search));   // added only when needed
// nothing has hit the DB yet...
int total = await query.CountAsync(ct);                  // ...now it does
```

🎯 **Say this in the interview:** "IQueryable builds an expression tree that the provider translates to SQL, so filters, sorting and paging run in the database. Once you call AsEnumerable or ToList, further LINQ runs in memory on all fetched rows. I keep queries IQueryable until the final materialization."

---

## 9.11 Pagination, filtering, sorting

```csharp
int page = 2, size = 2;
var items = await db.Products.AsNoTracking()
    .OrderBy(p => p.Name)                  // ALWAYS order before Skip/Take
    .Skip((page - 1) * size)
    .Take(size)
    .Select(p => p.Name)
    .ToListAsync();
```

Verified SQL:

```sql
SELECT "p"."Name"
FROM "Products" AS "p"
WHERE NOT ("p"."IsDeleted")
ORDER BY "p"."Name"
LIMIT @p OFFSET @p
```

On SQL Server this becomes `ORDER BY ... OFFSET @p ROWS FETCH NEXT @p0 ROWS ONLY`.

⚠️ **Interview trap:** `Skip/Take` **without `OrderBy`** – SQL gives no guaranteed order, so pages can overlap or skip rows; EF logs a warning. Order by a **unique** column (or add `ThenBy(p => p.Id)` as a tie-breaker).

**Complex example – full paged, filtered, sorted endpoint** (the sample app's `GetPageAsync` follows this shape):

```csharp
public record ProductQuery(int Page = 1, int PageSize = 20, string? Search = null,
                           decimal? MinPrice = null, string SortBy = "name", bool Desc = false);

public async Task<PagedResult<ProductDto>> SearchAsync(ProductQuery q, CancellationToken ct)
{
    int page = Math.Max(q.Page, 1);
    int size = Math.Clamp(q.PageSize, 1, 100);             // never trust client page size

    IQueryable<Product> query = db.Products.AsNoTracking();
    if (!string.IsNullOrWhiteSpace(q.Search)) query = query.Where(p => p.Name.Contains(q.Search));
    if (q.MinPrice is not null)               query = query.Where(p => p.Price >= q.MinPrice);

    query = (q.SortBy.ToLowerInvariant(), q.Desc) switch        // whitelist sort columns
    {
        ("price", false) => query.OrderBy(p => p.Price).ThenBy(p => p.Id),
        ("price", true)  => query.OrderByDescending(p => p.Price).ThenBy(p => p.Id),
        (_, false)       => query.OrderBy(p => p.Name).ThenBy(p => p.Id),
        (_, true)        => query.OrderByDescending(p => p.Name).ThenBy(p => p.Id),
    };

    int total = await query.CountAsync(ct);
    var items = await query.Skip((page - 1) * size).Take(size)
        .Select(p => new ProductDto(p.Id, p.Name, p.Price, p.Stock, p.Category!.Name))
        .ToListAsync(ct);

    return new PagedResult<ProductDto>(items, page, size, total);
}
```

**Offset vs keyset (seek) pagination:**

| | Offset (`Skip/Take`) | Keyset (`Where(p => p.Id > lastId).Take(n)`) |
|---|---|---|
| Jump to page 500 | ✅ | ❌ (only next/previous) |
| Performance on deep pages | ❌ DB still reads & skips N rows | ✅ uses index seek, constant |
| Stable when rows are inserted | ❌ items shift | ✅ |
| Use for | admin grids, small tables | infinite scroll, large tables, feeds |

---

## 9.12 Split queries and cartesian explosion

When you `Include` **two or more collections** in one query, the JOIN multiplies rows: a customer with 10 orders × 10 items each → 100 rows, each repeating all customer columns. Add another collection and it multiplies again. This is **cartesian explosion**.

**Fix:** `AsSplitQuery()` – EF sends **one query per included collection** and stitches results in memory.

```csharp
var customers = await db.Customers
    .Include(c => c.Orders).ThenInclude(o => o.Items)
    .Include(c => c.Profile)
    .AsSplitQuery()
    .ToListAsync();
```

Verified – 3 SQL statements:

```sql
SELECT "c"."Id", "c"."Email", "c"."Name", "c0"."Id", "c0"."Address", "c0"."CustomerId"
FROM "Customers" AS "c"
LEFT JOIN "CustomerProfile" AS "c0" ON "c"."Id" = "c0"."CustomerId"
ORDER BY "c"."Id", "c0"."Id"

SELECT "o"."Id", "o"."CustomerId", "o"."OrderDate", "o"."Status", "c"."Id", "c0"."Id"
FROM "Customers" AS "c"
LEFT JOIN "CustomerProfile" AS "c0" ON "c"."Id" = "c0"."CustomerId"
INNER JOIN "Orders" AS "o" ON "c"."Id" = "o"."CustomerId"
ORDER BY "c"."Id", "c0"."Id", "o"."Id"

SELECT "o0"."Id", "o0"."OrderId", "o0"."ProductId", "o0"."Quantity", "o0"."UnitPrice", "c"."Id", "c0"."Id", "o"."Id"
FROM "Customers" AS "c"
LEFT JOIN "CustomerProfile" AS "c0" ON "c"."Id" = "c0"."CustomerId"
INNER JOIN "Orders" AS "o" ON "c"."Id" = "o"."CustomerId"
INNER JOIN "OrderItems" AS "o0" ON "o"."Id" = "o0"."OrderId"
ORDER BY "c"."Id", "c0"."Id", "o"."Id"
```

| | Single query (default) | Split query |
|---|---|---|
| Round-trips | 1 | 1 + number of collection includes |
| Duplicated data | yes (cartesian) | no |
| Consistency | one snapshot | data may change between queries (use a transaction / snapshot isolation if critical) |
| Good for | reference navigations, 1 small collection | several / large collections |

Make it the default: `o.UseSqlServer(cs, sql => sql.UseQuerySplittingBehavior(QuerySplittingBehavior.SplitQuery))`, opt out with `.AsSingleQuery()`.

⚠️ **Interview trap:** `Skip/Take` with split queries needs a **unique, deterministic `OrderBy`**, otherwise the separate queries may page differently.

---

## 9.13 Transactions

### SaveChanges is already a transaction

**Simple words:** everything pending in the change tracker (all inserts, updates, deletes) is sent in **one database transaction** by a single `SaveChanges()`. Either all succeed or none.

```csharp
var order = new Order { CustomerId = 1, OrderDate = DateTime.UtcNow };
order.Items.Add(new OrderItem { ProductId = 1, Quantity = 2, UnitPrice = 800 });
order.Items.Add(new OrderItem { ProductId = 2, Quantity = 1, UnitPrice = 20 });
db.Orders.Add(order);
await db.SaveChangesAsync();     // INSERT order + 2 items in ONE transaction; FK values filled automatically
```

🏦 **Analogy:** a bank transfer – debit and credit happen together or not at all.

### Explicit transactions: when you need several SaveChanges / raw SQL / ExecuteUpdate as one unit

**Complex example – place an order and decrement stock atomically** (verified on SQLite):

```csharp
await using var tx = await db.Database.BeginTransactionAsync();
try
{
    var order = new Order { CustomerId = 1, OrderDate = DateTime.UtcNow,
                            Items = [new() { ProductId = 1, Quantity = 1, UnitPrice = 800 }] };
    db.Orders.Add(order);
    await db.SaveChangesAsync();                                     // 1st statement batch

    int rows = await db.Products.Where(p => p.Id == 1 && p.Stock >= 1)  // 2nd: set-based update
        .ExecuteUpdateAsync(s => s.SetProperty(p => p.Stock, p => p.Stock - 1));
    if (rows == 0) throw new InvalidOperationException("Out of stock");

    await tx.CommitAsync();                                          // both or nothing
}
catch
{
    await tx.RollbackAsync();      // optional: disposing an uncommitted tx rolls back anyway
    throw;
}
```

The `WHERE Stock >= 1` inside the same `UPDATE` makes the stock check **atomic** – no race between "read stock" and "write stock".

### Execution strategy (retries) + transactions (SQL Server)

With `EnableRetryOnFailure` (recommended for Azure SQL – transient errors happen), EF retries failed operations automatically. But it can't retry *half* of a user-initiated transaction, so this throws:

> `InvalidOperationException: The configured execution strategy 'SqlServerRetryingExecutionStrategy' does not support user-initiated transactions.`

Fix: wrap the **whole** transaction in the execution strategy so the whole unit is retried (compile-checked, SQL Server):

```csharp
builder.Services.AddDbContext<ShopContext>(o => o
    .UseSqlServer(cs, sql => sql.EnableRetryOnFailure(maxRetryCount: 5)));

IExecutionStrategy strategy = db.Database.CreateExecutionStrategy();
await strategy.ExecuteAsync(async () =>
{
    await using var tx = await db.Database.BeginTransactionAsync();
    db.Orders.Add(new Order { CustomerId = 1, OrderDate = DateTime.UtcNow });
    await db.SaveChangesAsync();
    await db.Products.Where(p => p.Id == 1)
        .ExecuteUpdateAsync(s => s.SetProperty(p => p.Stock, p => p.Stock - 1));
    await tx.CommitAsync();
});
```

Other transaction tools:

| Tool | Use |
|---|---|
| `BeginTransaction(IsolationLevel.Serializable)` | choose isolation level (`using System.Data`) |
| `tx.CreateSavepointAsync("x")` / `RollbackToSavepointAsync` | partial rollback (EF Core 5+) |
| `db.Database.UseTransactionAsync(existingTx)` | share a `DbTransaction` with Dapper/ADO.NET on the same connection |
| `TransactionScope` | ambient transaction across code; use `TransactionScopeAsyncFlowOption.Enabled` with async |

⚠️ **Interview trap:** "I need `BeginTransaction` around every `SaveChanges`." ❌ A single `SaveChanges` is already atomic. You need an explicit transaction only to group **multiple** operations.

---

## 9.14 Raw SQL: FromSql, FromSqlRaw, ExecuteSql, SqlQuery

Sometimes LINQ isn't enough (complex reporting SQL, hints, CTEs, stored procedures). EF lets you send SQL **while keeping parameters safe**.

### FromSql / FromSqlInterpolated (safe) vs FromSqlRaw (dangerous if misused)

```csharp
string name = "Laptop'; DROP TABLE Products; --";      // malicious input

// ✅ SAFE: interpolated -> {name} becomes a DbParameter (FromSql = EF Core 7+, same as FromSqlInterpolated)
var safe = await db.Products
    .FromSql($"SELECT * FROM Products WHERE Name = {name}")
    .ToListAsync();                                     // verified: 0 rows, table still there
```

Verified SQL – the value is sent as parameter `@p0`, never concatenated:

```sql
SELECT "p"."Id", "p"."CategoryId", "p"."IsDeleted", "p"."Name", "p"."Price", "p"."Stock", "p"."Version"
FROM (
    SELECT * FROM Products WHERE Name = @p0
) AS "p"
WHERE NOT ("p"."IsDeleted")
```

```csharp
// ❌ SQL INJECTION: string concatenation / interpolation into a RAW string
var evil = db.Products.FromSqlRaw("SELECT * FROM Products WHERE Name = '" + name + "'");
var evil2 = db.Products.FromSqlRaw($"SELECT * FROM Products WHERE Name = '{name}'");   // same bug!

// ✅ FromSqlRaw is fine WITH placeholders – values become parameters
var ok = db.Products.FromSqlRaw("SELECT * FROM Products WHERE Name = {0}", name);
var ok2 = db.Products.FromSqlRaw("SELECT * FROM Products WHERE Name = @name",
                                 new SqliteParameter("@name", name));   // SqlParameter on SQL Server
```

| Method | Parameters | Use when |
|---|---|---|
| `FromSql($"...{x}")` (EF 7+) / `FromSqlInterpolated` | **automatic** from interpolation holes | default choice |
| `FromSqlRaw("... {0}", x)` / with `DbParameter` | positional placeholders / explicit parameters | dynamic SQL **structure** (e.g. a column name you whitelisted), output params |
| `FromSqlRaw("..." + userInput)` | none | ❌ never |

**Composing LINQ on top of raw SQL** – EF wraps your SQL as a subquery (verified):

```csharp
decimal min = 30;
var composed = await db.Products
    .FromSql($"SELECT * FROM Products WHERE Price > {min}")
    .Where(p => p.CategoryId == 1)
    .OrderBy(p => p.Name)
    .ToListAsync();
```

```sql
SELECT "p"."Id", ... FROM (
    SELECT * FROM Products WHERE Price > @p0
) AS "p"
WHERE NOT ("p"."IsDeleted") AND "p"."CategoryId" = 1
ORDER BY "p"."Name"
```

Rules for `FromSql` on an entity set: must return **all columns** of the entity with matching names; the global query filter is still applied; result is tracked (unless `AsNoTracking`).

### ExecuteSql – commands that don't return entities

```csharp
int cat = 2;
int n = await db.Database.ExecuteSqlAsync(
    $"UPDATE Products SET Stock = Stock + 1 WHERE CategoryId = {cat}");   // returns rows affected
```

Verified SQL: `UPDATE Products SET Stock = Stock + 1 WHERE CategoryId = @p0`. (`ExecuteSqlRawAsync` is the raw counterpart.)

### SqlQuery&lt;T&gt; – map raw SQL to any type (EF Core 7+ scalars, EF Core 8+ unmapped types)

```csharp
public record CustomerOrderCount(string Name, int OrderCount);   // NOT an entity, not in the model

var counts = await db.Database.SqlQuery<CustomerOrderCount>($"""
    SELECT c.Name, COUNT(o.Id) AS OrderCount
    FROM Customers c LEFT JOIN Orders o ON o.CustomerId = c.Id
    GROUP BY c.Name
    """).ToListAsync();
// verified: { Name = Customer1, OrderCount = 3 }, { Name = Customer2, OrderCount = 2 }, ...

int total = await db.Database
    .SqlQuery<int>($"SELECT COUNT(*) AS Value FROM Orders")       // scalar: column MUST be named "Value"
    .SingleAsync();
```

⚠️ **Interview trap (verified):** for `SqlQuery<int>` the column must be aliased **`Value`**, because EF wraps your SQL as a subquery and selects `Value` from it so you can compose (`.Where(...)`) on top.

---

## 9.15 Stored procedures with EF Core (SQL Server)

The companion SQL book's **ShopDB** database (Customers, Products, Orders, OrderItems, Categories) has procedures like these:

```sql
-- SQL Server (T-SQL)
CREATE OR ALTER PROCEDURE dbo.usp_GetProductCountByCategory
    @CategoryId    INT,
    @TotalProducts INT OUTPUT
AS
BEGIN
    SET NOCOUNT ON;
    SELECT @TotalProducts = COUNT(*) FROM dbo.Products WHERE CategoryId = @CategoryId;
END;
GO

CREATE OR ALTER PROCEDURE dbo.usp_GetTopSellingProducts
    @Top      INT,
    @FromDate DATE
AS
BEGIN
    SET NOCOUNT ON;
    SELECT TOP (@Top)
           p.Name                         AS ProductName,
           SUM(oi.Quantity)               AS TotalQty,
           SUM(oi.Quantity * oi.UnitPrice) AS Revenue
    FROM dbo.OrderItems oi
    JOIN dbo.Orders   o ON o.Id = oi.OrderId
    JOIN dbo.Products p ON p.Id = oi.ProductId
    WHERE o.OrderDate >= @FromDate
    GROUP BY p.Name
    ORDER BY Revenue DESC;
END;
```

> Column names (`Id` vs `OrderID`, etc.) may differ slightly in your ShopDB – adjust the procedure. The C# below was **compile-checked** against EF Core 10 + `Microsoft.EntityFrameworkCore.SqlServer`, not executed (no SQL Server was connected).

### 1) Procedure returning rows → keyless entity

The result shape isn't a table, so map it to a **keyless entity type**:

```csharp
public class ProductSalesRow
{
    public string ProductName { get; set; } = "";
    public int TotalQty { get; set; }
    public decimal Revenue { get; set; }
}

// OnModelCreating
b.Entity<ProductSalesRow>().HasNoKey().ToView(null);   // keyless, not mapped to a table/view, no migration

// DbContext
public DbSet<ProductSalesRow> ProductSales => Set<ProductSalesRow>();

// Call it
public Task<List<ProductSalesRow>> GetTopSellingAsync(int top, DateTime from)
    => db.ProductSales
         .FromSql($"EXEC dbo.usp_GetTopSellingProducts @Top = {top}, @FromDate = {from}")
         .ToListAsync();
```

The same keyless mapping **was executed** on SQLite with a plain `SELECT ... GROUP BY` (SQLite has no stored procedures) and returned `Laptop: qty=7, revenue=5600` and `Mouse: qty=12, revenue=240`.

Alternatively, without adding anything to the model (EF Core 8+): `db.Database.SqlQuery<ProductSalesRow>($"EXEC dbo.usp_GetTopSellingProducts @Top = {top}, @FromDate = {from}")`.

⚠️ **Interview trap:** you **cannot compose** LINQ on top of `EXEC` (`.Where(...)` after `FromSql("EXEC ...")`) – SQL Server can't use `EXEC` as a subquery. Call `.AsEnumerable()` / `ToListAsync()` first, then filter in memory, or add parameters to the procedure.

### 2) Procedure with an OUTPUT parameter → `SqlParameter` + `ExecuteSqlRaw`

```csharp
using System.Data;
using Microsoft.Data.SqlClient;

public async Task<int> GetProductCountAsync(int categoryId)
{
    var categoryParam = new SqlParameter("@CategoryId", SqlDbType.Int) { Value = categoryId };
    var totalParam = new SqlParameter("@TotalProducts", SqlDbType.Int) { Direction = ParameterDirection.Output };

    await db.Database.ExecuteSqlRawAsync(
        "EXEC dbo.usp_GetProductCountByCategory @CategoryId, @TotalProducts OUTPUT",
        categoryParam, totalParam);

    return (int)totalParam.Value;      // read AFTER execution
}
```

Key points: use `Microsoft.Data.SqlClient.SqlParameter` (not the old `System.Data.SqlClient`), write the word **`OUTPUT`** in the SQL text too, read `.Value` after the call. For a **return value** use `Direction = ParameterDirection.ReturnValue` with `EXEC @rc = dbo.usp_X ...`.

### 3) Procedures for insert/update/delete (EF Core 7+)

You can tell EF to call procedures **instead of** generating INSERT/UPDATE/DELETE on `SaveChanges`:

```csharp
b.Entity<Product>()
 .InsertUsingStoredProcedure("usp_Product_Insert", sp => sp
     .HasParameter(p => p.Name)
     .HasParameter(p => p.Price)
     .HasParameter(p => p.CategoryId)
     .HasParameter(p => p.Stock)
     .HasParameter(p => p.IsDeleted)
     .HasParameter(p => p.Version)
     .HasResultColumn(p => p.Id));      // generated key comes back as a result column
```

Verified gotcha: **every** mapped property must be a parameter or result column – leaving out Stock, IsDeleted and Version fails model validation with *"The entity type 'Product' is mapped to the stored procedure 'usp_Product_Insert', however the properties {'IsDeleted', 'Stock', 'Version'} are not mapped to any parameter or result column."* Useful when a DBA mandates procedure-only access.

| Need | EF Core approach |
|---|---|
| SP returns entity-shaped rows | `db.Products.FromSql($"EXEC ...")` |
| SP returns custom shape | keyless entity + `FromSql`, or `Database.SqlQuery<T>` (EF 8+) |
| SP returns scalar | `Database.SqlQuery<int>($"...")` with `Value` column, or output parameter |
| SP with OUTPUT params | `ExecuteSqlRawAsync` + `SqlParameter { Direction = Output }` |
| SP does insert/update/delete only | `ExecuteSqlAsync($"EXEC ...")` |
| SPs for all CUD of an entity | `InsertUsingStoredProcedure` etc. (EF 7+) |
| SP returns multiple result sets | drop to ADO.NET (`DbCommand` + `NextResult`) or Dapper `QueryMultiple` |

🎯 **Say this in the interview:** "For a stored procedure that returns rows I map the result to a keyless entity or use SqlQuery<T>, and call it with FromSql and interpolated parameters so it's injection-safe. For output parameters I use ExecuteSqlRaw with a SqlParameter whose Direction is Output and read its Value after execution. I can't compose LINQ over EXEC."

---

## 9.16 Dapper – the micro-ORM alternative

**Dapper** (by Stack Overflow) is a tiny library of extension methods on `IDbConnection`: **you write the SQL, it maps rows to objects** – fast, no change tracking, no LINQ translation, no migrations.

**Verified on SQLite (sharing the EF connection):**

```csharp
using Dapper;

var rows = await conn.QueryAsync<(string Name, double Price)>(
    "SELECT Name, Price FROM Products WHERE Price > @min ORDER BY Price", new { min = 30 });
// Keyboard Pro 36 / C# in Depth 49.5 / Laptop 700
```

**Stored procedures with Dapper (SQL Server, compile-checked):**

```csharp
await using var c = new SqlConnection(connectionString);

// result set
var top = await c.QueryAsync<ProductSalesRow>(
    "dbo.usp_GetTopSellingProducts",
    new { Top = 5, FromDate = DateTime.UtcNow.AddMonths(-1) },
    commandType: CommandType.StoredProcedure);

// output parameter
var p = new DynamicParameters();
p.Add("@CategoryId", categoryId);
p.Add("@TotalProducts", dbType: DbType.Int32, direction: ParameterDirection.Output);
await c.ExecuteAsync("dbo.usp_GetProductCountByCategory", p, commandType: CommandType.StoredProcedure);
int total = p.Get<int>("@TotalProducts");
```

**Mixing EF and Dapper** is common: EF for writes and normal CRUD, Dapper for heavy read/report queries. Reuse EF's connection with `db.Database.GetDbConnection()` (and `db.Database.CurrentTransaction?.GetDbTransaction()` to join the same transaction).

---

## 9.17 Bulk operations: ExecuteUpdate / ExecuteDelete (EF Core 7+)

**Problem:** the classic way to update 10,000 rows is *load 10,000 entities → change → SaveChanges → 10,000 UPDATE statements*.

**Solution:** `ExecuteUpdateAsync` / `ExecuteDeleteAsync` translate a LINQ query directly into **one** set-based SQL statement – no loading, no change tracking.

```csharp
// +10% price and zero stock for all books
int up = await db.Products.Where(p => p.CategoryId == 2)
    .ExecuteUpdateAsync(s => s
        .SetProperty(p => p.Price, p => p.Price * 1.10m)    // based on current value
        .SetProperty(p => p.Stock, 0));                      // constant

int del = await db.Orders.Where(o => o.Status == "Cancelled").ExecuteDeleteAsync();
```

Verified SQL:

```sql
UPDATE "Products" AS "p"
SET "Price" = ef_multiply("p"."Price", '1.1'),
    "Stock" = @p
WHERE NOT ("p"."IsDeleted") AND "p"."CategoryId" = 2

DELETE FROM "Orders" AS "o"
WHERE "o"."Status" = 'Cancelled'
```

Notice the global query filter (`NOT IsDeleted`) **is applied** to `ExecuteUpdate`.

| | Load + modify + `SaveChanges` | `ExecuteUpdate` / `ExecuteDelete` |
|---|---|---|
| SQL statements | 1 SELECT + N UPDATEs (batched) | **1** |
| Memory | all entities loaded | none |
| Change tracker | updated | **not** updated (tracked entities become stale) |
| Concurrency token check | ✅ automatic | ❌ (add it to `Where` yourself) |
| Runs when | at `SaveChanges` | **immediately** (not part of `SaveChanges`) |
| Interceptors / `SaveChanges` overrides (audit fields) | run | **don't** run |

⚠️ **Interview trap:** "`ExecuteDelete` will run when I call `SaveChanges`." ❌ It executes **immediately** and bypasses the change tracker. If you need it in the same transaction as `SaveChanges`, use an explicit transaction (9.13).

---

## 9.18 Concurrency: optimistic concurrency tokens

**Problem – the "lost update":** Alice and Bob both open product #1 (price 800). Alice saves 850. Bob, still looking at 800, saves 700. Alice's change is silently lost.

**Optimistic concurrency** = don't lock; instead, include a **version column** in the `WHERE` of every `UPDATE/DELETE`. If someone changed the row since you read it, the version doesn't match, 0 rows are affected, and EF throws `DbUpdateConcurrencyException`.

🎫 **Analogy:** a shared Google Doc that says "this document changed since you opened it – reload?" instead of locking the document for everyone.

### SQL Server: `rowversion` (automatic)

```csharp
public class Invoice
{
    public int Id { get; set; }
    public decimal Amount { get; set; }
    [Timestamp]                          // SQL Server rowversion: DB changes it on every update
    public byte[] RowVersion { get; set; } = [];
}
// Fluent: e.Property(x => x.RowVersion).IsRowVersion();
```

### Other DBs / SQLite: a Guid token you change yourself (what the sample app does)

```csharp
// sample app: AppDbContext
e.Property(p => p.Version).IsConcurrencyToken();

// sample app: ProductService.UpdateAsync
product.Name = request.Name;
product.Price = request.Price;
product.Stock = request.Stock;
product.Version = Guid.NewGuid();
await db.SaveChangesAsync(ct);   // throws DbUpdateConcurrencyException on conflict
```

### Verified two-user simulation

```csharp
await using var db1 = NewContext(conn);   // Alice
await using var db2 = NewContext(conn);   // Bob

Product a = await db1.Products.SingleAsync(p => p.Id == 1);
Product b = await db2.Products.SingleAsync(p => p.Id == 1);

a.Price = 850; a.Version = Guid.NewGuid();
await db1.SaveChangesAsync();                    // Alice wins the race

b.Price = 700; b.Version = Guid.NewGuid();
try
{
    await db2.SaveChangesAsync();                // Bob: WHERE Version = <old> -> 0 rows
}
catch (DbUpdateConcurrencyException ex)
{
    var entry = ex.Entries.Single();
    var dbValues = await entry.GetDatabaseValuesAsync();
    Console.WriteLine($"Conflict! mine={entry.CurrentValues["Price"]}, database={dbValues!["Price"]}");
    // Conflict! mine=700, database=850.0

    // Strategy "client wins": accept the DB's version as the new original, then save again
    entry.OriginalValues.SetValues(dbValues);
    await db2.SaveChangesAsync();                // "Saved on retry (client wins)."
}
```

The UPDATE that failed:

```sql
UPDATE "Products" SET "Price" = @p0, "Version" = @p1
WHERE "Id" = @p2 AND "Version" = @p3
RETURNING 1;
```

Resolution strategies:

| Strategy | How | Use when |
|---|---|---|
| **Database wins** | `await entry.ReloadAsync()` and tell the user | default for most UIs |
| **Client wins** | `entry.OriginalValues.SetValues(dbValues)` then save again | last writer should win (rare) |
| **Merge** | compare property by property, decide per field | collaborative editing |
| **Return 409 Conflict** | catch in API, return `409` (or `412` with ETags) and let the client reload | **REST APIs** |

In an API:

```csharp
try { await service.UpdateAsync(id, request, ct); return NoContent(); }
catch (DbUpdateConcurrencyException)
{
    return Problem(statusCode: StatusCodes.Status409Conflict,
                   title: "The product was changed by someone else. Reload and try again.");
}
```

The sample app's test `Update_WhenRowChangedByOtherUser_ThrowsConcurrencyException` (`ShopApi.Tests/UnitTests.cs`) simulates "user B" by changing `Version` behind the tracked entity with `ExecuteSqlRawAsync("UPDATE Products SET Version = {0} WHERE Id = 1", Guid.NewGuid())` and asserts the exception.

| | Optimistic | Pessimistic |
|---|---|---|
| How | version check at write | DB lock at read (`SELECT ... WITH (UPDLOCK)` / `FOR UPDATE`) |
| EF support | built-in tokens | raw SQL + transaction only |
| Good for | web apps, low conflict rate | high contention, short critical sections |

⚠️ **Interview trap:** "Transactions prevent lost updates." ❌ Not with the default `READ COMMITTED` isolation – both users read, both write, both commit. You need concurrency tokens (or higher isolation/locks).

🎯 **Say this in the interview:** "I use optimistic concurrency: a rowversion column on SQL Server, configured as a concurrency token. EF adds it to the WHERE clause of updates; if 0 rows are affected it throws DbUpdateConcurrencyException, and in an API I translate that to 409 Conflict so the client reloads."

---

## 9.19 Global query filters (soft delete, multi-tenancy)

**Simple words:** a `WHERE` condition EF adds **automatically** to every query for an entity type.

```csharp
// sample app: AppDbContext
e.HasQueryFilter(p => !p.IsDeleted);   // global soft-delete filter

// sample app: ProductService.DeleteAsync
product.IsDeleted = true;              // soft delete = UPDATE, not DELETE
await db.SaveChangesAsync(ct);
```

Verified:

```csharp
// mark Mouse deleted
await db.Products.Where(p => p.Name == "Mouse").ExecuteUpdateAsync(s => s.SetProperty(p => p.IsDeleted, true));

int visible = await db.Products.CountAsync();                        // 3   -> WHERE NOT ("p"."IsDeleted")
int all     = await db.Products.IgnoreQueryFilters().CountAsync();   // 4   -> no WHERE
```

**Multi-tenancy** – filter referencing a context field (evaluated per context instance):

```csharp
public class TenantDbContext(DbContextOptions<TenantDbContext> o, ITenantProvider tenant) : DbContext(o)
{
    private readonly string _tenantId = tenant.TenantId;
    public DbSet<Invoice2> Invoices => Set<Invoice2>();
    protected override void OnModelCreating(ModelBuilder b)
        => b.Entity<Invoice2>().HasQueryFilter(i => i.TenantId == _tenantId);
}
```

Gotchas:

- Filters apply to `Include`d navigations too – an order item whose product is soft-deleted can "disappear" from a required `Include` (we saw `WHERE NOT ("p"."IsDeleted")` inside the `ThenInclude` SQL in 9.8).
- `FromSql` results are filtered too; `ExecuteUpdate/Delete` respect filters.
- Before EF Core 10 you could have **one** filter per entity (a second `HasQueryFilter` replaced the first; combine with `&&`). **EF Core 10** adds *named* filters so you can disable one selectively, e.g. `HasQueryFilter("SoftDelete", p => !p.IsDeleted)` and `IgnoreQueryFilters(["SoftDelete"])`.

---

## 9.20 Seeding data with HasData

```csharp
// sample app: AppDbContext
b.Entity<Category>().HasData(
    new Category { Id = 1, Name = "Electronics" },
    new Category { Id = 2, Name = "Books" });

b.Entity<Product>().HasData(
    new Product { Id = 1, Name = "Laptop", Price = 800m, Stock = 10, CategoryId = 1,
                  Version = new Guid("11111111-1111-1111-1111-111111111111") }, ...);
```

- Seed data becomes part of **migrations** (`InsertData`/`UpdateData`/`DeleteData`) and is inserted by `EnsureCreated()`.
- You must specify **primary keys** explicitly, and values must be **deterministic** – note the sample app uses fixed Guids, not `Guid.NewGuid()`, otherwise every migration would see "changed" seed data.
- For large or environment-specific data (test users, demo data) use custom startup code or EF Core 9+'s `UseSeeding` / `UseAsyncSeeding` options instead.

```csharp
// EF Core 9+
o.UseSqlServer(cs)
 .UseAsyncSeeding(async (ctx, _, ct) =>
 {
     if (!await ctx.Set<Category>().AnyAsync(ct))
     {
         ctx.Set<Category>().Add(new Category { Name = "Toys" });
         await ctx.SaveChangesAsync(ct);
     }
 });
```

---

## 9.21 Performance features: compiled queries, DbContext pooling

### Compiled queries

Every LINQ query must be translated to SQL. EF caches translations, but still has to compute a cache key from the expression tree each time. For **very hot** queries, compile once:

```csharp
static readonly Func<ShopContext, int, Task<Product?>> GetProductById =
    EF.CompileAsyncQuery((ShopContext db, int id) => db.Products.FirstOrDefault(p => p.Id == id));

Product? p = await GetProductById(db, 1);    // verified: "Laptop"
```

Gain is small (microseconds per call) – only worth it for queries executed thousands of times per second. EF Core 9+ also has experimental **precompiled queries** for NativeAOT.

### DbContext pooling

```csharp
builder.Services.AddDbContextPool<ShopContext>(o => o.UseSqlServer(cs), poolSize: 1024);
// or AddPooledDbContextFactory<ShopContext>(...)
```

Instead of creating a new `DbContext` per request, EF **resets and reuses** instances from a pool. Verified working (`pooled context ok`). Rules: the context must not hold per-request **state** in fields (it's reused!) and its constructor must accept only `DbContextOptions`; for per-request values (like tenant id) set them after resolving.

---

## 9.22 Logging and inspecting generated SQL

You can't fix what you can't see. Ways to see the SQL:

```csharp
// 1) appsettings.Development.json – via the normal ASP.NET Core logging
"Logging": { "LogLevel": { "Microsoft.EntityFrameworkCore.Database.Command": "Information" } }

// 2) LogTo – quick and explicit (used for every SQL listing in this chapter)
o.UseSqlServer(cs)
 .LogTo(Console.WriteLine, [DbLoggerCategory.Database.Command.Name], LogLevel.Information)
 .EnableSensitiveDataLogging()       // shows parameter VALUES – DEVELOPMENT ONLY (PII!)
 .EnableDetailedErrors();            // better materialization error messages – dev only

// 3) ToQueryString – SQL for a query without executing it (great in debugger/tests)
string sql = db.Products.Where(p => p.Price > 50).OrderBy(p => p.Name).ToQueryString();
```

Verified `ToQueryString()` output:

```sql
SELECT "p"."Id", "p"."CategoryId", "p"."IsDeleted", "p"."Name", "p"."Price", "p"."Stock", "p"."Version"
FROM "Products" AS "p"
WHERE NOT ("p"."IsDeleted") AND ef_compare("p"."Price", '50.0') > 0
ORDER BY "p"."Name"
```

Also: `TagWith("GetProducts endpoint")` adds a SQL comment that shows up in SQL Server Profiler / Query Store, and SQL Server tools (execution plans, Query Store) tell you about missing indexes.

⚠️ **Interview trap:** leaving `EnableSensitiveDataLogging()` on in production – it logs parameter values (emails, passwords hashes, card data) into your logs.

---

## 9.23 Performance tips (cheat table)

| Problem | Fix |
|---|---|
| N+1 queries | `Include` or (better) `Select` projection; avoid lazy loading in APIs |
| Loading full entities for read-only screens | `Select` into DTOs; `AsNoTracking()` |
| Filtering/paging in memory | keep `IQueryable` until `ToListAsync`; no early `ToList()`/`AsEnumerable()` |
| Cartesian explosion with multiple `Include`s | `AsSplitQuery()` |
| Deep pages slow | keyset pagination |
| Updating/deleting many rows | `ExecuteUpdateAsync` / `ExecuteDeleteAsync` |
| Inserting many rows | `AddRange` + one `SaveChanges` (EF batches); for 100k+ rows `SqlBulkCopy` / bulk libraries |
| Slow `Contains`/`Where` | add **indexes** (`HasIndex`), check the execution plan |
| `Count()` then `ToList()` = 2 round-trips | fine for paging; avoid `Count() > 0` → use `AnyAsync()` |
| Blocking threads | always `ToListAsync`, `SaveChangesAsync`, pass `CancellationToken` |
| Context creation overhead under heavy load | `AddDbContextPool` |
| Hot query translation cost | compiled queries |
| Transient Azure SQL failures | `EnableRetryOnFailure` (+ execution strategy for transactions) |
| Huge change tracker in long loops | create a new context per batch or `ChangeTracker.Clear()` |
| Selecting a `string` column of `nvarchar(max)` you don't need | project only needed columns |

---

## 9.24 EF Core vs Dapper vs ADO.NET

| | EF Core | Dapper | ADO.NET (`SqlConnection`/`SqlCommand`/`DataReader`) |
|---|---|---|---|
| Type | full ORM | micro-ORM | raw data access API |
| You write SQL? | no (LINQ), optional raw SQL | **yes** | yes |
| Mapping rows → objects | automatic | automatic (by column name) | **manual** (`reader.GetInt32(0)`) |
| Change tracking / unit of work | ✅ | ❌ | ❌ |
| Migrations, model | ✅ | ❌ | ❌ |
| LINQ, compile-time checking of queries | ✅ | ❌ (SQL strings) | ❌ |
| Raw speed | good (very close for no-tracking queries in EF 8+) | **excellent** | best (it's what the others use underneath) |
| Learning curve | higher (must understand generated SQL) | low (if you know SQL) | low API, lots of boilerplate |
| Best for | most CRUD apps, domain models, rapid development | read-heavy/reporting, complex hand-tuned SQL, SPs | special needs: bulk copy, multiple result sets, streaming, provider-specific features |

🎯 **Say this in the interview:** "I default to EF Core for productivity – LINQ, change tracking, migrations. For hot read paths or complex reporting SQL I use Dapper, sometimes on the same connection. ADO.NET is the foundation underneath both; I'd use it directly only for things like SqlBulkCopy or multiple result sets."

---

## 9.25 Putting it together – the sample app's ProductService

```csharp
public class ProductService(AppDbContext db, ILogger<ProductService> logger) : IProductService
{
    public async Task<PagedResult<ProductDto>> GetPageAsync(int page, int pageSize, string? search, CancellationToken ct)
    {
        page = Math.Max(page, 1);
        pageSize = Math.Clamp(pageSize, 1, 100);

        IQueryable<Product> query = db.Products.AsNoTracking();          // read-only
        if (!string.IsNullOrWhiteSpace(search))
            query = query.Where(p => p.Name.Contains(search));          // dynamic filter, still SQL

        int total = await query.CountAsync(ct);

        // Projection (Select) => only needed columns, JOIN generated for Category, no N+1.
        List<ProductDto> items = await query
            .OrderBy(p => p.Id)                                         // stable order before paging
            .Skip((page - 1) * pageSize)
            .Take(pageSize)
            .Select(p => new ProductDto(p.Id, p.Name, p.Price, p.Stock, p.Category!.Name))
            .ToListAsync(ct);

        return new PagedResult<ProductDto>(items, page, pageSize, total);
    }

    public async Task<ProductDto> CreateAsync(CreateProductRequest request, CancellationToken ct)
    {
        Category category = await db.Categories.FindAsync([request.CategoryId], ct)
            ?? throw new ArgumentException($"Category {request.CategoryId} does not exist.");

        var product = new Product                                       // DTO -> entity: no overposting
        {
            Name = request.Name,
            Price = request.Price,
            Stock = request.Stock,
            CategoryId = category.Id
        };
        db.Products.Add(product);
        await db.SaveChangesAsync(ct);                                  // INSERT, Id populated

        logger.LogInformation("Created product {ProductId} {ProductName}", product.Id, product.Name);
        return new ProductDto(product.Id, product.Name, product.Price, product.Stock, category.Name);
    }

    public async Task<bool> UpdateAsync(int id, UpdateProductRequest request, CancellationToken ct)
    {
        Product? product = await db.Products.FindAsync([id], ct);       // tracked
        if (product is null) return false;

        product.Name = request.Name;
        product.Price = request.Price;
        product.Stock = request.Stock;
        product.Version = Guid.NewGuid();                               // concurrency token
        await db.SaveChangesAsync(ct);   // throws DbUpdateConcurrencyException on conflict
        return true;
    }
}
```

Every interview topic in one service: scoped `DbContext` via DI, `AsNoTracking`, `IQueryable` composition, ordered paging, projection, DTO mapping (no overposting), `FindAsync` (checks the change tracker first, then DB), async + `CancellationToken`, concurrency token, soft delete via global filter.

---

## Try it yourself

**1.** This endpoint is slow and the SQL log shows 501 queries for 500 orders. Why, and fix it two ways.

```csharp
var orders = await db.Orders.ToListAsync();
return orders.Select(o => new OrderDto(o.Id, o.Customer!.Name)).ToList();   // lazy loading enabled
```

<details><summary>Answer</summary>

N+1: 1 query for orders, then lazy loading fires a query for each order's `Customer` (once per distinct customer). Fix 1 – eager loading: `db.Orders.Include(o => o.Customer).ToListAsync()`. Fix 2 (better) – project in the database:

```csharp
return await db.Orders
    .Select(o => new OrderDto(o.Id, o.Customer!.Name))
    .ToListAsync();
```

One query with a JOIN, only two columns, no tracking.
</details>

**2.** What SQL does this produce, and what's wrong?

```csharp
IEnumerable<Product> all = db.Products;
var page = all.Where(p => p.CategoryId == 1).Skip(20).Take(10).ToList();
```

<details><summary>Answer</summary>

Because the variable is typed `IEnumerable<Product>`, `Where/Skip/Take` bind to `Enumerable` (LINQ-to-Objects). The SQL is just `SELECT ... FROM Products` (plus any global filter) – **all rows** are loaded and filtered/paged in memory. Also there is no `OrderBy`. Fix: keep it `IQueryable<Product>`, add `OrderBy(p => p.Id)`, and use `ToListAsync()`.
</details>

**3.** Write the C# to call `dbo.usp_GetProductCountByCategory @CategoryId INT, @TotalProducts INT OUTPUT` (SQL Server) and return the count.

<details><summary>Answer</summary>

```csharp
var cat = new SqlParameter("@CategoryId", SqlDbType.Int) { Value = categoryId };
var total = new SqlParameter("@TotalProducts", SqlDbType.Int) { Direction = ParameterDirection.Output };
await db.Database.ExecuteSqlRawAsync(
    "EXEC dbo.usp_GetProductCountByCategory @CategoryId, @TotalProducts OUTPUT", cat, total);
return (int)total.Value;
```

With Dapper: `DynamicParameters` + `direction: ParameterDirection.Output`, then `p.Get<int>("@TotalProducts")`.
</details>

**4.** Is this safe from SQL injection? `db.Products.FromSqlRaw($"SELECT * FROM Products WHERE Name = '{name}'")`

<details><summary>Answer</summary>

**No.** `FromSqlRaw` receives an already-built string – the interpolation happened in C#, so `name` is concatenated into the SQL. Use `FromSql($"SELECT * FROM Products WHERE Name = {name}")` (no quotes – EF creates a parameter) or `FromSqlRaw("... WHERE Name = {0}", name)`.
</details>

**5.** Two admins edit the same product at the same time. How do you make sure the second save doesn't silently overwrite the first, and what HTTP status do you return?

<details><summary>Answer</summary>

Add a concurrency token: `[Timestamp] public byte[] RowVersion` (SQL Server rowversion) or a `Guid Version` with `IsConcurrencyToken()` changed on each update. EF adds it to the UPDATE's `WHERE`; the second save affects 0 rows and throws `DbUpdateConcurrencyException`. Catch it and return **409 Conflict** (or 412 Precondition Failed if you use ETags/If-Match), so the client reloads.
</details>

**6.** You must set `Status = 'Archived'` on 200,000 old orders. What's the best EF Core approach and what's one caveat?

<details><summary>Answer</summary>

```csharp
await db.Orders.Where(o => o.OrderDate < cutoff)
    .ExecuteUpdateAsync(s => s.SetProperty(o => o.Status, "Archived"));
```

One `UPDATE ... WHERE` statement, no entities loaded (EF Core 7+). Caveats: it runs immediately (not at `SaveChanges`), bypasses the change tracker (already-tracked entities are stale), `SaveChanges` interceptors/audit logic don't run, and concurrency tokens aren't checked automatically.
</details>

---

## Quick revision

- `DbContext` = unit of work + change tracker; **Scoped** (one per request), **not thread-safe** – use `IDbContextFactory` for parallel work/background services.
- Code-first + migrations (`dotnet ef migrations add`, `database update`, `migrations script --idempotent`); database-first = `dotnet ef dbcontext scaffold`.
- Fluent API beats data annotations and can configure everything; organize with `IEntityTypeConfiguration<T>`.
- Relationships: 1-many (FK on many side), 1-1 (`HasForeignKey<Dependent>`), many-many (`UsingEntity`, or an explicit join entity with payload).
- Entity states: Detached, Unchanged, Added, Modified, Deleted; tracked updates touch only changed columns, `Update()` marks all.
- `AsNoTracking()` / projections for read-only queries.
- N+1 = 1 + N queries; fix with `Include` or `Select` projection; lazy loading is off by default in EF Core.
- `IQueryable` → SQL; `IEnumerable`/`AsEnumerable()`/early `ToList()` → in-memory filtering of all rows.
- Always `OrderBy` before `Skip/Take`; consider keyset paging for deep pages; `AsSplitQuery()` against cartesian explosion.
- One `SaveChanges` is a transaction; use `BeginTransaction` for multiple operations; wrap it in `CreateExecutionStrategy().ExecuteAsync` when retries are on.
- `FromSql($"...{x}")` / `ExecuteSql` parameterize; `FromSqlRaw` with concatenation = SQL injection; can't compose over `EXEC`.
- Stored procs: keyless entity / `SqlQuery<T>` for rows, `SqlParameter { Direction = Output }` for outputs; `ExecuteUpdate/Delete` (EF 7+) for bulk; concurrency tokens → `DbUpdateConcurrencyException` → 409.

---

# 10. Authentication, Authorization, JWT, OAuth & CORS

> **Why interviewers ask this**
> Every real API is protected. Interviewers want to know if you understand *how* a request becomes "a logged-in user", *what* a JWT really is (and is not), the difference between **401 and 403**, **roles vs claims vs policies**, and why CORS errors happen. A 2-YOE developer is expected to wire up JWT end-to-end and explain each line.

---

## 10.1 Authentication vs Authorization — the airport analogy

**Simple words**

| | Authentication (AuthN) | Authorization (AuthZ) |
|---|---|---|
| Question | **Who are you?** | **What are you allowed to do?** |
| Airport analogy | Showing your **passport** at the check-in desk | Your **boarding pass** says Gate 12, Economy — you cannot enter the Business lounge |
| Happens | First | After authentication |
| Output | A `ClaimsPrincipal` in `HttpContext.User` | Allow / deny (`403`) |
| Failure status | **401 Unauthorized** ("I don't know you") | **403 Forbidden** ("I know you, but no") |
| Middleware | `app.UseAuthentication()` | `app.UseAuthorization()` |
| Attribute | — | `[Authorize]`, `[AllowAnonymous]` |

```text
Request ──► UseAuthentication ──► UseAuthorization ──► Endpoint
            "read the token,       "does this user
             build HttpContext.User"  satisfy the policy?"
```

⚠️ **Interview trap:** The HTTP status `401` is *named* "Unauthorized" but it actually means **unauthenticated**. `403 Forbidden` is the real "not authorized". Say this explicitly — interviewers love it.

⚠️ **Interview trap:** `UseAuthentication()` must come **before** `UseAuthorization()`. If you swap them, `HttpContext.User` is empty when authorization runs, so every `[Authorize]` endpoint returns 401.

The sample app (samples/ShopApi) wires them in the right order:

```csharp
app.UseCors("Frontend");            // after routing (implicit), before auth
app.UseAuthentication();            // who are you?
app.UseAuthorization();             // are you allowed?
app.UseRateLimiter();
```

🎯 **Say this in the interview:** "Authentication establishes identity — it reads a cookie or token and builds `HttpContext.User`. Authorization decides whether that identity can access a resource, using roles, claims or policies. Missing/invalid identity gives 401; valid identity without permission gives 403."

---

## 10.2 Claims, ClaimsIdentity, ClaimsPrincipal

**Simple words:** A **claim** is one fact about the user, written as *type = value*. Think of your **driving licence**: Name = Ravi, DOB = 1998-04-02, Licence class = Car. Each line is a claim. The licence card itself is a **ClaimsIdentity** (issued by one authority). You (the person) may hold several cards (licence, passport, employee badge) — you are the **ClaimsPrincipal**.

```text
ClaimsPrincipal  (HttpContext.User — "the person")
 └── ClaimsIdentity  (AuthenticationType = "Bearer")  ("one ID card")
      ├── Claim: name  = "admin"
      ├── Claim: role  = "Admin"
      ├── Claim: sub   = "admin"
      └── Claim: jti   = "5c1e..."
```

**Easy example — reading claims in a controller or Minimal API**

```csharp
app.MapGet("/api/me", (ClaimsPrincipal user) => new
{
    Name = user.Identity?.Name,                                  // ClaimTypes.Name
    Id = user.FindFirstValue(ClaimTypes.NameIdentifier),         // usually "sub"
    Roles = user.FindAll(ClaimTypes.Role).Select(c => c.Value)
}).RequireAuthorization();
```

In a controller you use the `User` property of `ControllerBase`: `User.IsInRole("Admin")`, `User.HasClaim("department", "Sales")`.

| API | Returns | Use for |
|---|---|---|
| `User.Identity.IsAuthenticated` | bool | Is anyone logged in? |
| `User.Identity.Name` | string? | Display name |
| `User.FindFirstValue(type)` | string? | One claim value |
| `User.FindAll(type)` | IEnumerable<Claim> | Multi-valued claims (roles) |
| `User.IsInRole("Admin")` | bool | Role check |
| `User.HasClaim(type, value)` | bool | Exact claim check |

⚠️ **Interview trap:** By default the JWT handler *maps* short JWT claim names to long .NET names (`sub` → `ClaimTypes.NameIdentifier`, `role` → `ClaimTypes.Role`). If `FindFirstValue("sub")` returns null, this mapping is why. You can turn it off with `options.MapInboundClaims = false` (then use `"sub"`/`"role"` and set `TokenValidationParameters.RoleClaimType = "role"`).

---

## 10.3 Authentication schemes and handlers

**Simple words:** A **scheme** is a *named way of proving identity* — "Bearer" (JWT), "Cookies", "Google", "ApiKey". Each scheme has a **handler** class that knows how to:

| Handler action | Meaning | Example (JWT) |
|---|---|---|
| **Authenticate** | Read the request, build a `ClaimsPrincipal` | Read `Authorization: Bearer xxx`, validate signature |
| **Challenge** | What to do when user is *not* authenticated | Return `401` + `WWW-Authenticate: Bearer` |
| **Forbid** | What to do when user *is* authenticated but not allowed | Return `403` |
| **SignIn / SignOut** | Only for cookie-like schemes | Write/delete cookie |

Analogy: the building has several entrances — a badge reader (cookie), a security guard checking passports (JWT), a delivery door with a PIN (API key). Each entrance is a scheme; the guard is the handler.

**Easy example — one scheme (the sample app):**

```csharp
builder.Services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)   // "Bearer" is the default
    .AddJwtBearer(o => { /* TokenValidationParameters ... */ });
```

**Complex example — two schemes (browser pages use cookies, mobile apps use JWT):**

```csharp
builder.Services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
    .AddJwtBearer(o => { /* ... */ })
    .AddCookie("Cookies", o =>
    {
        o.LoginPath = "/account/login";
        o.Cookie.HttpOnly = true;                          // JS cannot read it (XSS protection)
        o.Cookie.SecurePolicy = CookieSecurePolicy.Always; // HTTPS only
        o.Cookie.SameSite = SameSiteMode.Lax;              // CSRF mitigation
        o.ExpireTimeSpan = TimeSpan.FromHours(8);
        o.SlidingExpiration = true;
    });

// Choose the scheme per endpoint:
[Authorize(AuthenticationSchemes = "Cookies")]
public class AccountPagesController : Controller { /* ... */ }
```

🎯 **Say this in the interview:** "`AddAuthentication` registers schemes, each backed by a handler that can authenticate, challenge (401) and forbid (403). The default scheme is used when an endpoint doesn't specify one. You can have several — cookies for the web UI and JWT bearer for APIs."

---

## 10.4 Cookies vs JWT (tokens)

**Simple words:**
- **Cookie session** = cloakroom ticket. You hand over your coat, get ticket #42. The *server* remembers what #42 means (or encrypts the info inside the cookie). The browser sends the ticket automatically on every request.
- **JWT** = a signed **concert wristband**. All info is printed on it ("VIP, valid until 11 pm"), and it has a tamper-proof seal. The guard doesn't need to look anything up — they just check the seal.

| | Cookie auth | JWT bearer |
|---|---|---|
| Where stored on client | Browser cookie jar | Memory / storage in the client app |
| How sent | **Automatically** by the browser | **Manually**: `Authorization: Bearer <token>` |
| Server state | Often stateless in ASP.NET Core (encrypted cookie), but can use session store | Stateless (signature check only) |
| Best for | Server-rendered web apps (MVC/Razor), same-site SPAs (BFF pattern) | APIs, mobile apps, service-to-service, cross-domain |
| CSRF risk | **Yes** (browser sends it automatically) → need antiforgery / SameSite | **Low** (attacker site can't add the header) |
| XSS risk | Low if `HttpOnly` | Higher if token is kept in `localStorage` |
| Revocation | Easy (delete server session / change security stamp) | Hard — valid until it **expires** → keep it short + refresh tokens |
| Size | Small | Can grow big (every claim travels in every request) |
| Cross-domain | Hard (cookies are per domain) | Easy |

⚠️ **Interview trap:** "JWT is more secure than cookies." — Neither is "more secure"; they have **different** risks. Cookies → CSRF; JWT in `localStorage` → XSS theft and no easy revocation.

---

## 10.5 What is a JWT, really?

**JWT = JSON Web Token**, pronounced "jot". It has **three Base64Url parts separated by dots**:

```text
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9 . eyJzdWIiOiJhZG1pbiIsInJvbGUiOiJBZG1pbiIsImV4cCI6MTc2NDA... . 4Xq9k2...
└──────────── HEADER ─────────────┘   └──────────────────── PAYLOAD ────────────────────┘   └ SIGNATURE ┘
```

| Part | Contains | Example (decoded) |
|---|---|---|
| **Header** | Algorithm + type | `{ "alg": "HS256", "typ": "JWT" }` |
| **Payload** | Claims | `{ "sub": "admin", "role": "Admin", "iss": "ShopApi", "aud": "ShopApiClients", "exp": 1764090000, "jti": "5c1e..." }` |
| **Signature** | `HMACSHA256(base64url(header) + "." + base64url(payload), secretKey)` | binary, Base64Url-encoded |

**Base64Url** = Base64 where `+` → `-`, `/` → `_`, and `=` padding is removed, so it is safe inside URLs and headers.

### The #1 JWT fact: it is **signed, NOT encrypted**

Anyone can paste a JWT into jwt.io and **read** the payload. The signature only proves **nobody changed it** and **who issued it**.

```csharp
// Anyone can do this — no key needed!
string payload = token.Split('.')[1];
string json = Encoding.UTF8.GetString(Base64UrlEncoder.DecodeBytes(payload));
```

⚠️ **Interview trap:** Never put passwords, credit card numbers, or other secrets in a JWT payload. If you truly need confidentiality, use **JWE** (encrypted JWT) — rarely needed.

### Registered claims you must know

| Claim | Meaning |
|---|---|
| `iss` | Issuer — who created the token |
| `aud` | Audience — which API the token is for |
| `sub` | Subject — the user id |
| `exp` | Expiry (Unix seconds) |
| `nbf` | Not before |
| `iat` | Issued at |
| `jti` | Unique token id (useful for revocation lists) |

### Symmetric vs asymmetric signing

| | HS256 (symmetric) | RS256 / ES256 (asymmetric) |
|---|---|---|
| Keys | One shared secret | Private key signs, public key verifies |
| Who can create tokens? | Anyone who knows the secret (incl. every API that validates) | Only the identity provider |
| Used by | Small/single apps (the sample app) | Entra ID, Auth0, IdentityServer, any multi-service system |
| Key distribution | Must share secret securely | Public keys published at `/.well-known/openid-configuration` → `jwks_uri` |

🎯 **Say this in the interview:** "A JWT is header.payload.signature, each Base64Url-encoded. It's signed, not encrypted, so anyone can read it but nobody can change it without the key. The API validates signature, issuer, audience and lifetime, then turns the claims into `HttpContext.User`."

---

## 10.6 Validating JWTs — `AddJwtBearer` line by line

The sample app's real configuration (`Program.cs`):

```csharp
JwtOptions jwt = builder.Configuration.GetSection(JwtOptions.SectionName).Get<JwtOptions>() ?? new JwtOptions();
builder.Services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
    .AddJwtBearer(o =>
    {
        o.TokenValidationParameters = new TokenValidationParameters
        {
            ValidateIssuer = true,
            ValidIssuer = jwt.Issuer,
            ValidateAudience = true,
            ValidAudience = jwt.Audience,
            ValidateLifetime = true,
            ValidateIssuerSigningKey = true,
            IssuerSigningKey = new SymmetricSecurityKey(Encoding.UTF8.GetBytes(jwt.Key)),
            ClockSkew = TimeSpan.FromSeconds(30)
        };
    });
```

| Setting | What it protects against |
|---|---|
| `ValidateIssuer` / `ValidIssuer` | Tokens created by someone else |
| `ValidateAudience` / `ValidAudience` | A token meant for *another* API being replayed at yours |
| `ValidateLifetime` | Expired tokens (`exp`) |
| `ValidateIssuerSigningKey` + `IssuerSigningKey` | Forged/modified tokens |
| `ClockSkew` | Clock differences between servers. **Default is 5 minutes!** — so a "1-minute" token actually lives 6 minutes. The sample reduces it to 30 s. |

And the options class (`Options\JwtOptions.cs`) validated at startup:

```csharp
public class JwtOptions
{
    public const string SectionName = "Jwt";

    [Required] public string Issuer { get; set; } = "";
    [Required] public string Audience { get; set; } = "";

    // HS256 needs at least 32 bytes. In real apps keep this in Key Vault / user-secrets.
    [Required, MinLength(32)] public string Key { get; set; } = "";

    [Range(1, 1440)] public int ExpiryMinutes { get; set; } = 60;
}
```

**Real-world (Entra ID / any OpenID Connect provider)** — you don't set the key at all, the middleware downloads it:

```csharp
builder.Services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
    .AddJwtBearer(o =>
    {
        o.Authority = "https://login.microsoftonline.com/<tenant-id>/v2.0"; // discovers keys via jwks_uri
        o.Audience = "api://shop-api";
    });
```

⚠️ **Interview trap:** Setting `ValidateIssuerSigningKey = false` or `ValidateLifetime = false` "to make it work" in production is a serious security bug — any forged/expired token would be accepted.

---

## 10.7 Issuing tokens — the sample `AuthController`

```csharp
[ApiController]
[Route("api/[controller]")]
public class AuthController(IOptions<JwtOptions> jwtOptions) : ControllerBase
{
    // DEMO ONLY: hard-coded users. Real apps use ASP.NET Core Identity / Entra ID and hashed passwords.
    private static readonly Dictionary<string, (string Password, string Role)> Users = new()
    {
        ["admin"] = ("admin123", "Admin"),
        ["user"] = ("user123", "User")
    };

    [HttpPost("token")]
    public IActionResult Token(LoginRequest request)
    {
        if (!Users.TryGetValue(request.UserName, out var u) || u.Password != request.Password)
            return Unauthorized();

        JwtOptions o = jwtOptions.Value;
        var claims = new[]
        {
            new Claim(JwtRegisteredClaimNames.Sub, request.UserName),
            new Claim(ClaimTypes.Name, request.UserName),
            new Claim(ClaimTypes.Role, u.Role),
            new Claim(JwtRegisteredClaimNames.Jti, Guid.NewGuid().ToString())
        };

        var key = new SymmetricSecurityKey(Encoding.UTF8.GetBytes(o.Key));
        var token = new JwtSecurityToken(
            issuer: o.Issuer,
            audience: o.Audience,
            claims: claims,
            expires: DateTime.UtcNow.AddMinutes(o.ExpiryMinutes),
            signingCredentials: new SigningCredentials(key, SecurityAlgorithms.HmacSha256));

        return Ok(new { access_token = new JwtSecurityTokenHandler().WriteToken(token) });
    }
}
```

**End-to-end flow with the sample app:**

```bash
# 1. Login -> get a token
curl -X POST https://localhost:5001/api/auth/token \
     -H "Content-Type: application/json" \
     -d '{"userName":"admin","password":"admin123"}'
# {"access_token":"eyJhbGciOi..."}

# 2. Call a protected endpoint
curl -X DELETE https://localhost:5001/api/products/3 \
     -H "Authorization: Bearer eyJhbGciOi..."
# 204 No Content   (as "user"/"user123" you would get 403)
```

Notes:
- Notice the issuer and validator use the **same** `Issuer`, `Audience` and `Key` from config — if any differs you get 401 with a `WWW-Authenticate: Bearer error="invalid_token"` header.
- Newer code may use `JsonWebTokenHandler` + `SecurityTokenDescriptor` (faster, in `Microsoft.IdentityModel.JsonWebTokens`); `JwtSecurityTokenHandler` still works and is what most interview answers show.
- In production you usually **don't write your own token endpoint** — an identity provider (Entra ID, Auth0, Keycloak, Duende IdentityServer) issues tokens.

---

## 10.8 Refresh tokens

**Problem:** Short-lived access tokens (5–60 min) are safer (a stolen token expires quickly) but users hate logging in every 15 minutes.

**Solution:** two tokens.

| | Access token | Refresh token |
|---|---|---|
| Format | JWT | Random opaque string (not JWT usually) |
| Lifetime | Short (5–60 min) | Long (days/weeks) |
| Sent to | Every API call | **Only** the token endpoint |
| Stored on server? | No | **Yes** (DB, hashed) — so it can be revoked |

Analogy: the access token is a **day pass** to the gym; the refresh token is your **membership card** that you show at reception to get a new day pass. Reception can cancel your membership.

```text
Client                         API / Auth server
  │ POST /auth/token (user+pwd)  │
  │─────────────────────────────►│  issue access(15m) + refresh(7d), save refresh in DB
  │◄─────────────────────────────│
  │ GET /api/orders (access)     │
  │─────────────────────────────►│  200
  │ ... 15 minutes later ...     │
  │ GET /api/orders (access)     │  401 (expired)
  │ POST /auth/refresh (refresh) │
  │─────────────────────────────►│  check DB: exists? not revoked? not expired?
  │                              │  → ROTATE: revoke old refresh, issue new access + new refresh
  │◄─────────────────────────────│
```

Generating a refresh token (compile-verified):

```csharp
public record RefreshToken(string Token, string UserName, DateTime ExpiresUtc, bool Revoked);

public static class RefreshTokenFactory
{
    public static RefreshToken Create(string userName) =>
        new(Convert.ToBase64String(System.Security.Cryptography.RandomNumberGenerator.GetBytes(64)),
            userName, DateTime.UtcNow.AddDays(7), Revoked: false);
}
```

Best practices: **rotation** (each refresh token can be used once), **reuse detection** (if an old one is used again → revoke the whole family, someone stole it), store **hashed**, send to browsers in an `HttpOnly` cookie.

🎯 **Say this in the interview:** "Access tokens are short-lived and stateless; refresh tokens are long-lived, stored server-side so they can be revoked, and rotated on every use. That gives security without forcing users to log in constantly."

---

## 10.9 `[Authorize]`, `[AllowAnonymous]` and friends

```csharp
[ApiController]
[Route("api/[controller]")]
[Authorize]                                   // every action requires a logged-in user...
public class OrdersController : ControllerBase
{
    [HttpGet("public-stats")]
    [AllowAnonymous]                          // ...except this one
    public IActionResult Stats() => Ok();

    [HttpDelete("{id:int}")]
    [Authorize(Roles = "Admin,Manager")]     // Admin OR Manager
    public IActionResult Delete(int id) => NoContent();

    [HttpPost("{id:int}/refund")]
    [Authorize(Roles = "Admin")]
    [Authorize(Roles = "Finance")]           // Admin AND Finance (attributes are AND-ed)
    public IActionResult Refund(int id) => Ok();
}
```

The sample's `ProductsController`:

```csharp
[HttpPost]
[Authorize]                               // any logged-in user
public async Task<ActionResult<ProductDto>> Create(CreateProductRequest request, CancellationToken ct) { /* ... */ }

[HttpDelete("{id:int}")]
[Authorize(Policy = "AdminOnly")]         // only Admin role
public async Task<IActionResult> Delete(int id, CancellationToken ct) { /* ... */ }
```

Minimal API equivalents (from `Endpoints\CategoryEndpoints.cs`):

```csharp
group.MapPost("/", async Task<Results<Created<CategoryDto>, ValidationProblem>> (CreateCategoryRequest req, AppDbContext db, CancellationToken ct) =>
{
    /* ... */
}).RequireAuthorization("AdminOnly");

// also: .AllowAnonymous(), group.RequireAuthorization() for a whole group
```

**Secure-by-default (fallback policy)** — every endpoint requires authentication unless marked `[AllowAnonymous]`:

```csharp
builder.Services.AddAuthorizationBuilder()
    .SetFallbackPolicy(new AuthorizationPolicyBuilder().RequireAuthenticatedUser().Build());
```

| Rule | Behaviour |
|---|---|
| Multiple `[Authorize]` attributes | **AND** — all must pass |
| `Roles = "A,B"` in one attribute | **OR** — any role |
| `[AllowAnonymous]` | Wins over `[Authorize]` at any level |
| Fallback policy | Applies to endpoints with **no** authorization metadata |

⚠️ **Interview trap:** `[AllowAnonymous]` on an action beats `[Authorize]` on the controller, *and* `[AllowAnonymous]` on a controller beats `[Authorize]` on its actions — the anonymous one always wins. Be careful when placing it at class level.

---

## 10.10 Roles vs Claims vs Policies

**Simple words**
- **Role** = your job title ("Admin", "Manager"). Coarse-grained.
- **Claim** = any fact ("department = Sales", "subscription = Premium", "birthdate = 2000-01-01").
- **Policy** = a **named rule** that can combine roles, claims and custom code ("CanRefund = Admin OR has permission orders.refund").

Analogy (office building): Role = "Employee"/"Visitor" badge colour; Claim = "Floor access: 3, 5"; Policy = "Server room: must be IT staff AND have completed safety training AND it must be working hours".

| | Roles | Claims | Policies |
|---|---|---|---|
| Example | `[Authorize(Roles="Admin")]` | `RequireClaim("department","Sales")` | `[Authorize(Policy="CanRefund")]` |
| Flexibility | Low | Medium | **High** (any C# logic) |
| Where logic lives | Scattered strings in attributes | Policy registration | One central place |
| Testable | Hard | Medium | **Easy** (handler is a class) |
| Recommended | Small apps | Fine-grained facts | **Modern default** — roles and claims are just special policies |

**Easy → complex policy registration** (compile-verified):

```csharp
builder.Services.AddAuthorizationBuilder()                                  // .NET 8+ fluent API
    .AddPolicy("AdminOnly", p => p.RequireRole("Admin"))                    // the sample app's policy
    .AddPolicy("SalesDept", p => p.RequireClaim("department", "Sales", "Marketing"))
    .AddPolicy("AtLeast18", p => p.AddRequirements(new MinimumAgeRequirement(18)))
    .AddPolicy("OrderOwner", p => p.AddRequirements(new SameOwnerRequirement()))
    .AddPolicy("CanRefund", p => p.RequireAuthenticatedUser().RequireAssertion(ctx =>
        ctx.User.IsInRole("Admin") || ctx.User.HasClaim("permission", "orders.refund")));
```

> Before .NET 8 you wrote `builder.Services.AddAuthorization(o => o.AddPolicy("AdminOnly", p => p.RequireRole("Admin")));` — same thing.

🎯 **Say this in the interview:** "I prefer policy-based authorization. Under the hood roles and claims are just requirements inside policies. Policies put the rule in one named place, so controllers just say `[Authorize(Policy = "CanRefund")]`, and complex rules go into testable `AuthorizationHandler` classes."

---

## 10.11 Custom requirement + `AuthorizationHandler`

The pieces:

| Piece | Role | Analogy |
|---|---|---|
| `IAuthorizationRequirement` | Data describing the rule ("min age 18") | The sign "18+ only" |
| `AuthorizationHandler<TRequirement>` | Code that checks it | The bouncer checking IDs |
| Policy | Name → list of requirements | "Club entry policy" |
| `context.Succeed(req)` | Requirement met | Bouncer lets you in |
| Not calling `Succeed` | Requirement not met (another handler may still succeed) | Bouncer says nothing |
| `context.Fail()` | Hard fail — no other handler can rescue it | Bouncer calls security |

### Easy-ish example — MinimumAge (compile-verified and unit-tested)

```csharp
public class MinimumAgeRequirement(int minimumAge) : IAuthorizationRequirement
{
    public int MinimumAge { get; } = minimumAge;
}

public class MinimumAgeHandler(TimeProvider clock) : AuthorizationHandler<MinimumAgeRequirement>
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, MinimumAgeRequirement requirement)
    {
        Claim? birthClaim = context.User.FindFirst(c => c.Type == ClaimTypes.DateOfBirth);
        if (birthClaim is null)
            return Task.CompletedTask;               // no Succeed() => requirement not met

        DateOnly birthDate = DateOnly.Parse(birthClaim.Value, CultureInfo.InvariantCulture);
        DateOnly today = DateOnly.FromDateTime(clock.GetUtcNow().UtcDateTime);
        int age = today.Year - birthDate.Year;
        if (birthDate > today.AddYears(-age)) age--;  // birthday not reached yet this year

        if (age >= requirement.MinimumAge)
            context.Succeed(requirement);

        return Task.CompletedTask;
    }
}

// Registration
builder.Services.AddSingleton(TimeProvider.System);   // TimeProvider = .NET 8+, makes "now" testable
builder.Services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();
builder.Services.AddAuthorizationBuilder()
    .AddPolicy("AtLeast18", p => p.AddRequirements(new MinimumAgeRequirement(18)));

// Usage
[Authorize(Policy = "AtLeast18")]
[HttpGet("wine")]
public IActionResult BuyWine() => Ok();
```

### Complex example — resource-based authorization ("only the owner can edit")

Policy attributes run **before** your action, so they don't know *which* order is being edited. For "user can edit **their own** order", you must load the resource first and then call `IAuthorizationService` yourself. This is also how you prevent **IDOR** (see chapter 14).

```csharp
public class SameOwnerRequirement : IAuthorizationRequirement;

public class OrderOwnerHandler : AuthorizationHandler<SameOwnerRequirement, Order>
{
    protected override Task HandleRequirementAsync(
        AuthorizationHandlerContext context, SameOwnerRequirement requirement, Order order)
    {
        string? userId = context.User.FindFirstValue(ClaimTypes.NameIdentifier);

        if (context.User.IsInRole("Admin") || (userId is not null && userId == order.OwnerId))
            context.Succeed(requirement);

        return Task.CompletedTask;
    }
}

// Registration
builder.Services.AddSingleton<IAuthorizationHandler, OrderOwnerHandler>();
builder.Services.AddAuthorizationBuilder()
    .AddPolicy("OrderOwner", p => p.AddRequirements(new SameOwnerRequirement()));

// Endpoint
app.MapPut("/api/orders/{id:int}", async (int id, UpdateOrderRequest req, ClaimsPrincipal user,
    IOrderRepository repo, IAuthorizationService authz, CancellationToken ct) =>
{
    Order? order = await repo.FindAsync(id, ct);
    if (order is null) return Results.NotFound();

    AuthorizationResult result = await authz.AuthorizeAsync(user, order, "OrderOwner");
    if (!result.Succeeded) return Results.Forbid();   // 403

    order.Status = req.Status;
    await repo.SaveAsync(order, ct);
    return Results.NoContent();
}).RequireAuthorization();   // must be logged in (401 otherwise)
```

Verified behaviour in an integration test: owner "alice" → **204**, other user "bob" → **403**, "Admin" → **204**, anonymous → **401**.

In a controller: `if (!(await _authz.AuthorizeAsync(User, order, "OrderOwner")).Succeeded) return Forbid();`

⚠️ **Interview trap:** Handlers are resolved from DI — forgetting `AddSingleton<IAuthorizationHandler, ...>()` means `Succeed` is never called → every request gets **403** with no error. Also: if a handler depends on a scoped service (e.g. `DbContext`), register the handler as **scoped**.

---

## 10.12 401 vs 403 — the complete picture

| Situation | Status | Who produces it |
|---|---|---|
| No token / no cookie | 401 | Authentication handler **Challenge** |
| Token expired / bad signature / wrong audience | 401 | Challenge (`WWW-Authenticate: Bearer error="invalid_token"`) |
| Valid token, missing role / policy fails | 403 | Authentication handler **Forbid** |
| Resource-based check fails | 403 (`Results.Forbid()` / `Forbid()`) | Your code |
| Wrong username/password at login | 401 (sample's `AuthController`) | Your code |
| Cookie scheme, not logged in | **302 redirect** to `/account/login` (not 401!) | Cookie handler challenge |

The sample's integration tests prove it:

```csharp
[Fact]
public async Task CreateProduct_WithoutToken_Returns401() { /* ... */ Assert.Equal(HttpStatusCode.Unauthorized, res.StatusCode); }

[Fact]
public async Task DeleteProduct_AsUser_Returns403()
{
    HttpClient client = await ClientAsAsync("user", "user123");
    HttpResponseMessage res = await client.DeleteAsync("/api/products/3");
    Assert.Equal(HttpStatusCode.Forbidden, res.StatusCode);
}
```

⚠️ **Interview trap:** Some teams return **404 instead of 403** for resources the user may not even know exist (e.g. `/orders/123` of another customer) to avoid leaking that the id exists. Mention it as a conscious choice.

---

## 10.13 OAuth 2.0 vs OpenID Connect

**Simple words**
- **OAuth 2.0** = **authorization** (delegated access). "Let this app access *my photos* on Google without giving it my password." Output: an **access token**.
- **OpenID Connect (OIDC)** = a thin **identity layer on top of OAuth 2.0**. "Tell this app *who I am*." Output: an **ID token** (always a JWT) + access token.

Analogy: **Hotel key card**. OAuth = reception gives the cleaner a key card that opens *your room only, today only* — without giving them your passport. OIDC = reception additionally gives a signed note saying "This is Mr. Sharma, room 304".

| | OAuth 2.0 | OpenID Connect |
|---|---|---|
| Purpose | Authorization / API access | Authentication / login |
| Token | Access token (format not specified) | **ID token (JWT)** + access token |
| Scope | `orders.read`, `photos.write` | `openid` (required) `profile` `email` |
| Extras | — | `/userinfo`, discovery doc `/.well-known/openid-configuration` |
| Use | "Call the API on behalf of user" | "Sign in with Microsoft/Google" |

**Roles in OAuth:** Resource Owner (user), Client (app), Authorization Server (Entra ID), Resource Server (your API).

### Flows (grant types) you must know

| Flow | Who uses it | User involved? | Notes |
|---|---|---|---|
| **Authorization Code + PKCE** | SPAs, mobile, web apps | Yes | **Recommended for any user login**. PKCE protects public clients (no secret). |
| **Client Credentials** | Service-to-service, daemons, background jobs | No | App authenticates with its own secret/certificate |
| On-Behalf-Of (Entra) | API calling another API as the user | Yes (indirect) | Exchange incoming token for downstream token |
| Device code | TVs, CLIs | Yes (on another device) | `az login --use-device-code` |
| ~~Implicit~~ | Old SPAs | — | **Deprecated** — token in URL fragment |
| ~~Password (ROPC)~~ | Legacy | — | **Deprecated** — app sees the password |

**Authorization Code + PKCE, step by step:**

```text
1. App creates random code_verifier, computes code_challenge = SHA256(code_verifier)
2. Browser → /authorize?response_type=code&client_id=..&code_challenge=..&scope=openid api://shop/orders.read
3. User logs in at the identity provider (your app NEVER sees the password)
4. IdP redirects back → https://app/callback?code=AUTH_CODE
5. App → POST /token  { code=AUTH_CODE, code_verifier }   (back channel)
6. IdP checks SHA256(code_verifier) == code_challenge → returns id_token + access_token (+ refresh_token)
7. App calls API with Authorization: Bearer access_token
```

PKCE (Proof Key for Code Exchange, "pixie") stops an attacker who **steals the code** in step 4 — they don't have the `code_verifier`.

**Client credentials** (e.g. a nightly job calling the Shop API):

```text
POST /oauth2/v2.0/token
grant_type=client_credentials&client_id=..&client_secret=..&scope=api://shop-api/.default
→ { "access_token": "...", "expires_in": 3599 }
```

🎯 **Say this in the interview:** "OAuth 2.0 is about delegated authorization — getting an access token to call an API. OpenID Connect adds authentication on top, giving an ID token that says who the user is. For user sign-in I'd use Authorization Code with PKCE; for service-to-service, Client Credentials. Implicit and password grants are deprecated."

---

## 10.14 ASP.NET Core Identity, Entra ID and Microsoft.Identity.Web

| Option | What it is | When |
|---|---|---|
| **ASP.NET Core Identity** | A library: user/role tables in **your** DB (EF Core), password hashing, lockout, 2FA, email confirmation, `UserManager<T>`, `SignInManager<T>` | You own the user database (small/medium apps) |
| .NET 8+ **Identity API endpoints** | `builder.Services.AddIdentityApiEndpoints<IdentityUser>()` + `app.MapIdentityApi<IdentityUser>()` → `/register`, `/login`, `/refresh` | Quick token/cookie auth for SPAs without writing an AuthController |
| **Microsoft Entra ID** (formerly Azure AD) | Cloud identity provider (OIDC/OAuth2) | Enterprise / Microsoft-365 users, SSO |
| **Entra External ID / Azure AD B2C** | Customer identities | Public consumer apps |
| **Duende IdentityServer / Keycloak / Auth0** | Self-hosted or SaaS OIDC providers | You need your own token server |
| **Microsoft.Identity.Web** | NuGet that wires Entra ID into ASP.NET Core in a few lines, plus token acquisition for downstream APIs | APIs and web apps on Entra ID |

```csharp
// Identity basics
builder.Services.AddDbContext<AppIdentityDbContext>(o => o.UseSqlServer(cs));
builder.Services.AddIdentityApiEndpoints<IdentityUser>()
    .AddEntityFrameworkStores<AppIdentityDbContext>();
app.MapIdentityApi<IdentityUser>();

// Microsoft.Identity.Web (protect an API with Entra ID)
builder.Services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
    .AddMicrosoftIdentityWebApi(builder.Configuration.GetSection("AzureAd"));
```

```json
"AzureAd": {
  "Instance": "https://login.microsoftonline.com/",
  "TenantId": "<tenant-guid>",
  "ClientId": "<api-app-registration-id>",
  "Audience": "api://shop-api"
}
```

*(The Identity and Microsoft.Identity.Web snippets are shown for orientation and were not compile-verified in this book.)*

🎯 **Say this in the interview:** "If we own the users I'd use ASP.NET Core Identity — it already does password hashing, lockout and 2FA. In an enterprise I'd delegate to Entra ID via Microsoft.Identity.Web, so the API only validates tokens and never stores passwords."

---

## 10.15 Password hashing

**Simple words:** Never store passwords. Store a **slow, salted hash**. When the user logs in, hash what they typed and compare.

| Approach | Verdict |
|---|---|
| Plain text | ❌ Never (the sample's demo users are plain text **only** because it's a demo) |
| Encryption (AES) | ❌ Reversible — whoever has the key has all passwords |
| MD5 / SHA-256 | ❌ Too **fast** → billions of guesses/sec with GPUs |
| SHA-256 + salt | ❌ Still too fast |
| **PBKDF2 / bcrypt / scrypt / Argon2** with salt + many iterations | ✅ Deliberately slow |

- **Salt** = random bytes per user, stored with the hash → identical passwords get different hashes, rainbow tables useless.
- ASP.NET Core Identity's `PasswordHasher<T>` uses **PBKDF2 with HMAC-SHA512, 100,000 iterations** (v3 format, .NET 7+) and embeds the salt in the hash string.

Compile-verified usage without full Identity:

```csharp
public class UserPasswords(IPasswordHasher<AppUser> hasher)
{
    public void SetPassword(AppUser user, string plainPassword)
        => user.PasswordHash = hasher.HashPassword(user, plainPassword);

    public bool Verify(AppUser user, string plainPassword)
        => hasher.VerifyHashedPassword(user, user.PasswordHash, plainPassword)
           is PasswordVerificationResult.Success or PasswordVerificationResult.SuccessRehashNeeded;
}

// builder.Services.AddScoped<IPasswordHasher<AppUser>, PasswordHasher<AppUser>>();
```

`SuccessRehashNeeded` means "correct, but hashed with old settings — re-hash and save now".

---

## 10.16 Storing secrets (JWT keys, connection strings, client secrets)

| Where | Good for | Notes |
|---|---|---|
| `appsettings.json` in git | ❌ Never for real secrets | The sample's key is labelled `DEMO-ONLY-change-me...` |
| **User Secrets** (`dotnet user-secrets set "Jwt:Key" "..."`) | Local development | Stored in your user profile, outside the repo, only loaded in Development |
| **Environment variables** (`Jwt__Key`) | Containers, CI | `__` = `:` in config keys |
| **Azure Key Vault** (`builder.Configuration.AddAzureKeyVault(...)`) | Production | Use **Managed Identity** → no secret needed to read secrets |
| Kubernetes Secrets / Docker secrets | Containers | Mounted as files/env vars |

```bash
dotnet user-secrets init
dotnet user-secrets set "Jwt:Key" "a-very-long-random-local-dev-key-of-32-bytes+"
```

Because configuration sources are layered, the **same** code `builder.Configuration["Jwt:Key"]` reads from user-secrets locally and from Key Vault in production. More in chapter 14.

---

## 10.17 CORS — Cross-Origin Resource Sharing

**Simple words:** Browsers enforce the **Same-Origin Policy**: JavaScript loaded from `https://shop.com` may not read responses from `https://api.shop.com` (different *origin* = scheme + host + port). **CORS** is how the **server** tells the browser: "it's OK, I trust that origin".

Analogy: a **guest list** at the door of a private party. The bouncer (browser) checks whether the guest's name (Origin) is on the host's (API's) list.

⚠️ **Interview trap:** CORS is **not** a security feature that protects your API from attackers. It's enforced **only by browsers**. Postman, curl or a malicious server ignore it completely. It *relaxes* the browser's same-origin policy; it doesn't replace authentication.

| Request | Same origin? |
|---|---|
| `https://shop.com` → `https://shop.com/api` | ✅ Same |
| `https://shop.com` → `https://api.shop.com` | ❌ Different host |
| `http://localhost:4200` → `http://localhost:5000` | ❌ Different port |
| `http://shop.com` → `https://shop.com` | ❌ Different scheme |

### Simple vs preflight requests

A **preflight** is an automatic `OPTIONS` request the browser sends **before** the real request when the request is "non-simple": methods like `PUT`/`DELETE`, headers like `Authorization`, or `Content-Type: application/json`.

```text
Browser (origin http://localhost:4200)                 API
  OPTIONS /api/products                                 │
  Origin: http://localhost:4200                         │
  Access-Control-Request-Method: POST                   │
  Access-Control-Request-Headers: authorization,content-type
  ─────────────────────────────────────────────────────►│
  204  Access-Control-Allow-Origin: http://localhost:4200
       Access-Control-Allow-Methods: POST
       Access-Control-Allow-Headers: authorization,content-type
       Access-Control-Max-Age: 600
  ◄─────────────────────────────────────────────────────│
  POST /api/products  (the real request)                │
  ─────────────────────────────────────────────────────►│
```

**Easy — the sample app:**

```csharp
builder.Services.AddCors(o => o.AddPolicy("Frontend", p => p
    .WithOrigins("http://localhost:4200")
    .AllowAnyHeader()
    .AllowAnyMethod()));
// ...
app.UseCors("Frontend");
```

**Complex — production policy with cookies (compile-verified):**

```csharp
builder.Services.AddCors(o =>
{
    o.AddPolicy("Frontend", p => p
        .WithOrigins("https://shop.example.com", "http://localhost:4200")
        .WithMethods("GET", "POST", "PUT", "DELETE")
        .WithHeaders("Content-Type", "Authorization")
        .AllowCredentials()                               // allow cookies / auth headers
        .SetPreflightMaxAge(TimeSpan.FromMinutes(10)));    // cache preflight → fewer OPTIONS calls
});
```

⚠️ **Interview trap:** `AllowAnyOrigin()` + `AllowCredentials()` is **not allowed** — the CORS spec forbids `Access-Control-Allow-Origin: *` with credentials, and ASP.NET Core throws an `InvalidOperationException` when the policy is built. List explicit origins instead (or `SetIsOriginAllowed(...)` carefully).

⚠️ **Interview trap:** Middleware order: `UseCors` must be after `UseRouting` (implicit in minimal hosting) and **before** `UseAuthentication`/`UseAuthorization`, `UseResponseCaching` and endpoints — otherwise preflight requests get 401 or responses miss CORS headers.

Other ways to apply: `[EnableCors("Frontend")]`, `[DisableCors]`, `.RequireCors("Frontend")` on minimal endpoints.

🎯 **Say this in the interview:** "CORS lets a browser app from one origin call an API on another origin. For non-simple requests the browser sends an OPTIONS preflight, and the server answers with Access-Control-Allow-* headers. It's enforced only by browsers, so it's not a substitute for authentication. And you can't combine AllowAnyOrigin with AllowCredentials."

---

## 10.18 Putting it all together

```text
1. POST /api/auth/token {admin, admin123}
       → AuthController validates password → creates JWT (sub, name, role=Admin, jti, exp)
2. DELETE /api/products/3   Authorization: Bearer <jwt>
       → UseCors          (adds CORS headers for browser callers)
       → UseAuthentication (JwtBearer handler: check signature/iss/aud/exp → HttpContext.User)
       → UseAuthorization  (endpoint has Policy "AdminOnly" → RequireRole("Admin") → OK)
       → ProductsController.Delete → 204
   Same call as "user" → 403.   No header → 401.
```

---

## Try it yourself

1. In the sample app, change the "AdminOnly" policy so that users with the claim `permission = products.delete` are also allowed. What code would you write?
2. A colleague says: "Our JWT contains the user's salary; it's fine because it's signed." What do you reply?
3. Your Angular app at `http://localhost:4200` gets "blocked by CORS policy" on `PUT /api/products/1`, but GET works. What is the likely cause?
4. Your JWT is configured to expire in 1 minute, but it still works after 4 minutes. Why?
5. A user can view `/api/orders/57` which belongs to someone else. Name the vulnerability and the fix.
6. Write a policy "PremiumCustomer" that requires an authenticated user with claim `subscription` equal to `Gold` or `Platinum`.

<details><summary>Answer</summary>

1. Use `RequireAssertion`:
   ```csharp
   .AddPolicy("AdminOnly", p => p.RequireAssertion(ctx =>
       ctx.User.IsInRole("Admin") || ctx.User.HasClaim("permission", "products.delete")))
   ```
   (Or a custom requirement + handler; two handlers for the same requirement act as OR.)
2. JWTs are only **signed, not encrypted** — the payload is Base64Url and anyone holding the token can decode it. Remove sensitive data; look it up server-side by `sub` instead (or use JWE if truly needed).
3. PUT (and `Authorization`/JSON content type) triggers a **preflight**. Either the policy doesn't allow that method/header, `UseCors` is placed after auth (preflight gets 401), or the policy isn't applied to that endpoint. Check `WithMethods`/`AllowAnyMethod` and the middleware order.
4. `ClockSkew` defaults to **5 minutes**. Set `ClockSkew = TimeSpan.FromSeconds(30)` (like the sample) or `TimeSpan.Zero`.
5. **IDOR** (Insecure Direct Object Reference / Broken Object Level Authorization). Fix with **resource-based authorization**: load the order, then `IAuthorizationService.AuthorizeAsync(User, order, "OrderOwner")`, or filter queries by the current user id (`Where(o => o.OwnerId == userId)`).
6. ```csharp
   builder.Services.AddAuthorizationBuilder()
       .AddPolicy("PremiumCustomer", p => p
           .RequireAuthenticatedUser()
           .RequireClaim("subscription", "Gold", "Platinum"));
   ```
</details>

---

## Quick revision

- **AuthN = who are you (401); AuthZ = what can you do (403).** `UseAuthentication` before `UseAuthorization`.
- `HttpContext.User` is a **ClaimsPrincipal** → has **ClaimsIdentities** → have **Claims** (type/value).
- A **scheme** = named auth method; its **handler** does Authenticate / Challenge (401) / Forbid (403).
- **JWT = header.payload.signature**, Base64Url, **signed not encrypted** — never put secrets in it.
- Validate **issuer, audience, lifetime, signing key**; default **ClockSkew = 5 min**.
- HS256 = shared secret; RS256 = private key signs, public key (JWKS) verifies — used by Entra ID.
- Short access token + long, server-stored, **rotated refresh token**.
- **Policies** are the modern way; roles/claims are just built-in requirements. Custom rules → `IAuthorizationRequirement` + `AuthorizationHandler<T>` registered in DI.
- **Resource-based authorization** (`IAuthorizationService.AuthorizeAsync(user, resource, policy)`) prevents IDOR.
- **OAuth2 = authorization (access token); OIDC = authentication (ID token)**. Users → Auth Code + PKCE; services → Client Credentials.
- Passwords: **PBKDF2/bcrypt/Argon2 + salt** (`PasswordHasher<T>`), never plain/MD5/SHA.
- **CORS** is browser-only; preflight = `OPTIONS`; **AllowAnyOrigin + AllowCredentials is forbidden**; secrets → user-secrets locally, Key Vault in prod.

---

# 11. Error Handling, Logging & Health Checks

> **Why interviewers ask this**
> Code that works on your laptop is easy. Code that **fails gracefully in production** — returns a clean error, never leaks a stack trace, writes a useful log line with a correlation id, and tells the load balancer it's unhealthy — is what separates a 2-YOE developer from a beginner. Expect questions like *"How do you handle exceptions globally?"*, *"What is ProblemDetails?"*, *"Liveness vs readiness?"*

---

## 11.1 The big picture

Analogy: a **hospital**.
- **try/catch** = a nurse treating a small cut on the spot.
- **Global exception handler** = the emergency room — anything nobody else handled ends up here, gets stabilised, and the family gets a calm, standard message (not the doctor's raw notes = stack trace).
- **Logging** = the patient's medical record — detailed, structured, for the doctors (developers), not for the family (clients).
- **Health checks** = the heart-rate monitor the hospital management (load balancer / Kubernetes) watches.

```text
Request ─► UseExceptionHandler ─► ... ─► Endpoint throws KeyNotFoundException
                 ▲                                    │
                 └───────── exception bubbles up ─────┘
           IExceptionHandler maps it → 404 ProblemDetails JSON
           + logger.LogWarning(...) with CorrelationId in scope
```

---

## 11.2 Four ways to handle exceptions — compared

| Strategy | Scope | Best for | Downsides |
|---|---|---|---|
| **try/catch** in code | One method | **Expected**, recoverable errors you can actually handle (retry, fallback, translate) | Copy-paste everywhere if used for everything; easy to swallow errors |
| **Custom exception middleware** | Whole pipeline | Pre-.NET 8 global handling | You write response formatting yourself |
| **`UseExceptionHandler` + `IExceptionHandler`** (.NET 8+) | Whole pipeline | ✅ **Recommended global handler** | — |
| **Exception filters** (`IExceptionFilter`) | MVC controllers only | Controller-specific translation | Don't catch errors from middleware, minimal APIs, or result execution |

### 1. try/catch — only where you can *do something*

```csharp
public async Task<decimal> GetRateOrDefaultAsync(string currency, CancellationToken ct)
{
    try
    {
        return await _rates.GetRateAsync(currency, ct) ?? 1m;
    }
    catch (HttpRequestException ex)
    {
        _logger.LogWarning(ex, "Rate service down, using fallback rate for {Currency}", currency);
        return 1m;                                   // ✅ meaningful fallback
    }
}
```

❌ Anti-patterns:

```csharp
catch (Exception) { }                 // swallowing — the bug disappears silently
catch (Exception ex) { throw ex; }    // resets the stack trace! use "throw;"
catch (Exception ex) { _logger.LogError(ex, "x"); throw; }   // in every layer → same error logged 5 times
```

⚠️ **Interview trap:** `throw ex;` vs `throw;` — `throw;` **preserves** the original stack trace, `throw ex;` resets it to the current line. Always use `throw;` (or wrap: `throw new OrderException("...", ex);` keeping the inner exception).

### 2. Custom middleware (classic, pre-.NET 8) — compile-verified

```csharp
public class ExceptionHandlingMiddleware(RequestDelegate next, ILogger<ExceptionHandlingMiddleware> logger)
{
    public async Task InvokeAsync(HttpContext context)
    {
        try
        {
            await next(context);
        }
        catch (Exception ex)
        {
            logger.LogError(ex, "Unhandled exception for {Path}", context.Request.Path);
            context.Response.StatusCode = StatusCodes.Status500InternalServerError;
            await context.Response.WriteAsJsonAsync(new ProblemDetails
            {
                Status = 500,
                Title = "Something went wrong"
            }, options: null, contentType: "application/problem+json");
        }
    }
}
// app.UseMiddleware<ExceptionHandlingMiddleware>();   // register FIRST
```

Still very common in existing codebases — you should be able to write it on a whiteboard.

### 3. Exception filter (MVC only) — compile-verified

```csharp
public class NotFoundExceptionFilter : IExceptionFilter
{
    public void OnException(ExceptionContext context)
    {
        if (context.Exception is NotFoundException nf)
        {
            context.Result = new NotFoundObjectResult(new ProblemDetails { Status = 404, Title = "Not found", Detail = nf.Message });
            context.ExceptionHandled = true;
        }
    }
}
// builder.Services.AddControllers(o => o.Filters.Add<NotFoundExceptionFilter>());
```

⚠️ **Interview trap:** Exception filters **don't** catch exceptions thrown in middleware, in Minimal API endpoints, or during model binding/result serialization in all cases. That's why a pipeline-level handler is still needed. (See chapter 8 for the filter pipeline.)

### 4. `UseExceptionHandler` + `IExceptionHandler` (.NET 8+) — the recommended way

The sample app (samples/ShopApi) — `Errors\GlobalExceptionHandler.cs`:

```csharp
// .NET 8+: IExceptionHandler is the recommended central place to turn exceptions into ProblemDetails.
public class GlobalExceptionHandler(ILogger<GlobalExceptionHandler> logger, IProblemDetailsService problemDetails)
    : IExceptionHandler
{
    public async ValueTask<bool> TryHandleAsync(HttpContext context, Exception exception, CancellationToken ct)
    {
        (int status, string title) = exception switch
        {
            ArgumentException => (StatusCodes.Status400BadRequest, "Invalid request"),
            KeyNotFoundException => (StatusCodes.Status404NotFound, "Not found"),
            DbUpdateConcurrencyException => (StatusCodes.Status409Conflict, "The record was changed by someone else"),
            _ => (StatusCodes.Status500InternalServerError, "Something went wrong")
        };

        if (status == StatusCodes.Status500InternalServerError)
            logger.LogError(exception, "Unhandled exception");
        else
            logger.LogWarning("Handled {ExceptionType}: {Message}", exception.GetType().Name, exception.Message);

        context.Response.StatusCode = status;
        return await problemDetails.TryWriteAsync(new ProblemDetailsContext
        {
            HttpContext = context,
            Exception = exception,
            ProblemDetails = new ProblemDetails
            {
                Status = status,
                Title = title,
                // Never leak stack traces to clients; only show the message for 4xx.
                Detail = status < 500 ? exception.Message : null
            }
        });
    }
}
```

Registration and pipeline (from `Program.cs`):

```csharp
builder.Services.AddProblemDetails();
builder.Services.AddExceptionHandler<GlobalExceptionHandler>();
// ...
app.UseExceptionHandler();          // 1st: catches exceptions from everything below
app.UseStatusCodePages();           // empty 4xx/5xx -> ProblemDetails body
```

Line-by-line:

| Piece | Why |
|---|---|
| `exception switch { ... }` | One central **exception → status code map** |
| `LogError` only for 500 | 4xx are client mistakes → `Warning`, don't wake the on-call engineer |
| `Detail = status < 500 ? message : null` | Show helpful text for 4xx, **hide internals** for 500 |
| `IProblemDetailsService.TryWriteAsync` | Writes RFC-compliant JSON with `application/problem+json`, applies `CustomizeProblemDetails` |
| returns `true` | "I handled it" → stop. `false` → next `IExceptionHandler` runs |
| `UseExceptionHandler()` **first** | It can only catch exceptions from middleware registered **after** it |

The sample's integration test proves the mapping works — `ProductService.CreateAsync` throws `ArgumentException("Category 999 does not exist.")` and the client gets 400:

```csharp
[Fact]
public async Task CreateProduct_UnknownCategory_Returns400FromExceptionHandler()
{
    HttpClient client = await ClientAsAsync("user", "user123");

    HttpResponseMessage res = await client.PostAsJsonAsync("/api/products", new CreateProductRequest("Pen", 1m, 5, 999));

    Assert.Equal(HttpStatusCode.BadRequest, res.StatusCode);
    Assert.Contains("Category 999 does not exist", await res.Content.ReadAsStringAsync());
}
```

### Complex example — a chain of handlers (compile-verified)

You can register **several** handlers. They run **in registration order** until one returns `true`.

```csharp
public class ConflictException(string message) : Exception(message);

public class ConflictExceptionHandler(IProblemDetailsService problemDetails) : IExceptionHandler
{
    public async ValueTask<bool> TryHandleAsync(HttpContext context, Exception exception, CancellationToken ct)
    {
        if (exception is not ConflictException) return false;   // not mine -> next handler

        context.Response.StatusCode = StatusCodes.Status409Conflict;
        return await problemDetails.TryWriteAsync(new ProblemDetailsContext
        {
            HttpContext = context,
            Exception = exception,
            ProblemDetails = { Title = "Conflict", Detail = exception.Message, Status = 409 }
        });
    }
}

public class FallbackExceptionHandler(ILogger<FallbackExceptionHandler> logger) : IExceptionHandler
{
    public async ValueTask<bool> TryHandleAsync(HttpContext context, Exception exception, CancellationToken ct)
    {
        logger.LogError(exception, "Unhandled exception");
        context.Response.StatusCode = 500;
        await context.Response.WriteAsJsonAsync(
            new ProblemDetails { Status = 500, Title = "Something went wrong" }, ct);
        return true;
    }
}

builder.Services.AddExceptionHandler<ConflictExceptionHandler>();   // runs 1st
builder.Services.AddExceptionHandler<FallbackExceptionHandler>();   // runs 2nd
```

⚠️ **Interview trap:** `IExceptionHandler` instances are registered as **singletons**. Don't inject a scoped service (like `DbContext`) into the constructor — you'd get a captive dependency (chapter 4). Use `context.RequestServices` if you really need one.

🎯 **Say this in the interview:** "I use try/catch only where I can recover. For everything else there's one global handler: in .NET 8+ that's `UseExceptionHandler` with an `IExceptionHandler` that maps exception types to status codes and writes ProblemDetails. 500s are logged as errors with full details on the server but the client only sees a generic message and a trace id."

---

## 11.3 ProblemDetails (RFC 7807 → RFC 9457)

**Simple words:** A **standard JSON shape for errors**, so every client (React app, mobile, another service) can parse errors the same way. Like a standard **hospital discharge form** — every hospital uses the same fields.

RFC 7807 (2016) was replaced by **RFC 9457** (2023) — same format, small clarifications. Content type: **`application/problem+json`**.

```json
{
  "type": "https://tools.ietf.org/html/rfc9110#section-15.5.5",
  "title": "Not found",
  "status": 404,
  "detail": "Product 12345 does not exist.",
  "instance": "GET /api/products/12345",
  "traceId": "00-4bf92f3577b34da6a3ce929d0e0e4736-00f067aa0ba902b7-01"
}
```

| Field | Meaning |
|---|---|
| `type` | URI identifying the error type (documentation link) |
| `title` | Short, human-readable summary (same for every occurrence of this type) |
| `status` | HTTP status code |
| `detail` | Explanation specific to **this** occurrence |
| `instance` | Which request/resource |
| *extensions* | Anything else: `traceId`, `errors` (validation) |

Validation errors use the subclass **`ValidationProblemDetails`** with an `errors` dictionary — that's what `[ApiController]` returns automatically for invalid models (chapter 6):

```json
{
  "title": "One or more validation errors occurred.",
  "status": 400,
  "errors": { "Name": ["The Name field is required."], "Price": ["The field Price must be between 0.01 and 1000000."] }
}
```

### `AddProblemDetails` and customising it (compile-verified)

`builder.Services.AddProblemDetails()` registers `IProblemDetailsService`, so that `UseExceptionHandler`, `UseStatusCodePages`, and the developer exception page all produce ProblemDetails. Add common fields to **every** error:

```csharp
builder.Services.AddProblemDetails(o => o.CustomizeProblemDetails = ctx =>
{
    ctx.ProblemDetails.Instance = $"{ctx.HttpContext.Request.Method} {ctx.HttpContext.Request.Path}";
    ctx.ProblemDetails.Extensions["traceId"] =
        Activity.Current?.Id ?? ctx.HttpContext.TraceIdentifier;
});
```

Now a user can report "my order failed, traceId 00-4bf9..." and you can find the exact log lines.

Producing ProblemDetails manually:

```csharp
// Controller
return Problem(detail: "Stock too low", statusCode: 409, title: "Conflict");
return ValidationProblem(ModelState);

// Minimal API
return Results.Problem("Stock too low", statusCode: 409);
return TypedResults.ValidationProblem(new Dictionary<string, string[]> { ["Name"] = ["Name is required."] });
```

The sample's integration test checks the content type:

```csharp
[Fact]
public async Task GetProduct_Unknown_Returns404ProblemDetails()
{
    HttpResponseMessage res = await factory.CreateClient().GetAsync("/api/products/12345");

    Assert.Equal(HttpStatusCode.NotFound, res.StatusCode);
    Assert.Equal("application/problem+json", res.Content.Headers.ContentType?.MediaType);
}
```

Why is a plain `NotFound()` (no body) turned into ProblemDetails? In controllers, `[ApiController]` has **client error mapping**: status-code results ≥ 400 like `NotFound()` are converted into ProblemDetails automatically. For everything else (Minimal APIs, middleware, auth 401/403) the safety net is `UseStatusCodePages` — next section.

---

## 11.4 `UseStatusCodePages` and the developer exception page

| Middleware | Handles | Output |
|---|---|---|
| `UseExceptionHandler()` | **Exceptions** | 500 (or mapped code) + ProblemDetails |
| `UseStatusCodePages()` | Responses with an error status code (400–599) **and an empty body** | Adds a ProblemDetails body (when `AddProblemDetails` is registered) |
| `UseDeveloperExceptionPage()` | Exceptions, **Development only** | Full stack trace, headers, query — HTML or JSON |

- Since .NET 6, `WebApplication` **automatically** enables the developer exception page when the environment is `Development`.
- `TypedResults.NotFound()` in a Minimal API, or a bare 401/403 from the auth middleware, produces an error status with **no body** → `UseStatusCodePages` fills it with ProblemDetails. (Controllers with `[ApiController]` already get ProblemDetails from client error mapping — that's what the sample's 404 test sees.)
- Variants: `UseStatusCodePagesWithReExecute("/error/{0}")` (MVC pages), `UseStatusCodePagesWithRedirects(...)`.

### Never leak stack traces

| ❌ Leaky | ✅ Safe |
|---|---|
| `return StatusCode(500, ex.ToString());` | Generic title + traceId; details only in logs |
| `Detail = ex.Message` for 500s (could contain SQL, connection strings, file paths) | `Detail = status < 500 ? ex.Message : null` (the sample) |
| Developer exception page in Production | Only in `Development` |

Why? Stack traces reveal frameworks, versions, file paths, SQL table names — gold for an attacker (OWASP "Security Misconfiguration", chapter 14).

---

## 11.5 Mapping exceptions to status codes

A typical, interview-ready map:

| Exception (built-in or custom) | Status | Meaning |
|---|---|---|
| `ArgumentException`, `ValidationException` | 400 Bad Request | Client sent bad data |
| `UnauthorizedAccessException` | 403 (or 401) | Not allowed |
| `KeyNotFoundException`, `NotFoundException` | 404 Not Found | Resource missing |
| `DbUpdateConcurrencyException`, `ConflictException` | 409 Conflict | Optimistic concurrency / duplicate |
| `OperationCanceledException` (client aborted) | 499 (non-standard) / just log at Debug | Client disconnected |
| `TimeoutException`, `HttpRequestException` from dependency | 503 / 502 / 504 | Downstream problem |
| Anything else | 500 Internal Server Error | Bug |

**Custom domain exceptions** make the map clear:

```csharp
public class NotFoundException(string message) : Exception(message);
public class ConflictException(string message) : Exception(message);

// in a service:
Product product = await db.Products.FindAsync([id], ct)
    ?? throw new NotFoundException($"Product {id} does not exist.");
```

⚠️ **Interview trap:** Using exceptions for **normal control flow** (e.g. "user not found" during login thrown thousands of times per second) is slow (stack trace capture) and hides intent. That's where the **Result pattern** shines.

---

## 11.6 Result pattern vs exceptions

**Simple words:** Instead of *throwing* when something expected goes wrong, the method **returns** an object that says "success with value" or "failure with error". Like a **parcel tracking status**: "Delivered" or "Failed: address not found" — no alarm bells, just a status you must check.

| | Exceptions | Result pattern |
|---|---|---|
| For | **Unexpected** failures (DB down, bugs) | **Expected** business failures (validation, not found, insufficient funds) |
| Visible in signature? | No — caller may forget to catch | **Yes** — `Result<T>` forces the caller to handle it |
| Performance | Expensive when thrown | Cheap |
| Code style | Clean "happy path" | More `if (!result.IsSuccess)` checks |
| Libraries | Built-in | Hand-written, `FluentResults`, `ErrorOr`, `OneOf` |

**Easy-to-complex example (compile-verified + integration-tested):**

```csharp
public enum ErrorType { Validation, NotFound, Conflict }
public record Error(ErrorType Type, string Message);

public record Result<T>
{
    public T? Value { get; }
    public Error? Error { get; }
    public bool IsSuccess => Error is null;

    private Result(T value) => Value = value;
    private Result(Error error) => Error = error;

    public static Result<T> Ok(T value) => new(value);
    public static Result<T> Fail(ErrorType type, string message) => new(new Error(type, message));
}

public class TransferService
{
    private readonly Dictionary<int, decimal> _balances = new() { [1] = 100m, [2] = 0m };

    public Result<TransferReceipt> Transfer(TransferRequest r)
    {
        if (r.Amount <= 0)
            return Result<TransferReceipt>.Fail(ErrorType.Validation, "Amount must be positive.");
        if (!_balances.ContainsKey(r.FromAccount) || !_balances.ContainsKey(r.ToAccount))
            return Result<TransferReceipt>.Fail(ErrorType.NotFound, "Account not found.");
        if (_balances[r.FromAccount] < r.Amount)
            return Result<TransferReceipt>.Fail(ErrorType.Conflict, "Insufficient funds.");

        _balances[r.FromAccount] -= r.Amount;
        _balances[r.ToAccount] += r.Amount;
        return Result<TransferReceipt>.Ok(new TransferReceipt(Guid.NewGuid(), r.Amount));
    }
}

// Map Result -> HTTP in ONE place
public static class ResultExtensions
{
    public static IResult ToHttpResult<T>(this Result<T> result) =>
        result.IsSuccess
            ? Results.Ok(result.Value)
            : result.Error!.Type switch
            {
                ErrorType.Validation => Results.Problem(result.Error.Message, statusCode: 400),
                ErrorType.NotFound => Results.Problem(result.Error.Message, statusCode: 404),
                ErrorType.Conflict => Results.Problem(result.Error.Message, statusCode: 409),
                _ => Results.Problem(statusCode: 500)
            };
}

app.MapPost("/api/transfers", (TransferRequest req, TransferService svc) => svc.Transfer(req).ToHttpResult());
// Transfer of 1000 from an account with 100 → 409 application/problem+json  (verified by a test)
```

🎯 **Say this in the interview:** "Exceptions are for exceptional situations; expected business outcomes like 'not found' or 'insufficient funds' can be modelled with a Result type so the caller must handle them and we avoid the cost of throwing. In practice I combine both: Result for business rules, a global IExceptionHandler as a safety net for everything unexpected."

---

## 11.7 Logging best practices

(Logging basics — `ILogger<T>`, levels, providers, configuration — are in chapter 5. Here: production best practices.)

### Structured logging

**Simple words:** Log **data**, not just text. Message templates keep each value as a separate, searchable field.

```csharp
// ❌ String interpolation — one flat string, can't query by ProductId, allocates even if level disabled
logger.LogInformation($"Created product {product.Id} {product.Name}");

// ✅ Message template — ProductId and ProductName become properties (the sample's ProductService)
logger.LogInformation("Created product {ProductId} {ProductName}", product.Id, product.Name);
```

In Application Insights / Seq / Elasticsearch you can then query `ProductId == 42`.

| Level | Use for | Example |
|---|---|---|
| Trace / Debug | Developer details, off in prod | "Cache key product:1 hit" |
| Information | Business events, normal flow | "Order 77 placed" |
| Warning | Unexpected but handled | "Rate service slow, used fallback"; 4xx from handler |
| Error | Operation failed | Unhandled exception → 500 |
| Critical | App unusable | "Cannot connect to database at startup" |

**Best practices checklist**

| ✅ Do | ❌ Don't |
|---|---|
| Use message templates with PascalCase placeholders | String interpolation in log calls |
| Pass the exception as the **first** argument: `LogError(ex, "...")` | `LogError(ex.Message)` (loses stack trace) |
| Log **once**, at the boundary that handles it | Log-and-rethrow in every layer |
| Include ids (OrderId, UserId, CorrelationId) | Log passwords, tokens, card numbers, personal data |
| Configure levels per category in `appsettings.json` | Leave EF SQL logging at Information in prod |
| Use `[LoggerMessage]` source generator for hot paths (.NET 6+) | Expensive computations inside log arguments |

High-performance logging:

```csharp
public static partial class Log
{
    [LoggerMessage(Level = LogLevel.Information, Message = "Created product {ProductId} {ProductName}")]
    public static partial void ProductCreated(ILogger logger, int productId, string productName);
}
// Log.ProductCreated(logger, product.Id, product.Name);
```

### Correlation id — the sample's inline middleware

**Problem:** 1,000 requests per second write interleaved logs. How do you find all lines for *one* failing request — across 3 microservices?

**Solution:** give each request an id, return it to the caller, and attach it to **every** log line with a **logging scope**.

```csharp
// Inline middleware: correlation id for tracing
app.Use(async (context, next) =>
{
    string id = context.Request.Headers["X-Correlation-Id"].FirstOrDefault() ?? Guid.NewGuid().ToString();
    context.Response.Headers["X-Correlation-Id"] = id;
    using (app.Logger.BeginScope(new Dictionary<string, object> { ["CorrelationId"] = id }))
    {
        await next(context);
    }
});
```

| Line | Why |
|---|---|
| Reuse incoming `X-Correlation-Id` | The caller (front-end or upstream service) may already have one → one id across services |
| Write it to the response header | User/support can report it; tested by `Assert.True(res.Headers.Contains("X-Correlation-Id"))` |
| `BeginScope(dictionary)` | Every log written **inside** `next(context)` (controllers, services, EF) automatically gets `CorrelationId` as a property |
| `using` | Scope ends when the request ends |

To actually *see* scopes in console output: `"Logging": { "Console": { "IncludeScopes": true } }`. Structured sinks (Application Insights, Seq, Serilog) store them as properties.

> Modern note: ASP.NET Core already creates a W3C **trace id** (`Activity.Current.TraceId`) per request and propagates it via the `traceparent` header through `HttpClient`. With OpenTelemetry you get correlation "for free"; a custom `X-Correlation-Id` is still common and easy to explain.

### Serilog (very common in real projects)

```csharp
builder.Host.UseSerilog((ctx, cfg) => cfg
    .ReadFrom.Configuration(ctx.Configuration)
    .Enrich.FromLogContext()
    .WriteTo.Console()
    .WriteTo.Seq("http://localhost:5341"));
app.UseSerilogRequestLogging();   // one compact log line per request
```

*(Serilog snippet not compile-verified — third-party package.)*

---

## 11.8 Observability: OpenTelemetry & Application Insights

**Simple words:** **Observability** = being able to answer "what is my app doing and why is it slow/broken?" from the outside. Three pillars:

| Pillar | What | Example question |
|---|---|---|
| **Logs** | Events with details | "Why did order 77 fail?" |
| **Metrics** | Numbers over time | "Requests/sec, p95 latency, error rate, CPU" |
| **Traces** | The path of one request across services (spans) | "Which of the 6 calls made this request take 3 seconds?" |

Analogy: logs = diary entries, metrics = fitness tracker graphs, traces = GPS route of one trip.

**OpenTelemetry (OTel)** = vendor-neutral standard + SDK for all three. You instrument once and export to any backend (Azure Monitor / Application Insights, Jaeger, Grafana, Datadog). (Compile-verified with OpenTelemetry 1.x packages.)

```csharp
// Packages: OpenTelemetry.Extensions.Hosting, OpenTelemetry.Instrumentation.AspNetCore,
//           OpenTelemetry.Instrumentation.Http, OpenTelemetry.Exporter.OpenTelemetryProtocol
builder.Services.AddOpenTelemetry()
    .ConfigureResource(r => r.AddService("ShopApi"))
    .WithTracing(t => t.AddAspNetCoreInstrumentation().AddHttpClientInstrumentation().AddOtlpExporter())
    .WithMetrics(m => m.AddAspNetCoreInstrumentation().AddOtlpExporter());
```

**Application Insights** = Azure's APM. Modern way: the **Azure Monitor OpenTelemetry Distro** (`builder.Services.AddOpenTelemetry().UseAzureMonitor();` with `Azure.Monitor.OpenTelemetry.AspNetCore`) — it collects requests, dependencies (SQL, HTTP), exceptions, logs and metrics; you query them with **KQL**. Older apps use `AddApplicationInsightsTelemetry()`.

🎯 **Say this in the interview:** "I log with structured templates and a correlation id in a logging scope so I can filter one request's logs. For production observability I'd use OpenTelemetry for traces, metrics and logs, exported to Application Insights, so I can follow a request across services and set alerts on error rate and latency."

---

## 11.9 Health checks

**Simple words:** A URL (e.g. `/health`) that answers **"Are you OK?"** for machines — load balancers, Kubernetes, Azure App Service, uptime monitors. Like a **pilot's pre-flight checklist**: engines OK? fuel OK? → "Ready for take-off".

| Status | HTTP code (default) | Meaning |
|---|---|---|
| `Healthy` | 200 | All good |
| `Degraded` | 200 | Working, but something is slow/partially failing |
| `Unhealthy` | **503** | Don't send traffic |

### Easy — the sample app

```csharp
builder.Services.AddHealthChecks()
    .AddCheck<DatabaseHealthCheck>("database");
// ...
app.MapHealthChecks("/health");

// Custom health check
public class DatabaseHealthCheck(AppDbContext db) : IHealthCheck
{
    public async Task<HealthCheckResult> CheckHealthAsync(HealthCheckContext context, CancellationToken ct = default)
        => await db.Database.CanConnectAsync(ct)
            ? HealthCheckResult.Healthy("Database reachable")
            : HealthCheckResult.Unhealthy("Cannot reach database");
}
```

Tested in the sample:

```csharp
[Fact]
public async Task HealthCheck_ReturnsHealthy()
{
    HttpResponseMessage res = await factory.CreateClient().GetAsync("/health");
    Assert.Equal(HttpStatusCode.OK, res.StatusCode);
    Assert.Equal("Healthy", await res.Content.ReadAsStringAsync());
}
```

Notes: health checks registered with `AddCheck<T>` are created **per check execution** (transient-like), so injecting a scoped `DbContext` works. The built-in alternative is `.AddDbContextCheck<AppDbContext>()` from `Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore`. Community packages (`AspNetCore.HealthChecks.*`) cover SQL Server, Redis, RabbitMQ, URLs, etc.

### Liveness vs readiness

| | **Liveness** | **Readiness** | (Startup) |
|---|---|---|---|
| Question | "Is the process alive / not deadlocked?" | "Can I handle traffic **right now**?" | "Has it finished starting?" |
| Checks | **Nothing external** (or a trivial self check) | Dependencies: DB, cache, queue | Migrations, cache warm-up |
| On failure, Kubernetes... | **Restarts** the container | **Stops sending traffic** (no restart) | Waits |
| Example path | `/health/live` | `/health/ready` | `/health/startup` |

⚠️ **Interview trap:** Putting the **database** check in the **liveness** probe. If the DB goes down, Kubernetes restarts *all* your pods in a loop — restarting doesn't fix the DB and makes recovery worse. Dependencies belong in **readiness**.

### Complex — tags, filtering and a JSON response (compile-verified)

```csharp
builder.Services.AddHealthChecks()
    .AddCheck("self", () => HealthCheckResult.Healthy(), tags: ["live"])
    .AddDbContextCheck<ShopDb>("database", tags: ["ready"])
    .AddCheck<PaymentGatewayHealthCheck>("payments", failureStatus: HealthStatus.Degraded, tags: ["ready"]);

// Liveness: "is the process alive?" -> run NO dependency checks
app.MapHealthChecks("/health/live", new HealthCheckOptions
{
    Predicate = _ => false
});

// Readiness: "can I take traffic?" -> only checks tagged "ready"
app.MapHealthChecks("/health/ready", new HealthCheckOptions
{
    Predicate = check => check.Tags.Contains("ready"),
    ResponseWriter = WriteJsonAsync
});

static Task WriteJsonAsync(HttpContext context, HealthReport report)
{
    context.Response.ContentType = "application/json";
    var body = new
    {
        status = report.Status.ToString(),
        totalMs = report.TotalDuration.TotalMilliseconds,
        checks = report.Entries.Select(e => new
        {
            name = e.Key,
            status = e.Value.Status.ToString(),
            description = e.Value.Description,
            ms = e.Value.Duration.TotalMilliseconds
        })
    };
    return context.Response.WriteAsync(JsonSerializer.Serialize(body));
}
```

A custom check for an external dependency that reports **Degraded** instead of failing hard:

```csharp
public class PaymentGatewayHealthCheck(IHttpClientFactory httpClientFactory) : IHealthCheck
{
    public async Task<HealthCheckResult> CheckHealthAsync(HealthCheckContext context, CancellationToken ct = default)
    {
        try
        {
            HttpClient client = httpClientFactory.CreateClient("payments");
            using HttpResponseMessage res = await client.GetAsync("ping", ct);
            return res.IsSuccessStatusCode
                ? HealthCheckResult.Healthy()
                : HealthCheckResult.Degraded($"Payment gateway returned {(int)res.StatusCode}");
        }
        catch (Exception ex)
        {
            return HealthCheckResult.Unhealthy("Payment gateway unreachable", ex);
        }
    }
}
```

Sample output of `/health/ready`:

```json
{"status":"Degraded","totalMs":182.4,"checks":[
  {"name":"database","status":"Healthy","description":null,"ms":12.1},
  {"name":"payments","status":"Degraded","description":"Payment gateway returned 502","ms":170.2}]}
```

Security tips: don't expose detailed health JSON publicly (it reveals your dependencies) — use `.RequireHost("*:8081")`, `.RequireAuthorization()`, or a separate management port. Keep checks **fast** and use timeouts. How health checks are used during deployment (slots, Kubernetes probes) is in chapter 14.

🎯 **Say this in the interview:** "I expose separate liveness and readiness endpoints. Liveness checks only that the process responds, so the orchestrator restarts it if it's stuck. Readiness checks dependencies like the database via tagged health checks, so the load balancer stops sending traffic without restarting. Custom checks implement IHealthCheck and return Healthy, Degraded or Unhealthy."

---

## Try it yourself

1. In the sample's `GlobalExceptionHandler`, add support for a custom `NotFoundException` and `UnauthorizedAccessException` (→ 403). Where do you add it?
2. Your teammate wrote `catch (Exception ex) { _logger.LogError(ex.Message); throw ex; }`. List the two problems.
3. A controller action returns `NotFound()` with no body, yet the client receives a JSON ProblemDetails body. Which middleware did that?
4. Why should you not include the database check in a Kubernetes liveness probe?
5. Write a health check registration so that `/health/ready` runs only checks tagged `"ready"`.
6. Rewrite `logger.LogInformation($"User {userId} bought {count} items");` correctly and explain why.

<details><summary>Answer</summary>

1. Add arms to the `switch` **before** the `_` default arm (order matters — the first match wins):
   ```csharp
   NotFoundException => (StatusCodes.Status404NotFound, "Not found"),
   UnauthorizedAccessException => (StatusCodes.Status403Forbidden, "Forbidden"),
   ```
2. (a) `LogError(ex.Message)` loses the exception object/stack trace — use `LogError(ex, "message {Id}", id)`. (b) `throw ex;` resets the stack trace — use `throw;`. Also, logging and rethrowing usually duplicates logs; let the global handler log it once.
3. In a controller decorated with `[ApiController]`, it's the built-in **client error mapping** (status-code results ≥ 400 become ProblemDetails). Outside controllers (Minimal APIs, middleware, 401/403 from auth), it's `UseStatusCodePages()` combined with `AddProblemDetails()`, which writes a ProblemDetails body for error responses with empty bodies.
4. Liveness failure makes the orchestrator **restart** the container. A DB outage isn't fixed by restarting the API; all pods would restart in a loop. Put dependencies in **readiness** so traffic is just withheld.
5. ```csharp
   builder.Services.AddHealthChecks().AddCheck<DatabaseHealthCheck>("database", tags: ["ready"]);
   app.MapHealthChecks("/health/ready", new HealthCheckOptions { Predicate = c => c.Tags.Contains("ready") });
   ```
6. `logger.LogInformation("User {UserId} bought {ItemCount} items", userId, count);` — message templates keep `UserId`/`ItemCount` as structured, queryable properties, and avoid formatting the string when the level is disabled.
</details>

---

## Quick revision

- try/catch only where you can **recover**; everything else → **one global handler**.
- .NET 8+: `AddExceptionHandler<T>()` + `app.UseExceptionHandler()` (**first** in the pipeline); handlers run in order, return `true` = handled.
- Exception filters are **MVC-only**; middleware/minimal API exceptions need the pipeline handler.
- `throw;` keeps the stack trace, `throw ex;` destroys it.
- **ProblemDetails** (RFC 7807 → **RFC 9457**): `type, title, status, detail, instance` + extensions; content type `application/problem+json`.
- `[ApiController]` client error mapping + `AddProblemDetails()` + `UseStatusCodePages()` → even empty 404/401/403 responses get a ProblemDetails body.
- **Never leak stack traces**: generic message + traceId for 500s; developer exception page only in Development.
- Exceptions for the unexpected, **Result pattern** for expected business failures.
- **Structured logging** with templates (`{ProductId}`), not string interpolation; log exceptions as first arg; never log secrets.
- **Correlation id** via middleware + `BeginScope` → every log line of a request is linked.
- Observability = logs + metrics + traces → **OpenTelemetry** → Application Insights.
- **Liveness** = process alive (restart on fail); **readiness** = dependencies OK (stop traffic on fail). Use **tags** + `Predicate` to split them.

---

# 12. Performance: Async, Caching, Rate Limiting, HttpClient & Background Work

> **Why interviewers ask this**
> "The API is slow under load — what do you do?" is one of the most common practical questions. Interviewers check whether you understand **why** async matters (thread pool), **when** and **how** to cache (and invalidate!), how to protect the API (**rate limiting**), the famous **HttpClient socket-exhaustion** trap, and how to run **background work** safely with scoped services.

---

## 12.1 Async all the way & the thread pool

**Simple words:** ASP.NET Core serves requests using a limited pool of worker threads. Analogy: a **restaurant with 10 waiters**.
- **Synchronous** waiter: takes your order, walks to the kitchen, and **stands there** for 15 minutes waiting for the food. With 10 such waiters, the 11th guest waits at the door — even though the waiters are doing nothing.
- **Asynchronous** waiter: gives the order to the kitchen, **serves other tables**, and comes back when the bell rings.

`await` on I/O (database, HTTP, file) = "ring the bell when ready" → the thread goes back to the pool to serve other requests.

```csharp
// ❌ Blocks a thread for the whole DB round trip
public IActionResult Get(int id) => Ok(_db.Products.Find(id));

// ✅ Thread is free while SQL Server works
public async Task<IActionResult> Get(int id, CancellationToken ct) =>
    Ok(await _db.Products.FindAsync([id], ct));
```

| Fact | Explanation |
|---|---|
| Async ≠ faster for one request | It improves **throughput / scalability**, not single-request latency |
| Async helps only for **I/O-bound** work | DB, HTTP, files, queues. CPU-bound work still uses a thread |
| **Thread pool starvation** | All threads blocked → new requests queue → latency explodes, even with low CPU |
| `Task.Run` in ASP.NET Core | Doesn't help I/O; it just moves the work to *another* pool thread. Use only to offload CPU work in rare cases |

### "Async all the way" — avoid sync-over-async

```csharp
var product = _service.GetAsync(id).Result;          // ❌ blocks a thread, can deadlock in old frameworks
_service.SaveAsync(order).Wait();                    // ❌ same
var x = _service.GetAsync(id).GetAwaiter().GetResult(); // ❌ still blocking (nicer exception only)

var product = await _service.GetAsync(id, ct);       // ✅
```

Other async rules:

| Rule | Why |
|---|---|
| Pass `CancellationToken` everywhere (the sample does in every method) | If the client disconnects, `HttpContext.RequestAborted` cancels the DB query — no wasted work |
| Avoid `async void` (except event handlers) | Exceptions crash the process; can't be awaited |
| Run independent I/O in parallel: `await Task.WhenAll(t1, t2)` | Two 200 ms calls → 200 ms, not 400 ms |
| **Not** in parallel on the same `DbContext` | `DbContext` is not thread-safe |
| `ValueTask` for hot paths that usually complete synchronously | Fewer allocations (e.g. cache hits) |
| No `ConfigureAwait(false)` needed in ASP.NET Core app code | There's no `SynchronizationContext`; still used in **libraries** |

```csharp
// Complex: parallel independent calls with cancellation
Task<ProductDto?> productTask = _products.GetAsync(id, ct);
Task<ExchangeRate?> rateTask = _rates.GetRateAsync("USD", "EUR", ct);
await Task.WhenAll(productTask, rateTask);
decimal priceInEur = productTask.Result!.Price * rateTask.Result!.Rate;   // .Result is safe here: already completed
```

⚠️ **Interview trap:** "Async makes my code run on multiple threads / faster." — No. Async is about **not blocking** threads while waiting for I/O. The benefit is handling **more concurrent requests** with the same threads.

🎯 **Say this in the interview:** "In ASP.NET Core every request borrows a thread-pool thread. If I block it on I/O with `.Result` or `.Wait()`, under load the pool starves and latency explodes. So I use async all the way down to EF Core and HttpClient, pass CancellationTokens, and never mix sync-over-async."

---

## 12.2 Caching — the big picture

**Simple words:** Keep a copy of expensive data close by so you don't fetch it again. Analogy: you keep **a bottle of water on your desk** (memory cache) instead of walking to the kitchen (database) every time — but you must refill it when it's stale.

| Type | Where data lives | Shared across servers? | Survives restart? | Speed | API |
|---|---|---|---|---|---|
| **In-memory** | Web server RAM | ❌ No | ❌ | Fastest (no serialization) | `IMemoryCache` |
| **Distributed** | Redis / SQL Server / NCache | ✅ Yes | ✅ (Redis persistence) | Network hop + serialization | `IDistributedCache` |
| **Hybrid** (.NET 9+) | L1 memory + L2 distributed | ✅ | ✅ | Best of both | `HybridCache` |
| **Output cache** (.NET 7+) | Server, whole HTTP response | Per server (or Redis store .NET 8+) | — | Skips the endpoint entirely | `AddOutputCache` / `.CacheOutput()` |
| **Response cache** | Client/proxy via HTTP headers | — | — | Client doesn't even call you | `[ResponseCache]` |

### Cache-aside pattern (the most common)

```text
GET product 1
   │
   ├─► look in cache ── HIT ──► return
   │
   └── MISS ─► read DB ─► put in cache (with expiry) ─► return

UPDATE product 1 ─► write DB ─► REMOVE from cache (invalidate)
```

The application code "sits aside" the cache and manages it itself. (Alternatives: read-through / write-through where the cache library talks to the DB.)

---

## 12.3 `IMemoryCache`

**Easy example** — the sample registers it with `builder.Services.AddMemoryCache();`

```csharp
if (!_cache.TryGetValue("categories", out List<CategoryDto>? categories))
{
    categories = await _db.Categories.Select(c => new CategoryDto(c.Id, c.Name, c.Products.Count)).ToListAsync(ct);
    _cache.Set("categories", categories, TimeSpan.FromMinutes(5));
}
return categories;
```

**Complex example — cache-aside with `GetOrCreateAsync`, both expirations, size and invalidation (compile-verified + unit-tested):**

```csharp
public class CachedProductService(IMemoryCache cache, IProductRepository repo)
{
    private static string Key(int id) => $"product:{id}";

    public async Task<ProductDto?> GetAsync(int id, CancellationToken ct)
    {
        return await cache.GetOrCreateAsync(Key(id), async entry =>
        {
            entry.AbsoluteExpirationRelativeToNow = TimeSpan.FromMinutes(10); // hard limit
            entry.SlidingExpiration = TimeSpan.FromMinutes(2);               // idle limit
            entry.Size = 1;                                                  // needed if SizeLimit is set
            return await repo.GetByIdAsync(id, ct);
        });
    }

    public async Task UpdatePriceAsync(int id, decimal price, CancellationToken ct)
    {
        await repo.UpdatePriceAsync(id, price, ct);
        cache.Remove(Key(id));   // invalidate on write
    }
}

// Program.cs — limit memory usage
builder.Services.AddMemoryCache(o => o.SizeLimit = 10_000);   // units are YOUR choice (here: 1 per entry)
```

### Absolute vs sliding expiration

| | Absolute | Sliding |
|---|---|---|
| Rule | Expires at a **fixed** time after creation | Expires if **not accessed** for X time |
| Analogy | Milk carton "best before 25 Nov" | Hotel room: housekeeping removes your things if you don't come back for 2 days |
| Risk | — | A hot item that's read every minute **never** expires → stale forever |
| Best practice | Always set one | Combine sliding **with** absolute (as above) |

`SizeLimit`: once set, **every** entry **must** specify `Size`, otherwise `Set` throws `InvalidOperationException`. When the limit is reached, new entries are not added and compaction removes low-priority/expired ones (`CacheItemPriority`).

⚠️ **Interview trap:** `IMemoryCache` is **per server**. With 3 instances behind a load balancer, invalidating on server A leaves stale copies on B and C. Use a distributed cache / HybridCache, or keep TTLs short.

⚠️ **Interview trap:** The cache stores **object references**. If a caller mutates the cached object, everyone sees the change. Cache immutable DTOs/records.

---

## 12.4 `IDistributedCache` + Redis

**Simple words:** A cache **shared by all servers**, usually Redis (an in-memory key-value store). Analogy: the office **shared fridge** instead of each person's desk bottle.

```csharp
// Package: Microsoft.Extensions.Caching.StackExchangeRedis
builder.Services.AddStackExchangeRedisCache(o =>
{
    o.Configuration = builder.Configuration.GetConnectionString("Redis") ?? "localhost:6379";
    o.InstanceName = "shop:";          // key prefix
});
```

`IDistributedCache` stores **bytes** (or strings), so you serialize yourself (compile-verified):

```csharp
public class DistributedProductCache(IDistributedCache cache, IProductRepository repo)
{
    public async Task<ProductDto?> GetAsync(int id, CancellationToken ct)
    {
        string key = $"product:{id}";
        string? json = await cache.GetStringAsync(key, ct);
        if (json is not null)
            return JsonSerializer.Deserialize<ProductDto>(json);          // HIT

        ProductDto? product = await repo.GetByIdAsync(id, ct);            // MISS -> DB
        if (product is not null)
        {
            await cache.SetStringAsync(key, JsonSerializer.Serialize(product),
                new DistributedCacheEntryOptions { AbsoluteExpirationRelativeToNow = TimeSpan.FromMinutes(10) }, ct);
        }
        return product;
    }
}
```

| | `IMemoryCache` | `IDistributedCache` |
|---|---|---|
| Stores | Any object (reference) | `byte[]` / string |
| Serialization | None | You do it |
| Multi-server consistency | ❌ | ✅ |
| Latency | ns | ~0.5–2 ms (network) |
| Stampede protection | ❌ | ❌ |
| Implementations | Built-in | Redis, SQL Server, in-memory (`AddDistributedMemoryCache` for dev/tests) |

---

## 12.5 `HybridCache` (.NET 9+)

**Simple words:** The new recommended cache API that **combines** memory (L1) + distributed (L2), handles **serialization** for you, and has built-in **stampede protection** and **tag-based invalidation**. (Package `Microsoft.Extensions.Caching.Hybrid`, GA since .NET 9.)

```csharp
builder.Services.AddHybridCache(o =>
{
    o.DefaultEntryOptions = new HybridCacheEntryOptions
    {
        Expiration = TimeSpan.FromMinutes(5),
        LocalCacheExpiration = TimeSpan.FromMinutes(1)
    };
});
// If an IDistributedCache (e.g. Redis) is registered, HybridCache automatically uses it as L2.
```

Usage (compile-verified):

```csharp
public class HybridProductCache(HybridCache cache, IProductRepository repo)
{
    public async Task<ProductDto?> GetAsync(int id, CancellationToken ct) =>
        await cache.GetOrCreateAsync(
            $"product:{id}",
            async token => await repo.GetByIdAsync(id, token),
            new HybridCacheEntryOptions
            {
                Expiration = TimeSpan.FromMinutes(10),        // L2 (distributed) lifetime
                LocalCacheExpiration = TimeSpan.FromMinutes(1) // L1 (in-memory) lifetime
            },
            tags: ["products"],
            cancellationToken: ct);

    public async Task InvalidateAsync(int id, CancellationToken ct)
    {
        await cache.RemoveAsync($"product:{id}", ct);
    }

    public ValueTask InvalidateAllProductsAsync(CancellationToken ct) => cache.RemoveByTagAsync("products", ct);
}
```

| Feature | IMemoryCache | IDistributedCache | **HybridCache** |
|---|---|---|---|
| L1 memory | ✅ | ❌ | ✅ |
| L2 shared | ❌ | ✅ | ✅ (optional) |
| Auto serialization | n/a | ❌ | ✅ |
| Stampede protection | ❌ | ❌ | ✅ (one factory call per key) |
| Tags | ❌ | ❌ | ✅ |

🎯 **Say this in the interview:** "For a single server, IMemoryCache is enough. With multiple instances I use Redis via IDistributedCache. On .NET 9+ I'd pick HybridCache: it layers memory over Redis, serializes for me, prevents cache stampedes and supports tag-based invalidation."

---

## 12.6 Cache invalidation & cache stampede

> "There are only two hard things in Computer Science: cache invalidation and naming things." — Phil Karlton

### Invalidation strategies

| Strategy | How | Pros / cons |
|---|---|---|
| **TTL (expiry)** | Absolute/sliding expiry | Simple; data can be stale up to TTL |
| **Explicit removal on write** | `cache.Remove(key)` after update (sample above) | Fresh; must remember every write path; per-server for memory cache |
| **Tag-based** | `RemoveByTagAsync("products")`, `EvictByTagAsync` (output cache) | Invalidate groups |
| **Versioned keys** | `products:v42:page1` → bump version on change | No delete needed; old keys just expire |
| **Pub/sub / events** | Redis pub/sub or message bus tells all servers to evict | Multi-server memory caches |

**Rule of thumb:** cache data that is **read often, changes rarely, and can tolerate slight staleness** (categories, product catalog, config, exchange rates). Don't cache per-user sensitive data under shared keys!

### Cache stampede (thundering herd)

**Simple words:** A popular key expires → 1,000 concurrent requests all miss at the same moment → 1,000 identical DB queries → DB falls over. Analogy: a shop's single door opens at sale time and everyone rushes in.

Fixes:

| Fix | How |
|---|---|
| **Locking / single-flight** | Only one request rebuilds the value; others wait (manual `SemaphoreSlim`, or `HybridCache` built-in) |
| **Jittered TTLs** | Add random seconds to expiry so keys don't expire together |
| **Early / background refresh** | Refresh before expiry |
| Serve stale while refreshing | Return old value, refresh async |

Manual single-flight with double-check locking (compile-verified):

```csharp
public class StampedeSafeCache(IMemoryCache cache, IProductRepository repo)
{
    private static readonly SemaphoreSlim Lock = new(1, 1);

    public async Task<ProductDto?> GetAsync(int id, CancellationToken ct)
    {
        string key = $"product:{id}";
        if (cache.TryGetValue(key, out ProductDto? cached)) return cached;

        await Lock.WaitAsync(ct);
        try
        {
            if (cache.TryGetValue(key, out cached)) return cached;   // double-check
            ProductDto? fresh = await repo.GetByIdAsync(id, ct);
            cache.Set(key, fresh, new MemoryCacheEntryOptions { AbsoluteExpirationRelativeToNow = TimeSpan.FromMinutes(5), Size = 1 });
            return fresh;
        }
        finally
        {
            Lock.Release();
        }
    }
}
```

(One global lock is simple but serializes *all* keys; production code uses per-key locks — or just HybridCache.)

⚠️ **Interview trap:** `IMemoryCache.GetOrCreateAsync` is **not** stampede-safe — concurrent callers may all run the factory. Use `lock` (not allowed around `await`!) → `SemaphoreSlim`, or `HybridCache`.

---

## 12.7 Output caching vs response caching

| | **Response caching** (`[ResponseCache]` + `UseResponseCaching`) | **Output caching** (.NET 7+, `AddOutputCache` + `UseOutputCache`) |
|---|---|---|
| Where cached | Client / CDN / proxy (and optionally server) using **HTTP headers** | **Server** memory (or Redis store .NET 8+) |
| Controlled by | `Cache-Control` headers; **client can bypass** with `no-cache` | Server policy; client headers **don't** bypass by default |
| Invalidation | ❌ Impossible (it's in the browser) | ✅ `IOutputCacheStore.EvictByTagAsync` |
| Authenticated requests | Not cached | Not cached by default |
| Stampede protection | ❌ | ✅ (resource locking) |
| Best for | Public static-ish content, CDNs | APIs and pages you want to cache on the server |

**Easy — the sample app** (`Endpoints\CategoryEndpoints.cs`):

```csharp
builder.Services.AddOutputCache();
// ...
app.UseOutputCache();

group.MapGet("/", async (AppDbContext db, CancellationToken ct) =>
    await db.Categories
        .Select(c => new CategoryDto(c.Id, c.Name, c.Products.Count))
        .ToListAsync(ct))
    .CacheOutput(p => p.Expire(TimeSpan.FromSeconds(30)));
```

**Complex — named policies, vary-by-query, tags and eviction (compile-verified):**

```csharp
builder.Services.AddOutputCache(o =>
{
    o.AddBasePolicy(b => b.Expire(TimeSpan.FromSeconds(10)));
    o.AddPolicy("Products", b => b.Expire(TimeSpan.FromMinutes(1)).SetVaryByQuery("page", "pageSize").Tag("products"));
});

app.MapGet("/api/products", (int page, int pageSize) => Results.Ok(new { page, pageSize }))
   .CacheOutput("Products");

app.MapPost("/api/products/{id:int}/price", async (int id, decimal price, IOutputCacheStore store, CancellationToken ct) =>
{
    // ... update DB ...
    await store.EvictByTagAsync("products", ct);   // all cached product pages are now invalid
    return Results.NoContent();
}).RequireAuthorization("AdminOnly");

// Controllers: [OutputCache(PolicyName = "Products")]
```

Response caching example:

```csharp
[HttpGet("countries")]
[ResponseCache(Duration = 3600, Location = ResponseCacheLocation.Any)]   // Cache-Control: public,max-age=3600
public IActionResult Countries() => Ok(_countries);
```

---

## 12.8 Rate limiting (.NET 7+)

**Simple words:** Limit how many requests a client can make in a period, to protect the API from abuse, bugs (infinite retry loops), and cost spikes. Analogy: a **nightclub bouncer** letting in only 100 people per hour. Too many → **`429 Too Many Requests`**.

### The four algorithms

| Algorithm | How it works | Analogy | Good for |
|---|---|---|---|
| **Fixed window** | N requests per window (e.g. 100/min); counter resets at window boundary | Gym: 100 entries per clock hour | Simple quotas (sample app). Weakness: burst of 200 around the boundary |
| **Sliding window** | Window split into segments; old segments slide out | Same, but counting "last 60 min" continuously | Smoother than fixed |
| **Token bucket** | Bucket holds up to N tokens; refilled at a rate; each request uses 1 | Arcade tokens refilled every second | Allows short **bursts** but limits average rate |
| **Concurrency** | Max N requests **in progress at the same time** | Only 2 people in the changing room | Expensive endpoints (reports, exports) |

**Easy — the sample app:**

```csharp
builder.Services.AddRateLimiter(o =>
{
    o.RejectionStatusCode = StatusCodes.Status429TooManyRequests;   // default is 503!
    o.AddFixedWindowLimiter("fixed", l =>
    {
        l.PermitLimit = 100;
        l.Window = TimeSpan.FromMinutes(1);
        l.QueueLimit = 0;
    });
});
// ...
app.UseRateLimiter();
app.MapControllers().RequireRateLimiting("fixed");
```

⚠️ **Interview trap:** The default rejection status code is **503**, not 429. Always set `RejectionStatusCode = 429`.

⚠️ **Interview trap:** A named limiter like `"fixed"` is **one shared counter for all clients** — 100 requests/min for the *whole API*, so one noisy client can block everyone. For per-client limits use a **partitioned** limiter (below).

**Complex — all four + per-user partitioning + Retry-After (compile-verified):**

```csharp
builder.Services.AddRateLimiter(o =>
{
    o.RejectionStatusCode = StatusCodes.Status429TooManyRequests;

    o.AddFixedWindowLimiter("fixed", l =>
    {
        l.PermitLimit = 100;
        l.Window = TimeSpan.FromMinutes(1);
    });

    o.AddSlidingWindowLimiter("sliding", l =>
    {
        l.PermitLimit = 100;
        l.Window = TimeSpan.FromMinutes(1);
        l.SegmentsPerWindow = 6;          // 6 x 10-second segments
    });

    o.AddTokenBucketLimiter("bucket", l =>
    {
        l.TokenLimit = 20;                // burst size
        l.TokensPerPeriod = 5;            // refill 5 tokens...
        l.ReplenishmentPeriod = TimeSpan.FromSeconds(1); // ...every second
        l.QueueLimit = 0;
    });

    o.AddConcurrencyLimiter("reports", l =>
    {
        l.PermitLimit = 2;                // max 2 heavy reports at the same time
        l.QueueLimit = 5;
        l.QueueProcessingOrder = QueueProcessingOrder.OldestFirst;
    });

    // Per-user (or per-IP for anonymous) partitioned limiter
    o.AddPolicy("per-user", httpContext =>
    {
        string key = httpContext.User.FindFirstValue(ClaimTypes.NameIdentifier)
                     ?? httpContext.Connection.RemoteIpAddress?.ToString()
                     ?? "anonymous";
        return RateLimitPartition.GetFixedWindowLimiter(key, _ => new FixedWindowRateLimiterOptions
        {
            PermitLimit = 30,
            Window = TimeSpan.FromMinutes(1)
        });
    });

    o.OnRejected = async (ctx, ct) =>
    {
        if (ctx.Lease.TryGetMetadata(MetadataName.RetryAfter, out TimeSpan retryAfter))
            ctx.HttpContext.Response.Headers.RetryAfter = ((int)retryAfter.TotalSeconds).ToString();
        await ctx.HttpContext.Response.WriteAsync("Too many requests. Try again later.", ct);
    };
});

app.MapGet("/api/products", ...).RequireRateLimiting("per-user");
app.MapGet("/api/reports/sales", ...).RequireRateLimiting("reports");
// Controllers: [EnableRateLimiting("per-user")], [DisableRateLimiting]
```

Notes:
- The built-in limiter is **in-memory per instance**. With 5 servers, a "100/min" limit is effectively 500/min. For global limits use an API gateway (Azure API Management, Nginx, Azure Front Door) or a Redis-based limiter.
- Behind a proxy, `RemoteIpAddress` is the **proxy's** IP unless you configure `ForwardedHeaders` (chapter 14).

---

## 12.9 `IHttpClientFactory` — the HttpClient trap

### The problem, in simple words

`HttpClient` *looks* disposable, so people write:

```csharp
// ❌ BAD: new HttpClient per request
public async Task<string> GetRate()
{
    using var client = new HttpClient();
    return await client.GetStringAsync("https://example.com/api/rates");
}
```

Each `HttpClient` owns a handler with its own **TCP connections**. Disposing closes the socket, but the OS keeps it in **`TIME_WAIT`** for up to ~4 minutes. Under load you create thousands of sockets → **socket exhaustion** → `SocketException: Only one usage of each socket address is normally permitted`.

Analogy: making a **new phone line installed** for every call and leaving each old line in "hang-up cooldown" for 4 minutes. Soon the building runs out of lines.

"Fix" #2 — one static `HttpClient` forever:

```csharp
private static readonly HttpClient Client = new();   // ⚠️ reuses sockets, BUT...
```

…connections stay open forever, so **DNS changes are never seen** (e.g. blue/green deployment, failover moves `api.example.com` to a new IP → you keep calling the old one).

| Approach | Sockets | DNS changes | Verdict |
|---|---|---|---|
| `new HttpClient()` per request | ❌ Exhaustion | ✅ | ❌ |
| static singleton `HttpClient` | ✅ | ❌ Stale | ⚠️ OK only with `SocketsHttpHandler { PooledConnectionLifetime = ... }` |
| **`IHttpClientFactory`** | ✅ Pools handlers | ✅ Rotates handlers (default 2 min) | ✅ **Recommended** |

**`IHttpClientFactory`** pools and reuses `HttpMessageHandler`s, recycles them periodically (DNS refresh), integrates with DI, logging and resilience. The `HttpClient` objects it gives you are cheap and **you may create one per request**.

### Three ways to use it

| Style | How | When |
|---|---|---|
| Basic | `factory.CreateClient()` | Quick one-off |
| **Named** | `AddHttpClient("github", c => ...)` → `factory.CreateClient("github")` | Several configs, no wrapper class |
| **Typed** | `AddHttpClient<WeatherClient>(...)` → inject `WeatherClient` | ✅ Best: strongly typed, encapsulated, testable |

**Typed client — the sample app** (`Clients\ExchangeRateClient.cs`):

```csharp
public record ExchangeRate(string From, string To, decimal Rate);

// Typed HttpClient: IHttpClientFactory creates & pools the handler (no socket exhaustion, DNS refresh).
public class ExchangeRateClient(HttpClient http)
{
    public async Task<ExchangeRate?> GetRateAsync(string from, string to, CancellationToken ct)
    {
        using HttpResponseMessage response = await http.GetAsync($"rates?from={from}&to={to}", ct);
        if (!response.IsSuccessStatusCode) return null;
        return await response.Content.ReadFromJsonAsync<ExchangeRate>(ct);
    }
}

// Program.cs
builder.Services.AddHttpClient<ExchangeRateClient>(c =>
{
    c.BaseAddress = new Uri("https://example.com/api/");
    c.Timeout = TimeSpan.FromSeconds(10);
});
```

**Named client (compile-verified):**

```csharp
builder.Services.AddHttpClient("github", c =>
{
    c.BaseAddress = new Uri("https://api.github.com/");
    c.DefaultRequestHeaders.UserAgent.ParseAdd("ShopApi/1.0");
});

public class GitHubService(IHttpClientFactory factory)
{
    public async Task<string> GetZenAsync(CancellationToken ct)
    {
        HttpClient client = factory.CreateClient("github");
        return await client.GetStringAsync("zen", ct);
    }
}
```

⚠️ **Interview trap:** Typed clients are registered as **transient**. Injecting a typed client into a **singleton** (e.g. a `BackgroundService`) captures one `HttpClient` forever → its handler is never rotated → DNS problem returns. In singletons inject `IHttpClientFactory` and create clients when needed.

---

## 12.10 Resilience: retries, timeouts, circuit breakers

**Simple words:** Networks fail. A good client **retries** transient failures, gives up after a **timeout**, and stops hammering a dead service (**circuit breaker**).

| Strategy | What | Analogy |
|---|---|---|
| **Retry** (with exponential backoff + jitter) | Try again after 1s, 2s, 4s (+random) | Redialing a busy number, waiting a bit longer each time |
| **Timeout** | Cancel slow calls | Hanging up after 30 s of ringing |
| **Circuit breaker** | After many failures, **open** the circuit: fail fast for a while, then **half-open** to test | Electrical fuse trips to protect the house |
| **Fallback** | Return default/cached value | Use yesterday's exchange rate |
| **Bulkhead / rate limiter** | Limit concurrent calls | Ship compartments — one leak doesn't sink the ship |
| **Hedging** | Send a second request if the first is slow | Calling two taxi companies |

```text
Circuit breaker states:
 CLOSED ──(failure ratio > 50%)──► OPEN ──(break duration passes)──► HALF-OPEN
   ▲                                  (calls fail immediately)          │
   └──────────── trial call succeeds ◄──────────────────────────────────┘
                 trial call fails ──► back to OPEN
```

**Polly** is the classic .NET resilience library. Since .NET 8, **`Microsoft.Extensions.Http.Resilience`** (built on Polly v8) adds a one-liner **standard pipeline**: rate limiter → total timeout → retry → circuit breaker → attempt timeout.

```csharp
// Package: Microsoft.Extensions.Http.Resilience  (compile-verified)
builder.Services.AddHttpClient<WeatherClient>(c =>
{
    c.BaseAddress = new Uri("https://weather.example.com/");
})
.AddStandardResilienceHandler(o =>
{
    o.Retry.MaxRetryAttempts = 3;
    o.Retry.UseJitter = true;
    o.AttemptTimeout.Timeout = TimeSpan.FromSeconds(5);
    o.TotalRequestTimeout.Timeout = TimeSpan.FromSeconds(20);
    o.CircuitBreaker.FailureRatio = 0.5;
    o.CircuitBreaker.SamplingDuration = TimeSpan.FromSeconds(30);
});
```

The defaults (without the lambda) are already sensible: 3 retries with exponential backoff on 5xx/408/429/network errors, 10 s attempt timeout, 30 s total timeout.

⚠️ **Interview trap:** Retrying **non-idempotent** operations (e.g. `POST /payments`) can charge a customer twice. Retry only safe/idempotent calls, or use **idempotency keys**. (The standard handler retries all methods by default; you can disable retries for unsafe methods with `o.Retry.DisableForUnsafeHttpMethods()`.)

🎯 **Say this in the interview:** "I never `new` HttpClient per request — that causes socket exhaustion — and a static one ignores DNS changes. I use IHttpClientFactory with typed clients, and add Microsoft.Extensions.Http.Resilience's standard handler for retries with jitter, timeouts and a circuit breaker, careful to only retry idempotent calls."

---

## 12.11 Background work: `IHostedService` vs `BackgroundService`

**Simple words:** Code that runs **inside your web app but outside any HTTP request** — cleanup jobs, queue consumers, cache warm-up. Analogy: the **night-shift cleaner** in an office — works while no customers are around.

| | `IHostedService` | `BackgroundService` |
|---|---|---|
| What | Interface: `StartAsync` + `StopAsync` | Abstract base class implementing `IHostedService` |
| You write | Both methods, manage your own loop/task | Just `ExecuteAsync(CancellationToken)` |
| Use for | Short startup/shutdown tasks | **Long-running loops** (most cases) |
| Gotcha | `StartAsync` **blocks app startup** until it returns | `ExecuteAsync` must `await` early, otherwise startup blocks too |

**Easy — `IHostedService` (compile-verified):**

```csharp
public class StartupWarmupService(ILogger<StartupWarmupService> logger) : IHostedService
{
    public Task StartAsync(CancellationToken cancellationToken)
    {
        logger.LogInformation("Warming up caches...");
        return Task.CompletedTask;    // keep StartAsync short: it blocks app startup
    }

    public Task StopAsync(CancellationToken cancellationToken)
    {
        logger.LogInformation("Stopping...");
        return Task.CompletedTask;
    }
}
builder.Services.AddHostedService<StartupWarmupService>();
```

**Complex — the sample's `PendingOrderCleanupService`:**

```csharp
// BackgroundService is a SINGLETON. DbContext is SCOPED.
// So we create a scope per run with IServiceScopeFactory (avoid the captive-dependency bug).
public class PendingOrderCleanupService(IServiceScopeFactory scopeFactory, ILogger<PendingOrderCleanupService> logger)
    : BackgroundService
{
    protected override async Task ExecuteAsync(CancellationToken stoppingToken)
    {
        using var timer = new PeriodicTimer(TimeSpan.FromMinutes(5));
        do
        {
            try
            {
                await using AsyncServiceScope scope = scopeFactory.CreateAsyncScope();
                AppDbContext db = scope.ServiceProvider.GetRequiredService<AppDbContext>();

                DateTime cutoff = DateTime.UtcNow.AddDays(-1);
                int cancelled = await db.Orders
                    .Where(o => o.Status == "Pending" && o.CreatedUtc < cutoff)
                    .ExecuteUpdateAsync(s => s.SetProperty(o => o.Status, "Cancelled"), stoppingToken);

                if (cancelled > 0)
                    logger.LogInformation("Cancelled {Count} stale orders", cancelled);
            }
            catch (Exception ex) when (ex is not OperationCanceledException)
            {
                logger.LogError(ex, "Cleanup failed");   // never let the loop die
            }
        }
        while (await timer.WaitForNextTickAsync(stoppingToken));
    }
}
// builder.Services.AddHostedService<PendingOrderCleanupService>();
```

What makes it production-quality:

| Detail | Why |
|---|---|
| `IServiceScopeFactory` + `CreateAsyncScope()` per run | Hosted services are **singletons**; `DbContext` is **scoped**. Injecting `AppDbContext` directly = captive dependency (one context forever, not thread-safe, memory grows). |
| `PeriodicTimer` (.NET 6+) | Async-friendly timer, no overlapping runs, honours cancellation. Better than `Task.Delay` loops or `System.Threading.Timer` |
| `do { } while` | Runs once immediately at startup, then every 5 minutes |
| `try/catch` inside the loop | Since .NET 6, an unhandled exception in `ExecuteAsync` **stops the host** by default (`BackgroundServiceExceptionBehavior.StopHost`) |
| `when (ex is not OperationCanceledException)` | Shutdown cancellation should end the loop, not be logged as an error |
| `ExecuteUpdateAsync` (EF Core 7+) | One SQL `UPDATE` — no loading entities |
| `stoppingToken` passed everywhere | Graceful shutdown |

⚠️ **Interview trap:** In a web farm with 3 instances, this job runs **3 times** concurrently. For "run exactly once" jobs use a distributed lock, a single worker instance, or a scheduler like **Hangfire / Quartz.NET / Azure Functions timer trigger**. Also, in-process jobs die with the app (IIS app-pool recycle, scale-in) — for critical work use a durable queue.

---

## 12.12 Queues with `Channel<T>` — fire-and-forget done right

**Problem:** "After placing an order, send a confirmation email" — you don't want the user to wait 2 s for SMTP. `_ = Task.Run(() => SendEmail())` is dangerous: no scope, exceptions lost, work lost on shutdown, `HttpContext`/`DbContext` already disposed.

**Solution:** an **in-memory producer/consumer queue**. The endpoint writes a message to a `Channel<T>` and returns `202 Accepted`; a `BackgroundService` reads and processes it. Analogy: a restaurant **order ticket rail** — waiters clip tickets, the cook takes them one by one.

Compile-verified + unit-tested:

```csharp
public record EmailMessage(string To, string Subject, string Body);

public interface IEmailQueue
{
    ValueTask EnqueueAsync(EmailMessage message, CancellationToken ct = default);
    IAsyncEnumerable<EmailMessage> ReadAllAsync(CancellationToken ct);
}

public class EmailQueue : IEmailQueue
{
    private readonly Channel<EmailMessage> _channel = Channel.CreateBounded<EmailMessage>(
        new BoundedChannelOptions(capacity: 1000)
        {
            FullMode = BoundedChannelFullMode.Wait,   // back-pressure: producers wait when full
            SingleReader = true
        });

    public ValueTask EnqueueAsync(EmailMessage message, CancellationToken ct = default)
        => _channel.Writer.WriteAsync(message, ct);

    public IAsyncEnumerable<EmailMessage> ReadAllAsync(CancellationToken ct)
        => _channel.Reader.ReadAllAsync(ct);
}

public class EmailSenderWorker(IEmailQueue queue, IServiceScopeFactory scopeFactory, ILogger<EmailSenderWorker> logger)
    : BackgroundService
{
    protected override async Task ExecuteAsync(CancellationToken stoppingToken)
    {
        await foreach (EmailMessage message in queue.ReadAllAsync(stoppingToken))
        {
            try
            {
                await using AsyncServiceScope scope = scopeFactory.CreateAsyncScope();
                IEmailSender sender = scope.ServiceProvider.GetRequiredService<IEmailSender>();
                await sender.SendAsync(message, stoppingToken);
            }
            catch (Exception ex) when (ex is not OperationCanceledException)
            {
                logger.LogError(ex, "Failed to send email to {To}", message.To);
            }
        }
    }
}

// Program.cs
builder.Services.AddSingleton<IEmailQueue, EmailQueue>();        // ONE queue shared by producers & consumer
builder.Services.AddScoped<IEmailSender, SmtpEmailSender>();
builder.Services.AddHostedService<EmailSenderWorker>();

// Producer
app.MapPost("/api/orders/{id:int}/confirm", async (int id, IEmailQueue queue) =>
{
    await queue.EnqueueAsync(new EmailMessage("customer@example.com", $"Order {id} confirmed", "Thanks!"));
    return Results.Accepted();
});
```

| | Bounded channel | Unbounded channel |
|---|---|---|
| Memory | Capped | Can grow until OOM |
| When full | `Wait` / `DropOldest` / `DropNewest` / `DropWrite` | Never full |
| Recommended | ✅ For production | Only when producers are naturally limited |

⚠️ **Interview trap:** `Channel<T>` is **in-memory** — messages are lost on crash/restart and not shared across servers. For must-not-lose work use **Azure Service Bus / RabbitMQ / Azure Storage Queues** (+ the outbox pattern).

---

## 12.13 Other quick wins

### Response compression

```csharp
builder.Services.AddResponseCompression(o =>
{
    o.EnableForHttps = true;                          // off by default (BREACH/CRIME attack concerns)
    o.Providers.Add<BrotliCompressionProvider>();
    o.Providers.Add<GzipCompressionProvider>();
});
builder.Services.Configure<BrotliCompressionProviderOptions>(o => o.Level = CompressionLevel.Fastest);
app.UseResponseCompression();                         // early in the pipeline
```

Big JSON lists shrink 70–90%. If you're behind IIS/Nginx/a CDN, let **them** compress (more efficient). Don't compress already-compressed files (images, zip).

### Pagination — never return "all rows"

From the sample's `ProductService.GetPageAsync`:

```csharp
page = Math.Max(page, 1);
pageSize = Math.Clamp(pageSize, 1, 100);              // protect the server from pageSize=1000000

IQueryable<Product> query = db.Products.AsNoTracking();
// ...
int total = await query.CountAsync(ct);
List<ProductDto> items = await query
    .OrderBy(p => p.Id)                               // stable order is REQUIRED for paging
    .Skip((page - 1) * pageSize)
    .Take(pageSize)
    .Select(p => new ProductDto(p.Id, p.Name, p.Price, p.Stock, p.Category!.Name))
    .ToListAsync(ct);
```

| Offset paging (`Skip/Take`) | Keyset / cursor paging (`WHERE Id > @lastId`) |
|---|---|
| Simple, supports "jump to page 50" | Fast at any depth, stable when rows are inserted |
| Slow for deep pages (DB still scans skipped rows) | No random page jumps |

### EF Core essentials (details in chapter 9)

| Tip | Why |
|---|---|
| `AsNoTracking()` for read-only queries | No change-tracker overhead (sample does this) |
| **Project** with `Select` into DTOs | Only needed columns; avoids N+1 and over-fetching |
| Avoid N+1 (`Include` or projection) | 1 query instead of 1+N |
| `ExecuteUpdateAsync` / `ExecuteDeleteAsync` | Bulk changes without loading entities |
| Indexes on filter/sort columns | The biggest win is usually in the database |
| `AddDbContextPool` | Reuses DbContext instances under high load |
| Compiled queries / split queries | Hot paths / cartesian explosion |

### Profiling & diagnosing — "the API is slow, what do you do?"

1. **Measure first** — don't guess. Application Insights / OpenTelemetry: which endpoint, p95/p99 latency, dependency durations.
2. Check the **database**: slow queries (EF logging, Query Store, execution plans, missing indexes).
3. Check for **sync-over-async / thread pool starvation**: `dotnet-counters monitor -n ShopApi` (thread pool queue length, GC, exceptions/sec).
4. **Memory / CPU**: `dotnet-trace`, `dotnet-dump`, Visual Studio profiler, PerfView.
5. **Load test**: k6, JMeter, NBomber, Azure Load Testing.
6. **Benchmark** small hot code paths with **BenchmarkDotNet**.
7. Then apply: caching, pagination, projection, async, compression, scaling out.

🎯 **Say this in the interview:** "I measure before optimising: APM traces to find the slow endpoint and dependency, then usually it's the database — I check the SQL, indexes, N+1, tracking. Then caching for read-heavy data, pagination, async everywhere to avoid thread-pool starvation, and IHttpClientFactory with resilience for outbound calls."

---

## Try it yourself

1. A colleague writes `var data = _client.GetDataAsync().Result;` in a controller. It works on their machine. What happens in production under load and how do you fix it?
2. You cache product details in `IMemoryCache` for 1 hour. The app runs on 4 instances. An admin updates a price; some users still see the old price. Why, and give two fixes.
3. Choose a rate-limiting algorithm: (a) allow a mobile app short bursts but ~5 req/s on average, (b) at most 3 PDF exports running at once, (c) 1,000 requests per day per API key.
4. What's wrong with this hosted service?
   ```csharp
   public class ReportWorker(AppDbContext db) : BackgroundService { ... }
   ```
5. Why is `new HttpClient()` inside a `using` block per request a problem, and why isn't a single static `HttpClient` a perfect fix either?
6. You need to send a welcome email after registration without making the user wait. Sketch the design.

<details><summary>Answer</summary>

1. `.Result` blocks a thread-pool thread for the whole call (**sync-over-async**). Under load, the pool starves → requests queue → huge latency/timeouts even with low CPU. Fix: make the action `async Task<IActionResult>` and `await _client.GetDataAsync(ct)` — async all the way.
2. `IMemoryCache` is **per instance**; only the instance that handled the update removed its entry. Fixes: use **Redis / HybridCache** (shared L2, plus tag/key removal), shorter TTL, or broadcast invalidation (Redis pub/sub / message bus).
3. (a) **Token bucket** (burst size + refill rate). (b) **Concurrency limiter** (PermitLimit = 3). (c) **Fixed window** (or sliding) partitioned by API key with `Window = 1 day` — or better, enforce quotas at an API gateway if you have multiple instances.
4. Hosted services are **singletons**; `AppDbContext` is **scoped** → captive dependency (DI validation throws in Development: "Cannot consume scoped service from singleton"). Inject `IServiceScopeFactory` and create a scope per unit of work like `PendingOrderCleanupService`.
5. Per-request clients leave sockets in `TIME_WAIT` → **socket exhaustion**. A static client reuses connections but keeps them open forever → **DNS changes ignored**. Use **IHttpClientFactory** (pooled, rotated handlers) — or a static client with `SocketsHttpHandler.PooledConnectionLifetime`.
6. Endpoint enqueues an `EmailMessage` into a singleton bounded `Channel<T>` and returns quickly; an `EmailSenderWorker : BackgroundService` reads with `ReadAllAsync`, creates a scope per message, sends with retries, logs failures. If emails must never be lost → durable queue (Service Bus) + outbox pattern.
</details>

---

## Quick revision

- Async improves **throughput**, not single-request speed; never `.Result`/`.Wait()`; pass **CancellationToken**.
- Cache = read-often, change-rarely data. **Cache-aside**: check → miss → DB → store with TTL; **remove on write**.
- **Absolute** = fixed lifetime; **sliding** = idle timeout; combine them. `SizeLimit` ⇒ every entry needs `Size`.
- `IMemoryCache` is per server; `IDistributedCache` (Redis) is shared, stores bytes; **HybridCache (.NET 9+)** = L1+L2 + stampede protection + tags.
- **Stampede**: many misses at once → lock/single-flight, jitter, HybridCache.
- **Output cache** = server-side, invalidatable (tags); **response cache** = HTTP headers, client-side, not invalidatable.
- Rate limiting: fixed / sliding / token bucket / concurrency; set **429** (default 503); partition per user/IP.
- `new HttpClient()` per call → **socket exhaustion**; static → **stale DNS**; use **IHttpClientFactory** (named/typed).
- `AddStandardResilienceHandler()` = retry + timeouts + circuit breaker; retry only **idempotent** calls.
- `BackgroundService` is a **singleton** → create scopes with `IServiceScopeFactory`; use `PeriodicTimer`; catch exceptions inside the loop.
- `Channel<T>` = in-memory producer/consumer queue; bounded for back-pressure; not durable.
- Paginate with a stable `OrderBy` + clamp `pageSize`; `AsNoTracking` + projection; **measure before optimising**.

---

# 13. Testing ASP.NET Core Applications

> **Why interviewers ask this**
> "How do you test your API?" is asked in almost every interview. A 2-YOE developer should write **unit tests with mocks**, explain **what not to mock**, and write **integration tests with `WebApplicationFactory`** that hit real endpoints, including authenticated ones. Being able to show real tests from a project you built is a big plus — this chapter uses the sample app's **real, passing tests** (samples/ShopApi.Tests).

---

## 13.1 The test pyramid

**Simple words:** Write **many** small, fast tests and **few** big, slow ones. Analogy: building a **car**.
- **Unit tests** = test each part on a bench (does the spark plug spark?). Fast, cheap, thousands of them.
- **Integration tests** = assemble the engine and run it (do the parts work together?). Slower, hundreds.
- **End-to-end (E2E) tests** = drive the finished car on a real road. Slowest, most fragile, a handful.

```text
              ▲  slower, more expensive, more realistic
             ╱ ╲
            ╱E2E╲          Playwright / Selenium: browser → UI → API → DB
           ╱─────╲
          ╱ Integ- ╲       WebApplicationFactory: HTTP → pipeline → real DB (SQLite/Testcontainers)
         ╱  ration  ╲
        ╱────────────╲
       ╱     Unit     ╲    xUnit + Moq: one class, dependencies mocked, milliseconds
      ╱────────────────╲
              ▼  faster, cheaper, more isolated
```

| | Unit | Integration | E2E |
|---|---|---|---|
| Scope | One class/method | Several components + real infrastructure (pipeline, DI, DB) | Whole system through UI |
| Dependencies | Mocked/faked | Real (or in-memory replacements) | Real |
| Speed | ms | 100 ms – seconds | seconds – minutes |
| Finds | Logic bugs | Wiring bugs: DI, routing, auth, middleware order, serialization, SQL | User-journey bugs |
| Fragility | Low | Medium | High (flaky) |
| Tools | xUnit/NUnit/MSTest + Moq/NSubstitute | `WebApplicationFactory`, SQLite, Testcontainers | Playwright, Selenium |

⚠️ **Interview trap:** "We have 100% unit test coverage so we don't need integration tests." — Unit tests with mocks can all pass while the app is broken (wrong middleware order, missing DI registration, wrong route, `[Authorize]` missing). The sample's 401/403 tests can **only** be written as integration tests.

🎯 **Say this in the interview:** "I follow the test pyramid: lots of fast unit tests for business logic with dependencies mocked, a solid layer of integration tests with WebApplicationFactory that exercise the real pipeline — routing, validation, auth, the database — and a few E2E tests for critical user journeys."

---

## 13.2 xUnit basics

xUnit is the most common test framework for .NET (used by the ASP.NET Core team). NUnit and MSTest are equivalents.

| Concept | xUnit | NUnit | MSTest |
|---|---|---|---|
| Single test | `[Fact]` | `[Test]` | `[TestMethod]` |
| Parameterised test | `[Theory]` + `[InlineData]` | `[TestCase]` | `[DataTestMethod]` + `[DataRow]` |
| Per-test setup | **Constructor** | `[SetUp]` | `[TestInitialize]` |
| Per-test cleanup | **`IDisposable.Dispose`** / `IAsyncLifetime` | `[TearDown]` | `[TestCleanup]` |
| Shared per class | **`IClassFixture<T>`** | `[OneTimeSetUp]` | `[ClassInitialize]` |
| Shared across classes | `ICollectionFixture<T>` | `[SetUpFixture]` | `[AssemblyInitialize]` |

**Key xUnit fact:** a **new instance of the test class is created for every test**. So the constructor runs before *each* test → each test gets fresh state, tests are isolated.

### Arrange – Act – Assert (AAA)

Every test has three parts:
1. **Arrange** — set up objects and inputs.
2. **Act** — call the method under test (usually **one** line).
3. **Assert** — check the outcome.

### Easy example: `[Fact]` and `[Theory]` (compile-verified, all pass)

```csharp
public class PriceCalculator
{
    public decimal ApplyDiscount(decimal price, int percent)
    {
        if (percent is < 0 or > 100) throw new ArgumentOutOfRangeException(nameof(percent));
        return Math.Round(price * (100 - percent) / 100m, 2);
    }
}

public class PriceCalculatorTests
{
    private readonly PriceCalculator _sut = new();   // sut = "system under test"

    [Fact]
    public void ApplyDiscount_TenPercent_ReducesPrice()
    {
        // Arrange
        decimal price = 200m;
        // Act
        decimal result = _sut.ApplyDiscount(price, 10);
        // Assert
        Assert.Equal(180m, result);
    }

    [Theory]
    [InlineData(100, 0, 100)]
    [InlineData(100, 50, 50)]
    [InlineData(100, 100, 0)]
    [InlineData(19.99, 15, 16.99)]
    public void ApplyDiscount_VariousInputs(decimal price, int percent, decimal expected)
        => Assert.Equal(expected, _sut.ApplyDiscount(price, percent));

    [Theory]
    [InlineData(-1)]
    [InlineData(101)]
    public void ApplyDiscount_InvalidPercent_Throws(int percent)
        => Assert.Throws<ArgumentOutOfRangeException>(() => _sut.ApplyDiscount(100m, percent));
}
```

`[Theory]` runs the same test once per `[InlineData]` row → 4 + 2 = 6 test cases. For complex data use `[MemberData(nameof(Cases))]` or `[ClassData(typeof(...))]`.

Common assertions:

| Assertion | Use |
|---|---|
| `Assert.Equal(expected, actual)` | Values (order: **expected first**) |
| `Assert.True / False / Null / NotNull` | Booleans, nulls |
| `Assert.IsType<T>(obj)` | Exact type; returns the cast object |
| `Assert.Single(list) / Empty / Contains` | Collections |
| `Assert.Throws<T>(...)` / `await Assert.ThrowsAsync<T>(...)` | Exceptions |
| `Assert.Same(a, b)` | Same reference |

(Many teams use **FluentAssertions** / **Shouldly** for readable `result.Should().Be(180m)` — mention it, but plain `Assert` is fine.)

---

## 13.3 Mocking with Moq

**Simple words:** A **mock** is a fake object that pretends to be a dependency so you can test a class **in isolation**. Analogy: a **crash-test dummy** — you test the car's airbag without hurting a real person. You decide how the dummy behaves (`Setup`) and later check what happened to it (`Verify`).

| Term | Meaning |
|---|---|
| **Stub** | Fake that returns canned answers ("GetById(1) returns Laptop") |
| **Mock** | Fake you also **verify** interactions on ("Save was called once") |
| **Fake** | Lightweight working implementation (in-memory repository) |
| **Spy** | Records calls on a real object |

Moq cheat sheet:

| Moq | Meaning |
|---|---|
| `var m = new Mock<IProductService>();` | Create a mock of an interface (or virtual members of a class) |
| `m.Setup(s => s.GetByIdAsync(1, It.IsAny<CancellationToken>())).ReturnsAsync(dto)` | When called with these args, return this (async) |
| `.Returns(value)` | Sync return |
| `.ThrowsAsync(new TimeoutException())` | Simulate failure |
| `m.SetupSequence(...).ReturnsAsync(a).ReturnsAsync(b)` | Different result on each call |
| `It.IsAny<T>()`, `It.Is<int>(x => x > 0)` | Argument matchers |
| `m.Object` | The fake instance to pass to your class |
| `m.Verify(s => s.Save(...), Times.Once)` | Assert it was called (Once, Never, Exactly(n), AtLeastOnce) |
| `new Mock<T>(MockBehavior.Strict)` | Throws for any call that wasn't set up |

### The sample's real unit tests (samples/ShopApi.Tests/UnitTests.cs)

```csharp
// ---------- Unit test: controller with a MOCKED service ----------
public class ProductsControllerTests
{
    [Fact]
    public async Task GetById_WhenProductMissing_Returns404()
    {
        // Arrange
        var service = new Mock<IProductService>();
        service.Setup(s => s.GetByIdAsync(99, It.IsAny<CancellationToken>()))
               .ReturnsAsync((ProductDto?)null);
        var controller = new ProductsController(service.Object);

        // Act
        ActionResult<ProductDto> result = await controller.GetById(99, CancellationToken.None);

        // Assert
        Assert.IsType<NotFoundResult>(result.Result);
        service.Verify(s => s.GetByIdAsync(99, It.IsAny<CancellationToken>()), Times.Once);
    }

    [Fact]
    public async Task GetById_WhenProductExists_Returns200WithBody()
    {
        var dto = new ProductDto(1, "Laptop", 800m, 10, "Electronics");
        var service = new Mock<IProductService>();
        service.Setup(s => s.GetByIdAsync(1, It.IsAny<CancellationToken>())).ReturnsAsync(dto);

        ActionResult<ProductDto> result = await new ProductsController(service.Object).GetById(1, CancellationToken.None);

        OkObjectResult ok = Assert.IsType<OkObjectResult>(result.Result);
        Assert.Equal(dto, ok.Value);
    }
}
```

Points to explain:
- The controller depends on `IProductService` (an **interface**, injected via constructor) — that's what makes it mockable. DI and testability go together (chapter 4).
- `(ProductDto?)null` — the cast is needed so Moq knows which overload/type to use.
- `ActionResult<T>.Result` holds the `IActionResult` (`NotFoundResult`, `OkObjectResult`); `.Value` is set only when you return the object directly.
- `It.IsAny<CancellationToken>()` — we don't care which token is passed.
- `Verify(..., Times.Once)` — interaction testing; use it when the **call itself** is the behaviour (e.g. "email was sent"), not for every query.

### Complex Moq example: testing a caching service (compile-verified, all pass)

Testing the `CachedProductService` from chapter 12 — a real `MemoryCache` (it's cheap and in-memory, no need to mock it) + a mocked repository:

```csharp
public class CachedProductServiceTests
{
    [Fact]
    public async Task GetAsync_CalledTwice_HitsRepositoryOnlyOnce()
    {
        // Arrange
        var repo = new Mock<IProductRepository>();
        repo.Setup(r => r.GetByIdAsync(1, It.IsAny<CancellationToken>()))
            .ReturnsAsync(new ProductDto(1, "Laptop", 800m));
        using var cache = new MemoryCache(new MemoryCacheOptions());
        var sut = new CachedProductService(cache, repo.Object);

        // Act
        ProductDto? first = await sut.GetAsync(1, CancellationToken.None);
        ProductDto? second = await sut.GetAsync(1, CancellationToken.None);

        // Assert
        Assert.Equal("Laptop", first!.Name);
        Assert.Same(first, second);
        repo.Verify(r => r.GetByIdAsync(1, It.IsAny<CancellationToken>()), Times.Once);
    }

    [Fact]
    public async Task UpdatePrice_RemovesCachedEntry()
    {
        var repo = new Mock<IProductRepository>();
        repo.SetupSequence(r => r.GetByIdAsync(1, It.IsAny<CancellationToken>()))
            .ReturnsAsync(new ProductDto(1, "Laptop", 800m))
            .ReturnsAsync(new ProductDto(1, "Laptop", 750m));
        using var cache = new MemoryCache(new MemoryCacheOptions());
        var sut = new CachedProductService(cache, repo.Object);

        await sut.GetAsync(1, CancellationToken.None);
        await sut.UpdatePriceAsync(1, 750m, CancellationToken.None);
        ProductDto? after = await sut.GetAsync(1, CancellationToken.None);

        Assert.Equal(750m, after!.Price);
        repo.Verify(r => r.UpdatePriceAsync(1, 750m, It.IsAny<CancellationToken>()), Times.Once);
    }

    [Fact]
    public async Task Repository_Throws_ExceptionBubblesUp()
    {
        var repo = new Mock<IProductRepository>();
        repo.Setup(r => r.GetByIdAsync(It.IsAny<int>(), It.IsAny<CancellationToken>()))
            .ThrowsAsync(new TimeoutException());
        var sut = new CachedProductService(new MemoryCache(new MemoryCacheOptions()), repo.Object);

        await Assert.ThrowsAsync<TimeoutException>(() => sut.GetAsync(5, CancellationToken.None));
    }
}
```

### Testing an `AuthorizationHandler` (compile-verified, all pass)

The `MinimumAgeHandler` from chapter 10 takes a `TimeProvider` (.NET 8+), so "today" is controllable — no flaky date tests:

```csharp
public class FixedTimeProvider(DateTimeOffset now) : TimeProvider
{
    public override DateTimeOffset GetUtcNow() => now;
}

public class MinimumAgeHandlerTests
{
    private static async Task<bool> EvaluateAsync(string? birthDate)
    {
        var claims = birthDate is null ? [] : new[] { new Claim(ClaimTypes.DateOfBirth, birthDate) };
        var user = new ClaimsPrincipal(new ClaimsIdentity(claims, "Test"));
        var requirement = new MinimumAgeRequirement(18);
        var context = new AuthorizationHandlerContext([requirement], user, resource: null);
        var handler = new MinimumAgeHandler(new FixedTimeProvider(new DateTimeOffset(2026, 6, 15, 0, 0, 0, TimeSpan.Zero)));

        await handler.HandleAsync(context);
        return context.HasSucceeded;
    }

    [Theory]
    [InlineData("2008-06-15", true)]   // 18 today
    [InlineData("2008-06-16", false)]  // 18 tomorrow
    [InlineData("1990-01-01", true)]
    [InlineData(null, false)]          // no claim
    public async Task Handler_ChecksAge(string? birthDate, bool expected)
        => Assert.Equal(expected, await EvaluateAsync(birthDate));
}
```

(There is also the `Microsoft.Extensions.TimeProvider.Testing` package with `FakeTimeProvider` that supports advancing time.)

---

## 13.4 What to mock — and what NOT to mock

| ✅ Mock / fake | ❌ Don't mock |
|---|---|
| External HTTP APIs (payment, exchange rates) | **`DbContext` / `DbSet`** |
| Email/SMS senders, message buses | Value objects, DTOs, records |
| Clock (`TimeProvider`), random, GUID generators | The class under test itself |
| Your own interfaces at **architectural boundaries** (`IProductService` when testing a controller) | `IMemoryCache`, `ILogger` → use real `MemoryCache` / `NullLogger<T>.Instance` |
| Slow or non-deterministic things | Framework types you don't own (`HttpContext` in deep detail) — prefer integration tests |

**Rule:** "Don't mock what you don't own" — and **don't mock types that are easier to use for real**.

### Why not mock `DbContext`?

Mocking `DbSet<T>` with Moq requires faking `IQueryable` + async providers — dozens of lines — and **LINQ-to-Objects behaves differently from SQL** (case sensitivity, `null` handling, translation failures, constraints, transactions). Your tests pass and production fails.

### Options for testing EF Core code

| Option | Real SQL? | Relational features (FK, constraints, transactions, raw SQL) | Speed | Verdict |
|---|---|---|---|---|
| Mock `DbSet` | ❌ | ❌ | Fast | ❌ Avoid |
| **EF Core InMemory provider** | ❌ (LINQ to objects) | ❌ No FKs, no transactions, no `FromSql`, no `ExecuteUpdate`, case-sensitive compare | Fast | ⚠️ Microsoft **discourages** it for testing |
| **SQLite in-memory** (`Data Source=:memory:`) | ✅ Real relational DB | ✅ Mostly (some SQL Server differences) | Fast | ✅ Good default (the sample uses it) |
| **Testcontainers** (real SQL Server in Docker) | ✅ Exact production engine | ✅ All | Slower (container start) | ✅ Best fidelity |
| Repository pattern + mock the repository | n/a | n/a | Fast | ✅ For unit-testing services; test repositories separately with a real DB |

### The sample's `ProductServiceTests` — real service + SQLite in-memory

```csharp
// ---------- Unit/"narrow integration" test: real service + SQLite in-memory ----------
public class ProductServiceTests : IDisposable
{
    private readonly SqliteConnection _connection;
    private readonly AppDbContext _db;
    private readonly ProductService _service;

    public ProductServiceTests()
    {
        // In-memory SQLite lives as long as the connection is open.
        _connection = new SqliteConnection("Data Source=:memory:");
        _connection.Open();
        _db = new AppDbContext(new DbContextOptionsBuilder<AppDbContext>().UseSqlite(_connection).Options);
        _db.Database.EnsureCreated();   // creates tables + seed (3 products)
        _service = new ProductService(_db, NullLogger<ProductService>.Instance);
    }

    [Fact]
    public async Task GetPage_ReturnsRequestedPage()
    {
        PagedResult<ProductDto> page = await _service.GetPageAsync(page: 2, pageSize: 2, search: null, CancellationToken.None);

        Assert.Equal(3, page.TotalCount);
        Assert.Single(page.Items);
        Assert.Equal("C# in Depth", page.Items[0].Name);
    }

    [Fact]
    public async Task Delete_IsSoft_AndHidesProductFromQueries()
    {
        Assert.True(await _service.DeleteAsync(2, CancellationToken.None));

        Assert.Null(await _service.GetByIdAsync(2, CancellationToken.None));
        // Row still exists when we bypass the global query filter
        Assert.True(await _db.Products.IgnoreQueryFilters().AnyAsync(p => p.Id == 2 && p.IsDeleted));
    }

    [Fact]
    public async Task Create_WithUnknownCategory_Throws()
    {
        var request = new CreateProductRequest("Pen", 1m, 5, CategoryId: 999);
        await Assert.ThrowsAsync<ArgumentException>(() => _service.CreateAsync(request, CancellationToken.None));
    }

    [Fact]
    public async Task Update_WhenRowChangedByOtherUser_ThrowsConcurrencyException()
    {
        // User A loads the row (now tracked with the original Version)...
        await _db.Products.FindAsync(1);
        // ...then user B changes it "behind our back"
        await _db.Database.ExecuteSqlRawAsync(
            "UPDATE Products SET Version = {0} WHERE Id = 1", Guid.NewGuid());

        var request = new UpdateProductRequest("Laptop Pro", 900m, 5);
        await Assert.ThrowsAsync<DbUpdateConcurrencyException>(() => _service.UpdateAsync(1, request, CancellationToken.None));
    }

    public void Dispose()
    {
        _db.Dispose();
        _connection.Dispose();
    }
}
```

Why this is a good example to talk about:

| Detail | Explanation |
|---|---|
| Constructor = per-test setup | xUnit creates a new class instance per test → **fresh database per test** (isolation) |
| `IDisposable.Dispose` = per-test cleanup | Closing the connection destroys the in-memory DB |
| Connection opened manually | SQLite `:memory:` DB lives only while **that** connection is open — if EF opened/closed it, the DB would vanish |
| `EnsureCreated()` | Creates schema + `HasData` seed (no migrations needed for tests) |
| `NullLogger<T>.Instance` | Real no-op logger — no need to mock `ILogger` |
| Concurrency test | Tests a **relational** behaviour (concurrency token) — impossible to test reliably with mocks or the InMemory provider |
| Soft-delete test | Tests the **global query filter** + `IgnoreQueryFilters()` — real EF behaviour |

⚠️ **Interview trap:** Using the EF Core **InMemory** provider and claiming "the repository is tested". It doesn't enforce foreign keys, doesn't support transactions or raw SQL, and its string comparison differs from SQL Server. Say you prefer **SQLite in-memory** or **Testcontainers**.

---

## 13.5 Fixtures: sharing expensive setup

| Mechanism | Lifetime | Use for |
|---|---|---|
| Constructor + `Dispose` | **Per test** | Cheap, must-be-fresh state (SQLite connection above) |
| `IAsyncLifetime` (`InitializeAsync`/`DisposeAsync`) | Per test, async | Async setup (seeding via API) |
| **`IClassFixture<T>`** | **Once per test class** | Expensive things: `WebApplicationFactory`, Docker container |
| `ICollectionFixture<T>` + `[Collection("name")]` | Once for **several** classes | One DB container for the whole suite |

```csharp
public class ProductsApiTests(ShopApiFactory factory) : IClassFixture<ShopApiFactory>
{
    // factory is created ONCE for all tests in this class, injected into each test-class instance,
    // and disposed after the last test of the class.
}
```

⚠️ **Interview trap:** Shared fixtures mean **shared state**. Tests in the class must not depend on each other's data or on execution order (xUnit doesn't guarantee order). The sample's `DeleteProduct_AsAdmin_Returns204` creates its **own** product first instead of deleting a seeded one that another test may read.

---

## 13.6 Integration tests with `WebApplicationFactory`

**Simple words:** `WebApplicationFactory<TEntryPoint>` (package `Microsoft.AspNetCore.Mvc.Testing`) starts **your real app in memory** using `TestServer` — real `Program.cs`, real DI, real middleware, real routing, real auth — and gives you an `HttpClient` that talks to it **without a network port**. Analogy: a **flight simulator** — the real cockpit software, but no real plane in the air.

### Step 1: make `Program` visible

With top-level statements, the compiler-generated `Program` class is `internal`. The sample's last line of `Program.cs`:

```csharp
// Makes Program visible to WebApplicationFactory<Program> in integration tests
public partial class Program;
```

(Alternative: `<InternalsVisibleTo Include="ShopApi.Tests" />` in the csproj. In .NET 10 the source generator may also make `Program` public automatically — the explicit line is still the clearest answer.)

### Step 2: a custom factory — the sample's `ShopApiFactory`

```csharp
// Hosts the REAL app in memory (TestServer). Each factory gets its own SQLite file.
public class ShopApiFactory : WebApplicationFactory<Program>
{
    private readonly string _dbPath = Path.Combine(Path.GetTempPath(), $"shop-test-{Guid.NewGuid():N}.db");

    protected override void ConfigureWebHost(IWebHostBuilder builder)
    {
        builder.UseEnvironment("Development");
        builder.UseSetting("ConnectionStrings:Shop", $"Data Source={_dbPath};Pooling=False");
    }

    protected override void Dispose(bool disposing)
    {
        base.Dispose(disposing);
        if (File.Exists(_dbPath)) File.Delete(_dbPath);
    }
}
```

| Line | Why |
|---|---|
| `ConfigureWebHost` | Hook to change the app **before** it starts |
| `UseEnvironment("Development")` | Choose the environment (affects `appsettings.{env}.json`, dev exception page) |
| `UseSetting("ConnectionStrings:Shop", ...)` | **Override configuration** — here a unique SQLite file per factory so tests don't touch the dev DB |
| `Pooling=False` | So the file isn't locked and can be deleted |
| `Dispose` override | Clean up the temp DB |

### Step 3: the tests — the sample's `ProductsApiTests`

```csharp
public class ProductsApiTests(ShopApiFactory factory) : IClassFixture<ShopApiFactory>
{
    private async Task<HttpClient> ClientAsAsync(string user, string password)
    {
        HttpClient client = factory.CreateClient();
        HttpResponseMessage res = await client.PostAsJsonAsync("/api/auth/token", new LoginRequest(user, password));
        res.EnsureSuccessStatusCode();
        TokenResponse token = (await res.Content.ReadFromJsonAsync<TokenResponse>())!;
        client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token.access_token);
        return client;
    }

    private record TokenResponse(string access_token);

    [Fact]
    public async Task GetProducts_Returns200AndSeedData()
    {
        HttpClient client = factory.CreateClient();

        HttpResponseMessage res = await client.GetAsync("/api/products?page=1&pageSize=10");

        Assert.Equal(HttpStatusCode.OK, res.StatusCode);
        Assert.True(res.Headers.Contains("X-Correlation-Id"));
        PagedResult<ProductDto>? body = await res.Content.ReadFromJsonAsync<PagedResult<ProductDto>>();
        Assert.True(body!.TotalCount >= 3);
    }

    [Fact]
    public async Task CreateProduct_InvalidBody_Returns400()
    {
        HttpClient client = await ClientAsAsync("user", "user123");

        HttpResponseMessage res = await client.PostAsJsonAsync("/api/products", new CreateProductRequest("", -5m, 1, 1));

        Assert.Equal(HttpStatusCode.BadRequest, res.StatusCode);
        string json = await res.Content.ReadAsStringAsync();
        Assert.Contains("Name", json);
        Assert.Contains("Price", json);
    }

    [Fact]
    public async Task CreateProduct_WithToken_Returns201WithLocation()
    {
        HttpClient client = await ClientAsAsync("user", "user123");

        HttpResponseMessage res = await client.PostAsJsonAsync("/api/products", new CreateProductRequest("Keyboard", 30m, 15, 1));

        Assert.Equal(HttpStatusCode.Created, res.StatusCode);
        Assert.NotNull(res.Headers.Location);
    }

    [Fact]
    public async Task DeleteProduct_AsUser_Returns403()
    {
        HttpClient client = await ClientAsAsync("user", "user123");
        HttpResponseMessage res = await client.DeleteAsync("/api/products/3");
        Assert.Equal(HttpStatusCode.Forbidden, res.StatusCode);
    }

    [Fact]
    public async Task DeleteProduct_AsAdmin_Returns204()
    {
        HttpClient client = await ClientAsAsync("admin", "admin123");
        HttpResponseMessage create = await client.PostAsJsonAsync("/api/products", new CreateProductRequest("Temp", 1m, 1, 2));
        ProductDto created = (await create.Content.ReadFromJsonAsync<ProductDto>())!;

        HttpResponseMessage res = await client.DeleteAsync($"/api/products/{created.Id}");

        Assert.Equal(HttpStatusCode.NoContent, res.StatusCode);
    }

    // ... plus: 404 ProblemDetails, 401 without token, 400 from exception handler,
    //     wrong password 401, minimal API categories 200, /health "Healthy"
}
```

What each test proves that a unit test **cannot**:

| Test | Proves |
|---|---|
| `GetProducts_Returns200AndSeedData` | Routing, query binding, EF + SQLite, JSON serialization, **correlation-id middleware** |
| `CreateProduct_InvalidBody_Returns400` | `[ApiController]` automatic model validation with DataAnnotations |
| `CreateProduct_WithToken_Returns201WithLocation` | JWT issuing + validation, `CreatedAtRoute` Location header |
| `DeleteProduct_AsUser_Returns403` / `AsAdmin_Returns204` | `[Authorize(Policy = "AdminOnly")]` + role claim end-to-end |
| `CreateProduct_UnknownCategory_Returns400FromExceptionHandler` | `GlobalExceptionHandler` mapping `ArgumentException` → 400 |
| `HealthCheck_ReturnsHealthy` | Health check registration and DB connectivity |

Run them:

```bash
cd samples/ShopApi.Tests
dotnet test                       # 17 tests pass
dotnet test --filter "FullyQualifiedName~ProductsApiTests"
dotnet test --filter "DisplayName~Returns403"
```

---

## 13.7 Replacing services: `ConfigureTestServices`

**Problem:** your app calls a real exchange-rate API, sends real emails, or uses SQL Server. In tests you want fakes.

`ConfigureTestServices` runs **after** the app's own `Program.cs` registrations, so your replacements win. Complex example (compile-verified, all tests pass):

```csharp
public class ApiFactory : WebApplicationFactory<Program>
{
    public Mock<IRateService> RateService { get; } = new();

    protected override void ConfigureWebHost(IWebHostBuilder builder)
    {
        builder.UseEnvironment("Testing");
        builder.UseSetting("Jwt:Key", "integration-tests-key-integration-tests-key");

        builder.ConfigureTestServices(services =>
        {
            // 1. Replace a real dependency with a mock
            services.RemoveAll<IRateService>();            // Microsoft.Extensions.DependencyInjection.Extensions
            services.AddSingleton(RateService.Object);

            // 2. Replace JWT auth with a fake scheme (next section)
            services.AddAuthentication(TestAuthHandler.SchemeName)
                .AddScheme<AuthenticationSchemeOptions, TestAuthHandler>(TestAuthHandler.SchemeName, _ => { });
            services.PostConfigure<AuthenticationOptions>(o =>
            {
                o.DefaultAuthenticateScheme = TestAuthHandler.SchemeName;
                o.DefaultChallengeScheme = TestAuthHandler.SchemeName;
            });
        });
    }
}

public class ApiTests(ApiFactory factory) : IClassFixture<ApiFactory>
{
    [Fact]
    public async Task GetRate_UsesMockedService()
    {
        factory.RateService.Setup(s => s.GetRateAsync("EUR", It.IsAny<CancellationToken>())).ReturnsAsync(0.92m);

        decimal rate = await factory.CreateClient().GetFromJsonAsync<decimal>("/api/rates/EUR");

        Assert.Equal(0.92m, rate);
    }
}
```

Replacing the **database** (common interview question): remove the app's `DbContextOptions<AppDbContext>` registration and add SQLite/Testcontainers:

```csharp
builder.ConfigureTestServices(services =>
{
    services.RemoveAll<DbContextOptions<AppDbContext>>();
    services.AddDbContext<AppDbContext>(o => o.UseSqlite(_connection));   // shared open SqliteConnection
});
```

(For EF Core 9+, also remove `IDbContextOptionsConfiguration<AppDbContext>` or simply override the **connection string** with `UseSetting` like the sample does — the simplest approach.)

| Hook | Runs | Use |
|---|---|---|
| `UseSetting(key, value)` | Config | Override connection strings, feature flags, JWT key |
| `UseEnvironment(name)` | Host | Pick environment |
| `ConfigureAppConfiguration` | Config | Add in-memory config collection |
| `ConfigureServices` | **Before** app registrations | Rarely what you want |
| **`ConfigureTestServices`** | **After** app registrations | ✅ Replace services |
| `WithWebHostBuilder(b => ...)` | Per test | One-off variation of a shared factory |

---

## 13.8 Testing authenticated endpoints

Two approaches:

| Approach | How | Pros | Cons |
|---|---|---|---|
| **Real tokens** (the sample) | Call `/api/auth/token`, set `Authorization: Bearer` | Tests the real JWT setup end-to-end | Needs a login endpoint & known users; slower |
| **Fake auth handler** | Register a test `AuthenticationHandler` that builds any user you want | Any user/role/claim instantly, no IdP needed (works with Entra ID apps) | Doesn't test real token validation |

Sample approach (real tokens) — the helper `ClientAsAsync("admin", "admin123")` shown above.

Fake-handler approach (compile-verified, all pass):

```csharp
public class TestAuthHandler(IOptionsMonitor<AuthenticationSchemeOptions> options, ILoggerFactory logger, UrlEncoder encoder)
    : AuthenticationHandler<AuthenticationSchemeOptions>(options, logger, encoder)
{
    public const string SchemeName = "Test";

    protected override Task<AuthenticateResult> HandleAuthenticateAsync()
    {
        // Tests choose the user via headers; no header => anonymous
        if (!Request.Headers.TryGetValue("X-Test-User", out var userName))
            return Task.FromResult(AuthenticateResult.NoResult());

        var claims = new List<Claim>
        {
            new(ClaimTypes.Name, userName!),
            new(ClaimTypes.NameIdentifier, userName!)
        };
        foreach (string role in Request.Headers["X-Test-Role"].ToString().Split(',', StringSplitOptions.RemoveEmptyEntries))
            claims.Add(new Claim(ClaimTypes.Role, role));

        var principal = new ClaimsPrincipal(new ClaimsIdentity(claims, SchemeName));
        return Task.FromResult(AuthenticateResult.Success(new AuthenticationTicket(principal, SchemeName)));
    }
}

public class ApiTests(ApiFactory factory) : IClassFixture<ApiFactory>
{
    private HttpClient ClientAs(string? user = null, string? roles = null)
    {
        HttpClient client = factory.CreateClient();
        if (user is not null) client.DefaultRequestHeaders.Add("X-Test-User", user);
        if (roles is not null) client.DefaultRequestHeaders.Add("X-Test-Role", roles);
        return client;
    }

    [Fact]
    public async Task Me_Anonymous_Returns401()
    {
        HttpResponseMessage res = await ClientAs().GetAsync("/api/me");
        Assert.Equal(HttpStatusCode.Unauthorized, res.StatusCode);
    }

    [Theory]
    [InlineData("alice", "User", HttpStatusCode.NoContent)]   // owner
    [InlineData("bob", "User", HttpStatusCode.Forbidden)]     // not owner
    [InlineData("boss", "Admin", HttpStatusCode.NoContent)]   // admin
    public async Task UpdateOrder_ResourceBasedAuthorization(string user, string role, HttpStatusCode expected)
    {
        HttpResponseMessage res = await ClientAs(user, role).PutAsJsonAsync("/api/orders/1", new UpdateOrderRequest("Shipped"));
        Assert.Equal(expected, res.StatusCode);
    }
}
```

Notes:
- `AuthenticateResult.NoResult()` = "I don't know this user" → `[Authorize]` returns **401** via the challenge. Perfect for anonymous tests.
- Since .NET 8 the `AuthenticationHandler` constructor no longer takes `ISystemClock` (it's obsolete) — use the 3-parameter version above.
- Use the fake handler **only** in test projects!

---

## 13.9 Test naming conventions

Good names read like a specification. Popular patterns:

| Pattern | Example |
|---|---|
| `Method_Scenario_ExpectedResult` (Roy Osherove) — used by the sample | `GetById_WhenProductMissing_Returns404` |
| `Given_When_Then` (BDD) | `GivenUnknownCategory_WhenCreatingProduct_ThenReturns400` |
| `Should_..._When_...` | `Should_Return403_When_UserIsNotAdmin` |

Rules: one behaviour per test, no logic (`if`/loops) in tests, no dependency between tests, make the failure message obvious, test **behaviour not implementation**.

⚠️ **Interview trap:** Tests that verify **every** internal call with `Verify(...)` are brittle — a harmless refactor breaks them. Assert on **outcomes** (return values, state, HTTP responses); verify interactions only when the interaction *is* the requirement.

---

## 13.10 Code coverage with coverlet

**Simple words:** Coverage = % of your code lines/branches executed by tests. The sample's test project already references `coverlet.collector`.

```bash
dotnet test --collect:"XPlat Code Coverage"
# -> TestResults/<guid>/coverage.cobertura.xml

dotnet tool install -g dotnet-reportgenerator-globaltool
reportgenerator -reports:"**/coverage.cobertura.xml" -targetdir:coverage-report -reporttypes:Html
```

| Metric | Meaning |
|---|---|
| Line coverage | % of lines executed |
| Branch coverage | % of `if`/`switch` paths executed — more meaningful |

⚠️ **Interview trap:** "100% coverage = no bugs." Coverage shows what was **executed**, not what was **verified** — a test without asserts still increases coverage. Aim for high coverage on business logic (70–80% is a common team target) and use it to find **untested** areas, not as a goal by itself.

In CI (Azure DevOps / GitHub Actions) publish the Cobertura file to show coverage per PR, and optionally fail the build under a threshold.

---

## 13.11 TDD in one paragraph

**Test-Driven Development** = write a **failing** test first (**Red**), write the minimum code to make it pass (**Green**), then clean up the code while tests stay green (**Refactor**) — repeat in tiny steps. Benefits: testable design (forces DI and small classes), built-in regression suite, clear requirements. Cost: slower at first, needs discipline; best for business logic, less useful for UI glue or exploratory spikes. In an interview, say honestly whether you practise it fully or "test-first for complex logic, test-after for the rest".

---

## Try it yourself

1. Write an xUnit `[Theory]` for a method `bool IsValidSku(string sku)` that should accept `"ABC-123"` and reject `""`, `"abc-123"`, `"ABC123"`.
2. You must unit-test `OrderService.PlaceAsync` which uses `AppDbContext`, `IEmailSender` and `TimeProvider`. What do you mock and what do you make real?
3. Your integration tests suddenly read/write your **development** database. How do you stop that with `WebApplicationFactory`?
4. Why does the sample open the `SqliteConnection` manually in the test constructor?
5. Write the Moq setup + verify for: `IEmailSender.SendAsync(string to, string subject, CancellationToken ct)` must be called exactly once with `to == "a@b.com"`.
6. An integration test needs a user with role `Manager`, but your app uses Entra ID tokens which you can't create in tests. What do you do?

<details><summary>Answer</summary>

1. ```csharp
   [Theory]
   [InlineData("ABC-123", true)]
   [InlineData("", false)]
   [InlineData("abc-123", false)]
   [InlineData("ABC123", false)]
   public void IsValidSku_ReturnsExpected(string sku, bool expected)
       => Assert.Equal(expected, _sut.IsValidSku(sku));
   ```
2. **Mock** `IEmailSender` (external side effect; verify it was called). **Fake** `TimeProvider` (fixed time). **Don't mock** `AppDbContext` — use SQLite in-memory (or Testcontainers). Logger → `NullLogger<OrderService>.Instance`.
3. Override the connection string in your factory: `builder.UseSetting("ConnectionStrings:Shop", "Data Source=<temp file>")` (as the sample does), or replace the DbContext registration in `ConfigureTestServices`. Also use a test environment name.
4. SQLite `:memory:` databases exist only while their connection stays open. If EF opened and closed the connection itself, the schema and data would disappear between operations.
5. ```csharp
   var email = new Mock<IEmailSender>();
   email.Setup(e => e.SendAsync(It.IsAny<string>(), It.IsAny<string>(), It.IsAny<CancellationToken>()))
        .Returns(Task.CompletedTask);
   // ... act ...
   email.Verify(e => e.SendAsync("a@b.com", It.IsAny<string>(), It.IsAny<CancellationToken>()), Times.Once);
   ```
6. Register a **test authentication handler** in `ConfigureTestServices` (like `TestAuthHandler`), make it the default authenticate/challenge scheme, and let tests pass the desired user/roles (e.g. via headers). This tests your authorization policies without a real identity provider.
</details>

---

## Quick revision

- **Test pyramid**: many unit, fewer integration, few E2E.
- xUnit: `[Fact]` = one test, `[Theory]` + `[InlineData]` = parameterised; **new class instance per test**.
- Setup = **constructor**, cleanup = **`IDisposable`**; share expensive setup with **`IClassFixture<T>`**.
- **AAA**: Arrange, Act, Assert.
- Moq: `Setup(...).ReturnsAsync(...)`, `It.IsAny<T>()`, `.Object`, `Verify(..., Times.Once)`.
- Mock **boundaries** (HTTP APIs, email, clock); **don't mock DbContext** — use **SQLite in-memory** or **Testcontainers**; EF InMemory provider is discouraged.
- `WebApplicationFactory<Program>` runs the real app in memory; needs **`public partial class Program;`**.
- `UseSetting` overrides config; **`ConfigureTestServices`** replaces services (runs after `Program.cs`).
- Authenticated tests: get a **real token** (sample) or a **fake `AuthenticationHandler`**.
- 401/403, validation, middleware, routing bugs are caught by **integration tests**, not unit tests.
- Name tests `Method_Scenario_ExpectedResult`; assert outcomes, not internals.
- Coverage via **coverlet** (`--collect:"XPlat Code Coverage"`) — a guide, not a goal. TDD = Red → Green → Refactor.

---

# 14. Security (OWASP) & Deployment

> **Why interviewers ask this**
> Interviewers want to know you won't ship an API that leaks data through **SQL injection**, **IDOR**, or **over-posting**, and that you understand how your code actually **runs in production**: `dotnet publish`, IIS vs Kestrel, reverse proxies, Docker, Azure App Service, configuration per environment, and a CI/CD pipeline with zero-downtime deployment. At 2 YOE you're not expected to be a security engineer — but you are expected to know the **top risks and their standard fixes**.

---

# Part A — Security

## 14.1 OWASP API Security — the map

**OWASP** (Open Worldwide Application Security Project) publishes the famous **Top 10** lists (web apps and APIs). Analogy: a **home-security checklist** — locked doors (authN), each room key only for its owner (object-level authZ), no spare key under the mat (secrets), cameras (logging).

| Risk | One-line meaning | ASP.NET Core fix | Section |
|---|---|---|---|
| **Broken Object Level Authorization (IDOR)** | User A reads `/orders/57` which is user B's | Resource-based authz, filter by user id | 14.6 |
| Broken Authentication | Weak tokens, no expiry, no lockout | JWT validation, Identity lockout, MFA | Ch. 10 |
| **Broken Object Property Level Authorization (mass assignment / over-posting)** | Client sets `IsAdmin=true` in JSON | DTOs | 14.5 |
| Unrestricted Resource Consumption | No limits → DoS, huge bills | Rate limiting, pagination, request size limits | 14.8 |
| Broken Function Level Authorization | Normal user calls admin endpoint | `[Authorize(Policy=...)]`, fallback policy | Ch. 10 |
| **Injection** (SQL, command, LDAP) | Untrusted input executed as code | Parameterized queries / EF LINQ | 14.2 |
| Security Misconfiguration | Dev exception page in prod, verbose errors, open CORS | Environment config, ProblemDetails, headers | 14.7 |
| **XSS** | Injected script runs in the victim's browser | Razor encoding, CSP | 14.3 |
| **CSRF** | Victim's browser sends authenticated request to your site | Antiforgery tokens, SameSite cookies | 14.4 |
| Sensitive data exposure | Secrets in code, no HTTPS, plain passwords | Key Vault, HTTPS/HSTS, hashing, Data Protection | 14.7, 14.9 |
| Insufficient logging & monitoring | Attack goes unnoticed | Structured logs, alerts | Ch. 11 |

---

## 14.2 SQL injection

**Simple words:** The attacker types **SQL code** where you expected data, and your app glues it into a query. Analogy: a form says "Write your name: ____" and someone writes *"Ravi. Also, give me the safe's key."* — and the clerk reads it all out as an instruction.

```text
Input:  ' OR 1=1; DROP TABLE Customers; --
Query:  SELECT * FROM Customers WHERE Name = '' OR 1=1; DROP TABLE Customers; --'
```

**The fix: parameterize.** Parameters are sent **separately** from the SQL text, so the DB treats them strictly as **data**, never as code.

### EF Core — safe vs unsafe (compile-verified)

```csharp
public class CustomerSearch(ShopDb db)
{
    // ❌ VULNERABLE: string concatenation into raw SQL
    public Task<List<Customer>> SearchUnsafe(string name) =>
        db.Customers.FromSqlRaw("SELECT * FROM Customers WHERE Name = '" + name + "'").ToListAsync();

    // ✅ SAFE: FromSql with an interpolated string -> becomes a DbParameter
    public Task<List<Customer>> SearchSafe(string name) =>
        db.Customers.FromSql($"SELECT * FROM Customers WHERE Name = {name}").ToListAsync();

    // ✅ SAFE: FromSqlRaw with explicit placeholders
    public Task<List<Customer>> SearchSafeRaw(string name) =>
        db.Customers.FromSqlRaw("SELECT * FROM Customers WHERE Name = {0}", name).ToListAsync();

    // ✅ BEST: plain LINQ is always parameterized
    public Task<List<Customer>> SearchLinq(string name) =>
        db.Customers.Where(c => c.Name == name).ToListAsync();
}
```

When compiling this, the EF Core analyzer actually warned on the unsafe line:

```text
warning EF1003: Method 'FromSqlRaw' inserts concatenated strings directly into the SQL,
without any protection against SQL injection. Consider using 'FromSql' instead...
```

| API | Safe? | Why |
|---|---|---|
| LINQ (`Where`, `Select`...) | ✅ | Always parameterized |
| `FromSql($"... {x}")` / `FromSqlInterpolated` | ✅ | Interpolation holes become `DbParameter`s |
| `FromSqlRaw("... {0}", x)` | ✅ | Placeholders become parameters |
| `FromSqlRaw("..." + x)` or `FromSqlRaw($"... {x}")` | ❌ | The string is built **before** EF sees it |
| `ExecuteSqlRaw` vs `ExecuteSql` | Same rules | The sample's test uses `ExecuteSqlRawAsync("... {0} ...", Guid.NewGuid())` ✅ |
| ADO.NET / Dapper | ✅ only with `@parameters` | `cmd.Parameters.AddWithValue("@name", name)`; Dapper: `new { name }` |

⚠️ **Interview trap:** `FromSqlRaw($"SELECT ... WHERE Name = '{name}'")` — looks like interpolation but it's **`FromSqlRaw`**, which receives an already-built string → **vulnerable**. Only `FromSql`/`FromSqlInterpolated` treat `$""` safely.

⚠️ **Interview trap:** Parameters can't be used for **identifiers** (table/column names, `ORDER BY` column). For dynamic sorting, map user input to an **allow-list**: `sort switch { "price" => q.OrderBy(p => p.Price), _ => q.OrderBy(p => p.Id) }`.

Defence in depth: least-privilege DB user (no `db_owner` for the app), stored procedures with parameters, input validation.

---

## 14.3 XSS — Cross-Site Scripting

**Simple words:** An attacker stores or reflects **JavaScript** into your page (e.g. a product review `<script>steal(document.cookie)</script>`), and it runs in **other users' browsers**. Analogy: someone sticks a fake "Pay here" sign on the shop's notice board — customers trust the board, so they trust the sign.

| XSS type | How |
|---|---|
| Stored | Malicious input saved in DB, shown to everyone |
| Reflected | Malicious input in URL echoed back in the page |
| DOM-based | Front-end JS writes untrusted data into the DOM (`innerHTML`) |

**ASP.NET Core defences:**

| Defence | Detail |
|---|---|
| **Razor HTML-encodes by default** | `@Model.Review` → `&lt;script&gt;` — rendered as text, not executed |
| Avoid `@Html.Raw(userInput)` | Bypasses encoding — only for trusted/sanitized HTML (use a sanitizer like HtmlSanitizer) |
| APIs return JSON with `Content-Type: application/json` | Browser doesn't execute it; `X-Content-Type-Options: nosniff` prevents MIME sniffing |
| Front-end frameworks (React/Angular) encode by default | Avoid `dangerouslySetInnerHTML` / `bypassSecurityTrustHtml` |
| **Content-Security-Policy** header | Blocks inline/third-party scripts even if injected |
| `HttpOnly` cookies | Injected JS can't read the auth cookie |

```cshtml
<p>@Model.Review</p>              @* ✅ encoded *@
<p>@Html.Raw(Model.Review)</p>    @* ❌ XSS if Review contains <script> *@
```

For a pure JSON API, XSS is mostly the **front-end's** responsibility — but the API must not serve user content as `text/html`.

---

## 14.4 CSRF — Cross-Site Request Forgery

**Simple words:** You're logged in to `bank.com` (cookie in your browser). You visit `evil.com`, which has a hidden form that POSTs to `bank.com/transfer`. Your browser **automatically attaches the bank cookie** → the bank thinks *you* sent it. Analogy: someone slips a pre-filled, **pre-signed** cheque into your pile of papers and you hand it over without looking.

```html
<!-- on evil.com -->
<form action="https://bank.com/transfer" method="post">
  <input name="to" value="attacker" /><input name="amount" value="5000" />
</form>
<script>document.forms[0].submit()</script>
```

**Fixes:**

| Fix | How |
|---|---|
| **Antiforgery tokens** | Server puts a secret token in the form + a cookie; on POST both must match. `evil.com` can't read the token. |
| `SameSite` cookies (`Lax`/`Strict`) | Browser doesn't send the cookie on cross-site POSTs (default `Lax` in modern browsers) |
| Don't change state on GET | GET must be safe/idempotent |

ASP.NET Core:
- **MVC/Razor Pages**: Form tag helper adds the hidden `__RequestVerificationToken` automatically; Razor Pages validate automatically; MVC uses `[ValidateAntiForgeryToken]` or `AutoValidateAntiforgeryTokenAttribute` globally.
- **Minimal APIs (.NET 8+)**: `builder.Services.AddAntiforgery(); app.UseAntiforgery();` — endpoints that bind **form** data (`[FromForm]`, `IFormFile`) are validated automatically.

### Why JWT-in-header APIs are less exposed

CSRF works because the browser **automatically** sends **cookies**. A JWT sent in the `Authorization: Bearer` header is added by **your JavaScript**, and `evil.com` can't read your token (same-origin policy) or make your browser add that header. So pure bearer-token APIs **don't need antiforgery tokens**.

| Auth mechanism | CSRF risk | Needs antiforgery? |
|---|---|---|
| Cookie (MVC, Razor Pages, BFF SPA) | ✅ Yes | ✅ Yes (+ SameSite) |
| JWT in `Authorization` header | Very low | ❌ No |
| JWT stored in a **cookie** | ✅ Yes (cookie is sent automatically again!) | ✅ Yes |

⚠️ **Interview trap:** "We use JWT so we're immune to CSRF" — only if the JWT travels in a **header**. Put it in a cookie and CSRF is back. (And JWT in `localStorage` trades CSRF risk for **XSS** risk.)

---

## 14.5 Over-posting / mass assignment

**Simple words:** Your endpoint binds the request JSON directly to an **entity**, so the client can set properties you never intended — `IsAdmin`, `Price`, `OwnerId`, `IsDeleted`. Analogy: a hotel check-in form where the guest can also write "Room upgrade: Presidential Suite, Price: 0" and the clerk just copies everything into the system.

```csharp
// ❌ Binds directly to the EF entity
[HttpPut("{id}")]
public async Task<IActionResult> Update(int id, Product product)
{
    _db.Update(product);                // client can send "isDeleted": true, "version": ..., any field!
    await _db.SaveChangesAsync();
    return NoContent();
}
```

**Fix: DTOs (input models) that contain only what the client may change.** The sample app does exactly this:

```csharp
public record UpdateProductRequest(
    [Required, StringLength(200, MinimumLength = 2)] string Name,
    [Range(0.01, 1_000_000)] decimal Price,
    [Range(0, int.MaxValue)] int Stock);

// ProductService.UpdateAsync copies ONLY allowed fields:
product.Name = request.Name;
product.Price = request.Price;
product.Stock = request.Stock;
product.Version = Guid.NewGuid();      // server-controlled
```

`IsDeleted`, `Version`, `CategoryId`, `Id` cannot be set by the client — they're not in the DTO. Bonus: DTOs also prevent **over-exposure** on the way out (never return `PasswordHash` because you serialized the entity), decouple API contracts from DB schema, and avoid serialization cycles.

Alternatives (weaker): `[Bind("Name,Price")]` (MVC), `[BindNever]` on properties. DTOs are the standard answer.

---

## 14.6 IDOR — Insecure Direct Object Reference

**Simple words:** The user is authenticated, but the API **doesn't check they own the object** whose id is in the URL. Changing `/api/orders/57` to `/api/orders/58` shows someone else's order. Analogy: hotel key cards that open **any** room as long as you're a guest.

OWASP calls this **Broken Object Level Authorization (BOLA)** — the **#1 API risk**.

```csharp
// ❌ Any logged-in user can read any order
[Authorize]
[HttpGet("{id}")]
public async Task<IActionResult> Get(int id) => Ok(await _db.Orders.FindAsync(id));
```

**Fix 1 — scope queries to the current user:**

```csharp
string userId = User.FindFirstValue(ClaimTypes.NameIdentifier)!;
OrderDto? order = await _db.Orders
    .Where(o => o.Id == id && o.OwnerId == userId)        // ownership in the WHERE clause
    .Select(o => new OrderDto(o.Id, o.Status, o.Total))
    .FirstOrDefaultAsync(ct);
return order is null ? NotFound() : Ok(order);           // 404 hides existence
```

**Fix 2 — resource-based authorization** with `IAuthorizationService` + `AuthorizationHandler<SameOwnerRequirement, Order>` (full compile-verified code in chapter 10, §10.11; verified: owner → 204, other user → 403, admin → 204).

Extra hardening: GUIDs instead of sequential ints make ids hard to guess — but that's **obscurity, not authorization**; you still need the check.

🎯 **Say this in the interview:** "Authentication alone isn't enough — every endpoint that takes an id must check the caller is allowed to access *that* object. I either filter the query by the current user id or use resource-based authorization with IAuthorizationService. And I never bind entities directly; DTOs prevent over-posting."

---

## 14.7 HTTPS, HSTS and security headers

### HTTPS & HSTS

| Feature | What | ASP.NET Core |
|---|---|---|
| **HTTPS redirection** | Redirects `http://` → `https://` (307/308) | `app.UseHttpsRedirection()` |
| **HSTS** (HTTP Strict Transport Security) | Header telling the browser: "for the next N days, **only** use HTTPS for this domain — don't even try HTTP" | `app.UseHsts()` |

Analogy: HTTPS redirection = a sign at the side door "please use the main entrance"; HSTS = the browser **remembers** and never tries the side door again (stops SSL-stripping attacks on the first request).

The sample app:

```csharp
if (app.Environment.IsDevelopment())
{
    app.MapOpenApi();
}
else
{
    app.UseHsts();          // not in Development: HSTS is cached by the browser and would break localhost
}
app.UseHttpsRedirection();
```

Configure it (compile-verified):

```csharp
builder.Services.AddHsts(o =>
{
    o.MaxAge = TimeSpan.FromDays(365);     // default is 30 days
    o.IncludeSubDomains = true;
    o.Preload = true;                      // eligible for browsers' built-in HSTS preload list
});
```

⚠️ **Interview trap:** For **pure APIs** called by non-browser clients, HTTPS redirection doesn't help much — the client already sent the token over plain HTTP before being redirected. Better: don't listen on HTTP at all (or reject it), and HSTS only affects browsers.

### Security headers (compile-verified)

```csharp
public static class SecurityHeaders
{
    public static IApplicationBuilder UseSecurityHeaders(this IApplicationBuilder app) =>
        app.Use(async (context, next) =>
        {
            var h = context.Response.Headers;
            h["X-Content-Type-Options"] = "nosniff";
            h["X-Frame-Options"] = "DENY";
            h["Referrer-Policy"] = "no-referrer";
            h["Content-Security-Policy"] = "default-src 'none'; frame-ancestors 'none'";
            h.Remove("Server");
            await next(context);
        });
}
// app.UseSecurityHeaders();   (verified by an integration test: X-Content-Type-Options == nosniff)
```

| Header | Protects against |
|---|---|
| `Strict-Transport-Security` | Protocol downgrade (set by `UseHsts`) |
| `X-Content-Type-Options: nosniff` | MIME sniffing (treating JSON/text as script/HTML) |
| `X-Frame-Options: DENY` / CSP `frame-ancestors` | **Clickjacking** (your page in an invisible iframe) |
| `Content-Security-Policy` | XSS, data injection (allowed script/style sources). For an API: `default-src 'none'` |
| `Referrer-Policy` | Leaking URLs (with ids/tokens) to other sites |
| Remove `Server` header | Version disclosure (Kestrel: `builder.WebHost.ConfigureKestrel(o => o.AddServerHeader = false)`) |

Also part of "security misconfiguration": developer exception page only in Development, ProblemDetails without stack traces (chapter 11), restrictive CORS (chapter 10), `AllowedHosts` configured, Swagger/OpenAPI UI not public in production (the sample maps OpenAPI only in Development).

---

## 14.8 Rate limiting & resource limits

Covered in chapter 12 (§12.8). Security angle:

| Limit | How |
|---|---|
| Requests per client | `AddRateLimiter` partitioned by user/IP/API key; stricter on `/login` (brute force) |
| Page size | `Math.Clamp(pageSize, 1, 100)` (the sample) |
| Request body size | Kestrel `MaxRequestBodySize` (default ~30 MB), `[RequestSizeLimit(1_000_000)]` |
| Timeouts | `CancellationToken`, `AddRequestTimeouts()` (.NET 8+), HttpClient timeouts |
| Login brute force | Identity **lockout** (`MaxFailedAccessAttempts`) |

---

## 14.9 Secrets management & Data Protection

### Secrets

| ❌ Never | ✅ Do |
|---|---|
| Secrets in `appsettings.json` committed to git (the sample's `DEMO-ONLY` key is a demo) | **User Secrets** locally (`dotnet user-secrets set "Jwt:Key" "..."`) |
| Secrets in Docker images / Dockerfile `ENV` | Environment variables / mounted secrets at runtime |
| Secrets in pipeline YAML | Pipeline **secret variables** / variable groups linked to Key Vault |
| Long-lived passwords for Azure resources | **Managed Identity** → Key Vault, SQL, Storage without any secret |

```csharp
// Production: load secrets from Azure Key Vault using the app's Managed Identity
// Packages: Azure.Extensions.AspNetCore.Configuration.Secrets, Azure.Identity  (not compile-verified here)
builder.Configuration.AddAzureKeyVault(
    new Uri($"https://{builder.Configuration["KeyVaultName"]}.vault.azure.net/"),
    new DefaultAzureCredential());
// A Key Vault secret named "Jwt--Key" becomes config key "Jwt:Key"
```

Config precedence reminder (later wins): `appsettings.json` → `appsettings.{Environment}.json` → User Secrets (Development) → environment variables → command line (→ Key Vault if added last). Same code, different source per environment.

If a secret leaks into git: **rotate it immediately** — deleting the commit isn't enough.

### Data Protection API

**Simple words:** ASP.NET Core's built-in **encryption service** for small payloads that must come back to *your* app: auth cookies, antiforgery tokens, TempData, "unsubscribe"/"reset" links. It manages **key rotation** automatically. Analogy: a **tamper-evident sealed envelope** that only your office can open.

Compile-verified + unit-tested (round-trip works, tampered token → null):

```csharp
public class UnsubscribeLinkService(IDataProtectionProvider provider)
{
    private readonly ITimeLimitedDataProtector _protector =
        provider.CreateProtector("Newsletter.Unsubscribe.v1").ToTimeLimitedDataProtector();

    public string CreateToken(string email) => _protector.Protect(email, TimeSpan.FromDays(30));

    public string? ReadToken(string token)
    {
        try { return _protector.Unprotect(token); }
        catch (System.Security.Cryptography.CryptographicException) { return null; } // tampered or expired
    }
}
```

| Concept | Meaning |
|---|---|
| **Purpose string** (`"Newsletter.Unsubscribe.v1"`) | Isolation: a token protected for one purpose can't be unprotected by another |
| Key ring | Keys rotate every 90 days by default; old keys still decrypt |
| Not for passwords | Passwords are **hashed**, not encrypted (chapter 10) |

⚠️ **Interview trap (deployment!):** By default keys are stored **locally** on each machine/container. With **multiple instances** or **container restarts**, instance B can't decrypt instance A's cookie → users randomly logged out, antiforgery failures. Fix: persist keys to a shared store and protect them:

```csharp
builder.Services.AddDataProtection()
    .SetApplicationName("ShopApi")
    .PersistKeysToAzureBlobStorage(blobUri, new DefaultAzureCredential())      // Azure.Extensions.AspNetCore.DataProtection.Blobs
    .ProtectKeysWithAzureKeyVault(keyUri, new DefaultAzureCredential());       // Azure.Extensions.AspNetCore.DataProtection.Keys
// (not compile-verified here — requires Azure packages)
```

---

# Part B — Deployment

## 14.10 `dotnet publish` options

**Simple words:** `dotnet build` compiles for development; **`dotnet publish`** produces the **final folder** you deploy (your DLLs, dependencies, `appsettings.json`, `web.config` for IIS).

```bash
dotnet publish -c Release -o ./publish                                   # framework-dependent (default)
dotnet publish -c Release -r linux-x64 --self-contained true -o ./publish
dotnet publish -c Release -r win-x64 --self-contained -p:PublishSingleFile=true
dotnet publish -c Release -r linux-x64 -p:PublishTrimmed=true
dotnet publish -c Release -r linux-x64 -p:PublishAot=true                # Native AOT (.NET 8+ for ASP.NET Core)
```

| Mode | What you ship | Size | Needs .NET installed? | Notes |
|---|---|---|---|---|
| **Framework-dependent** (default) | Your DLLs only | Small (MBs) | ✅ Yes, matching runtime | Runtime patched by the host (App Service, base image) |
| **Self-contained** | Your DLLs + the whole runtime for one RID (`-r linux-x64`) | ~70–100 MB | ❌ No | You control the runtime version; you must patch it yourself |
| **Single file** | One executable (`PublishSingleFile`) | Same as above, 1 file | Depends | Easy copy-deploy; native libs may extract |
| **Trimmed** | Unused framework code removed | Smaller | — | Reflection-heavy code can break; warnings tell you |
| **Native AOT** | Native machine code, no JIT | Small, **fast startup**, low memory | ❌ No | Great for microservices/serverless/containers. Limits: no runtime codegen, reflection limits, **EF Core and MVC controllers have limited/no AOT support**; Minimal APIs + `CreateSlimBuilder` + source-generated JSON work best |

🎯 **Say this in the interview:** "Framework-dependent is the default — small and the host keeps the runtime patched. Self-contained bundles the runtime so the server needs nothing installed. Single-file, trimming and Native AOT reduce size and startup time, with AOT being best for minimal APIs in containers but not fully compatible with reflection-heavy libraries."

---

## 14.11 Hosting on IIS: in-process vs out-of-process

**Kestrel** is ASP.NET Core's built-in cross-platform web server — **always** there. In front of it you may have a **reverse proxy**: IIS (Windows), Nginx/Apache (Linux), YARP, Azure Front Door, a Kubernetes ingress.

On IIS, the **ASP.NET Core Module (ANCM)** — installed with the **.NET Hosting Bundle** — connects IIS to your app. `dotnet publish` generates the `web.config`:

```xml
<aspNetCore processPath="dotnet" arguments=".\ShopApi.dll" hostingModel="inprocess" stdoutLogEnabled="false" />
```

| | **In-process** (default since 3.0) | **Out-of-process** |
|---|---|---|
| Where app runs | **Inside** the IIS worker process `w3wp.exe` | Separate `dotnet.exe` process running **Kestrel** |
| Server | `IISHttpServer` (not Kestrel) | Kestrel; IIS forwards requests over HTTP (loopback) |
| Performance | ✅ Faster (no extra hop) | Slower (proxy hop) |
| One app per app pool | ✅ Required | Not required |
| Analogy | Chef cooks **in** the restaurant kitchen | Restaurant forwards orders to a **separate catering kitchen** next door |

IIS checklist: install Hosting Bundle → create site + app pool (**No Managed Code**) → copy publish folder → set `ASPNETCORE_ENVIRONMENT` (in `web.config` `<environmentVariables>` or app pool) → for startup errors enable `stdoutLogEnabled="true"` temporarily (classic "HTTP 500.30 – ASP.NET Core app failed to start").

---

## 14.12 Kestrel behind Nginx / a reverse proxy + Forwarded Headers

**Why a reverse proxy?** TLS termination, load balancing, static files, compression, request buffering, hiding internal ports, one public port 443 for many apps.

```text
Client ──HTTPS:443──► Nginx / Load balancer ──HTTP:5000──► Kestrel (your app)
                       adds X-Forwarded-For: <client IP>
                            X-Forwarded-Proto: https
```

**Problem:** your app sees the request as coming from the **proxy's IP** over **HTTP**. So:
- `HttpContext.Connection.RemoteIpAddress` = proxy IP → rate limiting by IP and logs are wrong.
- `Request.Scheme` = `http` → `UseHttpsRedirection` loops, generated links / OAuth redirect URIs use `http://`.

**Fix: Forwarded Headers Middleware** (compile-verified):

```csharp
builder.Services.Configure<ForwardedHeadersOptions>(o =>
{
    o.ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto;
    o.KnownProxies.Add(System.Net.IPAddress.Parse("10.0.0.10"));   // only trust YOUR proxy
});

var app = builder.Build();
app.UseForwardedHeaders();        // FIRST, before anything that uses scheme/IP (HSTS, auth, rate limiter, logging)
```

⚠️ **Interview trap:** Trusting `X-Forwarded-For` from **anyone** lets attackers spoof their IP (bypass IP rate limits/allow-lists). Only accept them from known proxies/networks (`KnownProxies` / `KnownNetworks`). On IIS and Azure App Service this is handled for you (ANCM / platform sets `ASPNETCORE_FORWARDEDHEADERS_ENABLED=true` for Linux containers).

Minimal Nginx config (not executable here):

```nginx
server {
    listen 443 ssl;
    server_name shop.example.com;
    ssl_certificate     /etc/ssl/shop.crt;
    ssl_certificate_key /etc/ssl/shop.key;

    location / {
        proxy_pass         http://127.0.0.1:5000;
        proxy_http_version 1.1;
        proxy_set_header   Host              $host;
        proxy_set_header   X-Forwarded-For   $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Proto $scheme;
    }
}
```

Run the app as a **systemd** service on Linux so it restarts on crash/boot.

---

## 14.13 Docker — multi-stage Dockerfile

**Simple words:** A **container image** packages your app + runtime + OS libraries into one portable unit that runs the same everywhere. Analogy: a **shipping container** — the ship (server) doesn't care what's inside.

**Multi-stage build:** stage 1 uses the big **SDK** image to build/publish; stage 2 copies only the output into the small **runtime** image. The final image has no SDK, no source code → smaller and more secure.

```dockerfile
# ---------- Stage 1: build ----------
FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build
WORKDIR /src

# Copy only the project file first -> 'dotnet restore' layer is cached until dependencies change
COPY ShopApi/ShopApi.csproj ShopApi/
RUN dotnet restore ShopApi/ShopApi.csproj

# Now copy the rest and publish
COPY . .
RUN dotnet publish ShopApi/ShopApi.csproj -c Release -o /app/publish --no-restore

# ---------- Stage 2: runtime ----------
FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS final
WORKDIR /app
COPY --from=build /app/publish .

# .NET 8+ images listen on port 8080 by default and include a non-root 'app' user
ENV ASPNETCORE_ENVIRONMENT=Production
EXPOSE 8080
USER app

ENTRYPOINT ["dotnet", "ShopApi.dll"]
```

```bash
docker build -t shopapi:1.0 .
docker run -d -p 8080:8080 \
  -e ConnectionStrings__Shop="Data Source=/data/shop.db" \
  -e Jwt__Key="<from a secret store>" \
  -v shopdata:/data \
  shopapi:1.0
curl http://localhost:8080/health
```

| Best practice | Why |
|---|---|
| Multi-stage | Small runtime image (~100 MB vs ~800 MB SDK) |
| Copy `.csproj` → restore → copy rest | Layer caching: dependencies restore only when csproj changes |
| `.dockerignore` (`bin/`, `obj/`, `.git`, secrets) | Faster builds, no leaked files |
| **Non-root user** (`USER app`) | Limit damage if compromised |
| Port **8080** (.NET 8+ default, `ASPNETCORE_HTTP_PORTS`) | Non-root can't bind port 80 |
| Config via **env vars** (`Section__Key`) | Same image for every environment |
| Pin tags / use `chiseled` or Alpine images | Reproducible, smaller attack surface |
| TLS at the ingress/load balancer | The container speaks plain HTTP internally |
| Alternative: `dotnet publish /t:PublishContainer` (.NET 8+) | Builds an image **without** a Dockerfile |

*(Dockerfile and Nginx config are shown for learning and could not be executed in this book's verification environment.)*

---

## 14.14 Azure App Service & containers

| Option | What | When |
|---|---|---|
| **Azure App Service** (Web App) | PaaS: upload code or container; Azure manages OS, patching, scaling, TLS | Most common for ASP.NET Core APIs |
| **Azure Container Apps** | Serverless containers (built on Kubernetes), scale to zero, Dapr | Microservices without managing K8s |
| **Azure Kubernetes Service (AKS)** | Full Kubernetes | Large, complex systems; you have K8s skills |
| Azure Functions | Event-driven serverless | Small background jobs, triggers |
| VM + IIS/Nginx | IaaS | Legacy, full control |

App Service features to mention:
- **Configuration → Application settings** become **environment variables** (`Jwt__Key`, connection strings) — override `appsettings.json`; can reference Key Vault (`@Microsoft.KeyVault(SecretUri=...)`).
- **Managed Identity** for Key Vault/SQL access without secrets.
- **Deployment slots** (staging/production) with **swap** → zero downtime (next section).
- **Health check** setting: App Service pings your path (e.g. `/health/ready`) and removes unhealthy instances from the load balancer.
- Scale up (bigger machine) / scale out (more instances, autoscale rules) — remember **in-memory caches and Data Protection keys are per instance**.
- Logs: Log stream, Application Insights integration.

---

## 14.15 Environment-specific configuration

```text
appsettings.json                 ← defaults for all environments
appsettings.Development.json     ← local overrides (logging Debug, local DB)
appsettings.Staging.json
appsettings.Production.json      ← no secrets! only non-secret prod values
+ environment variables / Key Vault (secrets, per-deployment values)
```

The environment comes from **`ASPNETCORE_ENVIRONMENT`** (or `DOTNET_ENVIRONMENT`); default is **Production** if not set (safe default!).

```csharp
if (app.Environment.IsDevelopment()) { app.MapOpenApi(); }
else { app.UseHsts(); }
```

| Rule | Why |
|---|---|
| **Build once, deploy many** | The **same** artifact/image goes to Dev → Staging → Prod; only configuration changes |
| Secrets never in `appsettings.*.json` | Use Key Vault / env vars |
| Validate options at startup (`ValidateOnStart`, like the sample's `JwtOptions`) | Misconfigured deployment fails **immediately**, not on first request |
| Feature flags for risky features | Turn on/off without redeploying (Azure App Configuration / `Microsoft.FeatureManagement`) |

---

## 14.16 CI/CD pipeline outline

**CI (Continuous Integration):** every push/PR → build + test automatically. **CD (Continuous Delivery/Deployment):** every successful build → deploy to environments automatically (with approvals for production).

```text
 git push / PR
     │
     ▼
 ┌───────── CI ─────────┐     ┌──────────────── CD ────────────────┐
 │ restore → build →     │     │ deploy to Staging slot → smoke     │
 │ unit + integration    │ ──► │ tests / health check → approval →  │
 │ tests → coverage →    │     │ swap to Production → monitor       │
 │ publish artifact/image│     │ (rollback = swap back)             │
 └───────────────────────┘     └────────────────────────────────────┘
```

**Azure Pipelines YAML example** (outline, not executed here):

```yaml
trigger:
  branches: { include: [ main ] }

variables:
  buildConfiguration: Release

stages:
- stage: Build
  jobs:
  - job: BuildAndTest
    pool: { vmImage: ubuntu-latest }
    steps:
    - task: UseDotNet@2
      inputs: { packageType: sdk, version: 10.0.x }
    - script: dotnet restore
    - script: dotnet build --no-restore -c $(buildConfiguration)
    - script: dotnet test --no-build -c $(buildConfiguration) --collect:"XPlat Code Coverage"
    - task: PublishCodeCoverageResults@2
      inputs: { summaryFileLocation: '$(Agent.TempDirectory)/**/coverage.cobertura.xml' }
    - script: dotnet publish ShopApi/ShopApi.csproj --no-build -c $(buildConfiguration) -o $(Build.ArtifactStagingDirectory)/app
    - publish: $(Build.ArtifactStagingDirectory)/app
      artifact: app

- stage: DeployStaging
  dependsOn: Build
  jobs:
  - deployment: Staging
    environment: shop-staging
    strategy:
      runOnce:
        deploy:
          steps:
          - task: AzureWebApp@1
            inputs:
              azureSubscription: shop-service-connection
              appName: shop-api
              deployToSlotOrASE: true
              slotName: staging
              package: $(Pipeline.Workspace)/app

- stage: SwapToProduction
  dependsOn: DeployStaging
  jobs:
  - deployment: Production
    environment: shop-production          # approvals/checks configured on this environment
    strategy:
      runOnce:
        deploy:
          steps:
          - task: AzureAppServiceManage@0
            inputs:
              azureSubscription: shop-service-connection
              action: Swap Slots
              webAppName: shop-api
              sourceSlot: staging
```

GitHub Actions equivalent: `actions/setup-dotnet` → `dotnet test` → `azure/webapps-deploy`. Other common pipeline steps: dependency vulnerability scan (`dotnet list package --vulnerable`), static analysis, container image scan, database migrations (EF **migration bundles** or idempotent scripts — chapter 9).

---

## 14.17 Zero-downtime deployment & health checks in deployment

| Strategy | How | Rollback |
|---|---|---|
| **Deployment slots (App Service)** | Deploy to `staging` slot → warm up → **swap** (VIP swap, no restart of prod) | Swap back |
| **Blue/Green** | Two identical environments; switch the router | Switch back |
| **Rolling** (Kubernetes default) | Replace pods a few at a time; readiness probe gates traffic | `kubectl rollout undo` |
| **Canary** | Send 5% of traffic to the new version, watch metrics, increase | Route 100% back |
| Feature flags | Deploy code "dark", enable later | Turn flag off |

**Health checks make it safe** (chapter 11):

| Where | Uses |
|---|---|
| App Service **Health check** path | Removes unhealthy instances; slot swap warm-up (`applicationInitialization` / `WEBSITE_SWAP_WARMUP_PING_PATH`) |
| Kubernetes **readinessProbe** → `/health/ready` | New pod gets traffic only when DB etc. are reachable |
| Kubernetes **livenessProbe** → `/health/live` | Restart hung pods |
| Pipeline smoke test | `curl -f https://shop-api-staging.azurewebsites.net/health` before swapping |

```yaml
# Kubernetes container spec (excerpt)
readinessProbe:
  httpGet: { path: /health/ready, port: 8080 }
  periodSeconds: 10
livenessProbe:
  httpGet: { path: /health/live, port: 8080 }
  initialDelaySeconds: 15
```

Zero-downtime gotchas:
- **Database migrations must be backward compatible** — during a swap/rolling update, old and new code run **at the same time**. Use expand → migrate → contract (add the new column first, remove the old one in a later release).
- **Graceful shutdown**: honour `CancellationToken`s; `HostOptions.ShutdownTimeout` gives in-flight requests and `BackgroundService`s time to finish.
- **Shared state**: sessions, caches, Data Protection keys must be shared (Redis/Blob) or the swap logs everyone out.

🎯 **Say this in the interview:** "Our pipeline builds once, runs unit and integration tests, publishes one artifact, deploys it to a staging slot with environment-specific config from App Settings and Key Vault, runs smoke tests against the health endpoint, then swaps slots for zero downtime — rollback is just swapping back. DB migrations are kept backward compatible because old and new versions overlap during the swap."

---

## Try it yourself

1. Is this safe? `db.Users.FromSqlRaw($"SELECT * FROM Users WHERE Email = '{email}'")` — fix it two ways.
2. A `PUT /api/users/{id}` endpoint binds the `User` entity directly. Name two attacks it enables and the fix.
3. Your React SPA stores a JWT in memory and sends it as `Authorization: Bearer`. Do you need antiforgery tokens? What changes if you move the JWT into a cookie?
4. Behind Nginx, your logs show every request coming from `127.0.0.1` and `UseHttpsRedirection` causes an infinite redirect loop. Why and how do you fix it?
5. After scaling your App Service from 1 to 3 instances, users are randomly logged out (cookie auth). What's the cause?
6. Explain framework-dependent vs self-contained publishing, and when you'd choose Native AOT.

<details><summary>Answer</summary>

1. **Not safe** — `FromSqlRaw` receives an already-concatenated string (SQL injection; the EF1003 analyzer warns). Fixes: `db.Users.FromSql($"SELECT * FROM Users WHERE Email = {email}")` (no quotes — it becomes a parameter), or `FromSqlRaw("SELECT * FROM Users WHERE Email = {0}", email)`, or just LINQ `db.Users.Where(u => u.Email == email)`.
2. **Over-posting / mass assignment** (client sets `IsAdmin`, `PasswordHash`, `Balance`...) and, if there's no ownership check, **IDOR** (edit other users by changing `{id}`). Fix: a DTO with only editable fields + map manually; check the caller owns `{id}` (or is admin) via resource-based authorization. Also return DTOs to avoid exposing sensitive fields.
3. No — CSRF relies on the browser auto-sending credentials; a header added by your JS isn't sent by `evil.com`. If the JWT moves into a cookie, the browser sends it automatically → CSRF risk returns → use `SameSite` + antiforgery tokens.
4. Nginx connects to Kestrel over HTTP from localhost, so the app sees the proxy IP and `http` scheme. Configure `ForwardedHeadersOptions` (`XForwardedFor | XForwardedProto`), call `app.UseForwardedHeaders()` first, make Nginx send the `X-Forwarded-*` headers, and trust only known proxies.
5. **Data Protection keys** are stored per instance by default, so a cookie encrypted on instance A can't be decrypted on B. Persist keys to a shared store (Blob Storage/Redis/DB) with the same application name (and protect them with Key Vault).
6. Framework-dependent: ships only your DLLs, needs the .NET runtime on the host (small, host patches runtime). Self-contained: bundles the runtime for a specific RID, runs on machines without .NET (bigger, you patch it). Native AOT: compiles to native code — very fast startup, low memory, small image — good for minimal-API microservices/serverless/containers, but limited reflection/dynamic code (EF Core and MVC have limited support).
</details>

---

## Quick revision

- OWASP API #1 = **BOLA/IDOR**: check ownership on every id (filter by user or resource-based authz).
- **SQL injection**: LINQ / `FromSql($"...")` / `FromSqlRaw("... {0}", x)` are safe; `FromSqlRaw` with a built string is **not** (EF1003 warns).
- **XSS**: Razor encodes by default; avoid `Html.Raw`; CSP + `nosniff`; `HttpOnly` cookies.
- **CSRF** hits **cookie** auth: antiforgery + SameSite. JWT in the `Authorization` header is not auto-sent → low CSRF risk.
- **Over-posting**: never bind entities — use **DTOs** (in and out).
- `UseHttpsRedirection` + `UseHsts` (not in Development); security headers: nosniff, frame-options, CSP, referrer-policy.
- Secrets: user-secrets locally, **Key Vault + Managed Identity** in Azure; rotate leaked secrets.
- **Data Protection** = encrypt cookies/tokens; share keys across instances.
- Publish: **framework-dependent** (small) vs **self-contained** (bundles runtime); single-file, trimming, **Native AOT** (fast startup, reflection limits).
- IIS: **ANCM**, **in-process** (w3wp, faster, default) vs **out-of-process** (Kestrel behind IIS).
- Behind Nginx/proxies: **`UseForwardedHeaders()` first**, trust only known proxies.
- Docker: **multi-stage** (SDK → aspnet runtime), port **8080**, non-root, config via env vars.
- CI/CD: build → test → publish once → deploy to staging slot → smoke test `/health` → **swap**; migrations backward compatible.

---

# 15. Architecture & Design Patterns

> **Why interviewers ask this**
> At 2 years of experience you are no longer judged only on "can you write a controller?". Interviewers want to know whether you can write code that **other people can change safely** six months later. SOLID, Clean Architecture, Repository/Unit of Work, CQRS and a handful of design patterns are the shared vocabulary for that. You don't need to be an architect — you need to explain **what problem each idea solves**, show a tiny example, and say **when NOT to use it**.

---

## 15.1 The big picture in simple words

Think of building software like building a **house**:

| House | Software |
|---|---|
| Rooms have one purpose (kitchen, bedroom) | **Single Responsibility** — each class has one job |
| You can add a new room without breaking the walls | **Open/Closed** — add features without editing old code |
| Sockets are a standard; any plug fits | **Interfaces / Dependency Inversion** |
| Foundation → walls → roof (layers) | **Layered / Clean Architecture** |
| Proven blueprints for stairs, doors, windows | **Design patterns** |

A **principle** (SOLID, DRY, KISS) is a *guideline* for writing good code.
A **pattern** (Strategy, Decorator, Repository) is a *named, reusable solution* to a common problem.
An **architecture** (N-tier, Clean, Microservices) is the *shape of the whole application*.

🎯 **Say this in the interview:** "Principles tell me *how to think*, patterns give me *proven solutions*, and architecture decides *how the big pieces depend on each other*. I apply them only when they reduce the cost of change — not by default."

---

## 15.2 SOLID — one principle at a time

SOLID is five principles collected by Robert C. Martin ("Uncle Bob"):

| Letter | Name | One-line meaning |
|---|---|---|
| **S** | Single Responsibility | A class should have **one reason to change** |
| **O** | Open/Closed | Open for **extension**, closed for **modification** |
| **L** | Liskov Substitution | A child class must be usable **wherever the parent is expected** |
| **I** | Interface Segregation | Many **small, focused interfaces** beat one fat interface |
| **D** | Dependency Inversion | Depend on **abstractions**, not concrete classes |

### S — Single Responsibility Principle (SRP)

**Simple words:** A class should do one job. If your manager, your DBA *and* your marketing team could each ask you to change the same class for different reasons — it has too many jobs.

**Analogy:** A Swiss-army knife is handy, but a chef uses a separate knife for bread, meat and vegetables. When the bread knife gets blunt, the meat knife is not affected.

❌ **Bad** — one class validates, saves, emails and logs:

```csharp
public class OrderService
{
    public void PlaceOrder(string customerEmail, decimal amount)
    {
        if (amount <= 0) throw new ArgumentException("Amount must be positive");   // validation
        // ... raw SQL INSERT here ...                                               // persistence
        Console.WriteLine($"Sending email to {customerEmail}...");                   // email
        File.AppendAllText("log.txt", $"Order placed {amount}\n");                   // logging
    }
}
```

Four reasons to change: validation rules, database, email provider, logging format.

✅ **Good** — each job lives in its own class; `OrderService` only **coordinates**:

```csharp
public record Order(string CustomerEmail, decimal Amount);

public interface IOrderValidator { void Validate(Order order); }
public interface IOrderRepository { Task SaveAsync(Order order); }
public interface IEmailSender { Task SendAsync(string to, string subject); }

public class OrderValidator : IOrderValidator
{
    public void Validate(Order order)
    {
        if (order.Amount <= 0) throw new ArgumentException("Amount must be positive");
    }
}

public class OrderService(IOrderValidator validator, IOrderRepository repo, IEmailSender email,
    ILogger<OrderService> logger)
{
    public async Task PlaceOrderAsync(Order order)
    {
        validator.Validate(order);
        await repo.SaveAsync(order);
        await email.SendAsync(order.CustomerEmail, "Order confirmed");
        logger.LogInformation("Order placed for {Amount}", order.Amount);
    }
}
```

⚠️ **Interview trap:** SRP does **not** mean "a class should have one method". It means *one reason to change*. `ProductService` in the sample app has 5 methods but one responsibility: product use-cases.

### O — Open/Closed Principle (OCP)

**Simple words:** When a new requirement arrives, you should be able to **add new code** instead of **editing old, tested code**.

**Analogy:** A power strip. You add a new device by plugging it in — you don't rewire the strip.

❌ **Bad** — every new customer type means editing this `switch` (and retesting everything):

```csharp
public class DiscountCalculator
{
    public decimal Calculate(string customerType, decimal amount) => customerType switch
    {
        "Regular"  => amount * 0.05m,
        "Gold"     => amount * 0.10m,
        "Platinum" => amount * 0.20m,
        _ => 0m
    };
}
```

✅ **Good** — each rule is a class; a new rule = a new class + one DI line:

```csharp
public interface IDiscountRule
{
    string CustomerType { get; }
    decimal Calculate(decimal amount);
}

public class RegularDiscount : IDiscountRule
{
    public string CustomerType => "Regular";
    public decimal Calculate(decimal amount) => amount * 0.05m;
}

public class GoldDiscount : IDiscountRule
{
    public string CustomerType => "Gold";
    public decimal Calculate(decimal amount) => amount * 0.10m;
}

// NEW requirement: students get 15%, max 50. Zero edits to existing classes.
public class StudentDiscount : IDiscountRule
{
    public string CustomerType => "Student";
    public decimal Calculate(decimal amount) => Math.Min(amount * 0.15m, 50m);
}

public class DiscountCalculator(IEnumerable<IDiscountRule> rules)   // DI injects ALL registered rules
{
    public decimal Calculate(string customerType, decimal amount) =>
        rules.FirstOrDefault(r => r.CustomerType == customerType)?.Calculate(amount) ?? 0m;
}

// Program.cs
builder.Services.AddSingleton<IDiscountRule, RegularDiscount>();
builder.Services.AddSingleton<IDiscountRule, GoldDiscount>();
builder.Services.AddSingleton<IDiscountRule, StudentDiscount>();
builder.Services.AddSingleton<DiscountCalculator>();
```

> 💡 Registering the same interface many times and injecting `IEnumerable<T>` is a very common ASP.NET Core trick — it is how the framework itself handles multiple `IHostedService`s and filters.

### L — Liskov Substitution Principle (LSP)

**Simple words:** If code works with a `Parent`, it must keep working when you give it a `Child`. A child must not **surprise** callers (throw "not supported", ignore inputs, tighten rules).

**Analogy:** If a rental company promises "any car", you expect every car to have brakes. A "car" without brakes breaks the promise even if it *is* technically a car.

❌ **Bad** — `ReadOnlyStorage` *is a* `FileStorage` but blows up on `Write`:

```csharp
public class FileStorage
{
    public virtual string Read(string path) => File.ReadAllText(path);
    public virtual void Write(string path, string content) => File.WriteAllText(path, content);
}

public class ReadOnlyStorage : FileStorage
{
    public override void Write(string path, string content) =>
        throw new NotSupportedException("This storage is read-only!");   // surprise!
}
```

Any method that accepts `FileStorage` and calls `Write` will crash at runtime when handed a `ReadOnlyStorage`.

✅ **Good** — model the real capabilities, so the compiler protects you:

```csharp
public interface IReadableStorage { string Read(string path); }
public interface IWritableStorage : IReadableStorage { void Write(string path, string content); }

public class LocalStorage : IWritableStorage
{
    public string Read(string path) => File.ReadAllText(path);
    public void Write(string path, string content) => File.WriteAllText(path, content);
}

public class ArchiveStorage : IReadableStorage          // can't be passed where writing is needed
{
    public string Read(string path) => File.ReadAllText(path);
}
```

⚠️ **Interview trap:** The classic textbook example is *Square inherits Rectangle* (setting Width on a square also changes Height, which surprises callers). Know it by name, but give a **practical** example like the one above — it shows real experience.

### I — Interface Segregation Principle (ISP)

**Simple words:** Don't force a class to depend on methods it doesn't use.

**Analogy:** A TV remote with 80 buttons when grandma only needs power, volume and channel.

❌ **Bad** — one fat interface; a read-only page now "depends" on Excel export and CSV import:

```csharp
public interface IProductStore
{
    Task<object?> GetAsync(int id);
    Task AddAsync(object product);
    Task DeleteAsync(int id);
    Task<byte[]> ExportToExcelAsync();
    Task ImportFromCsvAsync(Stream csv);
}
```

✅ **Good** — small role interfaces; consumers ask only for what they need:

```csharp
public record Product(int Id, string Name);
public interface IProductReader   { Task<Product?> GetAsync(int id); }
public interface IProductWriter   { Task AddAsync(Product p); Task DeleteAsync(int id); }
public interface IProductExporter { Task<byte[]> ExportToExcelAsync(); }

public class CatalogPageService(IProductReader reader)     // only needs reading
{
    public Task<Product?> ShowAsync(int id) => reader.GetAsync(id);
}
```

Bonus: unit tests are simpler — you mock one 1-method interface, not a 5-method monster.

### D — Dependency Inversion Principle (DIP)

**Simple words:** High-level business code should not `new` up low-level details (SMTP, SQL, HTTP). Both should depend on an **interface**. Dependency **Injection** (chapter 4) is the *technique* that delivers dependency **inversion** (the *principle*).

**Analogy:** A lamp has a plug (interface). It doesn't care whether the electricity comes from solar, coal or a generator.

❌ **Bad** — hard-wired to SMTP; impossible to unit-test without sending real emails:

```csharp
public class RegistrationService
{
    private readonly SmtpEmailSender _sender = new();          // concrete + 'new' = tight coupling
    public void Register(string email) => _sender.Send(email, "Welcome!");
}
```

✅ **Good** — depend on an abstraction; swap implementations via DI:

```csharp
public interface IEmailSender { Task SendAsync(string to, string body); }

public class SmtpEmailSender : IEmailSender
{
    public Task SendAsync(string to, string body) { /* real SMTP */ return Task.CompletedTask; }
}

public class FakeEmailSender : IEmailSender                  // used in tests
{
    public List<string> Sent { get; } = [];
    public Task SendAsync(string to, string body) { Sent.Add(to); return Task.CompletedTask; }
}

public class RegistrationService(IEmailSender sender)
{
    public Task RegisterAsync(string email) => sender.SendAsync(email, "Welcome!");
}
```

| Term | What it is |
|---|---|
| **Dependency Inversion** | The *principle*: depend on abstractions |
| **Inversion of Control** | The *idea*: the framework creates/calls your objects, not you |
| **Dependency Injection** | The *technique*: dependencies are passed in (usually via constructor) |
| **DI container** | The *tool*: `IServiceCollection` / `IServiceProvider` in ASP.NET Core |

🎯 **Say this in the interview:** "In my projects SOLID shows up mostly as small services behind interfaces, injected through the constructor. SRP keeps controllers thin, OCP lets me add a new payment or discount type as a new class, and DIP plus DI make everything unit-testable with mocks."

---

## 15.3 DRY, KISS, YAGNI

| Principle | Meaning | Example | Over-use danger |
|---|---|---|---|
| **DRY** – Don't Repeat Yourself | One piece of knowledge lives in one place | Validation rules in one validator, not copied into 3 controllers | Merging two things that only *look* similar creates wrong coupling |
| **KISS** – Keep It Simple, Stupid | Prefer the simplest thing that works | A `switch` with 3 cases is fine; no need for a plugin system | "Simple" ≠ "hacky" |
| **YAGNI** – You Aren't Gonna Need It | Don't build for imaginary future requirements | Don't add microservices, CQRS and event sourcing to a 5-table CRUD app | Ignoring *known* upcoming needs |

⚠️ **Interview trap:** OCP and YAGNI can look contradictory. The honest answer: "I start simple (KISS/YAGNI). When the **second or third** variation appears — e.g. a third payment method — I refactor to a Strategy (OCP). That's the *rule of three*."

---

## 15.4 Layered (N-tier) architecture

**Simple words:** Split the app into horizontal layers, each talking only to the layer below.

```
┌───────────────────────────┐
│  Presentation (API/MVC)   │  Controllers, DTOs, filters
├───────────────────────────┤
│  Business Logic (BLL)     │  Services, rules
├───────────────────────────┤
│  Data Access (DAL)        │  DbContext, repositories, SQL
├───────────────────────────┤
│        Database           │
└───────────────────────────┘
      dependencies point DOWN ↓
```

- ✅ Easy to understand, very common in companies.
- ❌ The **business layer depends on the data layer**, so your core rules are tied to EF Core/SQL. Changing the database touches business code; unit-testing business rules often requires the DAL.

The sample app (samples/ShopApi) is a **simple layered app inside one project**: `Controllers` → `Services` → `Data` (EF Core). For a small API that is perfectly fine (KISS).

---

## 15.5 Clean Architecture (Onion / Hexagonal)

**Simple words:** Put the **business rules in the centre** and make everything else (database, web, email, files) a **plug-in on the outside**. Dependencies always point **inwards**.

**Analogy:** An onion. The core (domain) knows nothing about the outer skins. You can peel off and replace the outer skin (swap SQL Server for PostgreSQL, REST for gRPC) without touching the core.

```
            ┌─────────────────────────────────────────────┐
            │  Presentation  (ShopApi: controllers,       │
            │                 Program.cs, middleware)      │
            │   ┌─────────────────────────────────────┐   │
            │   │ Infrastructure (EF Core, email,      │   │
            │   │  blob storage, HTTP clients)         │   │
            │   │   ┌─────────────────────────────┐   │   │
            │   │   │ Application (use cases,     │   │   │
            │   │   │  interfaces, DTOs, CQRS)    │   │   │
            │   │   │   ┌─────────────────────┐   │   │   │
            │   │   │   │ Domain (entities,   │   │   │   │
            │   │   │   │  value objects,     │   │   │   │
            │   │   │   │  business rules)    │   │   │   │
            │   │   │   └─────────────────────┘   │   │   │
            │   │   └─────────────────────────────┘   │   │
            │   └─────────────────────────────────────┘   │
            └─────────────────────────────────────────────┘

  THE DEPENDENCY RULE: source code dependencies point INWARD only.
  Domain references nothing. Application references Domain.
  Infrastructure references Application (implements its interfaces).
  Api references Application + Infrastructure (only to wire up DI).
```

### Typical folder / project structure

```
Shop.sln
├── src/
│   ├── Shop.Domain/                 // no NuGet packages at all (ideally)
│   │   ├── Entities/Product.cs, Order.cs
│   │   ├── ValueObjects/Money.cs
│   │   └── Exceptions/OutOfStockException.cs
│   ├── Shop.Application/            // references Domain
│   │   ├── Abstractions/IAppDbContext.cs, IEmailSender.cs
│   │   ├── Products/Queries/GetProductById/...
│   │   ├── Products/Commands/CreateProduct/...
│   │   └── DependencyInjection.cs   // AddApplication()
│   ├── Shop.Infrastructure/         // references Application; EF Core, SMTP, Azure SDKs live here
│   │   ├── Persistence/AppDbContext.cs, Migrations/
│   │   ├── Email/SmtpEmailSender.cs
│   │   └── DependencyInjection.cs   // AddInfrastructure(config)
│   └── Shop.Api/                    // references Application + Infrastructure
│       ├── Controllers/ or Endpoints/
│       ├── Middleware/
│       └── Program.cs
└── tests/
    ├── Shop.Application.UnitTests/
    └── Shop.Api.IntegrationTests/
```

The key trick is **Dependency Inversion across projects**: `Application` declares `IEmailSender`; `Infrastructure` implements it. At runtime DI glues them together:

```csharp
// Shop.Api/Program.cs
builder.Services
    .AddApplication()                         // handlers, validators
    .AddInfrastructure(builder.Configuration); // DbContext, SmtpEmailSender : IEmailSender
```

| | Layered (N-tier) | Clean / Onion |
|---|---|---|
| Centre of the app | Database | Domain / business rules |
| Business depends on DB? | **Yes** | **No** — DB depends on business (implements its interfaces) |
| Unit-testing rules | Harder | Easy (no infrastructure in core) |
| Number of projects | 1–3 | Usually 4+ |
| Learning curve / ceremony | Low | Medium–high |
| Best for | Small/medium CRUD apps | Long-lived apps with real business logic |

⚠️ **Interview trap:** "Clean Architecture means 4 projects." No — it means **the dependency rule**. You can follow it with folders in one project. Conversely, 4 projects where the Domain references EF Core is *not* clean.

🎯 **Say this in the interview:** "Clean Architecture puts business rules at the centre and makes the database and web framework details. The one rule is that dependencies point inwards — the Application layer defines interfaces like `IEmailSender` and Infrastructure implements them. It costs some ceremony, so for a simple CRUD service I'd use a simple layered structure, and move to Clean when the business logic grows."

**Vertical Slice Architecture** (worth one sentence): instead of layering by technical type, you group code by **feature** (`Features/Products/CreateProduct.cs` contains endpoint + handler + validator). It pairs naturally with CQRS and is popular in modern .NET teams.

---

## 15.6 Repository pattern + Unit of Work

### In simple words

- **Repository** = a **collection-like** object that hides *how* data is stored. `products.GetByIdAsync(5)` — you don't care if it's SQL, Mongo or an API.
- **Unit of Work (UoW)** = a **shopping cart** for changes. You put several changes in, then **commit them all at once** (one transaction) — or none.

**Analogy:** A librarian (repository) fetches books so you don't walk into the store room. The checkout counter (unit of work) records all the books you're borrowing in **one** transaction.

### Easy example — a generic repository

```csharp
public interface IRepository<T> where T : class
{
    Task<T?> GetByIdAsync(int id, CancellationToken ct = default);
    Task<List<T>> ListAsync(CancellationToken ct = default);
    void Add(T entity);
    void Remove(T entity);
}

public class EfRepository<T>(ShopDbContext db) : IRepository<T> where T : class
{
    public async Task<T?> GetByIdAsync(int id, CancellationToken ct = default) => await db.Set<T>().FindAsync([id], ct);
    public Task<List<T>> ListAsync(CancellationToken ct = default) => db.Set<T>().ToListAsync(ct);
    public void Add(T entity) => db.Set<T>().Add(entity);
    public void Remove(T entity) => db.Set<T>().Remove(entity);
}
```

Notice: `Add` and `Remove` **don't save**. Saving is the Unit of Work's job.

### Complex example — Unit of Work coordinating two repositories

```csharp
public interface IUnitOfWork
{
    IRepository<Product> Products { get; }
    IRepository<Order> Orders { get; }
    Task<int> SaveChangesAsync(CancellationToken ct = default);
}

public class UnitOfWork(ShopDbContext db) : IUnitOfWork
{
    public IRepository<Product> Products { get; } = new EfRepository<Product>(db);
    public IRepository<Order> Orders { get; } = new EfRepository<Order>(db);
    public Task<int> SaveChangesAsync(CancellationToken ct = default) => db.SaveChangesAsync(ct);
}

public class CheckoutService(IUnitOfWork uow)
{
    public async Task<int> PlaceOrderAsync(int productId, int qty, CancellationToken ct)
    {
        Product product = await uow.Products.GetByIdAsync(productId, ct)
            ?? throw new KeyNotFoundException($"Product {productId} not found");

        if (product.Stock < qty) throw new InvalidOperationException("Not enough stock");

        product.Stock -= qty;                                     // change 1 (tracked)
        var order = new Order { ProductId = productId, Quantity = qty };
        uow.Orders.Add(order);                                    // change 2

        await uow.SaveChangesAsync(ct);                           // ONE transaction: both or neither
        return order.Id;
    }
}

// Program.cs
builder.Services.AddScoped<IUnitOfWork, UnitOfWork>();   // scoped = same DbContext for the whole request
```

### The big debate: "DbContext already IS a Unit of Work and DbSet IS a repository"

This is a **favourite interview question**. Both sides are valid — show you know them:

| Argument | Against adding repositories over EF Core | For adding repositories |
|---|---|---|
| Abstraction | `DbContext` = UoW, `DbSet<T>` = repository already | Hides EF Core from business code (Clean Architecture) |
| Features | Generic repos often hide `Include`, projection, `AsNoTracking`, `AsSplitQuery` → slower queries | **Specific** repos (`IOrderRepository.GetWithItemsAsync`) can still use them inside |
| Testing | Use EF Core InMemory/SQLite or Testcontainers for real behaviour | Mock `IOrderRepository` easily in unit tests |
| Code | Extra boilerplate for little value in CRUD apps | Centralises complex/reused queries |
| Swapping DB | Rarely happens in real life | Matters if you truly have multiple stores |

⚠️ **Interview trap:** A *generic* repository that returns `IQueryable<T>` gives you **all the coupling and none of the abstraction** — callers still write EF-specific LINQ. If you add repositories, make them **specific to an aggregate** with intention-revealing methods.

🎯 **Say this in the interview:** "EF Core's `DbContext` already implements Unit of Work and `DbSet` is a repository, so for simple services I inject the DbContext into a service — like `ProductService` in our sample app. I add a repository when I want to keep EF out of my domain layer, when a complex query is reused in many places, or when I need to mock data access in unit tests. I avoid generic repositories that leak `IQueryable`."

---

## 15.7 CQRS — Command Query Responsibility Segregation

### In simple words

Split your operations into two kinds:

- **Commands** – *change* something, return little or nothing (`CreateProduct`, `PlaceOrder`).
- **Queries** – *read* something, **never** change state (`GetProductById`, `SearchProducts`).

**Analogy:** A restaurant has a **kitchen** (commands: cook, change state) and a **menu board** (queries: just read). The menu board can be optimised for fast reading — it doesn't need to know how the kitchen works.

### Levels of CQRS

| Level | What it means | When |
|---|---|---|
| 1. Code-level | Separate classes for commands and queries, **same database** | Most .NET apps that "use CQRS" |
| 2. Separate models | Queries use `AsNoTracking` + projections/Dapper/views; commands use rich EF entities | Read-heavy apps |
| 3. Separate stores | Writes go to SQL; reads from a denormalised read DB / search index, synced by events | Large scale; brings **eventual consistency** |

Start at level 1. Most interviews only need level 1–2.

### MediatR — and the 2025 licensing change

**MediatR** was the most popular library to dispatch commands/queries to handlers (`await mediator.Send(new GetProductByIdQuery(5))`). In **2025 MediatR (and AutoMapper) moved to a commercial license** for newer versions (free tiers exist for small companies/non-commercial use, older versions remain under the old license). As a result many teams now:

- stay on the last free version, **or**
- pay for the license, **or**
- write their **own tiny dispatcher** (it's ~20 lines!), **or**
- simply inject handlers directly / use plain services.

🎯 Knowing about this change shows you follow the ecosystem. Don't claim MediatR "is CQRS" — it's just a dispatcher; CQRS is the idea of separating reads from writes.

### Complex example — a tiny home-made dispatcher

```csharp
// 1. Contracts
public interface IRequest<TResponse>;

public interface IRequestHandler<TRequest, TResponse> where TRequest : IRequest<TResponse>
{
    Task<TResponse> HandleAsync(TRequest request, CancellationToken ct);
}

public interface IDispatcher
{
    Task<TResponse> SendAsync<TResponse>(IRequest<TResponse> request, CancellationToken ct = default);
}

// 2. The dispatcher: finds the handler in DI and calls it
public class Dispatcher(IServiceProvider sp) : IDispatcher
{
    public Task<TResponse> SendAsync<TResponse>(IRequest<TResponse> request, CancellationToken ct = default)
    {
        Type handlerType = typeof(IRequestHandler<,>).MakeGenericType(request.GetType(), typeof(TResponse));
        dynamic handler = sp.GetRequiredService(handlerType);
        return handler.HandleAsync((dynamic)request, ct);
    }
}

// 3. One query, one command
public record GetProductByIdQuery(int Id) : IRequest<ProductDto?>;
public record CreateProductCommand(string Name, decimal Price) : IRequest<int>;

public class GetProductByIdHandler(ShopDbContext db) : IRequestHandler<GetProductByIdQuery, ProductDto?>
{
    public Task<ProductDto?> HandleAsync(GetProductByIdQuery q, CancellationToken ct) =>
        db.Products.AsNoTracking()                                  // queries: no tracking + projection
          .Where(p => p.Id == q.Id)
          .Select(p => new ProductDto(p.Id, p.Name, p.Price, p.Stock, "n/a"))
          .FirstOrDefaultAsync(ct);
}

public class CreateProductHandler(ShopDbContext db) : IRequestHandler<CreateProductCommand, int>
{
    public async Task<int> HandleAsync(CreateProductCommand c, CancellationToken ct)
    {
        var p = new Product { Name = c.Name, Price = c.Price };
        db.Products.Add(p);
        await db.SaveChangesAsync(ct);
        return p.Id;
    }
}

// 4. Registration + endpoints
builder.Services.AddScoped<IDispatcher, Dispatcher>();
builder.Services.AddScoped<IRequestHandler<GetProductByIdQuery, ProductDto?>, GetProductByIdHandler>();
builder.Services.AddScoped<IRequestHandler<CreateProductCommand, int>, CreateProductHandler>();

app.MapGet("/cqrs/products/{id:int}", async (int id, IDispatcher d, CancellationToken ct) =>
    await d.SendAsync(new GetProductByIdQuery(id), ct) is { } dto ? Results.Ok(dto) : Results.NotFound());

app.MapPost("/cqrs/products", async (CreateProductCommand cmd, IDispatcher d, CancellationToken ct) =>
{
    int id = await d.SendAsync(cmd, ct);
    return Results.Created($"/cqrs/products/{id}", new { id });
});
```

> 💡 Real dispatchers add **pipeline behaviours** (cross-cutting steps like validation, logging, transactions around every handler) — that is just the **Decorator** pattern (§15.10) applied to handlers. `dynamic` keeps this demo short; production versions use cached generic wrappers to avoid reflection cost.

| CQRS pros | CQRS cons |
|---|---|
| Small, focused handlers (SRP) | More files/classes |
| Reads can be optimised independently | Overkill for simple CRUD |
| Easy to add cross-cutting behaviours | Level 3 brings eventual consistency |

---

## 15.8 Mediator pattern

**Simple words:** Objects don't talk to each other directly; they talk through a **middle-man**.

**Analogy:** An airport **control tower**. Planes don't radio each other — they all talk to the tower.

The dispatcher above *is* a mediator: the endpoint doesn't know which handler class exists — it just sends a message. Benefits: low coupling, one place for cross-cutting behaviour. Cost: "go to definition" no longer jumps straight to the code (indirection).

---

## 15.9 Strategy and Factory

### Strategy — swap an algorithm at runtime

**Simple words:** Several interchangeable ways to do the same job, chosen at runtime.
**Analogy:** Google Maps: *car*, *bike*, *walk* — same goal (route), different algorithm.

```csharp
public interface IPaymentStrategy
{
    Task<string> PayAsync(decimal amount);
}

public class CardPayment : IPaymentStrategy
{
    public Task<string> PayAsync(decimal amount) => Task.FromResult($"Paid {amount} by card");
}

public class UpiPayment : IPaymentStrategy
{
    public Task<string> PayAsync(decimal amount) => Task.FromResult($"Paid {amount} by UPI");
}

public class PayPalPayment : IPaymentStrategy
{
    public Task<string> PayAsync(decimal amount) => Task.FromResult($"Paid {amount} by PayPal");
}
```

### Factory — centralise "which object do I create?"

**Simple words:** A class/method whose only job is **creating the right object**, so callers don't need `new` and `if/else` everywhere.

**Easy version** — classic factory:

```csharp
public interface IPaymentStrategyFactory { IPaymentStrategy Create(string method); }

public class PaymentStrategyFactory : IPaymentStrategyFactory
{
    public IPaymentStrategy Create(string method) => method.ToLowerInvariant() switch
    {
        "card"   => new CardPayment(),
        "upi"    => new UpiPayment(),
        "paypal" => new PayPalPayment(),
        _ => throw new ArgumentException($"Unsupported payment method '{method}'")
    };
}
```

**Modern version (.NET 8+)** — let the DI container be the factory with **keyed services**:

```csharp
// Program.cs
builder.Services.AddKeyedScoped<IPaymentStrategy, CardPayment>("card");
builder.Services.AddKeyedScoped<IPaymentStrategy, UpiPayment>("upi");
builder.Services.AddKeyedScoped<IPaymentStrategy, PayPalPayment>("paypal");

// Pick at runtime (key comes from the request)
public class CheckoutService(IServiceProvider sp)
{
    public Task<string> CheckoutAsync(string method, decimal amount)
    {
        IPaymentStrategy strategy = sp.GetKeyedService<IPaymentStrategy>(method.ToLowerInvariant())
            ?? throw new ArgumentException($"Unsupported payment method '{method}'");
        return strategy.PayAsync(amount);
    }
}

// Or pick at compile time
app.MapPost("/pay/card", ([FromKeyedServices("card")] IPaymentStrategy card) => card.PayAsync(10m));
```

Other "factory" examples you already use: `IHttpClientFactory`, `ILoggerFactory`, `IDbContextFactory<T>`, `IServiceScopeFactory`.

| Pattern | Question it answers |
|---|---|
| **Strategy** | *How* should this be done? (behaviour) |
| **Factory** | *Which object* should I create? (creation) |

They are usually used **together**: the factory picks the strategy.

---

## 15.10 Decorator — add behaviour without touching the class

**Simple words:** Wrap an object in another object that has the **same interface**, adds something (caching, logging, retry), and forwards the call.

**Analogy:** Putting a phone in a protective case. It's still a phone (same "interface"), but now it also survives drops. You can stack cases.

### Complex example — caching decorator for `IProductService`

The sample app's `ProductService` talks to the database. We want caching for `GetByIdAsync` **without editing** `ProductService` (OCP + SRP):

```csharp
public class CachedProductService(IProductService inner, IMemoryCache cache) : IProductService
{
    private static string Key(int id) => $"product:{id}";

    public async Task<ProductDto?> GetByIdAsync(int id, CancellationToken ct) =>
        await cache.GetOrCreateAsync(Key(id), entry =>
        {
            entry.AbsoluteExpirationRelativeToNow = TimeSpan.FromMinutes(5);
            return inner.GetByIdAsync(id, ct);                 // cache miss -> real service
        });

    public Task<PagedResult<ProductDto>> GetPageAsync(int page, int pageSize, string? search, CancellationToken ct) =>
        inner.GetPageAsync(page, pageSize, search, ct);        // just forward

    public Task<ProductDto> CreateAsync(CreateProductRequest request, CancellationToken ct) =>
        inner.CreateAsync(request, ct);

    public async Task<bool> UpdateAsync(int id, UpdateProductRequest request, CancellationToken ct)
    {
        bool ok = await inner.UpdateAsync(id, request, ct);
        cache.Remove(Key(id));                                 // invalidate on write!
        return ok;
    }

    public async Task<bool> DeleteAsync(int id, CancellationToken ct)
    {
        bool ok = await inner.DeleteAsync(id, ct);
        cache.Remove(Key(id));
        return ok;
    }
}
```

Registration — the built-in container has no `Decorate()` method, so wire it manually (or use the **Scrutor** package: `services.Decorate<IProductService, CachedProductService>()`):

```csharp
builder.Services.AddMemoryCache();
builder.Services.AddScoped<ProductService>();                       // the real one, by concrete type
builder.Services.AddScoped<IProductService>(sp => new CachedProductService(
    sp.GetRequiredService<ProductService>(),
    sp.GetRequiredService<IMemoryCache>()));
```

`ProductsController` still asks for `IProductService` and has **no idea** caching exists. 🎉

⚠️ **Interview trap:** Forgetting **cache invalidation** on update/delete → users see stale data. Also: decorators are the idea behind **middleware**, **filters**, `DelegatingHandler` in `HttpClient`, and MediatR pipeline behaviours — mention this to show you see the pattern everywhere.

---

## 15.11 Options, Builder, Singleton, Adapter

### Options pattern

**Simple words:** Bind a config section to a **strongly-typed class** and inject it (deep dive in chapter 5).

```csharp
public class SmtpOptions
{
    public const string SectionName = "Smtp";
    public string Host { get; set; } = "";
    public int Port { get; set; } = 587;
}

builder.Services.Configure<SmtpOptions>(builder.Configuration.GetSection(SmtpOptions.SectionName));

public class EmailSender(IOptions<SmtpOptions> options)
{
    private readonly SmtpOptions _smtp = options.Value;
}
```

The sample app does the production-grade version with `AddOptions<JwtOptions>().Bind(...).ValidateDataAnnotations().ValidateOnStart()` so a bad config **fails at startup**, not on the first request.

### Builder pattern

**Simple words:** Build a complex object **step by step** with readable chained calls, then call `Build()` to validate and create it.

**Analogy:** Ordering a sandwich at Subway: bread → cheese → veggies → sauce → "done".

You use builders every day: `WebApplication.CreateBuilder(args)` → `builder.Build()`, `ModelBuilder` in EF Core, `AddAuthorizationBuilder()`, `StringBuilder`, `UriBuilder`.

```csharp
public class EmailMessageBuilder
{
    private string _to = "", _subject = "", _body = "";
    private readonly List<string> _cc = [];

    public EmailMessageBuilder To(string to) { _to = to; return this; }
    public EmailMessageBuilder Subject(string s) { _subject = s; return this; }
    public EmailMessageBuilder Body(string b) { _body = b; return this; }
    public EmailMessageBuilder Cc(string cc) { _cc.Add(cc); return this; }

    public EmailMessage Build()
    {
        if (string.IsNullOrWhiteSpace(_to)) throw new InvalidOperationException("'To' is required");
        return new EmailMessage { To = _to, Subject = _subject, Body = _body, Cc = [.. _cc] };
    }
}

EmailMessage msg = new EmailMessageBuilder()
    .To("asha@example.com")
    .Subject("Your order shipped")
    .Body("It will arrive tomorrow.")
    .Cc("support@example.com")
    .Build();
```

Builders are also great in **tests** ("test data builders": `new ProductBuilder().WithPrice(0).Build()`).

### Singleton — via DI, not via `static`

**Simple words:** Exactly **one instance** for the whole app.

❌ The old-school way — hard to test, hidden global state:

```csharp
public sealed class Clock
{
    public static Clock Instance { get; } = new();
    private Clock() { }
}
```

✅ The ASP.NET Core way — let DI own the lifetime:

```csharp
builder.Services.AddSingleton(TimeProvider.System);   // .NET 8+ abstraction for "now"
builder.Services.AddSingleton<IDiscountRule, GoldDiscount>();
```

⚠️ **Interview trap:** A singleton must be **thread-safe** (many requests use it at once) and must **never capture a scoped service** like `DbContext` (captive dependency — chapter 4). If a singleton needs a DbContext, inject `IServiceScopeFactory` and create a scope per operation.

### Adapter pattern

**Simple words:** Make an incompatible class fit the interface your app expects.
**Analogy:** A travel plug adapter — your Indian charger works in a UK socket.

```csharp
// Third-party SDK you cannot change
public class LegacySmsGateway
{
    public int SendText(string phoneNumber, string text, bool unicode) => 200;
}

// What YOUR app wants
public interface INotificationSender
{
    Task<bool> SendAsync(string recipient, string message);
}

// Adapter
public class SmsAdapter(LegacySmsGateway gateway) : INotificationSender
{
    public Task<bool> SendAsync(string recipient, string message)
    {
        int code = gateway.SendText(recipient, message, unicode: true);
        return Task.FromResult(code == 200);
    }
}
```

The sample app's typed `ExchangeRateClient` is a kind of adapter too: it turns a remote HTTP API into a simple C# method.

### Pattern summary table

| Pattern | Type | One-liner | Where you see it in ASP.NET Core |
|---|---|---|---|
| Strategy | Behavioural | Swap algorithms at runtime | Auth schemes, `IDiscountRule` |
| Mediator | Behavioural | Talk through a middle-man | MediatR / custom dispatcher |
| Factory | Creational | Hide *which* object is created | `IHttpClientFactory`, keyed services |
| Builder | Creational | Step-by-step construction | `WebApplicationBuilder`, `ModelBuilder` |
| Singleton | Creational | One instance | `AddSingleton`, `IMemoryCache` |
| Decorator | Structural | Wrap to add behaviour | Middleware, filters, `DelegatingHandler` |
| Adapter | Structural | Convert one interface to another | Typed HttpClients, SDK wrappers |
| Options | (ASP.NET) | Typed configuration | `IOptions<T>` |
| Repository/UoW | Data access | Hide storage / commit together | `DbSet<T>` / `DbContext` |

---

## 15.12 Result pattern — errors without exceptions

**Simple words:** Instead of **throwing** for expected failures ("not found", "out of stock"), a method returns an object that says **success + value** or **failure + error**.

**Analogy:** A delivery app shows "Restaurant closed" — it's an expected outcome, not a crash.

| Use exceptions for | Use Result for |
|---|---|
| Truly **unexpected** things (DB down, bug, null ref) | **Expected** business outcomes (validation, not found, conflict) |
| Handled globally (`IExceptionHandler`) | Handled by the caller explicitly |
| Expensive-ish (stack trace) | Cheap |

```csharp
public record Error(string Code, string Message);

public class Result<T>
{
    public bool IsSuccess { get; }
    public T? Value { get; }
    public Error? Error { get; }

    private Result(T value) { IsSuccess = true; Value = value; }
    private Result(Error error) { IsSuccess = false; Error = error; }

    public static Result<T> Success(T value) => new(value);
    public static Result<T> Failure(Error error) => new(error);
}

public static class Errors
{
    public static readonly Error NotFound   = new("product.not_found", "Product not found");
    public static readonly Error OutOfStock = new("product.out_of_stock", "Not enough stock");
}

public class StockService
{
    private readonly Dictionary<int, int> _stock = new() { [1] = 10 };

    public Result<int> Reserve(int productId, int qty)
    {
        if (!_stock.TryGetValue(productId, out int available)) return Result<int>.Failure(Errors.NotFound);
        if (available < qty) return Result<int>.Failure(Errors.OutOfStock);
        _stock[productId] = available - qty;
        return Result<int>.Success(available - qty);
    }
}

// Endpoint maps Result -> HTTP status
app.MapPost("/stock/{id:int}/reserve", (int id, int qty, StockService svc) =>
{
    Result<int> result = svc.Reserve(id, qty);
    if (result.IsSuccess) return Results.Ok(new { remaining = result.Value });

    return result.Error!.Code switch
    {
        "product.not_found" => Results.NotFound(result.Error),
        _ => Results.Conflict(result.Error)       // 409 for business-rule violations
    };
});
```

The sample app uses a lighter version of the same idea: `UpdateAsync` returns `bool` (false = not found) and the controller maps it to `404`/`204`. It uses exceptions + `GlobalExceptionHandler` for the truly exceptional cases (e.g. `DbUpdateConcurrencyException` → 409).

🎯 **Say this in the interview:** "I use exceptions for exceptional situations and a global `IExceptionHandler` to turn them into ProblemDetails. For expected business failures I prefer returning a Result or a simple bool/nullable, so the control flow is explicit and cheap."

---

## 15.13 DTO mapping — manual vs AutoMapper vs Mapster

**Why DTOs at all?** Never return EF entities directly: you'd leak internal columns (e.g. `IsDeleted`, `PasswordHash`), create JSON cycles (`Category.Products[0].Category...`), and tie your public API contract to your DB schema. The sample app returns `ProductDto` and accepts `CreateProductRequest` (records with validation attributes).

### Manual mapping (recommended default)

```csharp
public static class ProductMappings
{
    public static ProductDto ToDto(this Product p, string categoryName) =>
        new(p.Id, p.Name, p.Price, p.Stock, categoryName);

    public static Product ToEntity(this CreateProductRequest r) =>
        new() { Name = r.Name, Price = r.Price, Stock = r.Stock };
}
```

Even better for reads: **project directly in the query** (`.Select(p => new ProductDto(...))`) like `ProductService.GetPageAsync` — EF only fetches the needed columns.

| | Manual | AutoMapper | Mapster | Mapperly (source generator) |
|---|---|---|---|---|
| Speed | Fastest | Slower (reflection/expressions) | Fast | Fastest (compile-time) |
| Compile-time safety | ✅ renamed property = compile error | ❌ runtime surprise | Partial | ✅ |
| Boilerplate | More typing | Little | Little | Little |
| Debugging | Easy (just code) | "Magic", hard | Medium | Easy (generated code visible) |
| License | — | **Commercial since 2025** (new versions) | Free (MIT) | Free (MIT) |

⚠️ **Interview trap:** "AutoMapper saves time" — until a property is renamed and the mapping silently becomes `null` in production. Many teams now prefer manual mapping or a source generator (Mapperly) — and AutoMapper's 2025 license change accelerated that.

---

## 15.14 Microservices vs Modular Monolith

**Simple words:**
- **Monolith** – one deployable app. Simple, but can become a "big ball of mud".
- **Modular monolith** – still **one deployable**, but split into **strict modules** (Catalog, Orders, Payments) with clear boundaries — modules talk only through public interfaces.
- **Microservices** – each module is its **own service**, own database, deployed independently, talking over the network.

**Analogy:** A modular monolith is **one building with separate departments and locked doors**. Microservices are **separate buildings** — more independence, but now you need roads, couriers and phones between them.

| | Modular monolith | Microservices |
|---|---|---|
| Deployment | One unit | Many independent units |
| Scaling | Whole app | Per service (scale only Orders on Black Friday) |
| Data | One DB (schemas per module) | **Database per service** |
| Communication | In-process method calls (fast, transactional) | Network calls (latency, failures) |
| Transactions | Normal DB transactions | Distributed → **sagas/outbox**, eventual consistency |
| Team fit | 1–3 teams | Many autonomous teams |
| Ops complexity | Low | High (k8s, service discovery, tracing, gateway) |
| Debugging | Easy (one process) | Hard (distributed tracing needed) |
| Tech freedom | One stack | Each service can differ |

🎯 **Say this in the interview:** "I'd start with a modular monolith and extract a microservice only when there's a real reason — independent scaling, independent deployment by a separate team, or a different technology need. Microservices solve organisational scaling problems but add network, data-consistency and operational complexity."

### API Gateway

A **single entry point** in front of many services. It does routing (`/orders/*` → Orders service), authentication, rate limiting, SSL termination, request aggregation, caching.
Examples: **YARP** (Microsoft's reverse proxy library for .NET), **Azure API Management**, Ocelot, Kong, NGINX.

```
 Client ──► [ API Gateway: auth, rate limit, routing ] ──► Catalog API
                                                     ├──► Orders API
                                                     └──► Payments API
```

### Sync vs async communication

| | Synchronous (HTTP/REST, gRPC) | Asynchronous (messages/events) |
|---|---|---|
| How | Caller waits for the answer | Caller drops a message and moves on |
| Analogy | Phone call | WhatsApp message / email |
| Coupling | Temporal coupling: both must be up | Decoupled: consumer can be down for a while |
| Good for | Queries needing an immediate answer | "Something happened" (OrderPlaced → send email, update stock) |
| Failure handling | Retries, timeouts, circuit breaker (Polly / `Microsoft.Extensions.Http.Resilience`) | Retries, dead-letter queue, idempotent consumers |
| Consistency | Immediate | **Eventual** |

### Message brokers

A **broker** is a post office for messages: producers send, it stores reliably, consumers receive.

| | Azure Service Bus | RabbitMQ | (also) Kafka |
|---|---|---|---|
| Type | Managed cloud service | Self-hosted (or cloud) broker | Distributed log / streaming |
| Concepts | **Queues** (1 consumer) & **Topics/Subscriptions** (pub-sub) | Exchanges, queues, bindings | Topics, partitions, offsets |
| Extras | Dead-letter queue, sessions (FIFO), duplicate detection, scheduled messages | Flexible routing, plugins | Replay, very high throughput |

**Queue vs Topic:** a queue = one message is handled by **one** consumer (work distribution). A topic = one message is copied to **every** subscriber (pub-sub: email service *and* inventory service both get `OrderPlaced`).

Libraries such as **MassTransit** or **NServiceBus** (note: also commercial) or **Wolverine** wrap brokers with retries, outbox and sagas.

### Idempotency — "doing it twice = doing it once"

**Simple words:** An operation is **idempotent** if repeating it gives the same result as doing it once.
**Analogy:** Pressing the lift button 5 times still calls the lift once.

Why it matters: networks fail. The client sends "Pay ₹500", the response is lost, the client **retries** → customer charged twice! Message brokers also deliver **at-least-once**, so consumers may see duplicates.

| HTTP method | Idempotent? |
|---|---|
| GET, HEAD, PUT, DELETE | ✅ Yes (by definition) |
| POST | ❌ No — needs extra work |
| PATCH | ❌ Not guaranteed |

**Making a POST idempotent with an `Idempotency-Key` header:**

```csharp
app.MapPost("/orders", async (
    [FromHeader(Name = "Idempotency-Key")] string? key,
    PlaceOrderRequest req, OrdersDbContext db, OrderService orders, CancellationToken ct) =>
{
    if (string.IsNullOrWhiteSpace(key))
        return Results.BadRequest("Idempotency-Key header is required");

    IdempotencyRecord? existing = await db.IdempotencyRecords.FindAsync([key], ct);
    if (existing is not null)                                    // seen before -> replay stored response
        return Results.Content(existing.ResponseBody, "application/json", statusCode: existing.StatusCode);

    Guid orderId = await orders.PlaceOrderAsync(req.Email, req.Total, ct);
    string body = JsonSerializer.Serialize(new { orderId });

    db.IdempotencyRecords.Add(new IdempotencyRecord { Key = key, StatusCode = 201, ResponseBody = body });
    await db.SaveChangesAsync(ct);                               // Key is the primary key -> unique

    return Results.Content(body, "application/json", statusCode: 201);
});
```

In production: give the key a **unique constraint** (so two *simultaneous* retries can't both insert), store it in the **same transaction** as the order, and expire old keys (e.g. 24h). For message consumers: store processed **message IDs** and skip duplicates.

### Outbox pattern — never lose an event

**The problem (dual write):** You save the order to SQL, then publish `OrderPlaced` to Service Bus. If the app crashes **between** the two steps, the order exists but no email/stock update ever happens. If you publish first and the DB save fails, you announced an order that doesn't exist.

**Simple words:** Don't send the letter directly. Write it in an **"outbox" tray in the same database transaction** as the order. A background worker later picks letters from the tray and posts them, marking them as sent.

```
 [ API request ] ──one DB transaction──► Orders table + OutboxMessages table
                                                     │
                       [ Background publisher ] ◄────┘  polls every few seconds
                                │
                                ▼
                       Azure Service Bus / RabbitMQ ──► Email service, Inventory service
```

```csharp
public class OrderService(OrdersDbContext db)
{
    public async Task<Guid> PlaceOrderAsync(string email, decimal total, CancellationToken ct)
    {
        var order = new Order { CustomerEmail = email, Total = total };   // Guid Id generated in code
        db.Orders.Add(order);

        db.Outbox.Add(new OutboxMessage
        {
            Type = nameof(OrderPlaced),
            Payload = JsonSerializer.Serialize(new OrderPlaced(order.Id, email, total))
        });

        await db.SaveChangesAsync(ct);   // ONE SaveChanges = ONE transaction: both rows or neither
        return order.Id;
    }
}

public class OutboxPublisher(IServiceScopeFactory scopes, IMessageBus bus, ILogger<OutboxPublisher> logger)
    : BackgroundService                                       // singleton -> create a scope for DbContext!
{
    protected override async Task ExecuteAsync(CancellationToken stoppingToken)
    {
        while (!stoppingToken.IsCancellationRequested)
        {
            using IServiceScope scope = scopes.CreateScope();
            var db = scope.ServiceProvider.GetRequiredService<OrdersDbContext>();

            List<OutboxMessage> pending = await db.Outbox
                .Where(m => m.ProcessedUtc == null)
                .OrderBy(m => m.CreatedUtc)
                .Take(20)
                .ToListAsync(stoppingToken);

            foreach (OutboxMessage msg in pending)
            {
                try
                {
                    await bus.PublishAsync(msg.Type, msg.Payload, stoppingToken);
                    msg.ProcessedUtc = DateTime.UtcNow;
                }
                catch (Exception ex)
                {
                    logger.LogWarning(ex, "Publishing outbox message {Id} failed, will retry", msg.Id);
                }
            }

            await db.SaveChangesAsync(stoppingToken);
            await Task.Delay(TimeSpan.FromSeconds(5), stoppingToken);
        }
    }
}
```

⚠️ **Interview trap:** The outbox gives **at-least-once** delivery (if the app crashes after publishing but before marking processed, the message is sent again). So **consumers must be idempotent**. Outbox + idempotent consumer is the classic pair.

🎯 **Say this in the interview:** "To avoid the dual-write problem I save the business data and an outbox record in the same transaction; a background worker publishes outbox messages to the broker and marks them sent. Delivery is at-least-once, so consumers deduplicate by message ID."

---

## 15.15 Putting it together — how I'd structure a real API

| Concern | Small CRUD service | Growing business app |
|---|---|---|
| Structure | One project, folders (like samples/ShopApi) | Clean Architecture or vertical slices |
| Data access | Inject `DbContext` into services | Specific repositories or handlers per use-case |
| Reads vs writes | Same service | CQRS level 1–2 |
| Errors | Exceptions + `IExceptionHandler` | + Result pattern for business failures |
| Mapping | Manual / LINQ projection | Manual / Mapperly |
| Cross-cutting | Middleware, filters | + Decorators / pipeline behaviours |
| Integration | Direct HTTP calls via typed `HttpClient` | Messages + outbox + idempotent consumers |

See chapter 17 (Capstone) for how the sample Shop API wires all of this end-to-end.

---

## Try it yourself

**1.** This class violates SRP and DIP. Name the problems and sketch the fix.

```csharp
public class InvoiceController : ControllerBase
{
    [HttpPost]
    public IActionResult Create(InvoiceDto dto)
    {
        var db = new SqlConnection("Server=...;Password=secret");
        // INSERT ...
        var pdf = new PdfGenerator().Render(dto);
        new SmtpClient("smtp.company.com").Send("billing@x.com", dto.Email, "Invoice", "...");
        return Ok();
    }
}
```

<details><summary>Answer</summary>

- **SRP:** the controller does persistence, PDF generation, and email — three reasons to change, plus HTTP.
- **DIP:** it `new`s concrete classes (`SqlConnection`, `PdfGenerator`, `SmtpClient`) → untestable, tightly coupled.
- **Security:** connection string with password hard-coded (use configuration/Key Vault).
- **Fix:** create `IInvoiceService` (orchestrates), `IInvoiceRepository` or `DbContext`, `IPdfGenerator`, `IEmailSender`; register them in DI; the controller becomes `public async Task<IActionResult> Create(InvoiceDto dto, CancellationToken ct) => CreatedAtAction(..., await invoices.CreateAsync(dto, ct));`. Consider sending the email via a background queue/outbox so the request is fast.
</details>

**2.** You have `if (country == "IN") tax = 0.18m; else if (country == "US") ... else if ...` growing every sprint. Which principle is violated, and which pattern fixes it?

<details><summary>Answer</summary>

Open/Closed Principle — every new country edits tested code. Use the **Strategy** pattern: `ITaxCalculator { string Country; decimal Calculate(decimal amount); }`, one class per country, inject `IEnumerable<ITaxCalculator>` (or keyed services `AddKeyedScoped<ITaxCalculator, IndiaTax>("IN")`) and pick by country. A new country = a new class + one registration.
</details>

**3.** Write the registration lines to add a **logging decorator** `LoggingProductService` *around* the `CachedProductService` *around* the real `ProductService`. What order do calls go through?

<details><summary>Answer</summary>

```csharp
builder.Services.AddScoped<ProductService>();
builder.Services.AddScoped<IProductService>(sp =>
    new LoggingProductService(
        new CachedProductService(
            sp.GetRequiredService<ProductService>(),
            sp.GetRequiredService<IMemoryCache>()),
        sp.GetRequiredService<ILogger<LoggingProductService>>()));
```

Call order: Controller → `LoggingProductService` → `CachedProductService` → (on cache miss) `ProductService` → DB. Logging is outermost, so it also logs cache hits. With Scrutor: `services.Decorate<IProductService, CachedProductService>(); services.Decorate<IProductService, LoggingProductService>();` (last registered = outermost).
</details>

**4.** Your team lead says "Let's add a generic `IRepository<T>` with `IQueryable<T> Query()` on top of EF Core for all 40 entities." What do you reply?

<details><summary>Answer</summary>

Returning `IQueryable<T>` leaks EF Core (callers still write EF LINQ, `Include`, etc.), so we'd get the extra layer **without** real abstraction. `DbContext` already is a UoW and `DbSet<T>` already is a repository. I'd suggest either injecting `DbContext` into services/handlers, or, where we want isolation (Clean Architecture, heavy reuse, unit tests), **specific** repositories per aggregate with intention-revealing methods like `GetOrderWithItemsAsync(id)`.
</details>

**5.** A payment API sometimes charges customers twice when the mobile network is flaky. Explain why and how to fix it.

<details><summary>Answer</summary>

The client times out (response lost) and **retries** a non-idempotent POST, so the server processes it twice. Fix: require an **`Idempotency-Key`** header (client generates a GUID per logical payment, reuses it on retries). Server stores key + response with a **unique constraint**, in the same transaction as the payment; on a repeated key it returns the stored response instead of charging again. Expire keys after e.g. 24h.
</details>

**6.** After saving an order you publish `OrderPlaced` to Azure Service Bus. Sometimes the confirmation email never arrives even though the order exists. What's the likely cause and the pattern to fix it?

<details><summary>Answer</summary>

The **dual-write problem**: the DB commit succeeded but the publish failed (crash, network blip, broker throttling). Use the **Outbox pattern**: write the order and an `OutboxMessage` row in the **same transaction**; a `BackgroundService` publishes pending outbox rows and marks them processed, retrying failures. Because it's at-least-once, make the email consumer **idempotent** (track processed message IDs).
</details>

---

## Quick revision

1. **SOLID** = Single responsibility, Open/closed, Liskov substitution, Interface segregation, Dependency inversion.
2. SRP = one **reason to change**, not one method.
3. OCP in practice = Strategy + `IEnumerable<T>`/keyed services; add classes, don't edit `switch`es.
4. LSP = children must not surprise callers (no `NotSupportedException` overrides).
5. DIP is the principle; DI is the technique; the container is the tool.
6. Clean Architecture's only real rule: **dependencies point inward**; Domain references nothing.
7. `DbContext` = Unit of Work, `DbSet<T>` = repository; add specific repos only when they add value; avoid generic repos leaking `IQueryable`.
8. CQRS = separate commands (change) from queries (read); MediatR is just a dispatcher and **went commercial in 2025** (as did AutoMapper).
9. Decorator = same interface + extra behaviour (caching, logging); middleware and `DelegatingHandler` are decorators.
10. Expected failures → Result/bool/null; unexpected → exceptions + global `IExceptionHandler`.
11. Start with a **modular monolith**; microservices trade code complexity for network/data/ops complexity.
12. POST isn't idempotent → `Idempotency-Key`; dual-write → **Outbox**; at-least-once → **idempotent consumers**.

---

# 16. MVC, Razor Pages & Blazor — Quick Tour

> **Why interviewers ask this**
> Even for an **API-focused** role, interviewers often check that you know the rest of the ASP.NET Core family: "What's the difference between `Controller` and `ControllerBase`?", "ViewBag vs ViewData vs TempData?", "Razor Pages or MVC?", "What is Blazor?". You won't be asked to build a full UI — but blanking on these looks bad. This chapter is a **fast, practical tour**: enough to answer confidently and read existing code.

---

## 16.1 The MVC pattern in simple words

**MVC = Model – View – Controller.** It splits a UI app into three roles:

| Part | Job | Restaurant analogy |
|---|---|---|
| **Model** | Data + business rules (entities, view models, services) | The **kitchen** and ingredients |
| **View** | How data is shown (HTML via Razor) | The **plate presentation** |
| **Controller** | Receives the request, talks to the model, picks a view | The **waiter** |

```
 Browser ──GET /products──► Controller ──asks──► Model/Service ──► DB
                               │
                               └──passes model──► View (Razor .cshtml) ──HTML──► Browser
```

For a **Web API**, the "V" disappears: the controller returns **JSON** instead of HTML. That's why API controllers inherit from `ControllerBase`, not `Controller`.

### Controller vs ControllerBase

| | `ControllerBase` | `Controller` (inherits `ControllerBase`) |
|---|---|---|
| Used for | **Web APIs** (JSON) | **MVC with views** (HTML) |
| `Ok()`, `NotFound()`, `CreatedAtAction()`, `BadRequest()` | ✅ | ✅ |
| `View()`, `PartialView()`, `ViewComponent()` | ❌ | ✅ |
| `ViewBag`, `ViewData`, `TempData` | ❌ | ✅ |
| `Json()` helper | ❌ (just return the object) | ✅ |
| Typical attribute | `[ApiController]` | none (or `[AutoValidateAntiforgeryToken]`) |

The sample app (samples/ShopApi) uses `ProductsController : ControllerBase` with `[ApiController]` — correct for an API.

⚠️ **Interview trap:** "Should an API controller inherit from `Controller`?" — It works, but it pulls in view-related features you don't need. Use `ControllerBase` for APIs.

---

## 16.2 Easy MVC example — end to end

Register MVC with views and a conventional route in `Program.cs`:

```csharp
var builder = WebApplication.CreateBuilder(args);
builder.Services.AddControllersWithViews();     // APIs use AddControllers(); MVC adds views
builder.Services.AddScoped<IProductService, ProductService>();

var app = builder.Build();
app.UseStaticFiles();          // wwwroot: css, js, images  (.NET 9+: app.MapStaticAssets())
app.UseRouting();
app.UseAuthorization();

// Conventional routing: /{controller}/{action}/{id?}
app.MapControllerRoute(
    name: "default",
    pattern: "{controller=Home}/{action=Index}/{id?}");

app.Run();
```

A controller that returns views:

```csharp
public class ProductsController(IProductService service) : Controller
{
    // GET /Products  or  /Products/Index
    public async Task<IActionResult> Index(int page = 1, CancellationToken ct = default)
    {
        PagedResult<ProductDto> result = await service.GetPageAsync(page, 10, null, ct);
        return View(result);                // looks for Views/Products/Index.cshtml
    }

    // GET /Products/Details/5
    public async Task<IActionResult> Details(int id, CancellationToken ct)
    {
        ProductDto? product = await service.GetByIdAsync(id, ct);
        return product is null ? NotFound() : View(product);
    }
}
```

**View discovery:** `View()` looks in `Views/{ControllerName}/{ActionName}.cshtml`, then `Views/Shared/{ActionName}.cshtml`.

Typical folder structure:

```
MyMvcApp/
├── Controllers/ProductsController.cs
├── Models/                    // view models
├── Views/
│   ├── _ViewImports.cshtml    // shared @using + tag helpers
│   ├── _ViewStart.cshtml      // sets default Layout
│   ├── Shared/
│   │   ├── _Layout.cshtml     // master page
│   │   ├── _ProductCard.cshtml// partial view
│   │   └── Error.cshtml
│   └── Products/
│       ├── Index.cshtml
│       └── Details.cshtml
└── wwwroot/ (css, js, lib)
```

---

## 16.3 Razor syntax essentials

**Razor** = C# mixed into HTML. `@` switches from HTML to C#.

```razor
@model PagedResult<ProductDto>          @* strongly-typed model for this view *@

@{
    ViewData["Title"] = "Products";      // a C# code block
    var hasItems = Model.Items.Count > 0;
}

<h1>@ViewData["Title"]</h1>
<p>Total: @Model.TotalCount products</p>  @* implicit expression *@
<p>Price incl. tax: @(Model.Items.Sum(p => p.Price) * 1.18m)</p>  @* explicit expression *@

@if (!hasItems)
{
    <p class="text-muted">No products found.</p>
}
else
{
    <table class="table">
        <tr><th>Name</th><th>Price</th><th></th></tr>
        @foreach (var p in Model.Items)
        {
            <tr>
                <td>@p.Name</td>
                <td>@p.Price.ToString("C")</td>
                <td><a asp-action="Details" asp-route-id="@p.Id">Details</a></td>
            </tr>
        }
    </table>
}
```

| Syntax | Meaning |
|---|---|
| `@model T` | Declares the view's model type (`Model` property) |
| `@Model.Name` | Implicit expression — output, **HTML-encoded** |
| `@(a + b)` | Explicit expression |
| `@{ ... }` | Code block (no output) |
| `@if`, `@foreach`, `@switch` | Control flow |
| `@* comment *@` | Razor comment (not sent to browser) |
| `@Html.Raw(x)` | Output **without** encoding — ⚠️ XSS risk |
| `@using`, `@inject` | Import namespace / inject a service into the view |
| `@section Scripts { }` | Fill a named section in the layout |

⚠️ **Interview trap:** "Is Razor safe against XSS?" — Yes, by default `@value` is **HTML-encoded**. It becomes unsafe when you use `@Html.Raw()` with user input.

---

## 16.4 Tag Helpers

**Simple words:** HTML-looking attributes (`asp-*`) that run server-side C# to generate correct HTML (URLs, form names, validation).

```html
<!-- Generates href="/Products/Details/5" from routing, so links never break -->
<a asp-controller="Products" asp-action="Details" asp-route-id="5">View</a>

<!-- Form posting to ProductsController.Create, with antiforgery token added automatically -->
<form asp-action="Create" method="post">
    <label asp-for="Name"></label>
    <input asp-for="Name" class="form-control" />      <!-- name="Name", value, data-val-* -->
    <span asp-validation-for="Name" class="text-danger"></span>

    <select asp-for="CategoryId" asp-items="ViewBag.Categories"></select>
    <button type="submit">Save</button>
</form>

<environment include="Development"><script src="~/js/site.js"></script></environment>
<img src="~/img/logo.png" asp-append-version="true" />  <!-- cache-busting ?v=hash -->
```

| Tag helper | Purpose |
|---|---|
| `asp-controller`, `asp-action`, `asp-route-*` | Build URLs from routes |
| `asp-for` | Bind input/label to a model property (name, id, value, validation attributes) |
| `asp-validation-for`, `asp-validation-summary` | Show validation errors |
| `asp-items` | Fill `<select>` options |
| `asp-append-version` | Cache-busting for static files |
| `<environment>` | Render only in certain environments |
| `<partial name="...">` | Render a partial view |

Tag helpers are enabled in `_ViewImports.cshtml` (see next section). The older alternative is **HTML helpers** (`@Html.TextBoxFor(m => m.Name)`) — same result, less HTML-friendly.

---

## 16.5 Layouts, _ViewStart, _ViewImports, partial views

**Analogy:** A **layout** is the letterhead paper (header, menu, footer); each view writes only the letter body.

**`Views/Shared/_Layout.cshtml`**

```html
<!DOCTYPE html>
<html>
<head>
    <title>@ViewData["Title"] - Shop</title>
    <link rel="stylesheet" href="~/css/site.css" asp-append-version="true" />
</head>
<body>
    <nav><a asp-controller="Home" asp-action="Index">Home</a> | <a asp-controller="Products" asp-action="Index">Products</a></nav>

    <main>
        @RenderBody()                                  <!-- the view's content goes here -->
    </main>

    <footer>&copy; @DateTime.Now.Year Shop</footer>
    @await RenderSectionAsync("Scripts", required: false)
</body>
</html>
```

**`Views/_ViewStart.cshtml`** — runs before **every** view; sets the default layout:

```razor
@{
    Layout = "_Layout";
}
```

**`Views/_ViewImports.cshtml`** — shared `@using`s and tag helpers for all views in the folder (and sub-folders):

```razor
@using MyMvcApp
@using MyMvcApp.Models
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
```

| File | Runs/applies | Typical content |
|---|---|---|
| `_Layout.cshtml` | Wraps views | HTML skeleton, nav, `@RenderBody()` |
| `_ViewStart.cshtml` | Executes before each view | `Layout = "_Layout";` |
| `_ViewImports.cshtml` | Directives for all views in folder | `@using`, `@addTagHelper`, `@inject` |

### Partial views — reusable view fragments

**`Views/Shared/_ProductCard.cshtml`**

```razor
@model ProductDto
<div class="card">
    <h5>@Model.Name</h5>
    <p>@Model.Price.ToString("C") · @Model.Category</p>
</div>
```

Use it:

```razor
@foreach (var p in Model.Items)
{
    <partial name="_ProductCard" model="p" />
}
```

Partials have **no logic of their own** — they only render the model you give them. If you need logic + data access, use a **view component** (§16.7).

---

## 16.6 Passing data to views: ViewBag vs ViewData vs TempData vs strongly-typed model

```csharp
public IActionResult Edit(int id)
{
    ViewData["Title"] = "Edit product";                 // dictionary, string keys
    ViewBag.Categories = new SelectList(_categories, "Id", "Name");  // dynamic wrapper over ViewData
    TempData["Message"] = "Loaded!";                    // survives ONE redirect
    return View(new EditProductViewModel { Id = id });  // strongly-typed model ✅
}
```

| | **ViewData** | **ViewBag** | **TempData** | **Strongly-typed model** |
|---|---|---|---|---|
| Type | `ViewDataDictionary` (`string` → `object`) | `dynamic` wrapper over ViewData | `ITempDataDictionary` | Your class (`@model T`) |
| Lifetime | Current request only | Current request only | **Until read** (survives a redirect) | Current request |
| Compile-time safety | ❌ needs casts | ❌ typos = runtime error | ❌ | ✅ IntelliSense + compile errors |
| Storage | Memory | Memory | **Cookie** (default) or Session | Memory |
| Typical use | Page title | Dropdown lists (legacy) | "Saved successfully!" flash message after **PRG** | **Everything else** |

**PRG (Post-Redirect-Get) with TempData:**

```csharp
[HttpPost, ValidateAntiForgeryToken]
public async Task<IActionResult> Create(CreateProductRequest model, CancellationToken ct)
{
    if (!ModelState.IsValid) return View(model);          // redisplay form with errors
    await service.CreateAsync(model, ct);
    TempData["Message"] = "Product created!";
    return RedirectToAction(nameof(Index));               // browser refresh won't re-POST
}
```

```razor
@if (TempData["Message"] is string msg) { <div class="alert alert-success">@msg</div> }
```

`TempData.Keep("Message")` or `TempData.Peek("Message")` let you read without deleting.

⚠️ **Interview trap:** "ViewBag and ViewData — different storage?" No — **ViewBag is just a dynamic view over the same ViewData dictionary**. `ViewBag.Title` and `ViewData["Title"]` are the same value.

🎯 **Say this in the interview:** "I prefer strongly-typed view models for compile-time safety. ViewData/ViewBag only live for the current request, so I use them for small things like the page title. TempData survives one redirect, which makes it ideal for flash messages in the Post-Redirect-Get pattern."

---

## 16.7 View components — "mini controllers" for widgets

**Simple words:** A reusable UI widget **with its own logic and data access** — e.g. a shopping-cart badge, a "top 5 categories" sidebar, a login panel. Think "partial view + a tiny controller".

```csharp
public class TopProductsViewComponent(IProductService service) : ViewComponent
{
    public async Task<IViewComponentResult> InvokeAsync(int count = 5)
    {
        PagedResult<ProductDto> page = await service.GetPageAsync(1, count, null, HttpContext.RequestAborted);
        return View(page.Items);    // Views/Shared/Components/TopProducts/Default.cshtml
    }
}
```

`Views/Shared/Components/TopProducts/Default.cshtml`:

```razor
@model IReadOnlyList<ProductDto>
<ul>
    @foreach (var p in Model) { <li>@p.Name</li> }
</ul>
```

Invoke from any view or the layout:

```razor
<vc:top-products count="3"></vc:top-products>
@* or *@
@await Component.InvokeAsync("TopProducts", new { count = 3 })
```

| | Partial view | View component | Child action (old MVC 5) |
|---|---|---|---|
| Own logic / DI | ❌ | ✅ | ✅ |
| Reachable by URL | ❌ | ❌ | (was) |
| Model binding / filters | ❌ | ❌ | ✅ |
| Use for | Repeated markup | Self-contained widgets | — (removed in Core) |

---

## 16.8 Model binding with forms + antiforgery tokens

### Model binding & validation in MVC

Same model binding as APIs (chapter 6) but data typically comes from **form fields** (`application/x-www-form-urlencoded`). In MVC, invalid models are **not** auto-rejected — you check `ModelState.IsValid` and re-show the form.

```csharp
public class ProductFormModel
{
    [Required, StringLength(200, MinimumLength = 2)]
    public string Name { get; set; } = "";

    [Range(0.01, 1_000_000)]
    public decimal Price { get; set; }

    [Display(Name = "Category")]
    public int CategoryId { get; set; }
}

[HttpGet]
public IActionResult Create() => View(new ProductFormModel());

[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> Create(ProductFormModel form, CancellationToken ct)
{
    if (!ModelState.IsValid)
        return View(form);                           // errors appear via asp-validation-for

    await service.CreateAsync(new CreateProductRequest(form.Name, form.Price, 0, form.CategoryId), ct);
    TempData["Message"] = "Created!";
    return RedirectToAction(nameof(Index));
}
```

| | `[ApiController]` (API) | MVC controller |
|---|---|---|
| Invalid model | **Automatic 400** ProblemDetails | You check `ModelState.IsValid` |
| Body source | JSON (`[FromBody]` inferred) | Form fields |
| Response | JSON | HTML view / redirect |

⚠️ **Over-posting / mass assignment:** never bind directly to your EF entity — an attacker could post `IsDeleted=true` or `Role=Admin`. Bind to a **view model/DTO** with only the allowed fields (or use `[Bind("Name,Price")]`).

### Antiforgery tokens (CSRF protection)

**CSRF in simple words:** You're logged in to `bank.com` (cookie). A malicious site has a hidden form that POSTs to `bank.com/transfer`. Your browser **automatically sends your cookie**, so the bank thinks *you* did it.

**Fix:** The server puts a secret **antiforgery token** in the form (hidden field) and a matching cookie. The evil site can't read your token, so its forged POST fails validation.

```html
<form asp-action="Create" method="post">   <!-- form tag helper adds the hidden token automatically -->
    ...
</form>
<!-- renders: <input name="__RequestVerificationToken" type="hidden" value="CfDJ8..."> -->
```

```csharp
[ValidateAntiForgeryToken]           // on a single action
[AutoValidateAntiforgeryToken]       // on controller/globally: validates all unsafe methods (POST/PUT/DELETE)
[IgnoreAntiforgeryToken]             // opt out (e.g. webhook endpoint)

// Globally for MVC:
builder.Services.AddControllersWithViews(o => o.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

Razor Pages validate antiforgery tokens **automatically**. Minimal APIs (.NET 8+) use `app.UseAntiforgery()` for form-bound endpoints.

⚠️ **Interview trap:** "Does my JWT-based Web API need antiforgery tokens?" — Usually **no**. CSRF works because browsers send **cookies** automatically. A JWT in an `Authorization: Bearer` header is **not** sent automatically, so CSRF doesn't apply. If your API uses **cookie auth** (e.g. a SPA with cookies), you *do* need CSRF protection (antiforgery or SameSite cookies).

---

## 16.9 Razor Pages — page-focused alternative to MVC

**Simple words:** Instead of Controller + View in separate folders, each page is **one `.cshtml` file + one `PageModel` class** next to it. URL = file path.

**Analogy:** MVC is a restaurant with one waiter (controller) serving many tables (views). Razor Pages gives **each table its own waiter** — simpler for page-centric apps (forms, admin screens).

```
Pages/
├── _ViewImports.cshtml
├── _ViewStart.cshtml
├── Index.cshtml                 -> /
└── Products/
    ├── Index.cshtml (+ .cs)     -> /Products
    └── Edit.cshtml  (+ .cs)     -> /Products/Edit/5   (with @page "{id:int}")
```

```csharp
// Program.cs
builder.Services.AddRazorPages();
...
app.MapRazorPages();
```

**`Pages/Products/Edit.cshtml.cs`**

```csharp
public class EditModel(IProductService service) : PageModel
{
    [BindProperty]                                    // bound on POST
    public UpdateProductRequest Input { get; set; } = default!;

    public string? CategoryName { get; private set; }

    // GET /Products/Edit/5
    public async Task<IActionResult> OnGetAsync(int id, CancellationToken ct)
    {
        ProductDto? p = await service.GetByIdAsync(id, ct);
        if (p is null) return NotFound();
        Input = new UpdateProductRequest(p.Name, p.Price, p.Stock);
        CategoryName = p.Category;
        return Page();
    }

    // POST /Products/Edit/5
    public async Task<IActionResult> OnPostAsync(int id, CancellationToken ct)
    {
        if (!ModelState.IsValid) return Page();
        bool updated = await service.UpdateAsync(id, Input, ct);
        if (!updated) return NotFound();
        TempData["Message"] = "Saved!";
        return RedirectToPage("Index");
    }

    // Named handler: POST /Products/Edit/5?handler=Delete
    public async Task<IActionResult> OnPostDeleteAsync(int id, CancellationToken ct)
    {
        await service.DeleteAsync(id, ct);
        return RedirectToPage("Index");
    }
}
```

**`Pages/Products/Edit.cshtml`**

```razor
@page "{id:int}"
@model EditModel

<h1>Edit product (@Model.CategoryName)</h1>

<form method="post">
    <input asp-for="Input.Name" />
    <span asp-validation-for="Input.Name"></span>
    <input asp-for="Input.Price" />
    <input asp-for="Input.Stock" />
    <button type="submit">Save</button>
    <button type="submit" asp-page-handler="Delete">Delete</button>   <!-- calls OnPostDeleteAsync -->
</form>
```

| Concept | Meaning |
|---|---|
| `@page` | Marks the file as a Razor Page (required, first line); can include a route template |
| `PageModel` | Code-behind class: properties = model, methods = handlers |
| `OnGet`/`OnGetAsync`, `OnPost`/`OnPostAsync` | Handlers by HTTP verb |
| `OnPostDeleteAsync` + `asp-page-handler="Delete"` | **Named handler** (multiple buttons/forms on one page) |
| `[BindProperty]` | Bind form data into a property (POST only; `SupportsGet = true` for GET) |
| `Page()`, `RedirectToPage()` | Re-render this page / redirect |

### MVC vs Razor Pages

| | MVC | Razor Pages |
|---|---|---|
| Organisation | By **type**: Controllers/, Views/ | By **page/feature**: Pages/Products/Edit |
| Routing | Conventional or attribute routes | **File-system** based (+ `@page` template) |
| Best for | Complex apps, many actions sharing logic, APIs + views together | Page-centric CRUD/forms, admin sites |
| Antiforgery | Opt-in (`[ValidateAntiForgeryToken]`) | **Automatic** |
| Testability | Good | Good (PageModel is a class) |
| Microsoft's default template for new server-rendered UI | — | ✅ (Razor Pages, or Blazor) |

🎯 **Say this in the interview:** "Both use Razor and the same model binding, filters and DI. MVC organises by controllers and views; Razor Pages organises by page, with a PageModel containing OnGet/OnPost handlers. For form-heavy page-centric apps I'd pick Razor Pages; for APIs I use controllers with `ControllerBase` or minimal APIs."

---

## 16.10 Blazor in one section

**Simple words:** Blazor lets you build **interactive web UIs in C#** instead of JavaScript, using **Razor components** (`.razor` files).

```razor
@* Counter.razor *@
@page "/counter"
@rendermode InteractiveServer

<h3>Counter</h3>
<p>Current count: @count</p>
<button class="btn btn-primary" @onclick="Increment">Click me</button>

@code {
    private int count;
    private void Increment() => count++;
}
```

### Hosting models / render modes

| | **Blazor Server** (Interactive Server) | **Blazor WebAssembly** (Interactive WebAssembly) | **Static SSR** (.NET 8+) |
|---|---|---|---|
| Where C# runs | On the **server** | In the **browser** (.NET runtime compiled to WebAssembly) | Server, once per request |
| UI updates | Over a **SignalR (WebSocket)** connection | Locally in the browser | None (plain HTML; forms post back) |
| First load | Fast (small download) | Slower (downloads runtime + DLLs) | Fastest |
| Needs constant connection | ✅ yes — lost connection = UI stops | ❌ works offline (PWA possible) | ❌ |
| Server load | Memory per connected user (circuit) | Low (static files) | Low |
| Code/secrets | Stay on the server ✅ | **Downloaded to client** — never put secrets there | Server |
| Data access | Can call DB/services directly | Must call a **Web API** | Direct |

### .NET 8+ "Blazor Web App" — mix render modes per component

Since .NET 8 there is one unified template. Each page/component chooses a **render mode**:

```csharp
// Program.cs
builder.Services.AddRazorComponents()
    .AddInteractiveServerComponents()
    .AddInteractiveWebAssemblyComponents();

app.MapRazorComponents<App>()
    .AddInteractiveServerRenderMode()
    .AddInteractiveWebAssemblyRenderMode();
```

| Render mode | Directive | Meaning |
|---|---|---|
| Static SSR | *(none)* | Server renders HTML, no interactivity (fast, SEO-friendly) |
| Streaming rendering | `@attribute [StreamRendering]` | Send the page shell first, stream content as async data arrives |
| Interactive Server | `@rendermode InteractiveServer` | Interactive via SignalR |
| Interactive WebAssembly | `@rendermode InteractiveWebAssembly` | Interactive in the browser |
| Interactive Auto | `@rendermode InteractiveAuto` | Server first (fast start), then switches to WebAssembly once downloaded |

**Prerendering** (on by default) renders HTML on the server first so the user sees content immediately, then the component becomes interactive.

⚠️ **Interview trap:** "Blazor WebAssembly is secure because it's compiled." — No! The DLLs are downloaded to the browser and can be decompiled. Treat it like JavaScript: **all security checks must be on the server API**.

🎯 **Say this in the interview:** "Blazor lets us write interactive UI in C# with Razor components. Blazor Server runs on the server and syncs UI over SignalR; WebAssembly runs .NET in the browser. Since .NET 8, a Blazor Web App can mix static SSR, interactive server, WebAssembly and Auto render modes per component."

---

## 16.11 Big comparison — which UI tech when?

| Need | Choose |
|---|---|
| JSON API for SPA/mobile/other services | **Web API** (`ControllerBase`) or **Minimal APIs** |
| Server-rendered site, many controllers/actions, existing MVC skills | **MVC** |
| Page-centric forms / admin / CRUD screens | **Razor Pages** |
| Rich interactive UI in C# (dashboards, line-of-business apps) | **Blazor** |
| React/Angular front end | **Web API** + SPA (the sample app's CORS policy allows `http://localhost:4200` — an Angular dev server) |

All of them share the **same foundation**: middleware pipeline, DI, configuration, logging, authentication/authorization, model binding, filters (except minimal APIs use endpoint filters).

---

## Try it yourself

**1.** A colleague's API controller inherits from `Controller` and returns `Json(product)`. What would you change and why?

<details><summary>Answer</summary>

Inherit from `ControllerBase` and add `[ApiController]`. `Controller` brings view support (ViewBag, `View()`) an API doesn't need. Return `ActionResult<ProductDto>` with `Ok(product)` / `NotFound()` instead of `Json()` so content negotiation, status codes and OpenAPI metadata work properly. `[ApiController]` also gives automatic 400 validation responses and binding-source inference.
</details>

**2.** After a successful form POST you set `ViewBag.Message = "Saved!"` and `return RedirectToAction("Index")`. The message never shows. Why, and what's the fix?

<details><summary>Answer</summary>

`ViewBag`/`ViewData` live only for the **current request**. The redirect makes the browser issue a **new** GET request, so the value is gone. Use `TempData["Message"] = "Saved!"` — TempData survives until it's read (stored in a cookie by default), perfect for Post-Redirect-Get.
</details>

**3.** You need a "Cart (3)" badge in the header of every page, which reads from the database. Partial view or view component?

<details><summary>Answer</summary>

**View component** — it has its own logic and can inject services (e.g. `ICartService`), so it fetches its own data. A partial view has no logic; every controller action would have to load cart data and pass it in. Render it in `_Layout.cshtml` with `<vc:cart-badge></vc:cart-badge>` (or `@await Component.InvokeAsync("CartBadge")`).
</details>

**4.** In a Razor Page you have one form with two buttons: "Save" and "Archive". How do you route each to different code?

<details><summary>Answer</summary>

Use **named handlers**: write `OnPostSaveAsync` and `OnPostArchiveAsync` in the PageModel, and on the buttons use `asp-page-handler="Save"` / `asp-page-handler="Archive"`. The tag helper adds `?handler=Save` (or `Archive`) to the form action and Razor Pages calls the matching method.
</details>

**5.** Your team wants a Blazor app usable on a flaky factory Wi-Fi network, and some pages must be indexable by search engines. Which render modes?

<details><summary>Answer</summary>

Use **static SSR** (with prerendering) for public, SEO-relevant pages — plain HTML, no persistent connection. For interactive pages, prefer **Interactive WebAssembly** (or **Auto**) because Interactive Server needs a continuous SignalR connection that breaks on flaky networks. Keep secrets and authorization on the server API, since WebAssembly code runs on the client.
</details>

---

## Quick revision

1. MVC = Model (data), View (HTML), Controller (traffic cop); APIs drop the View and return JSON.
2. `ControllerBase` for APIs; `Controller` adds `View()`, ViewBag, ViewData, TempData.
3. `@model T` + `Model` gives strongly-typed views; Razor `@` output is **HTML-encoded** (`Html.Raw` is the XSS risk).
4. Tag helpers (`asp-for`, `asp-action`, `asp-route-id`) generate correct names/URLs; enabled in `_ViewImports`.
5. `_Layout` = master page (`@RenderBody()`); `_ViewStart` sets the layout; `_ViewImports` holds shared directives.
6. ViewBag is a dynamic wrapper over ViewData (same storage, current request); TempData survives **one redirect**.
7. Partial view = reusable markup; view component = widget with its own logic + DI.
8. MVC must check `ModelState.IsValid` manually; `[ApiController]` returns 400 automatically.
9. Antiforgery tokens stop CSRF for **cookie**-authenticated forms; bearer-token APIs don't need them.
10. Razor Pages = `@page` + `PageModel` with `OnGet`/`OnPost` and named handlers; antiforgery automatic.
11. Blazor Server = SignalR circuit on the server; WebAssembly = .NET in the browser (no secrets!).
12. .NET 8+ Blazor Web App mixes Static SSR, Interactive Server, WebAssembly and Auto **per component**.

---

# 17. Capstone: The Shop API End-to-End

> **Why this chapter?** The most common practical interview question is *"Walk me through how you would build a CRUD API"* or *"Explain a project you built."*
> This chapter is the **complete, compiled and tested** code of the sample app, with a guided tour. Learn to explain it in your own words and you have a strong project story.

---

## 17.1 The story you can tell in the interview

🎯 **Say this in the interview:**
> "I built a product/order API on ASP.NET Core. Requests go through a middleware pipeline: a global exception handler that returns ProblemDetails, a correlation-id middleware, a timing middleware, then authentication with JWT and policy-based authorization, rate limiting and output caching. Controllers are thin: they validate input via DTOs with data annotations and call a scoped service. The service uses EF Core with no-tracking projections for reads, pagination, soft delete via a global query filter, and optimistic concurrency. Configuration uses the options pattern with validation at startup. There's a background service that uses a scope factory to clean up stale orders, a typed HttpClient for an external API, health checks for the database, and both unit tests with Moq and integration tests using WebApplicationFactory."

That paragraph touches ~20 interview topics. Each one links to a chapter, so if the interviewer digs into any word, you are ready.

---

## 17.2 Request flow — what happens on `POST /api/products`

```text
1. Kestrel receives the HTTP request
2. UseExceptionHandler      – wraps everything after it in try/catch
3. UseStatusCodePages       – fills empty 4xx/5xx bodies with ProblemDetails
4. UseHttpsRedirection      – redirects http→https (when https port known)
5. Correlation-id middleware– reads/creates X-Correlation-Id, adds logging scope
6. RequestTimingMiddleware  – starts stopwatch, adds X-Elapsed-Ms header
7. (Routing)                – endpoint selected: ProductsController.Create
8. UseCors("Frontend")
9. UseAuthentication        – JwtBearer validates token → HttpContext.User
10. UseAuthorization        – [Authorize] → 401 if no user
11. UseRateLimiter          – "fixed" policy: 100 req/min → 429 if exceeded
12. UseOutputCache
13. MVC: model binding (JSON body → CreateProductRequest)
14. [ApiController] validation → 400 ValidationProblemDetails if invalid
15. LogActionFilter (global) before → action → after
16. ProductsController.Create → IProductService.CreateAsync (scoped) → AppDbContext (scoped)
17. Unknown category? → ArgumentException → GlobalExceptionHandler → 400 ProblemDetails
18. Success → 201 Created + Location: /api/products/{id}
```

---

## 17.3 Program.cs — DI registrations and the pipeline

Read the comments: section 1 registers services (**what** the app can use), section 2 builds the pipeline (**how** each request flows), section 3 maps endpoints (**where** requests go).

```csharp
using System.Text;
using System.Threading.RateLimiting;
using Microsoft.AspNetCore.Authentication.JwtBearer;
using Microsoft.AspNetCore.RateLimiting;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Diagnostics.HealthChecks;
using Microsoft.IdentityModel.Tokens;
using ShopApi.Background;
using ShopApi.Clients;
using ShopApi.Data;
using ShopApi.Endpoints;
using ShopApi.Errors;
using ShopApi.Filters;
using ShopApi.Middleware;
using ShopApi.Options;
using ShopApi.Services;

var builder = WebApplication.CreateBuilder(args);

// ---------------- 1. Register services (DI container) ----------------

// Options pattern + validation at startup (app fails fast if config is wrong)
builder.Services.AddOptions<JwtOptions>()
    .Bind(builder.Configuration.GetSection(JwtOptions.SectionName))
    .ValidateDataAnnotations()
    .ValidateOnStart();

// EF Core (DbContext is SCOPED = one per HTTP request)
builder.Services.AddDbContext<AppDbContext>(o =>
    o.UseSqlite(builder.Configuration.GetConnectionString("Shop")));

// Our services
builder.Services.AddScoped<IProductService, ProductService>();

// Controllers + a global filter
builder.Services.AddControllers(o => o.Filters.Add<LogActionFilter>());
builder.Services.AddOpenApi();

// Errors -> RFC 7807 ProblemDetails
builder.Services.AddProblemDetails();
builder.Services.AddExceptionHandler<GlobalExceptionHandler>();

// Authentication (who are you?) + Authorization (what can you do?)
JwtOptions jwt = builder.Configuration.GetSection(JwtOptions.SectionName).Get<JwtOptions>() ?? new JwtOptions();
builder.Services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
    .AddJwtBearer(o =>
    {
        o.TokenValidationParameters = new TokenValidationParameters
        {
            ValidateIssuer = true,
            ValidIssuer = jwt.Issuer,
            ValidateAudience = true,
            ValidAudience = jwt.Audience,
            ValidateLifetime = true,
            ValidateIssuerSigningKey = true,
            IssuerSigningKey = new SymmetricSecurityKey(Encoding.UTF8.GetBytes(jwt.Key)),
            ClockSkew = TimeSpan.FromSeconds(30)
        };
    });
builder.Services.AddAuthorizationBuilder()
    .AddPolicy("AdminOnly", p => p.RequireRole("Admin"));

// CORS: allow only our front-end
builder.Services.AddCors(o => o.AddPolicy("Frontend", p => p
    .WithOrigins("http://localhost:4200")
    .AllowAnyHeader()
    .AllowAnyMethod()));

// Performance
builder.Services.AddMemoryCache();
builder.Services.AddOutputCache();
builder.Services.AddRateLimiter(o =>
{
    o.RejectionStatusCode = StatusCodes.Status429TooManyRequests;
    o.AddFixedWindowLimiter("fixed", l =>
    {
        l.PermitLimit = 100;
        l.Window = TimeSpan.FromMinutes(1);
        l.QueueLimit = 0;
    });
});

// Typed HttpClient via IHttpClientFactory
builder.Services.AddHttpClient<ExchangeRateClient>(c =>
{
    c.BaseAddress = new Uri("https://example.com/api/");
    c.Timeout = TimeSpan.FromSeconds(10);
});

// Health checks
builder.Services.AddHealthChecks()
    .AddCheck<DatabaseHealthCheck>("database");

// Background worker (hosted service = singleton)
builder.Services.AddHostedService<PendingOrderCleanupService>();

var app = builder.Build();

// Create the SQLite DB + seed data (demo only; real apps use migrations)
using (IServiceScope scope = app.Services.CreateScope())
{
    scope.ServiceProvider.GetRequiredService<AppDbContext>().Database.EnsureCreated();
}

// ---------------- 2. Middleware pipeline (ORDER MATTERS) ----------------
app.UseExceptionHandler();          // 1st: catches exceptions from everything below
app.UseStatusCodePages();           // empty 4xx/5xx -> ProblemDetails body

if (app.Environment.IsDevelopment())
{
    app.MapOpenApi();               // /openapi/v1.json
}
else
{
    app.UseHsts();
}

app.UseHttpsRedirection();

// Inline middleware: correlation id for tracing
app.Use(async (context, next) =>
{
    string id = context.Request.Headers["X-Correlation-Id"].FirstOrDefault() ?? Guid.NewGuid().ToString();
    context.Response.Headers["X-Correlation-Id"] = id;
    using (app.Logger.BeginScope(new Dictionary<string, object> { ["CorrelationId"] = id }))
    {
        await next(context);
    }
});

app.UseRequestTiming();             // our custom class-based middleware

app.UseCors("Frontend");            // after routing (implicit), before auth
app.UseAuthentication();            // who are you?
app.UseAuthorization();             // are you allowed?
app.UseRateLimiter();
app.UseOutputCache();

// ---------------- 3. Endpoints ----------------
app.MapControllers().RequireRateLimiting("fixed");
app.MapCategoryEndpoints();
app.MapHealthChecks("/health");
app.MapGet("/", () => "Shop API is running");

app.Run();

// Custom health check
public class DatabaseHealthCheck(AppDbContext db) : IHealthCheck
{
    public async Task<HealthCheckResult> CheckHealthAsync(HealthCheckContext context, CancellationToken ct = default)
        => await db.Database.CanConnectAsync(ct)
            ? HealthCheckResult.Healthy("Database reachable")
            : HealthCheckResult.Unhealthy("Cannot reach database");
}

// Makes Program visible to WebApplicationFactory<Program> in integration tests
public partial class Program;
```

Key points to explain:

| Line(s) | What to say |
|---|---|
| `AddOptions<JwtOptions>().Bind(...).ValidateDataAnnotations().ValidateOnStart()` | Fail fast: app refuses to start with a bad/missing JWT key (Ch. 5) |
| `AddDbContext` | DbContext is registered **scoped** – one per request (Ch. 4, 9) |
| `AddScoped<IProductService, ProductService>` | Depend on abstractions → testable with Moq (Ch. 4, 13) |
| `AddControllers(o => o.Filters.Add<LogActionFilter>())` | Global MVC filter (Ch. 8) |
| `AddProblemDetails` + `AddExceptionHandler` | Consistent RFC 7807 errors (Ch. 11) |
| `AddAuthentication().AddJwtBearer(...)` + `AddAuthorizationBuilder().AddPolicy` | JWT + policy-based authz (Ch. 10) |
| `AddRateLimiter`, `AddOutputCache`, `AddMemoryCache` | Performance & protection (Ch. 12) |
| `AddHttpClient<ExchangeRateClient>` | IHttpClientFactory, no socket exhaustion (Ch. 12) |
| `AddHostedService<PendingOrderCleanupService>` | Background work (Ch. 12) |
| `app.UseExceptionHandler()` **first** | It can only catch exceptions from middleware registered **after** it (Ch. 3) |
| `UseAuthentication()` before `UseAuthorization()` | You must know *who* before deciding *what* (Ch. 10) |
| `public partial class Program;` | Lets `WebApplicationFactory<Program>` find the entry point (Ch. 13) |

⚠️ **Interview trap:** `EnsureCreated()` is used here only for a demo. In real apps use **migrations** (`dotnet ef migrations add`, `dotnet ef database update` or a migration bundle/script in CI). `EnsureCreated` and migrations **don't mix**.

---

## 17.4 The domain model and DbContext

```csharp
namespace ShopApi.Models;

public class Category
{
    public int Id { get; set; }
    public string Name { get; set; } = "";
    public List<Product> Products { get; set; } = new();
}

public class Product
{
    public int Id { get; set; }
    public string Name { get; set; } = "";
    public decimal Price { get; set; }
    public int Stock { get; set; }
    public int CategoryId { get; set; }
    public Category? Category { get; set; }
    public bool IsDeleted { get; set; }

    // Optimistic concurrency token: changed on every update by our code.
    public Guid Version { get; set; } = Guid.NewGuid();
}

public class Order
{
    public int Id { get; set; }
    public string CustomerEmail { get; set; } = "";
    public DateTime CreatedUtc { get; set; } = DateTime.UtcNow;
    public string Status { get; set; } = "Pending";
    public List<OrderItem> Items { get; set; } = new();
}

public class OrderItem
{
    public int Id { get; set; }
    public int OrderId { get; set; }
    public int ProductId { get; set; }
    public Product? Product { get; set; }
    public int Quantity { get; set; }
    public decimal UnitPrice { get; set; }
}
```

```csharp
using Microsoft.EntityFrameworkCore;
using ShopApi.Models;

namespace ShopApi.Data;

public class AppDbContext(DbContextOptions<AppDbContext> options) : DbContext(options)
{
    public DbSet<Category> Categories => Set<Category>();
    public DbSet<Product> Products => Set<Product>();
    public DbSet<Order> Orders => Set<Order>();
    public DbSet<OrderItem> OrderItems => Set<OrderItem>();

    protected override void OnModelCreating(ModelBuilder b)
    {
        b.Entity<Category>(e =>
        {
            e.Property(c => c.Name).HasMaxLength(100).IsRequired();
            e.HasIndex(c => c.Name).IsUnique();
        });

        b.Entity<Product>(e =>
        {
            e.Property(p => p.Name).HasMaxLength(200).IsRequired();
            e.Property(p => p.Price).HasPrecision(10, 2);
            e.Property(p => p.Version).IsConcurrencyToken();
            e.HasOne(p => p.Category)
             .WithMany(c => c.Products)
             .HasForeignKey(p => p.CategoryId)
             .OnDelete(DeleteBehavior.Restrict);
            e.HasQueryFilter(p => !p.IsDeleted);   // global soft-delete filter
        });

        b.Entity<OrderItem>(e =>
        {
            e.Property(i => i.UnitPrice).HasPrecision(10, 2);
            e.HasOne(i => i.Product).WithMany().HasForeignKey(i => i.ProductId);
        });

        b.Entity<Category>().HasData(
            new Category { Id = 1, Name = "Electronics" },
            new Category { Id = 2, Name = "Books" });

        b.Entity<Product>().HasData(
            new Product { Id = 1, Name = "Laptop", Price = 800m, Stock = 10, CategoryId = 1, Version = new Guid("11111111-1111-1111-1111-111111111111") },
            new Product { Id = 2, Name = "Mouse", Price = 20m, Stock = 100, CategoryId = 1, Version = new Guid("22222222-2222-2222-2222-222222222222") },
            new Product { Id = 3, Name = "C# in Depth", Price = 45m, Stock = 30, CategoryId = 2, Version = new Guid("33333333-3333-3333-3333-333333333333") });
    }
}
```

| Feature | Where | Interview topic |
|---|---|---|
| Fluent API (`HasMaxLength`, `HasPrecision`, `HasIndex().IsUnique()`) | `OnModelCreating` | Fluent API vs data annotations |
| `IsConcurrencyToken()` on `Version` | Product | Optimistic concurrency (two users edit same row) |
| `DeleteBehavior.Restrict` | Product→Category | Prevent accidental cascade deletes |
| `HasQueryFilter(p => !p.IsDeleted)` | Product | Soft delete; bypass with `IgnoreQueryFilters()` |
| `HasData(...)` | Seed | Seeding; note seed rows need fixed keys and fixed values |

> Why is `Version` a `Guid` set by our code instead of SQL Server `rowversion`? Because SQLite has no `rowversion`. On SQL Server you'd use `[Timestamp] public byte[] RowVersion { get; set; }` and the database updates it automatically.

---

## 17.5 DTOs — never expose entities directly

```csharp
using System.ComponentModel.DataAnnotations;

namespace ShopApi.Dtos;

public record ProductDto(int Id, string Name, decimal Price, int Stock, string Category);

public record CreateProductRequest(
    [Required, StringLength(200, MinimumLength = 2)] string Name,
    [Range(0.01, 1_000_000)] decimal Price,
    [Range(0, int.MaxValue)] int Stock,
    [Range(1, int.MaxValue)] int CategoryId);

public record UpdateProductRequest(
    [Required, StringLength(200, MinimumLength = 2)] string Name,
    [Range(0.01, 1_000_000)] decimal Price,
    [Range(0, int.MaxValue)] int Stock);

public record PagedResult<T>(IReadOnlyList<T> Items, int Page, int PageSize, int TotalCount);

public record LoginRequest([Required] string UserName, [Required] string Password);
```

- **Records** = immutable, value equality, concise → perfect for DTOs.
- Validation attributes on positional record parameters work with `[ApiController]` automatic validation.
- `CreateProductRequest` has **no `Id`, `IsDeleted`, `Version`** → clients can't over-post those fields (mass assignment protection).

---

## 17.6 The service layer (business logic + EF Core)

```csharp
using Microsoft.EntityFrameworkCore;
using ShopApi.Data;
using ShopApi.Dtos;
using ShopApi.Models;

namespace ShopApi.Services;

public interface IProductService
{
    Task<PagedResult<ProductDto>> GetPageAsync(int page, int pageSize, string? search, CancellationToken ct);
    Task<ProductDto?> GetByIdAsync(int id, CancellationToken ct);
    Task<ProductDto> CreateAsync(CreateProductRequest request, CancellationToken ct);
    Task<bool> UpdateAsync(int id, UpdateProductRequest request, CancellationToken ct);
    Task<bool> DeleteAsync(int id, CancellationToken ct);
}

public class ProductService(AppDbContext db, ILogger<ProductService> logger) : IProductService
{
    public async Task<PagedResult<ProductDto>> GetPageAsync(int page, int pageSize, string? search, CancellationToken ct)
    {
        page = Math.Max(page, 1);
        pageSize = Math.Clamp(pageSize, 1, 100);

        IQueryable<Product> query = db.Products.AsNoTracking();
        if (!string.IsNullOrWhiteSpace(search))
            query = query.Where(p => p.Name.Contains(search));

        int total = await query.CountAsync(ct);

        // Projection (Select) => only needed columns, JOIN generated for Category, no N+1.
        List<ProductDto> items = await query
            .OrderBy(p => p.Id)
            .Skip((page - 1) * pageSize)
            .Take(pageSize)
            .Select(p => new ProductDto(p.Id, p.Name, p.Price, p.Stock, p.Category!.Name))
            .ToListAsync(ct);

        return new PagedResult<ProductDto>(items, page, pageSize, total);
    }

    public Task<ProductDto?> GetByIdAsync(int id, CancellationToken ct) =>
        db.Products.AsNoTracking()
            .Where(p => p.Id == id)
            .Select(p => new ProductDto(p.Id, p.Name, p.Price, p.Stock, p.Category!.Name))
            .FirstOrDefaultAsync(ct);

    public async Task<ProductDto> CreateAsync(CreateProductRequest request, CancellationToken ct)
    {
        Category category = await db.Categories.FindAsync([request.CategoryId], ct)
            ?? throw new ArgumentException($"Category {request.CategoryId} does not exist.");

        var product = new Product
        {
            Name = request.Name,
            Price = request.Price,
            Stock = request.Stock,
            CategoryId = category.Id
        };
        db.Products.Add(product);
        await db.SaveChangesAsync(ct);

        logger.LogInformation("Created product {ProductId} {ProductName}", product.Id, product.Name);
        return new ProductDto(product.Id, product.Name, product.Price, product.Stock, category.Name);
    }

    public async Task<bool> UpdateAsync(int id, UpdateProductRequest request, CancellationToken ct)
    {
        Product? product = await db.Products.FindAsync([id], ct);
        if (product is null) return false;

        product.Name = request.Name;
        product.Price = request.Price;
        product.Stock = request.Stock;
        product.Version = Guid.NewGuid();
        await db.SaveChangesAsync(ct);   // throws DbUpdateConcurrencyException on conflict
        return true;
    }

    public async Task<bool> DeleteAsync(int id, CancellationToken ct)
    {
        Product? product = await db.Products.FindAsync([id], ct);
        if (product is null) return false;

        product.IsDeleted = true;        // soft delete
        await db.SaveChangesAsync(ct);
        return true;
    }
}
```

What to highlight:

1. **`AsNoTracking()`** for read-only queries → faster, less memory.
2. **Projection with `Select` into a DTO** → SQL selects only needed columns and joins `Category` automatically — **no N+1**, no `Include` needed.
3. **`Math.Clamp(pageSize, 1, 100)`** → never let a client request 1,000,000 rows.
4. **`OrderBy` before `Skip/Take`** → paging without ordering is non-deterministic.
5. **`CancellationToken` everywhere** → if the client disconnects, the DB query is cancelled.
6. **Structured logging**: `"Created product {ProductId}"` – not string interpolation.
7. **Soft delete** sets a flag; the global filter hides the row.
8. **Concurrency**: `SaveChangesAsync` throws `DbUpdateConcurrencyException` if `Version` changed since load → mapped to **409 Conflict** by the exception handler.

---

## 17.7 The controller — thin and HTTP-focused

```csharp
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Mvc;
using ShopApi.Dtos;
using ShopApi.Services;

namespace ShopApi.Controllers;

[ApiController]
[Route("api/[controller]")]
public class ProductsController(IProductService service) : ControllerBase
{
    // GET api/products?page=1&pageSize=10&search=lap
    [HttpGet]
    public async Task<ActionResult<PagedResult<ProductDto>>> GetAll(
        [FromQuery] int page = 1,
        [FromQuery] int pageSize = 10,
        [FromQuery] string? search = null,
        CancellationToken ct = default)
        => Ok(await service.GetPageAsync(page, pageSize, search, ct));

    // GET api/products/5
    [HttpGet("{id:int}", Name = nameof(GetById))]
    [ProducesResponseType(StatusCodes.Status200OK)]
    [ProducesResponseType(StatusCodes.Status404NotFound)]
    public async Task<ActionResult<ProductDto>> GetById(int id, CancellationToken ct)
    {
        ProductDto? product = await service.GetByIdAsync(id, ct);
        return product is null ? NotFound() : Ok(product);
    }

    // POST api/products   (body validated automatically by [ApiController] -> 400)
    [HttpPost]
    [Authorize]
    [ProducesResponseType(StatusCodes.Status201Created)]
    [ProducesResponseType(StatusCodes.Status400BadRequest)]
    public async Task<ActionResult<ProductDto>> Create(CreateProductRequest request, CancellationToken ct)
    {
        ProductDto created = await service.CreateAsync(request, ct);
        return CreatedAtRoute(nameof(GetById), new { id = created.Id }, created);
    }

    // PUT api/products/5
    [HttpPut("{id:int}")]
    [Authorize]
    public async Task<IActionResult> Update(int id, UpdateProductRequest request, CancellationToken ct)
        => await service.UpdateAsync(id, request, ct) ? NoContent() : NotFound();

    // DELETE api/products/5
    [HttpDelete("{id:int}")]
    [Authorize(Policy = "AdminOnly")]
    public async Task<IActionResult> Delete(int id, CancellationToken ct)
        => await service.DeleteAsync(id, ct) ? NoContent() : NotFound();
}
```

| Endpoint | Success | Failure codes |
|---|---|---|
| `GET /api/products` | 200 + `PagedResult<ProductDto>` | 429 (rate limit) |
| `GET /api/products/{id:int}` | 200 | 404 ProblemDetails |
| `POST /api/products` | **201 Created** + `Location` header | 400 validation, 401 no token |
| `PUT /api/products/{id}` | **204 No Content** | 400, 401, 404, 409 conflict |
| `DELETE /api/products/{id}` | 204 | 401, **403 (not Admin)**, 404 |

⚠️ **Interview trap (found while verifying this code):** we first had `[Produces("application/json")]` on the controller. A test then failed: the 404 came back as `application/json` instead of `application/problem+json`. `[Produces]` forces the content type for **every** response, including ProblemDetails. We removed it.

---

## 17.8 Auth — issuing a demo JWT

```csharp
using System.ComponentModel.DataAnnotations;

namespace ShopApi.Options;

public class JwtOptions
{
    public const string SectionName = "Jwt";

    [Required] public string Issuer { get; set; } = "";
    [Required] public string Audience { get; set; } = "";

    // HS256 needs at least 32 bytes. In real apps keep this in Key Vault / user-secrets.
    [Required, MinLength(32)] public string Key { get; set; } = "";

    [Range(1, 1440)] public int ExpiryMinutes { get; set; } = 60;
}
```

```csharp
using System.IdentityModel.Tokens.Jwt;
using System.Security.Claims;
using System.Text;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Options;
using Microsoft.IdentityModel.Tokens;
using ShopApi.Dtos;
using ShopApi.Options;

namespace ShopApi.Controllers;

[ApiController]
[Route("api/[controller]")]
public class AuthController(IOptions<JwtOptions> jwtOptions) : ControllerBase
{
    // DEMO ONLY: hard-coded users. Real apps use ASP.NET Core Identity / Entra ID and hashed passwords.
    private static readonly Dictionary<string, (string Password, string Role)> Users = new()
    {
        ["admin"] = ("admin123", "Admin"),
        ["user"] = ("user123", "User")
    };

    [HttpPost("token")]
    public IActionResult Token(LoginRequest request)
    {
        if (!Users.TryGetValue(request.UserName, out var u) || u.Password != request.Password)
            return Unauthorized();

        JwtOptions o = jwtOptions.Value;
        var claims = new[]
        {
            new Claim(JwtRegisteredClaimNames.Sub, request.UserName),
            new Claim(ClaimTypes.Name, request.UserName),
            new Claim(ClaimTypes.Role, u.Role),
            new Claim(JwtRegisteredClaimNames.Jti, Guid.NewGuid().ToString())
        };

        var key = new SymmetricSecurityKey(Encoding.UTF8.GetBytes(o.Key));
        var token = new JwtSecurityToken(
            issuer: o.Issuer,
            audience: o.Audience,
            claims: claims,
            expires: DateTime.UtcNow.AddMinutes(o.ExpiryMinutes),
            signingCredentials: new SigningCredentials(key, SecurityAlgorithms.HmacSha256));

        return Ok(new { access_token = new JwtSecurityTokenHandler().WriteToken(token) });
    }
}
```

⚠️ Demo only: real systems never store plain-text passwords. Use ASP.NET Core Identity (hashed passwords) or an identity provider (Entra ID, Auth0, Duende) and keep the signing key in Key Vault/user-secrets.

---

## 17.9 Minimal API endpoints in the same app

```csharp
using Microsoft.AspNetCore.Http.HttpResults;
using Microsoft.EntityFrameworkCore;
using ShopApi.Data;
using ShopApi.Models;

namespace ShopApi.Endpoints;

public record CategoryDto(int Id, string Name, int ProductCount);
public record CreateCategoryRequest(string Name);

// Minimal API style: endpoints grouped in an extension method.
public static class CategoryEndpoints
{
    public static RouteGroupBuilder MapCategoryEndpoints(this IEndpointRouteBuilder app)
    {
        RouteGroupBuilder group = app.MapGroup("/api/categories").WithTags("Categories");

        group.MapGet("/", async (AppDbContext db, CancellationToken ct) =>
            await db.Categories
                .Select(c => new CategoryDto(c.Id, c.Name, c.Products.Count))
                .ToListAsync(ct))
            .CacheOutput(p => p.Expire(TimeSpan.FromSeconds(30)));

        group.MapGet("/{id:int}", async Task<Results<Ok<CategoryDto>, NotFound>> (int id, AppDbContext db, CancellationToken ct) =>
        {
            CategoryDto? c = await db.Categories
                .Where(x => x.Id == id)
                .Select(x => new CategoryDto(x.Id, x.Name, x.Products.Count))
                .FirstOrDefaultAsync(ct);
            return c is null ? TypedResults.NotFound() : TypedResults.Ok(c);
        });

        group.MapPost("/", async Task<Results<Created<CategoryDto>, ValidationProblem>> (CreateCategoryRequest req, AppDbContext db, CancellationToken ct) =>
        {
            if (string.IsNullOrWhiteSpace(req.Name))
                return TypedResults.ValidationProblem(new Dictionary<string, string[]>
                {
                    ["Name"] = ["Name is required."]
                });

            var category = new Category { Name = req.Name.Trim() };
            db.Categories.Add(category);
            await db.SaveChangesAsync(ct);
            return TypedResults.Created($"/api/categories/{category.Id}", new CategoryDto(category.Id, category.Name, 0));
        }).RequireAuthorization("AdminOnly");

        return group;
    }
}
```

Controllers and minimal APIs **can live in the same app**. `Results<Ok<T>, NotFound>` makes the possible responses part of the method signature → better OpenAPI docs and easier unit tests.

---

## 17.10 Cross-cutting pieces

### Custom middleware
```csharp
using System.Diagnostics;

namespace ShopApi.Middleware;

// Convention-based middleware: constructor takes RequestDelegate, InvokeAsync takes HttpContext.
// It is created ONCE (singleton-like) so inject scoped services into InvokeAsync, not the constructor.
public class RequestTimingMiddleware(RequestDelegate next, ILogger<RequestTimingMiddleware> logger)
{
    public async Task InvokeAsync(HttpContext context)
    {
        var sw = Stopwatch.StartNew();

        context.Response.OnStarting(() =>
        {
            context.Response.Headers["X-Elapsed-Ms"] = sw.ElapsedMilliseconds.ToString();
            return Task.CompletedTask;
        });

        await next(context);      // call the next middleware

        sw.Stop();
        logger.LogInformation("{Method} {Path} -> {StatusCode} in {Elapsed} ms",
            context.Request.Method, context.Request.Path, context.Response.StatusCode, sw.ElapsedMilliseconds);
    }
}

public static class RequestTimingMiddlewareExtensions
{
    public static IApplicationBuilder UseRequestTiming(this IApplicationBuilder app)
        => app.UseMiddleware<RequestTimingMiddleware>();
}
```
> Why `Response.OnStarting`? Headers can only be changed **before** the response starts. After `await next(context)` the body may already be sent, so setting a header there could throw *"Headers are read-only, response has already started."*

### Global exception handler
```csharp
using Microsoft.AspNetCore.Diagnostics;
using Microsoft.AspNetCore.Mvc;
using Microsoft.EntityFrameworkCore;

namespace ShopApi.Errors;

// .NET 8+: IExceptionHandler is the recommended central place to turn exceptions into ProblemDetails.
public class GlobalExceptionHandler(ILogger<GlobalExceptionHandler> logger, IProblemDetailsService problemDetails)
    : IExceptionHandler
{
    public async ValueTask<bool> TryHandleAsync(HttpContext context, Exception exception, CancellationToken ct)
    {
        (int status, string title) = exception switch
        {
            ArgumentException => (StatusCodes.Status400BadRequest, "Invalid request"),
            KeyNotFoundException => (StatusCodes.Status404NotFound, "Not found"),
            DbUpdateConcurrencyException => (StatusCodes.Status409Conflict, "The record was changed by someone else"),
            _ => (StatusCodes.Status500InternalServerError, "Something went wrong")
        };

        if (status == StatusCodes.Status500InternalServerError)
            logger.LogError(exception, "Unhandled exception");
        else
            logger.LogWarning("Handled {ExceptionType}: {Message}", exception.GetType().Name, exception.Message);

        context.Response.StatusCode = status;
        return await problemDetails.TryWriteAsync(new ProblemDetailsContext
        {
            HttpContext = context,
            Exception = exception,
            ProblemDetails = new ProblemDetails
            {
                Status = status,
                Title = title,
                // Never leak stack traces to clients; only show the message for 4xx.
                Detail = status < 500 ? exception.Message : null
            }
        });
    }
}
```

### Global action filter
```csharp
using Microsoft.AspNetCore.Mvc.Filters;

namespace ShopApi.Filters;

// Async action filter: runs code before and after every controller action.
public class LogActionFilter(ILogger<LogActionFilter> logger) : IAsyncActionFilter
{
    public async Task OnActionExecutionAsync(ActionExecutingContext context, ActionExecutionDelegate next)
    {
        string action = context.ActionDescriptor.DisplayName ?? "unknown";
        logger.LogDebug("Before {Action}", action);

        ActionExecutedContext executed = await next();   // runs the action (and inner filters)

        if (executed.Exception is null)
            logger.LogDebug("After {Action}", action);
    }
}
```

### Background service (singleton using scoped DbContext safely)
```csharp
using Microsoft.EntityFrameworkCore;
using ShopApi.Data;

namespace ShopApi.Background;

// BackgroundService is a SINGLETON. DbContext is SCOPED.
// So we create a scope per run with IServiceScopeFactory (avoid the captive-dependency bug).
public class PendingOrderCleanupService(IServiceScopeFactory scopeFactory, ILogger<PendingOrderCleanupService> logger)
    : BackgroundService
{
    protected override async Task ExecuteAsync(CancellationToken stoppingToken)
    {
        using var timer = new PeriodicTimer(TimeSpan.FromMinutes(5));
        do
        {
            try
            {
                await using AsyncServiceScope scope = scopeFactory.CreateAsyncScope();
                AppDbContext db = scope.ServiceProvider.GetRequiredService<AppDbContext>();

                DateTime cutoff = DateTime.UtcNow.AddDays(-1);
                int cancelled = await db.Orders
                    .Where(o => o.Status == "Pending" && o.CreatedUtc < cutoff)
                    .ExecuteUpdateAsync(s => s.SetProperty(o => o.Status, "Cancelled"), stoppingToken);

                if (cancelled > 0)
                    logger.LogInformation("Cancelled {Count} stale orders", cancelled);
            }
            catch (Exception ex) when (ex is not OperationCanceledException)
            {
                logger.LogError(ex, "Cleanup failed");   // never let the loop die
            }
        }
        while (await timer.WaitForNextTickAsync(stoppingToken));
    }
}
```

### Typed HttpClient
```csharp
namespace ShopApi.Clients;

public record ExchangeRate(string From, string To, decimal Rate);

// Typed HttpClient: IHttpClientFactory creates & pools the handler (no socket exhaustion, DNS refresh).
public class ExchangeRateClient(HttpClient http)
{
    public async Task<ExchangeRate?> GetRateAsync(string from, string to, CancellationToken ct)
    {
        using HttpResponseMessage response = await http.GetAsync($"rates?from={from}&to={to}", ct);
        if (!response.IsSuccessStatusCode) return null;
        return await response.Content.ReadFromJsonAsync<ExchangeRate>(ct);
    }
}
```

### Configuration
```json
{
  "ConnectionStrings": {
    "Shop": "Data Source=shop.db"
  },
  "Jwt": {
    "Issuer": "ShopApi",
    "Audience": "ShopApiClients",
    "Key": "DEMO-ONLY-change-me-this-key-must-be-32+-bytes!",
    "ExpiryMinutes": 60
  },
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning",
      "Microsoft.EntityFrameworkCore.Database.Command": "Warning"
    }
  },
  "AllowedHosts": "*"
}
```

---

## 17.11 The tests (17 passing)

### Unit tests — controller with Moq, service with SQLite in-memory
```csharp
using Microsoft.AspNetCore.Mvc;
using Microsoft.Data.Sqlite;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Logging.Abstractions;
using Moq;
using ShopApi.Controllers;
using ShopApi.Data;
using ShopApi.Dtos;
using ShopApi.Services;

namespace ShopApi.Tests;

// ---------- Unit test: controller with a MOCKED service ----------
public class ProductsControllerTests
{
    [Fact]
    public async Task GetById_WhenProductMissing_Returns404()
    {
        // Arrange
        var service = new Mock<IProductService>();
        service.Setup(s => s.GetByIdAsync(99, It.IsAny<CancellationToken>()))
               .ReturnsAsync((ProductDto?)null);
        var controller = new ProductsController(service.Object);

        // Act
        ActionResult<ProductDto> result = await controller.GetById(99, CancellationToken.None);

        // Assert
        Assert.IsType<NotFoundResult>(result.Result);
        service.Verify(s => s.GetByIdAsync(99, It.IsAny<CancellationToken>()), Times.Once);
    }

    [Fact]
    public async Task GetById_WhenProductExists_Returns200WithBody()
    {
        var dto = new ProductDto(1, "Laptop", 800m, 10, "Electronics");
        var service = new Mock<IProductService>();
        service.Setup(s => s.GetByIdAsync(1, It.IsAny<CancellationToken>())).ReturnsAsync(dto);

        ActionResult<ProductDto> result = await new ProductsController(service.Object).GetById(1, CancellationToken.None);

        OkObjectResult ok = Assert.IsType<OkObjectResult>(result.Result);
        Assert.Equal(dto, ok.Value);
    }
}

// ---------- Unit/"narrow integration" test: real service + SQLite in-memory ----------
public class ProductServiceTests : IDisposable
{
    private readonly SqliteConnection _connection;
    private readonly AppDbContext _db;
    private readonly ProductService _service;

    public ProductServiceTests()
    {
        // In-memory SQLite lives as long as the connection is open.
        _connection = new SqliteConnection("Data Source=:memory:");
        _connection.Open();
        _db = new AppDbContext(new DbContextOptionsBuilder<AppDbContext>().UseSqlite(_connection).Options);
        _db.Database.EnsureCreated();   // creates tables + seed (3 products)
        _service = new ProductService(_db, NullLogger<ProductService>.Instance);
    }

    [Fact]
    public async Task GetPage_ReturnsRequestedPage()
    {
        PagedResult<ProductDto> page = await _service.GetPageAsync(page: 2, pageSize: 2, search: null, CancellationToken.None);

        Assert.Equal(3, page.TotalCount);
        Assert.Single(page.Items);
        Assert.Equal("C# in Depth", page.Items[0].Name);
    }

    [Fact]
    public async Task Delete_IsSoft_AndHidesProductFromQueries()
    {
        Assert.True(await _service.DeleteAsync(2, CancellationToken.None));

        Assert.Null(await _service.GetByIdAsync(2, CancellationToken.None));
        // Row still exists when we bypass the global query filter
        Assert.True(await _db.Products.IgnoreQueryFilters().AnyAsync(p => p.Id == 2 && p.IsDeleted));
    }

    [Fact]
    public async Task Create_WithUnknownCategory_Throws()
    {
        var request = new CreateProductRequest("Pen", 1m, 5, CategoryId: 999);
        await Assert.ThrowsAsync<ArgumentException>(() => _service.CreateAsync(request, CancellationToken.None));
    }

    [Fact]
    public async Task Update_WhenRowChangedByOtherUser_ThrowsConcurrencyException()
    {
        // User A loads the row (now tracked with the original Version)...
        await _db.Products.FindAsync(1);
        // ...then user B changes it "behind our back"
        await _db.Database.ExecuteSqlRawAsync(
            "UPDATE Products SET Version = {0} WHERE Id = 1", Guid.NewGuid());

        var request = new UpdateProductRequest("Laptop Pro", 900m, 5);
        await Assert.ThrowsAsync<DbUpdateConcurrencyException>(() => _service.UpdateAsync(1, request, CancellationToken.None));
    }

    public void Dispose()
    {
        _db.Dispose();
        _connection.Dispose();
    }
}
```

### Integration tests — the real app hosted in memory
```csharp
using System.Net;
using System.Net.Http.Headers;
using System.Net.Http.Json;
using Microsoft.AspNetCore.Hosting;
using Microsoft.AspNetCore.Mvc.Testing;
using ShopApi.Dtos;

namespace ShopApi.Tests;

// Hosts the REAL app in memory (TestServer). Each factory gets its own SQLite file.
public class ShopApiFactory : WebApplicationFactory<Program>
{
    private readonly string _dbPath = Path.Combine(Path.GetTempPath(), $"shop-test-{Guid.NewGuid():N}.db");

    protected override void ConfigureWebHost(IWebHostBuilder builder)
    {
        builder.UseEnvironment("Development");
        builder.UseSetting("ConnectionStrings:Shop", $"Data Source={_dbPath};Pooling=False");
    }

    protected override void Dispose(bool disposing)
    {
        base.Dispose(disposing);
        if (File.Exists(_dbPath)) File.Delete(_dbPath);
    }
}

public class ProductsApiTests(ShopApiFactory factory) : IClassFixture<ShopApiFactory>
{
    private async Task<HttpClient> ClientAsAsync(string user, string password)
    {
        HttpClient client = factory.CreateClient();
        HttpResponseMessage res = await client.PostAsJsonAsync("/api/auth/token", new LoginRequest(user, password));
        res.EnsureSuccessStatusCode();
        TokenResponse token = (await res.Content.ReadFromJsonAsync<TokenResponse>())!;
        client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token.access_token);
        return client;
    }

    private record TokenResponse(string access_token);

    [Fact]
    public async Task GetProducts_Returns200AndSeedData()
    {
        HttpClient client = factory.CreateClient();

        HttpResponseMessage res = await client.GetAsync("/api/products?page=1&pageSize=10");

        Assert.Equal(HttpStatusCode.OK, res.StatusCode);
        Assert.True(res.Headers.Contains("X-Correlation-Id"));
        PagedResult<ProductDto>? body = await res.Content.ReadFromJsonAsync<PagedResult<ProductDto>>();
        Assert.True(body!.TotalCount >= 3);
    }

    [Fact]
    public async Task GetProduct_Unknown_Returns404ProblemDetails()
    {
        HttpResponseMessage res = await factory.CreateClient().GetAsync("/api/products/12345");

        Assert.Equal(HttpStatusCode.NotFound, res.StatusCode);
        Assert.Equal("application/problem+json", res.Content.Headers.ContentType?.MediaType);
    }

    [Fact]
    public async Task CreateProduct_WithoutToken_Returns401()
    {
        HttpResponseMessage res = await factory.CreateClient()
            .PostAsJsonAsync("/api/products", new CreateProductRequest("Pen", 1m, 5, 1));

        Assert.Equal(HttpStatusCode.Unauthorized, res.StatusCode);
    }

    [Fact]
    public async Task CreateProduct_InvalidBody_Returns400()
    {
        HttpClient client = await ClientAsAsync("user", "user123");

        HttpResponseMessage res = await client.PostAsJsonAsync("/api/products", new CreateProductRequest("", -5m, 1, 1));

        Assert.Equal(HttpStatusCode.BadRequest, res.StatusCode);
        string json = await res.Content.ReadAsStringAsync();
        Assert.Contains("Name", json);
        Assert.Contains("Price", json);
    }

    [Fact]
    public async Task CreateProduct_UnknownCategory_Returns400FromExceptionHandler()
    {
        HttpClient client = await ClientAsAsync("user", "user123");

        HttpResponseMessage res = await client.PostAsJsonAsync("/api/products", new CreateProductRequest("Pen", 1m, 5, 999));

        Assert.Equal(HttpStatusCode.BadRequest, res.StatusCode);
        Assert.Contains("Category 999 does not exist", await res.Content.ReadAsStringAsync());
    }

    [Fact]
    public async Task CreateProduct_WithToken_Returns201WithLocation()
    {
        HttpClient client = await ClientAsAsync("user", "user123");

        HttpResponseMessage res = await client.PostAsJsonAsync("/api/products", new CreateProductRequest("Keyboard", 30m, 15, 1));

        Assert.Equal(HttpStatusCode.Created, res.StatusCode);
        Assert.NotNull(res.Headers.Location);
    }

    [Fact]
    public async Task DeleteProduct_AsUser_Returns403()
    {
        HttpClient client = await ClientAsAsync("user", "user123");
        HttpResponseMessage res = await client.DeleteAsync("/api/products/3");
        Assert.Equal(HttpStatusCode.Forbidden, res.StatusCode);
    }

    [Fact]
    public async Task DeleteProduct_AsAdmin_Returns204()
    {
        HttpClient client = await ClientAsAsync("admin", "admin123");
        HttpResponseMessage create = await client.PostAsJsonAsync("/api/products", new CreateProductRequest("Temp", 1m, 1, 2));
        ProductDto created = (await create.Content.ReadFromJsonAsync<ProductDto>())!;

        HttpResponseMessage res = await client.DeleteAsync($"/api/products/{created.Id}");

        Assert.Equal(HttpStatusCode.NoContent, res.StatusCode);
    }

    [Fact]
    public async Task Login_WrongPassword_Returns401()
    {
        HttpResponseMessage res = await factory.CreateClient()
            .PostAsJsonAsync("/api/auth/token", new LoginRequest("admin", "wrong"));
        Assert.Equal(HttpStatusCode.Unauthorized, res.StatusCode);
    }

    [Fact]
    public async Task MinimalApi_Categories_Returns200()
    {
        HttpResponseMessage res = await factory.CreateClient().GetAsync("/api/categories");
        Assert.Equal(HttpStatusCode.OK, res.StatusCode);
        Assert.Contains("Electronics", await res.Content.ReadAsStringAsync());
    }

    [Fact]
    public async Task HealthCheck_ReturnsHealthy()
    {
        HttpResponseMessage res = await factory.CreateClient().GetAsync("/health");
        Assert.Equal(HttpStatusCode.OK, res.StatusCode);
        Assert.Equal("Healthy", await res.Content.ReadAsStringAsync());
    }
}
```

Verified result:
```text
Passed!  - Failed: 0, Passed: 17, Skipped: 0, Total: 17 - ShopApi.Tests.dll (net10.0)
```

---

## 17.12 How would you extend this? (common follow-up questions)

| Interviewer asks | Good answer |
|---|---|
| "Add orders with stock check" | Service method in a **transaction**: load products, check stock, create Order + OrderItems, decrement stock, `SaveChanges` once (atomic). Handle concurrency on `Stock`. |
| "Make it production ready" | Migrations, SQL Server, secrets in Key Vault, HTTPS, structured logging to App Insights/OpenTelemetry, health checks for readiness, rate limits per user, API versioning, CI/CD. |
| "Scale it" | Stateless API behind a load balancer, distributed cache (Redis), DB indexes, async messaging for slow work, read replicas. |
| "How would you test the orders feature?" | Unit-test the service (SQLite in-memory), integration-test the endpoint (WebApplicationFactory), test concurrency/edge cases (stock 0, invalid ids). |
| "What would you change in this design?" | Replace demo auth with Identity/Entra ID, add FluentValidation for complex rules, maybe the Result pattern instead of exceptions for expected errors, add idempotency keys for POST. |

---

## Try it yourself

1. Add `GET /api/products/{id}/exists` returning 200 or 404 — controller or minimal API?
2. Add an `[Range(1, 100)]` check so `pageSize` can't exceed 100 even without `Math.Clamp`. Where does the attribute go?
3. Write an integration test proving `PUT /api/products/1` without token returns 401.
4. What would break if `ProductService` were registered as **Singleton**?
5. Why does `GetPageAsync` call `CountAsync` separately instead of `ToList().Count`?

<details><summary>Answer</summary>

1. Either works. Minimal API: `group.MapGet("/{id:int}/exists", async (int id, AppDbContext db) => await db.Products.AnyAsync(p => p.Id == id) ? Results.Ok() : Results.NotFound());` — `AnyAsync` translates to SQL `EXISTS`, which is cheaper than loading the row.
2. On the action parameter: `[FromQuery, Range(1, 100)] int pageSize = 10`. `[ApiController]` validates action parameters too, returning 400.
3. ```csharp
   [Fact]
   public async Task UpdateProduct_WithoutToken_Returns401()
   {
       var res = await factory.CreateClient().PutAsJsonAsync("/api/products/1", new UpdateProductRequest("X Name", 1m, 1));
       Assert.Equal(HttpStatusCode.Unauthorized, res.StatusCode);
   }
   ```
4. It depends on the scoped `AppDbContext`. The DI container would throw at startup in Development (scope validation: *"Cannot consume scoped service from singleton"*). If validation is off, one DbContext would be shared by all requests: not thread-safe, it keeps growing its change tracker, and you get stale data. That is the **captive dependency** bug.
5. `CountAsync` runs `SELECT COUNT(*)` in the database and transfers one number. `ToList().Count` would load **every row** into memory just to count them.
</details>

## Quick revision
- Pipeline order: ExceptionHandler → HTTPS → custom → CORS → AuthN → AuthZ → RateLimiter → OutputCache → endpoints.
- Thin controllers, logic in scoped services, DTO records with validation.
- Reads: `AsNoTracking` + `Select` projection + `OrderBy` + `Skip/Take`.
- Errors: `IExceptionHandler` maps exceptions → ProblemDetails (400/404/409/500).
- JWT: issuer, audience, lifetime, signing key validated; roles in claims; `AdminOnly` policy.
- Background services are singletons → create a scope for DbContext.
- Tests: Moq for controller, SQLite in-memory for service, WebApplicationFactory for HTTP.
- `public partial class Program;` enables integration testing.

---

# 18. Theory Q&A (75 questions)

> **How to use this chapter**
> These are the questions that come up again and again in ASP.NET Core interviews for 2–4 years of experience: **75 core questions** grouped by topic, followed by **12 scenario-based questions**.
> Read each **Q**, try to answer **out loud** in 30–60 seconds, *then* read the model answer. Answers are deliberately short — the deep explanation lives in the chapter mentioned in brackets.
> 🔥 = asked very often. The **Scenario-based** section at the end is where good candidates stand out — practise those twice.

---

## Fundamentals

**Q1. 🔥 What is ASP.NET Core and how is it different from ASP.NET (Framework)?**

ASP.NET Core is a **cross-platform, open-source, high-performance** framework for web apps and APIs, running on modern .NET.
Compared to classic ASP.NET it:
- runs on Windows, Linux, macOS and in containers (no `System.Web`, no IIS dependency);
- has **built-in DI**, a **modular middleware pipeline** and unified MVC + Web API;
- reads configuration from many sources (JSON, env vars, Key Vault…);
- ships with **Kestrel**, a very fast built-in web server. (Ch. 2)

**Q2. What is Kestrel? Do I still need IIS or Nginx?**

Kestrel is the **cross-platform web server built into ASP.NET Core**. It can face the internet directly, but is often placed behind a **reverse proxy** (IIS, Nginx, YARP, Azure front ends) for TLS offloading, load balancing and request filtering.
On IIS the app runs **in-process** (inside `w3wp.exe`, fastest) or **out-of-process** (IIS forwards to Kestrel).

**Q3. 🔥 Explain `Program.cs` in a minimal hosting app.**

Three phases:
1. `WebApplication.CreateBuilder(args)` – sets up configuration, logging, the DI container.
2. `builder.Services.Add...` – **register services**.
3. `var app = builder.Build();` then `app.Use...` / `app.Map...` – build the **middleware pipeline and endpoints**; `app.Run()` starts listening.

```csharp
var builder = WebApplication.CreateBuilder(args);
builder.Services.AddControllers();                 // 1-2: services
var app = builder.Build();
app.UseAuthentication(); app.UseAuthorization();   // 3: pipeline
app.MapControllers();                              //    endpoints
app.Run();
```

The sample app's `Program.cs` (samples/ShopApi) is commented section by section.

**Q4. What is the Generic Host?**

The host is the object that owns the app's **lifetime**, DI container, configuration, logging and **hosted services**. `WebApplication` wraps the generic host and adds Kestrel + the HTTP pipeline. Worker services use the same host without HTTP (`Host.CreateApplicationBuilder`).

**Q5. What are environments and how do you use them?**

Set by the `ASPNETCORE_ENVIRONMENT` variable — `Development`, `Staging`, `Production` (**default is Production**). The environment decides which `appsettings.{Environment}.json` is loaded and enables checks like:

```csharp
if (app.Environment.IsDevelopment()) app.MapOpenApi();
else app.UseHsts();
```

**Q6. What is `launchSettings.json`?**

A **local-development-only** file (`Properties/launchSettings.json`) with profiles used by `dotnet run` and Visual Studio: URLs, environment variables, launch browser. It is **not deployed** and has no effect in production.

---

## Middleware

**Q7. 🔥 What is middleware?**

A component in the **request pipeline** that can inspect/modify the request, call the **next** component, and then inspect/modify the response on the way back. The pipeline is a chain of `RequestDelegate`s — like airport security checkpoints: each one can let you through or stop you. (Ch. 3)

**Q8. 🔥 `Use` vs `Run` vs `Map`?**

| Method | Meaning |
|---|---|
| `app.Use(...)` | Middleware that **can call `next`** |
| `app.Run(...)` | **Terminal** middleware — never calls next |
| `app.Map("/path", ...)` / `MapWhen` | **Branches** the pipeline by path/condition |
| `app.MapGet/MapControllers` | Map **endpoints** (routing), not branching |

**Q9. 🔥 Why does middleware order matter? Give the correct order.**

Middleware runs in registration order on the way **in** and reverse order on the way **out**. Typical order:

```
ExceptionHandler → HSTS → HttpsRedirection → StaticFiles → Routing → CORS
→ Authentication → Authorization → RateLimiter/OutputCache/custom → Endpoints
```

Classic bug: `UseAuthorization()` before `UseAuthentication()` → the user is always anonymous during authorization → 401 everywhere.

**Q10. How do you write custom middleware?**

Convention-based class: constructor takes `RequestDelegate next`, method `InvokeAsync(HttpContext context)`; register with `app.UseMiddleware<T>()`.

```csharp
public class RequestTimingMiddleware(RequestDelegate next, ILogger<RequestTimingMiddleware> logger)
{
    public async Task InvokeAsync(HttpContext context)
    {
        var sw = Stopwatch.StartNew();
        await next(context);
        logger.LogInformation("{Path} took {Ms} ms", context.Request.Path, sw.ElapsedMilliseconds);
    }
}
```

Alternatives: implement `IMiddleware`, or inline `app.Use(async (ctx, next) => { ...; await next(ctx); })`. The full version is `RequestTimingMiddleware` in the sample.

**Q11. Convention-based middleware vs `IMiddleware`?**

Convention-based middleware is created **once** (singleton-like) → inject **scoped** services as `InvokeAsync` parameters, **not** in the constructor.
`IMiddleware` is resolved from DI **per request** using its registered lifetime, so constructor injection of scoped services is fine — but you must register it (`builder.Services.AddScoped<MyMiddleware>()`).

**Q12. What is short-circuiting?**

A middleware writes a response **without calling `next`** — e.g. static files serving a file, the rate limiter returning **429**, or a custom API-key check returning **401**. Later middleware and the endpoint never run.

```csharp
app.Use(async (ctx, next) =>
{
    if (!ctx.Request.Headers.ContainsKey("X-Api-Key")) { ctx.Response.StatusCode = 401; return; }
    await next(ctx);
});
```

---

## Dependency Injection

**Q13. 🔥 What is Dependency Injection and why use it?**

Classes receive their dependencies (usually through the constructor) instead of creating them with `new`.
Benefits: **loose coupling**, easy **unit testing** with mocks, central **lifetime management**, swap implementations by changing one registration line. (Ch. 4)

**Q14. 🔥 Explain Singleton, Scoped, Transient.**

| Lifetime | One instance per… | Typical examples |
|---|---|---|
| **Transient** | every resolution | lightweight stateless helpers |
| **Scoped** | HTTP request (scope) | `DbContext`, unit of work, per-request services |
| **Singleton** | application | `IMemoryCache`, configuration, `TimeProvider`, caches |

```csharp
builder.Services.AddTransient<IPriceFormatter, PriceFormatter>();
builder.Services.AddScoped<IProductService, ProductService>();
builder.Services.AddSingleton(TimeProvider.System);
```

**Q15. 🔥 What is a captive dependency?**

A longer-lived service holding a shorter-lived one — typically a **singleton capturing a scoped `DbContext`**. The scoped object is effectively promoted to singleton: shared across requests, not thread-safe, stale data. In Development, scope validation throws *"Cannot consume scoped service … from singleton …"*. Fix: `IServiceScopeFactory` (see Q77).

**Q16. How do you register multiple implementations of one interface and pick one?**

- Register several and inject `IEnumerable<IRule>` to get **all** (injecting a single `IRule` gives the **last** registered).
- .NET 8+ **keyed services**:

```csharp
builder.Services.AddKeyedScoped<IPaymentStrategy, CardPayment>("card");
builder.Services.AddKeyedScoped<IPaymentStrategy, UpiPayment>("upi");

app.MapPost("/pay", ([FromKeyedServices("card")] IPaymentStrategy p) => p.PayAsync(10m));
```

**Q17. `AddScoped` vs `TryAddScoped`?**

`Add*` always adds another registration. `TryAdd*` adds **only if the service type is not registered yet** — libraries use it so they don't override your registrations. `TryAddEnumerable` prevents the same implementation being added twice to an `IEnumerable<T>` set.

**Q18. Constructor injection vs `[FromServices]` vs `IServiceProvider`?**

- **Constructor injection** – default choice: explicit and testable.
- `[FromServices]` – inject into a single action (useful for a rarely used, expensive service). Minimal APIs infer it automatically.
- `IServiceProvider.GetService<T>()` everywhere = **service locator anti-pattern** (hidden dependencies). OK only in factories/infrastructure.

---

## Configuration

**Q19. 🔥 How does configuration work? What's the precedence?**

Configuration is a key/value system built from **providers**; later providers override earlier ones. Default order (lowest → highest):

```
appsettings.json → appsettings.{Env}.json → User Secrets (Development only)
→ Environment variables → Command-line args
```

Nested keys use `:` (`Jwt:Key`); in environment variables use `__` (`Jwt__Key`) because `:` isn't allowed everywhere. (Ch. 5)

**Q20. 🔥 `IOptions` vs `IOptionsSnapshot` vs `IOptionsMonitor`?**

| | Lifetime | Sees config changes? | Use in |
|---|---|---|---|
| `IOptions<T>` | Singleton | ❌ read once | Most services |
| `IOptionsSnapshot<T>` | Scoped | ✅ per request | Scoped/transient services (not singletons) |
| `IOptionsMonitor<T>` | Singleton | ✅ `CurrentValue` + `OnChange` | Singletons, background services |

**Q21. How do you validate configuration at startup?**

```csharp
builder.Services.AddOptions<JwtOptions>()
    .Bind(builder.Configuration.GetSection(JwtOptions.SectionName))
    .ValidateDataAnnotations()
    .ValidateOnStart();          // app fails FAST at startup if config is invalid
```

Exactly what the sample app does — a missing JWT key crashes at startup, not on the first login.

---

## Web API, Routing & Model binding

**Q22. 🔥 What does `[ApiController]` do?**

- **Automatic 400** `ValidationProblemDetails` when `ModelState` is invalid (no `if (!ModelState.IsValid)` needed).
- **Binding-source inference**: complex types → `[FromBody]`, route values → `[FromRoute]`, others → `[FromQuery]`.
- **Attribute routing required**.
- Error status codes get **ProblemDetails** bodies. (Ch. 6)

**Q23. Conventional vs attribute routing?**

**Conventional**: one central template, e.g. `{controller=Home}/{action=Index}/{id?}` — used by MVC sites.
**Attribute**: routes on controllers/actions — the **standard for APIs**:

```csharp
[Route("api/[controller]")]
public class ProductsController : ControllerBase
{
    [HttpGet("{id:int}")] public IActionResult GetById(int id) => Ok();
}
```

**Q24. What are route constraints?**

Rules on route parameters: `{id:int}`, `{id:guid}`, `{slug:alpha:minlength(2)}`, `{page:range(1,100)}`. They help **select** the right endpoint (`/products/5` vs `/products/featured`). They are **not** input validation — a failed constraint gives **404**, not 400.

**Q25. 🔥 Explain model binding sources.**

| Attribute | Reads from |
|---|---|
| `[FromRoute]` | `/products/{id}` |
| `[FromQuery]` | `?page=2` |
| `[FromBody]` | JSON body (**only one** per action) |
| `[FromForm]` | form fields / files |
| `[FromHeader]` | request headers |
| `[FromServices]` / `[FromKeyedServices]` | DI container |

**Q26. 🔥 `IActionResult` vs `ActionResult<T>` vs returning `T`?**

- `T` → always 200 (errors only via exceptions).
- `IActionResult` → any status code, but OpenAPI doesn't know the body type.
- **`ActionResult<T>`** → best of both:

```csharp
public async Task<ActionResult<ProductDto>> GetById(int id, CancellationToken ct)
{
    ProductDto? product = await service.GetByIdAsync(id, ct);
    return product is null ? NotFound() : Ok(product);
}
```

**Q27. 🔥 Which status codes should a REST API return?**

| Code | When |
|---|---|
| 200 OK | GET, PUT/PATCH returning a body |
| **201 Created** + `Location` | POST created a resource (`CreatedAtAction/CreatedAtRoute`) |
| **204 No Content** | PUT/DELETE success without body |
| 400 Bad Request | validation errors |
| **401** Unauthorized | not authenticated |
| **403** Forbidden | authenticated but not allowed |
| 404 Not Found | resource doesn't exist |
| **409 Conflict** | concurrency conflict / duplicate |
| 422 Unprocessable Entity | business-rule validation (optional) |
| 429 Too Many Requests | rate limited |
| 500 | unhandled server error |

**Q28. PUT vs PATCH vs POST?**

**POST** creates a resource — **not idempotent**. **PUT** replaces the whole resource — **idempotent**. **PATCH** applies a partial update (JSON Patch / merge patch) — **not guaranteed idempotent**.

**Q29. How do you implement validation?**

- **Data annotations** on DTOs — like the sample's `CreateProductRequest`:
  ```csharp
  public record CreateProductRequest(
      [Required, StringLength(200, MinimumLength = 2)] string Name,
      [Range(0.01, 1_000_000)] decimal Price, ...);
  ```
- `IValidatableObject` for cross-field rules; **FluentValidation** for complex rules.
- `[ApiController]` returns 400 automatically. Business rules ("category must exist") belong in the **service**.

**Q30. What is content negotiation?**

The server chooses the response format from the client's `Accept` header. JSON is the default; add XML with `AddControllers().AddXmlSerializerFormatters()`. Set `options.ReturnHttpNotAcceptable = true` to return **406** for unsupported formats.

---

## Minimal APIs

**Q31. 🔥 Minimal APIs vs controllers?**

| | Minimal APIs | Controllers |
|---|---|---|
| Style | Lambdas mapped in `Program.cs`/extension methods | Classes + attributes |
| Ceremony | Low | Higher |
| Filters | Endpoint filters | Rich MVC filter pipeline |
| Validation | Built-in since .NET 10 (`AddValidation()`) | `[ApiController]` automatic 400 |
| Native AOT | ✅ | ❌ |
| Best for | Microservices, small APIs, vertical slices | Large APIs, teams used to MVC |

Both share routing, DI, auth, OpenAPI. (Ch. 7)

**Q32. What are `TypedResults` and route groups?**

```csharp
RouteGroupBuilder group = app.MapGroup("/api/categories").WithTags("Categories");

group.MapGet("/{id:int}", async Task<Results<Ok<CategoryDto>, NotFound>> (int id, AppDbContext db, CancellationToken ct) =>
    await db.Categories.Where(c => c.Id == id)
        .Select(c => new CategoryDto(c.Id, c.Name, c.Products.Count))
        .FirstOrDefaultAsync(ct) is { } dto
        ? TypedResults.Ok(dto)
        : TypedResults.NotFound());
```

`TypedResults` + `Results<...>` give accurate OpenAPI metadata and easy unit tests. `MapGroup` shares a prefix and conventions (auth, tags, filters) — see `CategoryEndpoints` in the sample.

**Q33. How do you add cross-cutting logic to minimal APIs?**

**Endpoint filters** — the minimal-API version of action filters:

```csharp
group.MapPost("/", Create).AddEndpointFilter(async (ctx, next) =>
{
    var logger = ctx.HttpContext.RequestServices.GetRequiredService<ILogger<Program>>();
    logger.LogInformation("Creating category");
    return await next(ctx);
});
```

---

## Filters

**Q34. 🔥 What are filters and their order?**

Filters run **inside the MVC pipeline**, around action execution:

```
Authorization → Resource → [model binding] → Action (before) → ACTION → Action (after)
→ Exception (if thrown) → Result (before) → RESULT EXECUTES → Result (after)
```

Exception filters only catch exceptions from MVC (actions/filters/binding), not from other middleware. (Ch. 8)

**Q35. Filters vs middleware?**

| | Middleware | Filters |
|---|---|---|
| Runs for | **Every request** | Only MVC/Razor Pages actions |
| Knows about | `HttpContext` | Action, arguments, `ModelState`, result |
| Use for | Logging, correlation id, exception handling, CORS | Validation, auditing specific actions, result shaping |

**Q36. How do you use DI inside a filter?**

- Global by type: `AddControllers(o => o.Filters.Add<LogActionFilter>())` — what the sample does.
- `[ServiceFilter(typeof(AuditFilter))]` — filter must be registered in DI.
- `[TypeFilter(typeof(AuditFilter))]` — created via DI without registering.
Plain attribute constructors can't receive DI services.

---

## EF Core

**Q37. 🔥 What is `DbContext` and what lifetime should it have?**

`DbContext` = a **session with the database**: queries, change tracking, `SaveChanges` (unit of work). Register it as **scoped** (`AddDbContext` does this) — one per request. It is **not thread-safe**. In singletons/background services use `IServiceScopeFactory` or `IDbContextFactory<T>`. (Ch. 9)

**Q38. 🔥 What is the N+1 problem and how do you fix it?**

Load N orders, then lazily load each order's items → **1 + N queries**.

```csharp
// ❌ N+1 (with lazy loading or a query inside the loop)
foreach (var o in await db.Orders.ToListAsync())
    total += o.Items.Sum(i => i.UnitPrice);          // one query per order

// ✅ one query with projection
var totals = await db.Orders
    .Select(o => new { o.Id, Total = o.Items.Sum(i => i.UnitPrice * i.Quantity) })
    .ToListAsync();
```

Other fixes: `Include()`, `AsSplitQuery()` for large collections. Spot it via EF SQL logging or APM.

**Q39. 🔥 Eager vs lazy vs explicit loading?**

```csharp
var o1 = await db.Orders.Include(o => o.Items).FirstAsync(o => o.Id == id);  // eager
var o2 = await db.Orders.FirstAsync(o => o.Id == id);
await db.Entry(o2).Collection(o => o.Items).LoadAsync();                       // explicit
// lazy: needs UseLazyLoadingProxies() + virtual navigations; loads on first access (N+1 risk)
```

**Q40. 🔥 What does `AsNoTracking()` do?**

Returns entities **without change tracking** → faster, less memory for **read-only** queries. Don't use it when you will modify and save the entities. Projections to DTOs (`Select(...)`) are untracked anyway.

**Q41. `IQueryable` vs `IEnumerable`?**

`IQueryable<T>` builds an **expression tree** that EF translates to **SQL** — filters run in the database. `IEnumerable<T>` runs in **memory**:

```csharp
db.Products.Where(p => p.Price > 100).ToListAsync();          // WHERE in SQL ✅
db.Products.AsEnumerable().Where(p => p.Price > 100).ToList(); // loads ALL rows, filters in C# ❌
```

**Q42. How do migrations work?**

```bash
dotnet ef migrations add AddProductVersion   # compare model to snapshot -> C# migration
dotnet ef database update                    # apply locally
dotnet ef migrations script --idempotent -o migrate.sql   # for production pipelines
```

In production prefer reviewed **SQL scripts** or **migration bundles** in CI/CD over `Database.Migrate()` at app startup.

**Q43. 🔥 How do you handle transactions in EF Core?**

One `SaveChangesAsync()` is **already a transaction**. For several `SaveChanges` calls or raw SQL + EF:

```csharp
await using var tx = await db.Database.BeginTransactionAsync(ct);
// ... SaveChangesAsync / ExecuteSqlAsync ...
await tx.CommitAsync(ct);          // disposing without commit = rollback
```

With retrying execution strategies (`EnableRetryOnFailure`) wrap it in `db.Database.CreateExecutionStrategy().ExecuteAsync(...)`.

**Q44. How do you call raw SQL / stored procedures safely?**

```csharp
var list = await db.Products.FromSql($"EXEC dbo.GetProductsByCategory {categoryId}").ToListAsync();
int rows = await db.Database.ExecuteSqlAsync($"UPDATE Products SET Stock = 0 WHERE Id = {id}");
```

Interpolated `FromSql`/`ExecuteSqlAsync` are **parameterised** (safe). Never concatenate user input into `FromSqlRaw`. Dapper is a common choice for heavy read queries.

**Q45. What are global query filters?**

```csharp
b.Entity<Product>().HasQueryFilter(p => !p.IsDeleted);   // added to EVERY query on Product
db.Products.IgnoreQueryFilters();                        // bypass when needed (admin, restore)
```

Used for **soft delete** (as in the sample) and multi-tenancy (`t => t.TenantId == _tenantId`).

---

## Authentication & Authorization

**Q46. 🔥 Authentication vs authorization? 401 vs 403?**

Authentication = **who are you?** Authorization = **what are you allowed to do?**
**401 Unauthorized** = not authenticated (missing/invalid/expired token). **403 Forbidden** = authenticated, but lacking permission. (Ch. 10)

**Q47. 🔥 How does JWT authentication work?**

1. User logs in → server issues a **signed** token `header.payload.signature` with claims (`sub`, `role`, `exp`).
2. Client sends `Authorization: Bearer <token>` on each request.
3. JwtBearer middleware validates **signature, issuer, audience, lifetime** and fills `HttpContext.User`.

Stateless — no server session. The payload is only Base64-encoded, **not encrypted**: never put secrets in it.

**Q48. Roles vs claims vs policies?**

```csharp
builder.Services.AddAuthorizationBuilder()
    .AddPolicy("AdminOnly", p => p.RequireRole("Admin"))
    .AddPolicy("HrOnly",   p => p.RequireClaim("department", "HR"));

[Authorize(Roles = "Admin")]         // role check
[Authorize(Policy = "AdminOnly")]    // policy check (recommended) – sample's Delete action
```

Roles = groups; claims = facts about the user; **policies** = named rules (can combine requirements or use a custom `IAuthorizationHandler`, e.g. "user owns this order").

**Q49. How do refresh tokens work?**

Access tokens are short-lived (5–60 min). A long-lived, **server-stored, revocable** refresh token is exchanged for a new access token without re-login. **Rotate** the refresh token on every use and revoke all tokens on logout or if reuse (theft) is detected.

**Q50. What is OAuth 2.0 / OpenID Connect?**

**OAuth 2.0** = delegated **authorization** (obtain an access token to call an API). **OpenID Connect** = **authentication** layer on top (ID token, user info). In real projects use an identity provider (Microsoft Entra ID, Auth0, Duende IdentityServer) with the **authorization code + PKCE** flow instead of writing your own login.

---

## Error handling & Logging

**Q51. 🔥 How do you handle exceptions globally?**

```csharp
builder.Services.AddProblemDetails();
builder.Services.AddExceptionHandler<GlobalExceptionHandler>();   // IExceptionHandler (.NET 8+)
...
app.UseExceptionHandler();   // FIRST in the pipeline
```

`GlobalExceptionHandler` (sample) maps `ArgumentException` → 400, `KeyNotFoundException` → 404, `DbUpdateConcurrencyException` → 409, others → 500, and writes **ProblemDetails** — without stack traces. (Ch. 11)

**Q52. What is ProblemDetails?**

The standard JSON error format (RFC 7807 / RFC 9457):

```json
{ "type": "https://tools.ietf.org/html/rfc9110#section-15.5.10", "title": "The record was changed by someone else",
  "status": 409, "traceId": "00-4bf9...-01" }
```

Clients get one consistent error shape; validation errors add an `errors` dictionary.

**Q53. 🔥 What is structured logging and why use message templates?**

```csharp
logger.LogInformation("Created product {ProductId} {ProductName}", product.Id, product.Name);  // ✅
logger.LogInformation($"Created product {product.Id}");                                         // ❌
```

Templates keep `ProductId` as a **queryable property** in Seq/Application Insights/Elastic, and avoid string allocation when the level is disabled. Interpolation flattens everything to text.

**Q54. Log levels?**

`Trace < Debug < Information < Warning < Error < Critical` (`None` disables). Production: usually `Information` for your code, `Warning` for `Microsoft.AspNetCore`:

```json
"Logging": { "LogLevel": { "Default": "Information", "Microsoft.AspNetCore": "Warning" } }
```

**Q55. What are health checks?**

```csharp
builder.Services.AddHealthChecks().AddCheck<DatabaseHealthCheck>("database");
app.MapHealthChecks("/health");
```

Load balancers / Kubernetes probe them. Separate **liveness** (process alive) from **readiness** (dependencies ready) using tags + filtered endpoints.

---

## Performance & Caching

**Q56. 🔥 What caching options exist in ASP.NET Core?**

| Type | API | Scope |
|---|---|---|
| In-memory | `IMemoryCache` | One server |
| Distributed | `IDistributedCache` (Redis, SQL) | Shared across servers |
| Hybrid (.NET 9+) | `HybridCache` | Memory + distributed, stampede protection |
| Output caching | `AddOutputCache` / `.CacheOutput()` | Whole responses, server-side |
| Response caching | `[ResponseCache]` | HTTP headers for browsers/proxies |

(Ch. 12)

**Q57. 🔥 Why `IHttpClientFactory` instead of `new HttpClient()`?**

`new HttpClient()` per call → **socket exhaustion** (sockets stuck in `TIME_WAIT`). One static client forever → misses **DNS changes**. The factory **pools and recycles handlers** and supports typed clients + resilience:

```csharp
builder.Services.AddHttpClient<ExchangeRateClient>(c =>
{
    c.BaseAddress = new Uri("https://example.com/api/");
    c.Timeout = TimeSpan.FromSeconds(10);
});
```

**Q58. What is rate limiting?**

Built-in middleware that limits requests per client/window: fixed window, sliding window, token bucket, concurrency. Rejected requests get **429**. The sample defines a `"fixed"` policy (100 requests/minute) and applies it with `MapControllers().RequireRateLimiting("fixed")`.

**Q59. How do you return large lists efficiently?**

**Paginate** (`Skip/Take`, or keyset pagination for deep pages), project to DTOs, `AsNoTracking`, filter in SQL, enable response compression, stream with `IAsyncEnumerable<T>` for huge results, and **clamp** page size (the sample clamps to 1–100).

---

## Async

**Q60. 🔥 Why use async/await in ASP.NET Core?**

While awaiting I/O (DB, HTTP, files), the **thread returns to the pool** and can serve other requests → **higher throughput/scalability**. It does not make one request faster, and CPU-bound work gains nothing.

**Q61. 🔥 Why avoid `.Result` / `.Wait()`?**

They **block** a thread-pool thread while waiting (sync-over-async). Under load this causes **thread-pool starvation** (everything slows down), and in apps with a synchronization context it can **deadlock**. Go "async all the way".

**Q62. Why avoid `async void`?**

The caller can't `await` it, and an exception inside it **can't be caught by the caller — it crashes the process**. Use `async Task`. Only exception: event handlers.

**Q63. What is `CancellationToken` for?**

Lets work stop early when the client disconnects or a timeout fires. In actions, a `CancellationToken` parameter is bound to `HttpContext.RequestAborted`; pass it down to EF Core and `HttpClient` calls (every method in the sample's `IProductService` takes one).

**Q64. `Task.WhenAll` vs awaiting one by one?**

```csharp
var a = GetRatesAsync(ct); var b = GetWeatherAsync(ct);
await Task.WhenAll(a, b);        // concurrent: total ≈ slowest call
```

`await A(); await B();` is sequential. ⚠️ Never run parallel queries on the **same `DbContext`** (not thread-safe).

---

## Testing

**Q65. 🔥 Unit vs integration tests in ASP.NET Core?**

**Unit tests** test one class in isolation with mocks (xUnit + Moq/NSubstitute). **Integration tests** host the real app in memory with **`WebApplicationFactory<Program>`** and call it over `HttpClient` — catching routing, DI, middleware, serialization and auth issues. (Ch. 13)

```csharp
public class ProductsApiTests(WebApplicationFactory<Program> factory) : IClassFixture<WebApplicationFactory<Program>>
{
    [Fact]
    public async Task GetById_returns_404_for_missing_product()
    {
        HttpResponseMessage res = await factory.CreateClient().GetAsync("/api/products/999");
        Assert.Equal(HttpStatusCode.NotFound, res.StatusCode);
    }
}
```

**Q66. How do you replace the database in integration tests?**

Override registrations in `ConfigureTestServices`: remove the real `DbContextOptions<AppDbContext>` and add **SQLite in-memory**, or use **Testcontainers** to run real SQL Server/Postgres in Docker. EF's InMemory provider isn't relational (no constraints/transactions), so it can hide bugs.

**Q67. What do you mock and what not?**

Mock **external boundaries you own an interface for**: email, payment gateway, third-party HTTP APIs, time (`TimeProvider`). Don't mock `DbContext`/EF internals — use a real lightweight database instead. Don't test framework behaviour.

---

## Security

**Q68. 🔥 What is CORS?**

A **browser** rule: JavaScript from origin A may call API origin B only if B answers with permission headers (`Access-Control-Allow-Origin`). Configure specific origins:

```csharp
builder.Services.AddCors(o => o.AddPolicy("Frontend", p => p
    .WithOrigins("http://localhost:4200").AllowAnyHeader().AllowAnyMethod()));
app.UseCors("Frontend");
```

CORS is **not** server protection — curl/Postman ignore it. Never combine `AllowAnyOrigin()` with credentials.

**Q69. 🔥 How do you prevent common attacks (OWASP)?**

| Attack | Defence |
|---|---|
| SQL injection | EF Core / parameterised queries |
| XSS | Razor auto-encoding, input validation, CSP headers |
| CSRF | Antiforgery tokens (cookie auth), SameSite cookies |
| Over-posting | Bind to DTOs, not entities |
| Broken access control | `[Authorize]`, policies, **resource ownership checks** |
| Secrets leak | Key Vault, no secrets in git/logs |
| Brute force / abuse | Rate limiting, lockout |

Plus HTTPS + HSTS and dependency vulnerability scanning. (Ch. 14)

**Q70. 🔥 How do you store secrets?**

Never in source control or `appsettings.json`. **Development**: `dotnet user-secrets set "Jwt:Key" "..."`. **Production**: **Azure Key Vault** accessed with **Managed Identity**, or platform secrets (App Service settings, Kubernetes secrets). Prefer passwordless (Entra ID) connections to SQL/Storage; rotate keys.

---

## Deployment

**Q71. How do you deploy an ASP.NET Core app?**

`dotnet publish -c Release` and deploy to **Azure App Service**, **containers** (Azure Container Apps, AKS), IIS (with the Hosting Bundle) or Linux + Nginx + systemd. Automate with CI/CD: build → test → publish → deploy to a **staging slot** → smoke test → swap. Since .NET 8, `dotnet publish /t:PublishContainer` builds a container image without a Dockerfile.

**Q72. Framework-dependent vs self-contained vs Native AOT?**

| | Size | Needs .NET runtime on server? | Notes |
|---|---|---|---|
| Framework-dependent | Small | ✅ | Default |
| Self-contained | Large | ❌ | Bundles runtime |
| Native AOT | Small, native | ❌ | Fast startup, low memory; no MVC/heavy reflection (minimal APIs OK) |

---

## Architecture

**Q73. 🔥 Explain SOLID briefly with an ASP.NET Core example.**

**S** – thin controllers; logic in services. **O** – add a new `IPaymentStrategy` class instead of editing a `switch`. **L** – implementations honour the interface (no `NotSupportedException`). **I** – small interfaces like `IProductReader`. **D** – controllers depend on `IProductService`, injected via DI. (Ch. 15)

**Q74. 🔥 Do you need the Repository pattern with EF Core?**

`DbContext` already is a **Unit of Work** and `DbSet<T>` a **repository**. Add **specific** repositories when you want to isolate EF from the domain (Clean Architecture), reuse complex queries, or simplify unit tests. Avoid generic repositories that expose `IQueryable<T>` — extra layer, no real abstraction.

**Q75. What are Clean Architecture and CQRS in one breath?**

**Clean Architecture**: domain at the centre; dependencies point **inward**; infrastructure implements interfaces defined by the application layer. **CQRS**: separate **commands** (writes) from **queries** (reads) so each is simple and optimisable. MediatR is a popular dispatcher but became **commercially licensed in 2025**, so many teams write a small dispatcher themselves.

---

## Scenario-based questions (where you stand out)

**Q76. 🔥 "Your API is slow. How do you investigate?"**

1. **Clarify & measure:** which endpoints, how slow (p95/p99), since when, for everyone or some users? Use **Application Insights / OpenTelemetry traces**, logs with timings (the sample's `RequestTimingMiddleware` adds `X-Elapsed-Ms`).
2. **Find where the time goes:**
   - **Database** – log EF SQL, count queries per request (**N+1**), missing **indexes**, execution plans, too much data (no paging/projection).
   - **External calls** – no timeouts, retry storms, sequential calls that could run in parallel.
   - **Threading** – `.Result`/`.Wait()` → thread-pool starvation (`dotnet-counters`).
   - **CPU/memory** – big serializations, GC pressure (`dotnet-trace`, profilers).
3. **Fix:** projection + `AsNoTracking`, indexes, pagination, caching (memory/Redis/output cache), async all the way, `IHttpClientFactory` with timeouts, background queues for slow work.
4. **Verify & prevent:** load test (k6, JMeter, NBomber), dashboards + alerts on latency.

**Q77. 🔥 "How do you use a scoped service (e.g. DbContext) inside a singleton or BackgroundService?"**

Never inject it directly — that's a captive dependency. Inject **`IServiceScopeFactory`**, create a scope per unit of work, resolve inside it, dispose the scope:

```csharp
public class CleanupWorker(IServiceScopeFactory scopes) : BackgroundService
{
    protected override async Task ExecuteAsync(CancellationToken stoppingToken)
    {
        using var timer = new PeriodicTimer(TimeSpan.FromMinutes(5));
        while (await timer.WaitForNextTickAsync(stoppingToken))
        {
            await using AsyncServiceScope scope = scopes.CreateAsyncScope();
            var db = scope.ServiceProvider.GetRequiredService<AppDbContext>();
            // ... use db for this run only ...
        }
    }
}
```

The sample's `PendingOrderCleanupService` follows exactly this pattern. `IDbContextFactory<T>` is an EF-specific alternative.

**Q78. 🔥 "Two users update the same record at the same time. What happens and how do you handle it?"**

By default **last write wins** — user A's changes are silently overwritten by user B.
Use **optimistic concurrency**:
1. Add a concurrency token — `[Timestamp] public byte[] RowVersion` (SQL Server), or a `Guid Version` configured with `IsConcurrencyToken()` and changed on every update (the sample's `Product`).
2. EF adds `WHERE Id = @id AND Version = @originalVersion`; 0 rows affected → **`DbUpdateConcurrencyException`**.
3. Return **409 Conflict** (the sample's `GlobalExceptionHandler` does) and let the client reload and retry/merge.
Over HTTP you can expose the version as an **ETag** and require `If-Match` → **412 Precondition Failed** on mismatch. Pessimistic locks are rarely right for web apps.

**Q79. 🔥 "How do you version an API?"**

| Strategy | Example | Notes |
|---|---|---|
| **URL segment** | `/api/v1/products` | Most common, visible, cache-friendly |
| Query string | `/api/products?api-version=1.0` | Easy default version |
| Header | `api-version: 1.0` | Clean URLs, less discoverable |
| Media type | `Accept: application/json;v=1` | Most "RESTful", most complex |

Use the **`Asp.Versioning.Mvc` / `Asp.Versioning.Http`** packages:

```csharp
builder.Services.AddApiVersioning(o =>
{
    o.DefaultApiVersion = new ApiVersion(1, 0);
    o.AssumeDefaultVersionWhenUnspecified = true;
    o.ReportApiVersions = true;                 // api-supported-versions header
}).AddMvc();

[ApiVersion("1.0")]
[Route("api/v{version:apiVersion}/[controller]")]
public class ProductsController : ControllerBase { }
```

Bump the major version only for **breaking changes**; adding optional fields isn't breaking. Announce deprecations with a timeline.

**Q80. 🔥 "How do you make an endpoint idempotent?"**

- GET, PUT, DELETE are idempotent by design — keep them that way (no side effects in GET!).
- For POST, require an **`Idempotency-Key`** header (client sends a GUID per logical operation and reuses it on retry). Store key + status + response with a **unique constraint**, in the **same transaction** as the work; on a repeated key return the stored response instead of executing again. Expire keys (e.g. 24 h).
- For message consumers (at-least-once delivery), store processed **message IDs** and skip duplicates.
- Natural uniqueness helps too: a unique index on `(OrderId)` in a Payments table makes double payment impossible. (Ch. 15)

**Q81. 🔥 "How do you secure secrets (connection strings, API keys)?"**

1. Nothing secret in git (add secret scanning to CI).
2. Development: **User Secrets**.
3. Cloud: **Azure Key Vault** + **Managed Identity** — no credentials anywhere:
   ```csharp
   builder.Configuration.AddAzureKeyVault(
       new Uri("https://shop-kv.vault.azure.net/"), new DefaultAzureCredential());
   ```
4. Even better: **passwordless** connections (Entra ID auth to Azure SQL/Storage/Service Bus).
5. Least privilege, rotation, and **never log** secrets or tokens.

**Q82. 🔥 "How would you design a CRUD API end-to-end?"**

1. **Contract:** resource `/api/products` – `GET` (paging, filter, sort), `GET /{id}`, `POST`, `PUT /{id}`, `DELETE /{id}`; request/response **DTOs** separate from entities; status codes 200 / 201 + Location / 204 / 400 / 404 / 409.
2. **Layers:** thin controller → `IProductService` → EF Core `DbContext` (scoped); projection + `AsNoTracking` for reads.
3. **Validation:** data annotations / FluentValidation → automatic 400 ProblemDetails; business rules in the service.
4. **Errors:** `UseExceptionHandler` + `IExceptionHandler` → ProblemDetails; concurrency token → 409.
5. **Data:** migrations, indexes, soft delete via query filter, optimistic concurrency.
6. **Security:** JWT bearer, `[Authorize]`, an `AdminOnly` policy for delete, CORS, HTTPS, rate limiting.
7. **Ops:** structured logging + correlation id, health checks, OpenAPI, versioning, caching where safe.
8. **Tests:** unit tests for services, integration tests with `WebApplicationFactory`.
9. **Delivery:** CI/CD pipeline, container or App Service, staging slot.
This is the sample app — walk through it using chapter 17.

**Q83. 🔥 "What happens when a request hits your API, step by step?"**

1. Client resolves DNS and opens a TCP + TLS connection (often via a load balancer / reverse proxy).
2. **Kestrel** parses the HTTP request and creates an **`HttpContext`**; a **DI scope** is created for the request.
3. The request passes through **middleware** in order: exception handler → HTTPS redirection → correlation id / timing → **routing** (selects an endpoint) → CORS → **authentication** (JWT → `HttpContext.User`) → **authorization** (policy check, 401/403) → rate limiter → output cache.
4. The **endpoint** runs. For a controller: authorization & resource **filters** → **model binding** + **validation** (`[ApiController]` → 400) → action filters → controller **created via DI** → action → service → EF Core → database.
5. The result is executed: **content negotiation**, `System.Text.Json` serialization, status code + headers.
6. The response travels **back out** through the middleware (timing logs the duration, headers added) and Kestrel writes it to the socket.
7. The request **scope is disposed** (DbContext disposed). Any exception along the way is caught by the exception handler → ProblemDetails.

**Q84. "A third-party API you depend on is slow or down. What do you do?"**

Use `IHttpClientFactory` with **timeouts**, **retries with exponential backoff + jitter** (only for idempotent calls), and a **circuit breaker**:

```csharp
builder.Services.AddHttpClient<ExchangeRateClient>(...)
    .AddStandardResilienceHandler();   // Microsoft.Extensions.Http.Resilience: retry + circuit breaker + timeouts
```

Add a fallback (cached last-known value), move non-urgent calls to a **background queue**, and monitor/alert on dependency failures.

**Q85. "Your app works locally but returns 500 in production. How do you debug?"**

Take the **traceId** from the ProblemDetails response and search **logs / Application Insights**. Usual suspects: missing configuration (env var, Key Vault permission, connection string), wrong environment name, migrations not applied, proxy/HTTPS headers (`UseForwardedHeaders`), file permissions. Raise the log level temporarily — never enable the developer exception page in production.

**Q86. "How would you implement a long-running task (e.g. generating a report) from an API?"**

Don't run it inside the request. Return **202 Accepted** with a status URL, enqueue the job (a `Channel<T>` + `BackgroundService` for simple cases; Azure Service Bus / Hangfire / Azure Functions for durable jobs), store job status in the DB, and let the client **poll** `GET /reports/{id}` or get notified (SignalR, webhook, email).

**Q87. "You must make a breaking change to a DTO used by mobile apps. How?"**

Don't break existing clients: prefer **additive** changes (new optional fields); otherwise ship a **new API version** (v2), keep v1 running, monitor v1 traffic, announce deprecation (e.g. `Deprecation`/`Sunset` headers + docs), and remove v1 only after clients migrate.

---

> 💡 **Final tip:** For any scenario question, use the structure **Clarify → Measure/Identify → Fix → Prevent**, and name real tools/APIs (Application Insights, `IServiceScopeFactory`, `DbUpdateConcurrencyException`, `Asp.Versioning`, `AddStandardResilienceHandler`). Concrete names signal real experience.

---

# 19. Mock Interview & Cheat Sheet

> **How to use this chapter**
> Part 1 is a **45-minute mock interview** — do it with a friend, or alone with a timer, answering **out loud** before opening each answer. Part 2 is a **cheat sheet** to skim on the morning of the interview. Code here matches the sample app (samples/ShopApi); chapter 17 walks through it end-to-end and chapter 18 has the full Q&A bank.

---

# Part 1 — 45-minute mock interview

| Time | Section |
|---|---|
| 0–5 min | Introduction ("Tell me about yourself") |
| 5–20 min | 15 rapid-fire questions (~1 min each) |
| 20–40 min | 3 coding tasks |
| 40–45 min | Your questions for the interviewer |

## 1. "Tell me about yourself" (aim for 90 seconds)

Use the **Present → Past → Projects → Future** structure. Fill in the brackets with *your* facts:

> "Hi, I'm **[Name]**. I'm a .NET developer with about **two years** of experience at **[Company]**, where I build **REST APIs with ASP.NET Core** and **EF Core** on **SQL Server**, deployed to **[Azure App Service / containers]**.
>
> Most recently I worked on **[project — e.g. an order-management API used by 3 internal apps]**. I owned features end-to-end: designing endpoints and DTOs, writing services and EF Core queries, adding **JWT authentication with role-based policies**, global error handling with **ProblemDetails**, and **unit + integration tests** with xUnit and `WebApplicationFactory`.
>
> One thing I'm proud of: **[a measurable win — e.g. I found an N+1 query on the orders list, replaced it with a projection and added an index, and the endpoint went from 2.3 s to 180 ms]**.
>
> I also **[review PRs / set up the CI pipeline / mentor an intern]**. I'm now looking for a role where I can **[work on larger-scale/distributed systems, go deeper into cloud and architecture]**, which is why this position interests me."

✅ Do: numbers, one concrete story, technologies that match the job description.
❌ Don't: recite your CV line by line, talk about personal life, criticise your current employer.

---

## 2. Rapid-fire round (15 questions)

Answer each in **under a minute**, then open the answer.

**1. What is middleware, and why does order matter?**
<details><summary>Answer</summary>

A component in the request pipeline that handles the request, optionally calls `next`, and handles the response on the way back. Order matters because each one sees only what earlier ones did: exception handler must be first to catch everything; `UseAuthentication` must come before `UseAuthorization`; CORS before auth; endpoints last.
</details>

**2. Singleton vs Scoped vs Transient — and where does `DbContext` go?**
<details><summary>Answer</summary>

Singleton = one for the app; Scoped = one per HTTP request; Transient = new every time it's resolved. `DbContext` is **scoped** (one unit of work per request, not thread-safe).
</details>

**3. What is a captive dependency?**
<details><summary>Answer</summary>

A longer-lived service holding a shorter-lived one — e.g. a singleton with a scoped `DbContext` injected. The DbContext then lives forever and is shared across threads. Fix: inject `IServiceScopeFactory` and create a scope per operation.
</details>

**4. `IOptions` vs `IOptionsSnapshot` vs `IOptionsMonitor`?**
<details><summary>Answer</summary>

`IOptions` – singleton, read once. `IOptionsSnapshot` – scoped, re-read per request. `IOptionsMonitor` – singleton with `CurrentValue` and `OnChange` (for reloading in singletons/background services).
</details>

**5. What does `[ApiController]` give you?**
<details><summary>Answer</summary>

Automatic 400 ProblemDetails on invalid models, binding-source inference (`[FromBody]` etc.), required attribute routing, ProblemDetails for error codes.
</details>

**6. 401 vs 403?**
<details><summary>Answer</summary>

401 = not authenticated (no/invalid token). 403 = authenticated but not allowed (e.g. not in the Admin role).
</details>

**7. What's in a JWT and how is it validated?**
<details><summary>Answer</summary>

`header.payload.signature`: algorithm, claims (sub, role, exp, iss, aud), and a signature. The API validates **signature, issuer, audience and lifetime**. The payload is Base64 — readable by anyone — so no secrets in it.
</details>

**8. What is the N+1 problem?**
<details><summary>Answer</summary>

One query for N parents + one query per parent for children = N+1 round trips. Fix with projection (`Select` into a DTO), `Include`, or `AsSplitQuery`.
</details>

**9. When do you use `AsNoTracking()`?**
<details><summary>Answer</summary>

For read-only queries — skips change tracking, so it's faster and uses less memory. Not when you'll modify and save the entities.
</details>

**10. How do you handle exceptions globally?**
<details><summary>Answer</summary>

`AddProblemDetails()` + `AddExceptionHandler<GlobalExceptionHandler>()` + `app.UseExceptionHandler()` first in the pipeline. The `IExceptionHandler` maps exception types to status codes (400/404/409/500) and writes ProblemDetails without stack traces.
</details>

**11. Why `IHttpClientFactory`?**
<details><summary>Answer</summary>

`new HttpClient()` per call exhausts sockets; one static client ignores DNS changes. The factory pools/rotates handlers and supports typed clients, configuration and resilience (retry, circuit breaker).
</details>

**12. Why not `.Result` or `async void`?**
<details><summary>Answer</summary>

`.Result`/`.Wait()` block a thread → thread-pool starvation/deadlocks. `async void` can't be awaited and its exceptions crash the process. Use `async Task` and `await` all the way.
</details>

**13. Filters vs middleware?**
<details><summary>Answer</summary>

Middleware runs for every request and only sees `HttpContext`. Filters run only around MVC actions and see action arguments, `ModelState` and results. Order: Authorization → Resource → Action → Exception → Result.
</details>

**14. How do you handle two users updating the same row?**
<details><summary>Answer</summary>

Optimistic concurrency: a concurrency token (`RowVersion` or a `Guid Version` with `IsConcurrencyToken()`). EF throws `DbUpdateConcurrencyException` when the row changed → return 409 Conflict and let the client reload.
</details>

**15. Do you need a repository on top of EF Core?**
<details><summary>Answer</summary>

Not necessarily — `DbContext` is already a Unit of Work and `DbSet` a repository. I add specific repositories when I want EF isolated from the domain, reuse complex queries, or easier unit tests; I avoid generic repos exposing `IQueryable`.
</details>

---

## 3. Coding tasks

Give yourself ~6 minutes per task. Write on paper or in an editor **without** IntelliSense first.

### Task A — CRUD actions with validation and proper status codes

> "Using an existing `IProductService` (with `GetByIdAsync`, `CreateAsync`, `UpdateAsync`, `DeleteAsync`), write a `ProductsController` with GET by id, POST, PUT and DELETE. Validate input (name 2–200 chars, price > 0) and return correct status codes. Only admins may delete."

<details><summary>Answer</summary>

```csharp
public record CreateProductRequest(
    [Required, StringLength(200, MinimumLength = 2)] string Name,
    [Range(0.01, 1_000_000)] decimal Price,
    [Range(0, int.MaxValue)] int Stock,
    [Range(1, int.MaxValue)] int CategoryId);

[ApiController]                                   // automatic 400 on invalid model
[Route("api/[controller]")]
public class ProductsController(IProductService service) : ControllerBase
{
    [HttpGet("{id:int}", Name = nameof(GetById))]
    [ProducesResponseType(StatusCodes.Status200OK)]
    [ProducesResponseType(StatusCodes.Status404NotFound)]
    public async Task<ActionResult<ProductDto>> GetById(int id, CancellationToken ct)
    {
        ProductDto? product = await service.GetByIdAsync(id, ct);
        return product is null ? NotFound() : Ok(product);                    // 200 / 404
    }

    [HttpPost]
    [Authorize]
    [ProducesResponseType(StatusCodes.Status201Created)]
    [ProducesResponseType(StatusCodes.Status400BadRequest)]
    public async Task<ActionResult<ProductDto>> Create(CreateProductRequest request, CancellationToken ct)
    {
        ProductDto created = await service.CreateAsync(request, ct);
        return CreatedAtRoute(nameof(GetById), new { id = created.Id }, created);  // 201 + Location
    }

    [HttpPut("{id:int}")]
    [Authorize]
    public async Task<IActionResult> Update(int id, UpdateProductRequest request, CancellationToken ct)
        => await service.UpdateAsync(id, request, ct) ? NoContent() : NotFound();  // 204 / 404

    [HttpDelete("{id:int}")]
    [Authorize(Policy = "AdminOnly")]                                              // 401 / 403
    public async Task<IActionResult> Delete(int id, CancellationToken ct)
        => await service.DeleteAsync(id, ct) ? NoContent() : NotFound();
}
```

Points that impress the interviewer:
- `ControllerBase` (not `Controller`), `[ApiController]`, attribute routing with `{id:int}` constraint.
- **DTOs** with data annotations — never bind entities (over-posting).
- `ActionResult<T>` + `[ProducesResponseType]` for accurate OpenAPI.
- **201 + Location** via `CreatedAtRoute`/`CreatedAtAction`; **204** for PUT/DELETE; **404** when missing.
- `CancellationToken` passed down; async all the way.
- Concurrency conflicts (`DbUpdateConcurrencyException`) → **409** via the global exception handler, not try/catch in every action.
</details>

### Task B — Custom middleware that logs request time

> "Write middleware that measures how long each request takes, logs method, path, status code and elapsed ms, and adds an `X-Elapsed-Ms` response header. Register it."

<details><summary>Answer</summary>

```csharp
public class RequestTimingMiddleware(RequestDelegate next, ILogger<RequestTimingMiddleware> logger)
{
    public async Task InvokeAsync(HttpContext context)
    {
        var sw = Stopwatch.StartNew();

        // Headers must be set BEFORE the response starts streaming
        context.Response.OnStarting(() =>
        {
            context.Response.Headers["X-Elapsed-Ms"] = sw.ElapsedMilliseconds.ToString();
            return Task.CompletedTask;
        });

        try
        {
            await next(context);
        }
        finally                                   // log even if an exception bubbles up
        {
            sw.Stop();
            logger.LogInformation("{Method} {Path} -> {StatusCode} in {Elapsed} ms",
                context.Request.Method, context.Request.Path, context.Response.StatusCode, sw.ElapsedMilliseconds);
        }
    }
}

public static class RequestTimingMiddlewareExtensions
{
    public static IApplicationBuilder UseRequestTiming(this IApplicationBuilder app)
        => app.UseMiddleware<RequestTimingMiddleware>();
}

// Program.cs — after UseExceptionHandler, before auth/endpoints
app.UseExceptionHandler();
app.UseRequestTiming();
```

Points that impress:
- **Structured logging** with a message template (not `$"..."`).
- `Response.OnStarting` — setting headers after the body has started throws "headers are read-only".
- `try/finally` so slow failing requests are logged too.
- Convention-based middleware is **singleton-like**: inject scoped services into `InvokeAsync(HttpContext, IMyScopedService)`, not the constructor.
- Quick inline alternative: `app.Use(async (ctx, next) => { var sw = Stopwatch.StartNew(); await next(ctx); ... });`
</details>

### Task C — Find and fix the bugs

> "This code compiles and 'works on my machine', but has at least **five** serious problems. Find them and fix them."

```csharp
// Program.cs
builder.Services.AddDbContext<AppDbContext>(...);         // scoped
builder.Services.AddSingleton<StockAlertService>();

public class StockAlertService(AppDbContext db)            // singleton
{
    public Task<int> CountLowStockAsync() => db.Products.CountAsync(p => p.Stock < 5);
}

[ApiController]
[Route("api/orders")]
public class OrdersController(AppDbContext db, StockAlertService alerts) : ControllerBase
{
    [HttpGet("rate")]
    public IActionResult GetRate()
    {
        var client = new HttpClient();
        string json = client.GetStringAsync("https://example.com/api/rates/EUR").Result;
        return Ok(json);
    }

    [HttpGet("summary")]
    public async Task<IActionResult> Summary()
    {
        var orders = await db.Orders.ToListAsync();
        var result = new List<object>();
        foreach (var o in orders)
        {
            var items = await db.OrderItems.Where(i => i.OrderId == o.Id).ToListAsync();
            result.Add(new { o.Id, Total = items.Sum(i => i.UnitPrice * i.Quantity) });
        }
        return Ok(result);
    }

    [HttpPost("{id:int}/notify")]
    public IActionResult Notify(int id)
    {
        SendEmail(id);
        return Accepted();
    }

    private async void SendEmail(int id)
    {
        await Task.Delay(100);                       // pretend to call an email API
        throw new InvalidOperationException("SMTP down");
    }
}
```

<details><summary>Answer</summary>

| # | Bug | Why it's bad | Fix |
|---|---|---|---|
| 1 | **Scoped `DbContext` in a singleton** (`StockAlertService`) | Captive dependency: one DbContext shared by all requests/threads → concurrency exceptions, stale data. (Development scope validation throws at startup.) | Make the service scoped, or inject `IServiceScopeFactory` and create a scope per call |
| 2 | **`new HttpClient()` per call** | Socket exhaustion under load; no central config/timeouts | Typed client via `AddHttpClient<RatesClient>` |
| 3 | **`.Result`** | Blocks a thread-pool thread → starvation; deadlock risk | `await` + `async Task` |
| 4 | **N+1 queries** in `Summary` | 1 query for orders + 1 per order; loads every column and every order | Single projected query (`Select` + `Sum`), `AsNoTracking`, paging |
| 5 | **`async void`** | Exception can't be caught → **crashes the process**; work may be cut off mid-way | `async Task` and await it, or hand it to a background queue |
| + | No `CancellationToken`, no paging, returns anonymous types | Wasted work on disconnect, huge responses | Pass `ct`, page, return DTOs |

**Fixed code:**

```csharp
// Program.cs
builder.Services.AddDbContext<AppDbContext>(...);
builder.Services.AddSingleton<StockAlertService>();
builder.Services.AddHttpClient<RatesClient>(c =>
{
    c.BaseAddress = new Uri("https://example.com/api/");
    c.Timeout = TimeSpan.FromSeconds(10);
});
builder.Services.AddScoped<IEmailSender, EmailSender>();

// 1. Singleton that needs a scoped DbContext -> create a scope per call
public class StockAlertService(IServiceScopeFactory scopes)
{
    public async Task<int> CountLowStockAsync(CancellationToken ct)
    {
        await using AsyncServiceScope scope = scopes.CreateAsyncScope();
        var db = scope.ServiceProvider.GetRequiredService<AppDbContext>();
        return await db.Products.CountAsync(p => p.Stock < 5, ct);
    }
}

// 2. Typed HttpClient (pooled handlers, central config)
public class RatesClient(HttpClient http)
{
    public Task<string> GetRateJsonAsync(string currency, CancellationToken ct) =>
        http.GetStringAsync($"rates/{currency}", ct);
}

public record OrderSummaryDto(int Id, decimal Total);

[ApiController]
[Route("api/orders")]
public class OrdersController(AppDbContext db, RatesClient rates, IEmailSender email) : ControllerBase
{
    // 3. async all the way
    [HttpGet("rate")]
    public async Task<IActionResult> GetRate(CancellationToken ct) =>
        Ok(await rates.GetRateJsonAsync("EUR", ct));

    // 4. One SQL query: projection computes totals in the database
    [HttpGet("summary")]
    public async Task<ActionResult<List<OrderSummaryDto>>> Summary(
        int page = 1, int pageSize = 50, CancellationToken ct = default)
    {
        pageSize = Math.Clamp(pageSize, 1, 100);
        return await db.Orders
            .AsNoTracking()
            .OrderBy(o => o.Id)
            .Skip((Math.Max(page, 1) - 1) * pageSize)
            .Take(pageSize)
            .Select(o => new OrderSummaryDto(o.Id, o.Items.Sum(i => i.UnitPrice * i.Quantity)))
            .ToListAsync(ct);
    }

    // 5. async Task, awaited -> exceptions reach the global exception handler
    [HttpPost("{id:int}/notify")]
    public async Task<IActionResult> Notify(int id, CancellationToken ct)
    {
        await email.SendOrderEmailAsync(id, ct);
        return Accepted();
    }
}

public interface IEmailSender { Task SendOrderEmailAsync(int orderId, CancellationToken ct); }

public class EmailSender(ILogger<EmailSender> logger) : IEmailSender
{
    public async Task SendOrderEmailAsync(int orderId, CancellationToken ct)
    {
        await Task.Delay(100, ct);                   // real email API call here
        logger.LogInformation("Email sent for order {OrderId}", orderId);
    }
}
```

If the email must not slow the request, don't fire-and-forget with `Task.Run` — enqueue it (a `Channel<T>` read by a `BackgroundService`, or Azure Service Bus + outbox) and return **202 Accepted**.
</details>

---

## 4. Questions to ask the interviewer (pick 2–3)

- "What does a typical feature look like from ticket to production here? How often do you deploy?"
- "What's the current architecture — monolith, modular monolith, microservices? What are you most trying to improve?"
- "How do you handle code reviews and testing — is there a target for unit/integration tests?"
- "What does success look like for this role in the first 3–6 months?"
- "What's the biggest technical challenge the team is facing right now?"
- "How is on-call / production support organised?"

---

# Part 2 — Cheat sheet

## Program.cs skeleton (memorise this shape)

```csharp
var builder = WebApplication.CreateBuilder(args);

// ---- 1. Services ----
builder.Services.AddOptions<JwtOptions>().Bind(builder.Configuration.GetSection("Jwt"))
    .ValidateDataAnnotations().ValidateOnStart();
builder.Services.AddDbContext<AppDbContext>(o => o.UseSqlServer(builder.Configuration.GetConnectionString("Shop")));
builder.Services.AddScoped<IProductService, ProductService>();
builder.Services.AddControllers();
builder.Services.AddOpenApi();
builder.Services.AddProblemDetails();
builder.Services.AddExceptionHandler<GlobalExceptionHandler>();
builder.Services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme).AddJwtBearer(/* ... */);
builder.Services.AddAuthorizationBuilder().AddPolicy("AdminOnly", p => p.RequireRole("Admin"));
builder.Services.AddCors(o => o.AddPolicy("Frontend", p => p.WithOrigins("http://localhost:4200").AllowAnyHeader().AllowAnyMethod()));
builder.Services.AddMemoryCache();
builder.Services.AddOutputCache();
builder.Services.AddRateLimiter(/* ... */);
builder.Services.AddHttpClient<ExchangeRateClient>();
builder.Services.AddHealthChecks();

var app = builder.Build();

// ---- 2. Middleware (ORDER MATTERS) ----
app.UseExceptionHandler();
if (app.Environment.IsDevelopment()) app.MapOpenApi(); else app.UseHsts();
app.UseHttpsRedirection();
app.UseCors("Frontend");
app.UseAuthentication();
app.UseAuthorization();
app.UseRateLimiter();
app.UseOutputCache();

// ---- 3. Endpoints ----
app.MapControllers();
app.MapHealthChecks("/health");
app.Run();
```

## Middleware order

```
1 ExceptionHandler   2 HSTS   3 HttpsRedirection   4 StaticFiles   5 Routing (implicit)
6 CORS   7 Authentication   8 Authorization   9 RateLimiter / OutputCache / custom   10 Endpoints
```

## DI lifetimes

| Lifetime | Instance per | Example | Can depend on |
|---|---|---|---|
| Singleton | App | `IMemoryCache`, `TimeProvider`, typed config | Singletons only |
| Scoped | Request | `DbContext`, `IProductService` | Singleton, Scoped |
| Transient | Resolution | Stateless helpers | Anything (but beware being captured) |

## HTTP status codes

| Code | Use |
|---|---|
| 200 | OK (GET / PUT with body) |
| 201 | Created + `Location` (POST) |
| 202 | Accepted (queued work) |
| 204 | No Content (PUT/DELETE) |
| 400 | Validation failed |
| 401 | Not authenticated |
| 403 | Authenticated, not allowed |
| 404 | Not found |
| 409 | Conflict (concurrency / duplicate) |
| 412 | Precondition failed (ETag `If-Match`) |
| 422 | Business-rule validation (optional) |
| 429 | Rate limited |
| 500 / 503 | Server error / unavailable |

## IOptions variants

| | Lifetime | Reloads | Use |
|---|---|---|---|
| `IOptions<T>` | Singleton | No | Default |
| `IOptionsSnapshot<T>` | Scoped | Per request | Scoped services |
| `IOptionsMonitor<T>` | Singleton | Yes + `OnChange` | Singletons / workers |

## Filter order

```
Authorization → Resource → [model binding] → Action → (action) → Exception → Result
```
Register: global `o.Filters.Add<T>()`, `[ServiceFilter(typeof(T))]`, `[TypeFilter(typeof(T))]`. Minimal APIs → `AddEndpointFilter`.

## EF Core loading & performance

| Technique | Code |
|---|---|
| Eager | `db.Orders.Include(o => o.Items)` |
| Explicit | `await db.Entry(o).Collection(x => x.Items).LoadAsync()` |
| Lazy | `UseLazyLoadingProxies()` + `virtual` navs (N+1 risk) |
| Projection | `.Select(p => new ProductDto(...))` ← best for reads |
| Read-only | `.AsNoTracking()` |
| Big includes | `.AsSplitQuery()` |
| Bulk | `ExecuteUpdateAsync` / `ExecuteDeleteAsync` |
| Concurrency | `[Timestamp]` / `IsConcurrencyToken()` → `DbUpdateConcurrencyException` → 409 |

## Auth snippet (JWT)

```csharp
builder.Services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
    .AddJwtBearer(o => o.TokenValidationParameters = new TokenValidationParameters
    {
        ValidateIssuer = true, ValidIssuer = jwt.Issuer,
        ValidateAudience = true, ValidAudience = jwt.Audience,
        ValidateLifetime = true,
        ValidateIssuerSigningKey = true,
        IssuerSigningKey = new SymmetricSecurityKey(Encoding.UTF8.GetBytes(jwt.Key)),
        ClockSkew = TimeSpan.FromSeconds(30)
    });
builder.Services.AddAuthorizationBuilder().AddPolicy("AdminOnly", p => p.RequireRole("Admin"));

[Authorize]                       // any authenticated user
[Authorize(Policy = "AdminOnly")] // policy
[AllowAnonymous]                  // opt out
```

## Caching snippet

```csharp
// In-memory (per server)
public Task<ProductDto?> GetAsync(int id, CancellationToken ct) =>
    cache.GetOrCreateAsync($"product:{id}", e =>
    {
        e.AbsoluteExpirationRelativeToNow = TimeSpan.FromMinutes(5);
        return inner.GetByIdAsync(id, ct);
    });

// Output cache (whole response)
builder.Services.AddOutputCache();
app.UseOutputCache();
app.MapGet("/api/categories", GetAll).CacheOutput(p => p.Expire(TimeSpan.FromSeconds(30)));

// Distributed (Redis): builder.Services.AddStackExchangeRedisCache(o => o.Configuration = "...");
// .NET 9+: builder.Services.AddHybridCache();
```

## Common CLI commands

```bash
dotnet new webapi -n ShopApi -f net10.0     # new API (also: web, mvc, razor, blazor, xunit, classlib, sln)
dotnet new sln -n Shop && dotnet sln add ShopApi/ShopApi.csproj
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add reference ../Shop.Application/Shop.Application.csproj
dotnet restore
dotnet build
dotnet run                                  # or: dotnet watch (hot reload)
dotnet test                                 # --filter "FullyQualifiedName~Products"
dotnet publish -c Release -o ./publish
dotnet user-secrets init && dotnet user-secrets set "Jwt:Key" "dev-only-secret"

dotnet tool install --global dotnet-ef
dotnet ef migrations add InitialCreate
dotnet ef database update
dotnet ef migrations remove                 # undo last (unapplied) migration
dotnet ef migrations script --idempotent -o migrate.sql
dotnet ef dbcontext scaffold "<conn>" Microsoft.EntityFrameworkCore.SqlServer -o Models   # DB-first
```

## Ten "say this" one-liners

1. "Middleware order = request order; exception handler first, auth before authorization, endpoints last."
2. "DbContext is scoped; in singletons I use `IServiceScopeFactory`."
3. "I return DTOs, never entities, and project with `Select` + `AsNoTracking` for reads."
4. "Validation → 400 automatically via `[ApiController]`; business errors → ProblemDetails via `IExceptionHandler`."
5. "401 is who-are-you, 403 is you-can't."
6. "`IHttpClientFactory` + resilience handler for outgoing calls."
7. "Async all the way; no `.Result`, no `async void`; pass `CancellationToken`."
8. "Optimistic concurrency with a row version → 409."
9. "Secrets live in Key Vault via Managed Identity; user-secrets locally."
10. "Unit tests with mocks, integration tests with `WebApplicationFactory`."

## Last-day checklist ✅

- [ ] Can I **draw the middleware pipeline** and explain each step? (Ch. 3)
- [ ] Can I explain **DI lifetimes + captive dependency** with an example? (Ch. 4)
- [ ] Can I explain **every line of `Program.cs`** in the sample app? (Ch. 2, 17)
- [ ] Do I know **status codes** for each CRUD operation? (Ch. 6)
- [ ] Can I explain **N+1, AsNoTracking, Include vs projection, migrations, concurrency**? (Ch. 9)
- [ ] Can I explain **JWT flow, 401 vs 403, roles vs policies**? (Ch. 10)
- [ ] Can I describe **global exception handling + ProblemDetails + structured logging**? (Ch. 11)
- [ ] Can I answer the **scenario questions** Q76–Q87 out loud? (Ch. 18)
- [ ] Have I re-done the **3 coding tasks** above without looking?
- [ ] Have I prepared my **"tell me about yourself"** and **one performance/bug story** with numbers?
- [ ] Have I re-read the **job description** and matched 3 of my experiences to it?
- [ ] Laptop charged, camera/mic tested, IDE open, water nearby, **sleep 7+ hours**. 😴

> 💡 If you don't know an answer: say what you **do** know, how you'd **find out** ("I'd check the docs / write a quick spike"), and relate it to something similar you've done. Honest + structured beats bluffing every time. Good luck! 🍀
