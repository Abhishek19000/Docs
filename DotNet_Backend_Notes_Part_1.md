# .NET Backend Engineering --- Part 1

## Revision Notes: Backend Days 1--11

> These notes consolidate the concepts covered in Backend Days 1--11.
> Examples are teaching examples, not proof that they have been executed
> in the IncidentHub project. Practical items still pending are listed
> at the end.

## How to revise

For each topic ask: What problem does it solve? How does it work? Why
use this design? What can fail? How would I explain it in an interview?

# Day 1 --- ASP.NET Core and Web API foundations

## API request/response flow

A Web API receives an HTTP request, runs it through the ASP.NET Core
pipeline and endpoint logic, may call application and data-access code,
then returns an HTTP response.

``` text
Client -> HTTP request -> Middleware -> Controller/Endpoint
       -> Service -> Data access -> Database
       <- HTTP response
```

A request commonly contains a method, URL, headers and optional body. A
response contains a status code, headers and optional body.

## HTTP methods

  Method   Typical purpose
  -------- --------------------------------------------
  GET      Read a resource
  POST     Create a resource or initiate an operation
  PUT      Replace a resource
  PATCH    Partially update a resource
  DELETE   Delete a resource

## Common status codes

  Code   Meaning
  ------ --------------------------------------------
  200    Successful request
  201    Resource created
  204    Successful operation with no response body
  400    Invalid request
  401    Authentication missing or invalid
  403    Authenticated caller is not allowed
  404    Resource not found
  409    Conflict with current resource state
  500    Unexpected server failure

## Controllers and action results

Controllers expose endpoints. Action results let an endpoint return an
appropriate HTTP outcome.

``` csharp
[ApiController]
[Route("api/[controller]")]
public class IncidentsController : ControllerBase
{
    [HttpGet("{id:int}")]
    public ActionResult<string> Get(int id)
    {
        if (id <= 0) return BadRequest("Invalid incident id");
        return Ok($"Incident {id}");
    }
}
```

## DTOs

A Data Transfer Object defines the data shape exchanged across an
application boundary. It separates the API contract from the persistence
entity, prevents accidental exposure of internal fields, and lets
request and response shapes evolve independently.

``` csharp
public sealed class CreateIncidentRequest
{
    public string Title { get; set; } = "";
    public string Description { get; set; } = "";
}

public sealed class IncidentResponse
{
    public int Id { get; set; }
    public string Title { get; set; } = "";
    public string Status { get; set; } = "";
}
```

**Interview answer:** A DTO is an API-facing data contract, separate
from the database entity.

# Day 2 --- Application structure, EF Core and security foundations

## Separation of responsibilities

``` text
Controller -> Service -> Data access -> Database
```

-   Controller: HTTP concerns, binding and response codes.
-   Service: business rules and use-case orchestration.
-   Data access: persistence operations.
-   Database: durable storage.

Separating responsibilities keeps HTTP handling, business rules and
persistence from becoming tangled.

## Entity Framework Core

EF Core is an Object-Relational Mapper (ORM). It maps .NET objects to
relational data and translates supported LINQ expressions into database
queries.

-   `DbContext`: coordinates entity tracking and persistence; represents
    a unit of work.
-   `DbSet<TEntity>`: queryable collection for an entity type.
-   Entity: a .NET type mapped to stored data.
-   Migration: versioned database schema change.
-   Change tracking: EF Core tracks changes to entities it is tracking.

``` csharp
public class AppDbContext : DbContext
{
    public AppDbContext(DbContextOptions<AppDbContext> options)
        : base(options) { }

    public DbSet<Incident> Incidents => Set<Incident>();
}
```

## Dependency Injection

DI supplies a class with its dependencies rather than making the class
construct them itself.

``` csharp
public class IncidentService
{
    private readonly AppDbContext _db;

    public IncidentService(AppDbContext db) => _db = db;
}
```

DI makes dependencies explicit, reduces tight coupling, supports
substitutes in tests, and centralizes object construction/lifetime
management.

## Authentication and authorization

-   Authentication: establishes who the caller is.
-   Authorization: decides what the caller is allowed to do.
-   Claim: a statement about a subject, such as user ID or team.
-   Role: a named grouping of permissions/responsibilities.
-   Policy: a named authorization rule.

An authenticated identity does not automatically have permission to
access every resource.

# Day 3 --- Routing, model binding and DTO mapping

## Routing

Routing maps an incoming URL and HTTP method to an endpoint.

``` csharp
[ApiController]
[Route("api/incidents")]
public class IncidentsController : ControllerBase
{
    [HttpGet("{id:int}")]
    public IActionResult GetById(int id) => Ok(id);
}
```

## Model binding

Model binding takes values from the request and supplies them as action
parameters or model properties. Common sources are route values, query
strings, request bodies and headers.

## Validation and mapping

Validation checks incoming data against rules before business
processing. Request DTOs help define the accepted input. Business rules
still belong in the appropriate application layer.

Mapping transfers values between representations, such as request DTO to
entity or entity to response DTO. Do not blindly copy client-controlled
fields into persistence entities.

# Day 4 --- Filtering, sorting, pagination and errors

## Filtering and searching

Filtering narrows results by criteria such as status, severity or
assignee. Searching matches records against a search term.

Example: `GET /api/incidents?status=Open&search=deployment`

Validate query parameters. Do not accept arbitrary client-provided sort
expressions.

## Stable sorting

A stable sort uses a deterministic tie-breaker. For example, sort by
creation time and then ID. Without a tie-breaker, records with equal
primary sort values may move between pages.

## Pagination

-   Offset pagination (`page=2&pageSize=20`) is simple, but large
    offsets can be expensive and concurrent data changes can shift
    records between pages.
-   Cursor/keyset pagination uses a last-seen key, such as
    `(CreatedAt, Id)`. It can work better for large or changing
    datasets, but needs a suitable ordering and cursor design.

A paged response may include items, page/cursor, page size, total count
(if needed), and whether more results exist. Avoid expensive counts if
the product does not need them.

## Error responses

Use consistent error shapes. ASP.NET Core supports Problem Details. Do
not expose raw exception messages or stack traces to clients. Log
diagnostic details internally, using correlation identifiers where
appropriate.

# Day 5 --- EF Core fundamentals and persistence

## DbContext and queries

A `DbContext` coordinates tracking and persistence for a logical unit of
work. It is normally scoped per request and is not thread-safe.

``` csharp
var incidents = await db.Incidents
    .Where(x => x.Status == "Open")
    .ToListAsync();
```

EF Core translates supported query expressions into SQL and executes
them through the configured provider.

## Tracking vs no tracking

-   Tracking query: EF Core tracks returned entities and detects
    changes.
-   `AsNoTracking()`: useful for read-only queries where tracking is
    unnecessary.

``` csharp
var incident = await db.Incidents
    .AsNoTracking()
    .FirstOrDefaultAsync(x => x.Id == id);
```

Do not use no-tracking blindly if the entity is intended to be modified
and saved through that context.

## SaveChangesAsync and transactions

Changing a tracked entity and calling `SaveChangesAsync()` persists the
change.

``` csharp
incident.Status = "Resolved";
await db.SaveChangesAsync();
```

For relational providers, EF Core generally wraps changes from one
`SaveChanges` call in a transaction when supported. This does not
include external side effects such as email or HTTP calls.

## Relationships and N+1

EF Core supports eager, explicit and (when configured) lazy loading. N+1
occurs when one query loads a collection and then additional queries are
issued per item to load related data. `Include` can help, but loading
too much data can also be expensive. Inspect generated SQL and query
counts.

## Migrations

Migrations version schema changes. Review generated migrations,
especially destructive changes, data transformations, indexes and
production rollout concerns.

# Day 6 --- Authentication and authorization

## JWT access tokens

A JSON Web Token commonly carries signed claims. A typical JWT has a
header, payload and signature. A signed JWT is not automatically
encrypted: do not put secrets in its payload.

The API validates signature, issuer, audience and lifetime according to
configuration.

## Claims, roles and policies

Claims describe identity attributes or permissions. Roles are one
possible authorization input. Policies define named authorization rules
and can combine requirements.

Resource-based authorization may be necessary. For example, a user may
view incidents for their team but not another team. A generic
authenticated role is not enough to authorize access to every incident.

## 401 vs 403

-   401: authentication was not successful.
-   403: caller is authenticated but access is forbidden.

## Access vs refresh tokens

-   Access token: short-lived credential sent to protected APIs.
-   Refresh token: longer-lived credential used to obtain a new access
    token, subject to server-side rules.

A secure implementation considers storage, rotation, reuse detection,
revocation, expiration and logout. Token issuance, refresh-token
persistence/rotation/revocation, password hashing and security tests
remain practical work to implement.

# Day 7 --- HTTPS, CORS and API security

## HTTPS

HTTPS uses TLS to protect data in transit, helping protect credentials
and API data from interception or modification.

## CORS

Cross-Origin Resource Sharing is a browser-enforced mechanism
controlling whether a page from one origin can read responses from
another origin. An origin consists of scheme, host and port.

CORS is not authentication and does not prevent non-browser clients from
calling an API. Some requests trigger an `OPTIONS` preflight. Configure
only required origins, methods and headers.

## Bearer tokens and cookies

Bearer tokens are commonly sent in the `Authorization` header. Cookies
are automatically attached by browsers according to cookie rules, so
CSRF protection must be considered.

Do not assume browser local storage is automatically safe for tokens.
Choose storage and transport based on client type and threat model.

## BOLA / IDOR

Broken Object Level Authorization (BOLA), also called Insecure Direct
Object Reference (IDOR), occurs when an API allows access to an object
without checking whether the caller is authorized for that specific
object.

Changing `/api/incidents/101` to `/api/incidents/102` must not grant
access to incident 102. Perform resource-level authorization.

# Day 8 --- ASP.NET Core middleware

## Middleware and pipeline

Middleware is a component in the HTTP request pipeline. It can inspect
or modify the request, call the next component, inspect or modify the
response, or short-circuit the pipeline.

``` text
Request -> Middleware A (before)
        -> Middleware B (before)
        -> Endpoint
        <- Middleware B (after)
        <- Middleware A (after)
Response
```

Calling `next` passes control downstream. Awaiting it lets the current
middleware resume after downstream work finishes. If middleware does not
call next, it short-circuits.

## Appropriate responsibilities

Middleware is suitable for cross-cutting concerns such as exception
handling, request logging, correlation IDs and timing. Business-specific
incident rules generally belong in application services.

## Exception middleware

Place exception-handling middleware early enough to catch downstream
failures. If the response has already started, it may not be possible to
replace it with a clean error response. Do not expose internal exception
details to clients.

## Middleware lifetime

Conventional middleware is typically constructed when the pipeline is
built and reused. Avoid constructor-injecting scoped services such as
`DbContext` into conventional middleware. Use per-invocation injection
or `IMiddleware` with DI activation when a scoped dependency is needed.

# Day 9 --- Dependency Injection lifetimes

  -----------------------------------------------------------------------
  Lifetime                Meaning                 Typical use
  ----------------------- ----------------------- -----------------------
  Transient               New instance each time  Lightweight stateless
                          DI resolves it          service

  Scoped                  One instance per scope  `DbContext`,
                                                  request-level service

  Singleton               One shared instance for Shared thread-safe
                          provider lifetime       service
  -----------------------------------------------------------------------

Transient means a new instance per DI resolution, not per method call.

`DbContext` is usually scoped because a request commonly represents a
logical unit of work. It is not thread-safe; do not share one context
across concurrent operations.

## Captive dependency

A captive dependency occurs when a longer-lived service retains a
shorter-lived dependency beyond its intended lifetime. A singleton
constructor-injecting a scoped `DbContext` may retain and reuse it
across requests.

Fix the lifetime design. Do not simply change lifetimes without
understanding the consequences. A singleton/background worker that needs
scoped services should create a scope for the work.

# Day 10 --- BackgroundService, scopes and resilient processing

## BackgroundService

`BackgroundService` is a base class for hosted background work. It
provides `ExecuteAsync(CancellationToken)`.

``` csharp
services.AddHostedService<IncidentBackgroundService>();
```

Hosted workers are long-lived. They should not retain a scoped
`DbContext` for their entire lifetime.

## IServiceScopeFactory

A worker can create a scope for a processing cycle, resolve scoped
dependencies, and dispose the scope afterwards.

``` csharp
await using var scope = _scopeFactory.CreateAsyncScope();
var db = scope.ServiceProvider.GetRequiredService<AppDbContext>();
```

Create a fresh scope per cycle or batch as appropriate. This gives the
work a correctly scoped context and ensures disposal.

## CancellationToken

The host passes a cancellation token to signal graceful shutdown. Pass
it to supported async operations. Distinguish expected host shutdown
from other cancellation or timeout conditions.

## Exceptions and retries

A worker needs an explicit failure policy: log failures, decide whether
to retry/continue/stop, and do not silently swallow exceptions. Retry
only failures that may be transient. Use bounded retries, backoff and
often jitter. Do not retry validation failures or permanent errors
indefinitely.

## Idempotency and concurrency

An operation is idempotent when repeating it does not create additional
unintended effects. Jobs may run more than once due to retries, restarts
or competing workers.

A simple "check status, then act" can race under concurrency. Consider
atomic updates, unique constraints, durable job state, or outbox/inbox
patterns where appropriate. Exactly-once external side effects should
not be assumed.

# Day 11 --- Unit testing with xUnit and Moq

## Why unit tests?

A unit test checks a small unit of behavior in isolation. It gives fast
feedback and helps detect regressions. It does not prove that a real
database, network or external service works correctly.

## Arrange--Act--Assert

1.  Arrange: prepare inputs and dependencies.
2.  Act: execute the behavior.
3.  Assert: check the expected outcome.

## xUnit

-   `[Fact]`: one fixed scenario.
-   `[Theory]`: same test logic with multiple data sets.
-   `[InlineData]`: supplies data to a theory.

``` csharp
[Theory]
[InlineData(2, 3, 5)]
[InlineData(5, 5, 10)]
public void Add_ReturnsExpectedResult(int a, int b, int expected)
{
    Assert.Equal(expected, a + b);
}
```

## Moq

Moq creates test doubles for dependencies.

``` csharp
var mockRepository = new Mock<IIncidentRepository>();

mockRepository
    .Setup(x => x.GetStatusAsync(101))
    .ReturnsAsync("Open");
```

`Setup()` configures behavior. `ReturnsAsync()` supplies a successful
async result. `ThrowsAsync()` simulates an async exception. `Verify()`
checks an interaction. `Times.Once` requires exactly one invocation.

``` csharp
mockRepository.Verify(
    x => x.GetStatusAsync(101),
    Times.Once);
```

Verify interactions when the interaction matters to the behavior; do not
verify every internal call mechanically.

## Exception testing

``` csharp
await Assert.ThrowsAsync<Exception>(
    () => service.ResolveIncidentAsync(101));
```

This checks that the expected exception type is thrown. Prefer a
specific meaningful exception type over broad `Exception` in
production-quality tests when possible.

## Mock vs stub

A stub supplies predefined responses. A mock is commonly used to verify
interactions. Moq can create test doubles used in either way; the
distinction is how the test uses the double.

## Unit vs integration tests

  -----------------------------------------------------------------------
  Unit test                           Integration test
  ----------------------------------- -----------------------------------
  Isolates a unit                     Exercises components together

  Often uses mocks                    Uses real components where
                                      practical

  Fast and focused                    More realistic, often slower

  Tests business behavior             Tests wiring, persistence and
                                      component interaction
  -----------------------------------------------------------------------

Mocking `DbContext` does not reproduce real provider behavior such as
SQL translation, relational constraints, transactions or
provider-specific features. For database integration tests, prefer the
actual database engine in an isolated test environment when practical.
SQLite can help for some tests but is not identical to PostgreSQL. EF
Core InMemory is not a relational database substitute.

## Test isolation

Each test should prepare its own data and not rely on another test
having run first. Tests should be repeatable and safe to run in
different orders. Shared database infrastructure must isolate test data
and clean up reliably.

# Cross-day engineering principles

1.  **Keep responsibilities clear:** controllers handle HTTP, services
    handle use cases/business rules, persistence components handle
    database access, middleware handles cross-cutting pipeline concerns.
2.  **Authentication is not authorization:** a valid identity does not
    grant access to every incident.
3.  **A mock is not a real dependency:** a mock can verify behavior
    against configured responses; it cannot prove PostgreSQL accepted a
    query.
4.  **Treat failures as normal:** design for missing resources, invalid
    input, permission failures, network/database failures, cancellation,
    duplicate jobs and concurrency races.
5.  **Understand transaction boundaries:** a database transaction does
    not automatically include external email, message publishing or HTTP
    calls.

# Interview quick revision

1.  **What is a DTO?** An API data contract separate from persistence
    entities.
2.  **What is DI?** Supplying dependencies from outside a class rather
    than constructing them inside it.
3.  **What is DbContext?** EF Core's unit-of-work and change-tracking
    context.
4.  **What is AsNoTracking?** A query mode that avoids tracking returned
    entities.
5.  **What is N+1?** One query for a collection followed by additional
    queries per item for related data.
6.  **401 vs 403?** Authentication unsuccessful vs authenticated but
    forbidden.
7.  **What is CORS?** A browser mechanism controlling cross-origin
    response access.
8.  **What is BOLA?** Missing object-level authorization that permits
    access to another user's resource.
9.  **What is middleware?** A component in the HTTP request/response
    pipeline.
10. **What is a captive dependency?** A longer-lived service retaining a
    shorter-lived dependency.
11. **Why use IServiceScopeFactory in a worker?** To create a scope for
    scoped dependencies during background processing.
12. **Why use CancellationToken?** To support cooperative cancellation
    and graceful shutdown.
13. **What is idempotency?** Repeating an operation does not create
    additional unintended effects.
14. **What is AAA?** Arrange, Act, Assert.
15. **What does Moq Setup do?** Configures a mock's behavior.
16. **What does Moq Verify do?** Checks whether an expected interaction
    occurred.
17. **Why integration tests?** To verify real components work together,
    including persistence and framework wiring.

# Practical work still pending

These items were discussed conceptually or through illustrative examples
but still need to be executed and verified in the actual solution:

-   Run xUnit/Moq examples in a real test project.
-   Build service tests for success, not-found and failure scenarios.
-   Create an ASP.NET Core integration test project.
-   Run the application through `WebApplicationFactory`.
-   Use an isolated PostgreSQL test database.
-   Verify actual inserts, updates, API status codes and authorization
    behavior.

**Revision rule:** Update these notes when practical implementation
reveals a correction, useful edge case or better engineering
explanation. Keep conceptual coverage and verified implementation
clearly distinguished.
