# C# Engineering --- Complete Revision Notes

## Days 1--12 \| Product-Company / FAANG Interview Preparation

> **Purpose:** These notes consolidate the C# concepts covered in Days
> 1--12 and frame them from an experienced product-company engineer's
> perspective. The focus is not only what a feature does, but why it
> exists, what happens at runtime, when to use it, what can go wrong,
> and how to explain the trade-offs in an interview.
>
> **Coverage note:** This document reflects the earlier C# Days 1--12
> learning material. A topic being documented means it was covered
> conceptually; it does not claim that every example has been
> independently executed, benchmarked, or used in a production project.

------------------------------------------------------------------------

# How to use this document

For every concept, practice answering these questions:

1.  **What is it?**
2.  **Why does it exist?**
3.  **What happens when the code runs?**
4.  **When would I use it?**
5.  **What can go wrong?**
6.  **What is the trade-off or alternative?**
7.  **How would I explain it to an interviewer or teammate?**

Do not memorize one-line definitions. Be able to explain the behavior,
show a small example, and discuss a realistic engineering decision.

# Day 1 --- C# Fundamentals, Types and Parameter Passing

## 1. Value types vs reference types

The beginner shortcut below is misleading:

``` text
Value type     = stack
Reference type = heap
```

The more accurate distinction is about **type semantics and copying
behavior**. Storage location depends on context and runtime
implementation.

### Value type

A value-type assignment copies the value.

``` csharp
int first = 10;
int second = first;

second = 20;

Console.WriteLine(first);  // 10
Console.WriteLine(second); // 20
```

`second` has its own copy of the integer value.

Common value types include: - `int`, `long`, `double`, `decimal` -
`bool`, `char` - `struct` - `enum`

### Reference type

A reference-type assignment copies the reference, not the referenced
object.

``` csharp
var first = new Person { Name = "Abhishek" };
var second = first;

second.Name = "Rahul";

Console.WriteLine(first.Name); // Rahul
```

Both variables refer to the same object.

``` text
first  ─────┐
            ├──> Person object { Name = "Rahul" }
second ─────┘
```

**Why does this matter?** A method can mutate an object through a copied
reference. That mutation is visible through every reference to the same
object.

### Important nuance: reference types can still be immutable

A reference-type variable can refer to an immutable object. Copying the
reference still copies the reference, but the object's state cannot be
changed through its public API. Do not confuse reference semantics with
mutability.

## 2. Why the variable is not the object

``` csharp
Person person = new Person();
```

`person` is a variable that contains a reference. The `Person` instance
is the object.

Do not claim that every reference variable is always stored on the stack
or every object is always stored in one simple heap location. The
compiler and runtime determine storage based on context, lifetime, and
implementation details.

## 3. `ref`

`ref` passes the caller's variable by reference. The method can replace
the caller's variable.

``` csharp
static void Replace(ref Person person)
{
    person = new Person { Name = "New person" };
}

Person original = new Person { Name = "Abhishek" };
Replace(ref original);

Console.WriteLine(original.Name); // New person
```

Without `ref`, the method receives a copy of the reference. It can
mutate the referenced object, but reassigning its local parameter does
not replace the caller's variable.

## 4. `out`

`out` is used when a method must assign a value back to the caller. The
called method must definitely assign the `out` parameter before
returning normally.

``` csharp
static bool TryGetAge(string input, out int age)
{
    return int.TryParse(input, out age);
}

bool parsed = TryGetAge("25", out int age);
```

A common .NET pattern is the `TryX` pattern: return whether the
operation succeeded and provide the result through an `out` parameter.

## 5. `in`

`in` passes an argument by readonly reference. It can avoid copying a
larger value type while preventing reassignment of the parameter through
that reference.

``` csharp
static decimal Calculate(in LargeValue value)
{
    return value.Amount;
}
```

Use it when measurement shows that avoiding a copy is useful. Do not add
`in` to every parameter; it adds API complexity and may not improve
performance for small values.

## 6. Boxing and unboxing

Boxing converts a value type to `object` or an implemented interface
type.

``` csharp
int number = 10;
object boxed = number;  // boxing
int copy = (int)boxed;  // unboxing
```

Boxing generally creates an object representation and copies the value
into it. Unboxing requires a compatible boxed value type.

Potential costs: - allocation and garbage-collection pressure -
copying - runtime type checks - invalid cast exceptions if the boxed
type is not what the code expects

Generics often avoid boxing for value types:

``` csharp
var numbers = new List<int>();
numbers.Add(10);
```

### Product-company interview points

**Q: Are all value types stored on the stack?**

No. Value/reference type describes semantics, not a universal stack/heap
rule. Storage depends on context.

**Q: What happens when a reference type is copied?**

The reference is copied. Both variables may refer to the same object.

**Q: What is boxing?**

Converting a value type to `object` or an interface representation,
which can introduce allocation and runtime overhead.

------------------------------------------------------------------------

# Day 2 --- OOP: Classes, Encapsulation, Inheritance, Polymorphism and Abstraction

## 1. Class and object

A class defines a type and its members. An object is an instance of that
type.

``` csharp
public class Employee
{
    public string Name { get; set; } = "";
}

var employee = new Employee { Name = "Abhishek" };
```

Why use a class? It groups state and behavior into a model that
represents a concept in the application.

## 2. Encapsulation

Encapsulation means controlling access to an object's state and exposing
safe operations.

Weak design:

``` csharp
public class Employee
{
    public decimal Salary;
}
```

Any caller can assign an invalid value:

``` csharp
employee.Salary = -50000;
```

A stronger design protects the invariant:

``` csharp
public class Employee
{
    public decimal Salary { get; private set; }

    public void ChangeSalary(decimal newSalary)
    {
        if (newSalary < 0)
            throw new ArgumentOutOfRangeException(nameof(newSalary));

        Salary = newSalary;
    }
}
```

The important idea is not merely making fields private. It is ensuring
that an object cannot be placed into an invalid state through its public
API.

## 3. Inheritance

Inheritance models an actual **is-a** relationship.

``` csharp
public class Animal
{
    public void Eat() { }
}

public class Dog : Animal
{
}
```

Use inheritance when the derived type can honor the base type's
contract. Avoid inheritance only to reuse a few lines of code;
composition is often more flexible.

## 4. Polymorphism

Polymorphism lets callers use a common abstraction while implementations
provide different behavior.

``` csharp
public class Animal
{
    public virtual void Speak() =>
        Console.WriteLine("Animal sound");
}

public class Dog : Animal
{
    public override void Speak() =>
        Console.WriteLine("Bark");
}

Animal animal = new Dog();
animal.Speak(); // Bark
```

The runtime type determines which overridden implementation is called.

## 5. Abstraction

Abstraction exposes the capability a caller needs while hiding
implementation details.

``` csharp
public interface IPayment
{
    Task PayAsync(decimal amount);
}
```

A caller can depend on `IPayment` without knowing whether the
implementation uses UPI, a card gateway, or another provider.

## 6. Abstract class vs interface

An abstract class can provide shared state, constructors, fields, and
implementation, as well as abstract members.

An interface primarily expresses a contract/capability. Modern C#
interfaces can also contain default implementations and static members,
so avoid the outdated claim that interfaces can contain only method
declarations.

``` csharp
public abstract class PaymentBase
{
    protected PaymentBase(string providerName)
    {
        ProviderName = providerName;
    }

    public string ProviderName { get; }

    public abstract Task PayAsync(decimal amount);
}

public interface IRefundable
{
    Task RefundAsync(string transactionId);
}
```

Use an abstract base class when there is meaningful shared state or
behavior and a genuine common base concept. Use an interface when you
need a capability/contract that different types can implement, including
types that cannot share a base class.

### Interview answer

"An abstract class is useful when related types share state or
implementation. An interface is useful when different types need to
promise the same capability. I choose based on the relationship and the
design, not because one is always better."

------------------------------------------------------------------------

# Day 3 --- Access Modifiers, `static`, Fields, Properties and Exceptions

## 1. Access modifiers

  -----------------------------------------------------------------------
  Modifier                            Meaning
  ----------------------------------- -----------------------------------
  `public`                            Accessible wherever the containing
                                      type is accessible

  `private`                           Accessible within the containing
                                      type

  `protected`                         Accessible within the containing
                                      type and derived types

  `internal`                          Accessible within the same assembly

  `protected internal`                Same assembly OR derived type
                                      access

  `private protected`                 Derived types within the same
                                      assembly
  -----------------------------------------------------------------------

Default to the least visibility that supports the design. Public API
surface is a long-term compatibility commitment.

## 2. `static`

A static member belongs to the type rather than an individual instance.

``` csharp
public class RequestCounter
{
    public static int Total;
}

RequestCounter.Total++;
```

There is one static field per type/load context, not one per object
instance.

Engineering caution: - Shared mutable static state can create
concurrency problems. - It can make tests order-dependent. - It can make
dependencies hidden. - Static does not automatically mean thread-safe.

Use static for genuinely type-level behavior or immutable/shared data
where appropriate.

## 3. Fields vs properties

A field stores data. A property exposes accessors and can enforce rules
or control visibility.

``` csharp
public class Incident
{
    public string Title { get; private set; } = "";

    public void Rename(string title)
    {
        if (string.IsNullOrWhiteSpace(title))
            throw new ArgumentException("Title is required.", nameof(title));

        Title = title;
    }
}
```

Properties provide a stable public API while allowing implementation
details to change.

## 4. Exception handling

``` csharp
try
{
    await ProcessAsync();
}
catch (SpecificException ex)
{
    logger.LogError(ex, "Processing failed");
    throw;
}
finally
{
    // Cleanup that must run when control leaves the try/catch.
}
```

Guidelines: - Catch exceptions when the current layer can recover, add
useful context, or translate them appropriately. - Prefer specific
exception types. - Do not silently swallow unexpected failures. - Do not
use exceptions for ordinary branching. - Do not expose stack traces or
sensitive internal details to API clients.

`finally` is used for cleanup that must run when control leaves the
`try`/`catch` flow, subject to process termination and other runtime
limits. For disposable resources, prefer `using`/`await using`.

------------------------------------------------------------------------

# Day 4 --- `var`, `dynamic`, Nullable Types, Structs, Records and Equality

## 1. `var`

`var` is compile-time type inference, not dynamic typing.

``` csharp
var count = 10; // inferred as int
```

The compiler determines the type at compile time. The variable does not
change to another type later.

Use `var` when the type is obvious from the right-hand side or when it
improves readability. Do not use it when it hides an important type from
the reader.

## 2. `dynamic`

`dynamic` defers member binding/type checks to runtime.

``` csharp
dynamic value = 10;
value = "hello";

Console.WriteLine(value.Length);
```

A member call can compile but fail at runtime if the runtime object does
not support it.

Use `dynamic` intentionally for scenarios such as dynamic interop or
APIs whose shape is not known at compile time. Prefer static typing for
ordinary application code because compiler checks catch many errors
earlier.

## 3. Nullable value types and nullable reference types

Nullable value type:

``` csharp
int? age = null;
```

This represents an integer value or absence (`null`).

Nullable reference type annotations:

``` csharp
string? description = null;
string title = "Deployment failed";
```

Nullable reference types provide compiler analysis and warnings; they do
not create a different runtime string type.

Treat null as a meaningful domain state when appropriate. Validate
required input at system boundaries.

## 4. Structs

A `struct` is a value type. It is often appropriate for small value-like
data with value semantics.

``` csharp
public struct Point
{
    public int X { get; set; }
    public int Y { get; set; }
}
```

Do not choose a struct solely because someone says it is faster.
Consider: - size and copying cost - mutability - identity vs value
semantics - boxing - expected usage patterns

Prefer small, usually immutable structs for value-like concepts.

## 5. Records

Records are useful for data-centric models and provide value-based
equality by default.

``` csharp
public record Person(string Name, int Age);

var first = new Person("Asha", 25);
var second = new Person("Asha", 25);

Console.WriteLine(first == second); // True
```

Records are not automatically immutable in every respect. A positional
record class commonly exposes init-only properties, but record
properties can be designed differently. A `record struct` is a value
type; a `record class` is a reference type.

## 6. Equality

Two different questions:

-   Reference equality: are these references pointing to the same
    object?
-   Value equality: do these objects represent equivalent values?

Classes typically use reference equality by default unless equality is
overridden. Records synthesize value-oriented equality.

Equality matters for: - dictionary keys and hash sets - deduplication -
caching - comparing domain values - tests

If you override equality, implement a consistent `GetHashCode()`
contract: equal objects must have equal hash codes.

### Interview answer

"`var` is compile-time inference; `dynamic` defers binding to runtime.
Nullable types represent or annotate possible absence. Structs have
value semantics, while record types provide convenient value-based
equality. I choose based on semantics and usage, not a simplistic
performance rule."

------------------------------------------------------------------------

# Day 5 --- Collections and Big-O Complexity

## 1. Array

An array is a fixed-length indexed collection.

``` csharp
int[] values = { 10, 20, 30 };
Console.WriteLine(values[1]); // 20
```

Index access is O(1). Insertion/removal in the middle generally requires
shifting elements.

## 2. List`<T>`{=html}

`List<T>` is a resizable array-backed collection.

``` csharp
var values = new List<int>();
values.Add(10);
values.Add(20);
```

Typical costs: - Index access: O(1) - Append: amortized O(1) -
Insert/remove in the middle: O(n), due to shifting - Search by value:
O(n)

Amortized O(1) append means most appends are constant time, while
occasional capacity growth copies elements.

## 3. Dictionary\<TKey,TValue\>

A dictionary maps keys to values using hashing.

``` csharp
var users = new Dictionary<int, string>();
users[101] = "Asha";

if (users.TryGetValue(101, out var name))
{
    Console.WriteLine(name);
}
```

Lookup/insert/remove are average O(1) under suitable hashing and load
conditions; worst-case behavior can be worse. Hash collisions are
handled by the implementation.

Prefer `TryGetValue` when checking and retrieving a key to avoid a
separate lookup.

## 4. HashSet`<T>`{=html}

A hash set stores unique elements and is useful for membership checks
and set operations.

``` csharp
var processedIds = new HashSet<int>();
bool firstAdd = processedIds.Add(101);
bool secondAdd = processedIds.Add(101);

Console.WriteLine(firstAdd);  // True
Console.WriteLine(secondAdd); // False
```

Average membership checks are generally O(1), subject to hashing
behavior.

## 5. Big-O

Big-O describes how resource usage grows as input size grows. It is not
an exact runtime measurement.

  Complexity   Typical interpretation
  ------------ --------------------------------
  O(1)         Constant
  O(log n)     Logarithmic
  O(n)         Linear
  O(n log n)   Common efficient sorting scale
  O(n²)        Quadratic

Example:

``` csharp
for (int i = 0; i < n; i++)
{
    Process(i);
}
```

This performs work proportional to `n`: O(n), assuming `Process` is
O(1).

Nested loops over the same input often produce O(n²), but analyze the
actual loop bounds rather than blindly counting loops.

## 6. Product-company reasoning

Weak answer: "Dictionary is O(1)."

Stronger answer: "Dictionary lookup is average O(1) because it uses
hashing. Collisions and implementation details mean it is not an
unconditional guarantee. I would use it for key-based lookup when the
key semantics and memory trade-off make sense."

Consider memory overhead, key equality, hash quality, ordering
requirements, and expected collection size.

------------------------------------------------------------------------

# Day 6 --- Generics and Interfaces

## 1. Generics

Generics let us write reusable, type-safe code.

``` csharp
public class Box<T>
{
    public T? Value { get; set; }
}

var intBox = new Box<int> { Value = 10 };
var textBox = new Box<string> { Value = "hello" };
```

Benefits: - compile-time type safety - reuse across types - fewer
casts - can avoid boxing for value types in generic collections

## 2. Generic constraints

Constraints restrict which types can be used and communicate
requirements to callers.

``` csharp
public class Repository<T>
    where T : class
{
}
```

Other constraints include `struct`, `notnull`, `new()`, base-class
constraints, interface constraints, and combinations.

Use constraints to express what the generic implementation actually
needs.

## 3. Interfaces

An interface defines a contract/capability.

``` csharp
public interface IPaymentProcessor
{
    Task ProcessAsync(decimal amount);
}
```

Different implementations can honor the same contract:

``` csharp
public sealed class CardPaymentProcessor : IPaymentProcessor
{
    public Task ProcessAsync(decimal amount) => Task.CompletedTask;
}

public sealed class UpiPaymentProcessor : IPaymentProcessor
{
    public Task ProcessAsync(decimal amount) => Task.CompletedTask;
}
```

The caller can depend on `IPaymentProcessor` rather than a concrete
payment provider.

## 4. Why generics and interfaces are different

-   Generics parameterize code over types.
-   Interfaces define contracts/capabilities.
-   They can be combined, for example `IRepository<T>`.

Do not use an interface for every class automatically. Add an
abstraction when it provides a useful boundary, variation point, or test
seam.

------------------------------------------------------------------------

# Day 7 --- Delegates, Func, Action and Events

## 1. Delegate

A delegate is a type-safe reference to a method with a compatible
signature.

``` csharp
public delegate void Notify(string message);

static void Send(string message)
{
    Console.WriteLine(message);
}

Notify notify = Send;
notify("Incident created");
```

A delegate lets behavior be passed as data.

## 2. Action and Func

`Action` represents a method that returns `void`.

``` csharp
Action<string> print = message => Console.WriteLine(message);
print("Hello");
```

`Func` represents a method that returns a value. The last generic type
argument is the return type.

``` csharp
Func<int, int> square = number => number * number;
Console.WriteLine(square(5)); // 25
```

`Predicate<T>` is another common delegate type that returns `bool`.

## 3. Events

Events provide a publisher/subscriber pattern built on delegates.

``` csharp
public sealed class Order
{
    public event EventHandler? Placed;

    public void Place()
    {
        // Validate and update order state.
        Placed?.Invoke(this, EventArgs.Empty);
    }
}
```

A subscriber can react:

``` csharp
order.Placed += OnOrderPlaced;
```

Events help decouple the publisher from the subscribers' concrete
behavior. The publisher announces that something happened; subscribers
decide how to react.

## 4. Event lifetime and memory retention

A publisher's event delegate holds references to subscribed handlers. If
a long-lived publisher retains a short-lived subscriber, the subscriber
may remain reachable longer than intended.

``` text
Long-lived publisher
       |
       v
event delegate -> subscriber
```

Unsubscribe when the subscriber's lifetime ends, or use an appropriate
subscription-lifetime pattern.

Managed code can still retain objects unintentionally. The GC cannot
collect an object that remains reachable.

## 5. Engineering trade-offs

Events are useful for in-process notifications. They are not
automatically a durable messaging system: - subscribers run in the same
process - failures and ordering need consideration - events are not
persisted by default - long-running or distributed workflows may need a
message broker or durable event/outbox design

------------------------------------------------------------------------

# Day 8 --- LINQ, Deferred Execution, IEnumerable and IQueryable

## 1. LINQ

LINQ provides query operators over data sources.

``` csharp
var adultNames = people
    .Where(person => person.Age >= 18)
    .OrderBy(person => person.Name)
    .Select(person => person.Name);
```

Common operators:

  Operator              Purpose
  --------------------- ----------------------------------------------
  `Where`               Filter
  `Select`              Project/transform
  `OrderBy`             Sort
  `GroupBy`             Group
  `Any`                 Check whether at least one matches
  `All`                 Check whether all match
  `First`               First matching item; throws if none
  `FirstOrDefault`      First item or default
  `Single`              Exactly one; throws if zero or more than one
  `Count`               Count items
  `Sum`, `Min`, `Max`   Aggregates

Know the behavioral difference between `First`, `FirstOrDefault`,
`Single`, and `SingleOrDefault`. Choose the operator that expresses the
actual cardinality rule.

## 2. Deferred execution

Many LINQ queries are not executed when constructed. They execute when
enumerated.

``` csharp
var query = people.Where(person => person.Age >= 18);

// Query may execute when enumerated:
foreach (var person in query)
{
    Console.WriteLine(person.Name);
}
```

This can be useful because it allows composition. But it can also
surprise you: - the query may execute multiple times - the underlying
data may change between enumerations - database queries may be
repeated - exceptions may occur at enumeration time

## 3. Materialization with ToList

``` csharp
var adults = people
    .Where(person => person.Age >= 18)
    .ToList();
```

`ToList()` executes the query and stores the results in memory. It
creates a snapshot of the returned elements at that time, though
referenced objects themselves may still be mutable.

Do not call `ToList()` too early on a large database query; that can
pull unnecessary data into application memory.

## 4. IEnumerable`<T>`{=html} vs IQueryable`<T>`{=html}

-   `IEnumerable<T>` represents a sequence that can be enumerated.
    LINQ-to-Objects operators run as .NET code over in-memory elements.
-   `IQueryable<T>` represents a query with an expression tree and a
    provider. A provider, such as EF Core, may translate supported
    expressions into SQL or another query language.

Example:

``` csharp
IQueryable<Incident> query = db.Incidents
    .Where(x => x.Status == "Open");

var result = await query
    .OrderBy(x => x.Id)
    .ToListAsync();
```

The provider can translate the composed query before execution.

Be careful when switching from `IQueryable` to `IEnumerable`: operators
applied after materialization or client-side enumeration may run in
memory and process far more data than intended.

### Interview answer

"`IEnumerable` is the enumerable sequence abstraction; `IQueryable`
carries an expression tree that a provider can translate. With EF Core,
I keep filtering and projection in the query as long as the provider can
translate it, and materialize only when I need the data in memory."

------------------------------------------------------------------------

# Day 9 --- Exception Design and Error Handling

## 1. Exception flow

``` text
Operation
   |
   v
Exception occurs
   |
   v
Nearest matching catch
   |
   +--> Recover / translate / add context
   |
   +--> Rethrow / propagate
   |
   v
Finally cleanup
```

## 2. `throw` vs `throw ex`

Inside a catch block:

``` csharp
catch
{
    throw;
}
```

preserves the original stack trace.

This form:

``` csharp
catch (Exception ex)
{
    throw ex;
}
```

resets the stack trace origin and makes diagnosis harder. If rethrowing
the same exception, use `throw;`.

## 3. When should we catch?

Ask: can this layer do something useful?

Useful reasons to catch: - recover from a known condition - translate a
lower-level exception into a meaningful application exception - add
context and log at an appropriate boundary - perform a required
compensating action

Avoid catch-and-ignore. Avoid logging the same exception at every layer,
which can create duplicate noise.

## 4. Global exception handling

In ASP.NET Core, centralized exception handling can: - log unexpected
failures - return a consistent Problem Details response - avoid
repetitive try/catch blocks in every controller

Do not send internal stack traces, connection strings, or database
details to clients.

### Interview answer

"I catch an exception at a layer that can recover or translate it.
Otherwise I let it propagate to a boundary that can handle it
consistently. I preserve the original stack trace and never silently
turn a failed operation into success."

------------------------------------------------------------------------

# Day 10 --- Async/Await, Task, Task.Run, ThreadPool and Cancellation

## 1. What does async/await solve?

Asynchronous programming is useful when an operation may take time,
especially I/O such as database, HTTP, file, or network operations.

``` csharp
public async Task<string> GetDataAsync()
{
    string result = await SomeOperationAsync();
    return result;
}
```

`await` does not mean "create a new thread." In normal asynchronous I/O,
the method can yield while the I/O is pending rather than blocking a
thread just to wait.

## 2. I/O-bound vs CPU-bound

  -----------------------------------------------------------------------
  I/O-bound                           CPU-bound
  ----------------------------------- -----------------------------------
  Waiting for database/network/file   Performing computation

  Async I/O APIs are useful           Parallelism or background execution
                                      may help

  Thread need not sit blocked during  CPU time is still required
  async wait                          
  -----------------------------------------------------------------------

Examples: - I/O-bound: `HttpClient.GetAsync`, EF Core `ToListAsync`. -
CPU-bound: image transformation, compression, large numerical
computation.

## 3. Task and Task`<T>`{=html}

-   `Task` represents an asynchronous operation with no returned result.
-   `Task<T>` represents an asynchronous operation that eventually
    produces a `T`.
-   A task is an operation handle, not necessarily a dedicated thread.

``` csharp
Task saveTask = SaveAsync();
Task<int> countTask = CountAsync();
```

## 4. Task.Run

`Task.Run` commonly schedules work on a ThreadPool thread.

``` csharp
int result = await Task.Run(() => Calculate());
```

This can be useful for CPU-bound work when moving it off a responsive UI
thread is appropriate. In ASP.NET Core, offloading CPU work to the
ThreadPool does not create more CPU capacity and can hurt throughput if
overused.

Do not wrap naturally asynchronous I/O in `Task.Run` just to make it
appear asynchronous:

``` csharp
// Prefer the async API:
await httpClient.GetAsync(url);
```

## 5. ThreadPool

The .NET ThreadPool maintains reusable worker threads. Reuse avoids
creating a brand-new thread for every small unit of work.

`Task.Run` commonly schedules work there. It is not a reason to create
unbounded background work. Excessive blocking or CPU-heavy work can
starve the pool and reduce application responsiveness.

## 6. CancellationToken

Cancellation is cooperative. A token communicates a request to stop; it
does not forcibly terminate arbitrary code.

``` csharp
public async Task ProcessAsync(CancellationToken token)
{
    for (int i = 0; i < 10; i++)
    {
        token.ThrowIfCancellationRequested();
        await Task.Delay(TimeSpan.FromSeconds(1), token);
    }
}
```

Caller:

``` csharp
using var source = new CancellationTokenSource();

Task work = ProcessAsync(source.Token);
source.Cancel();

await work;
```

The operation must observe the token or call APIs that support it.

Pass cancellation tokens through layers to database and HTTP operations
when supported. Distinguish expected cancellation from genuine failure.

### Interview answers

**Does async create a thread?** No. Async/await represents asynchronous
control flow. Naturally asynchronous I/O can yield without occupying a
thread while waiting.

**What is Task.Run for?** Commonly scheduling CPU-bound work on a
ThreadPool thread when that is appropriate; it is not the definition of
async.

**What is cancellation?** A cooperative request for an operation to
stop.

------------------------------------------------------------------------

# Day 11 --- Garbage Collection, Reachability, IDisposable and Resource Management

## 1. Garbage Collection

The .NET Garbage Collector reclaims managed memory occupied by objects
that are no longer reachable from GC roots.

``` csharp
var person = new Person();
person = null;
```

Setting a reference to null removes that reference. It does not
immediately free the object. If no other references remain, the object
may become eligible for collection.

## 2. Reachability

An object is alive from the GC's perspective while it remains reachable
through roots such as active stack references, static fields, and other
runtime roots.

Example:

``` csharp
public static class Cache
{
    public static readonly List<Person> People = new();
}
```

If an object remains in this static list, it remains reachable through
the static field and cannot be collected.

Managed applications can therefore have memory-retention problems even
without manual memory allocation.

Common causes: - static collections that grow without bounds - event
subscriptions not removed - long-lived caches without eviction -
references held by long-lived services

## 3. GC generations and performance intuition

The .NET GC uses generations based on object lifetime patterns.
Short-lived objects are commonly collected more frequently than
long-lived objects. Allocation is often cheap, but excessive allocation
increases GC work and can affect latency.

Do not call `GC.Collect()` as routine application cleanup. The runtime
generally manages collection more effectively using its knowledge of
allocation and reachability.

## 4. IDisposable

Garbage collection manages managed memory. It does not replace
deterministic cleanup of resources such as file handles, sockets, and
other resources owned by objects.

``` csharp
public sealed class ResourceOwner : IDisposable
{
    public void Dispose()
    {
        // Release owned resources.
    }
}
```

Dispose releases resources owned by the object. It does not mean that
the object itself is instantly removed from memory.

## 5. using and await using

``` csharp
using var stream = File.OpenRead("data.txt");
```

The compiler ensures disposal when control leaves the scope, including
when an exception occurs.

For asynchronously disposable resources:

``` csharp
await using var resource = new AsyncResource();
```

Dispose only resources your code owns or is responsible for disposing.
Do not dispose a dependency that is owned and managed by a DI container
unless ownership was explicitly transferred.

## 6. HttpClient lifetime

Creating and disposing a new `HttpClient` for every request is generally
not the recommended modern ASP.NET Core pattern.

Repeated connection creation can waste resources and contribute to
socket exhaustion or connection-management problems. Common approaches
include: - reusing a long-lived client with an appropriate connection
lifetime - using `IHttpClientFactory` in ASP.NET Core to manage handler
pooling and client creation

`IHttpClientFactory` is especially useful for centralized configuration,
logging, delegating handlers, and handler lifetime management.

### Interview answer

"GC reclaims unreachable managed objects. `Dispose` releases resources
an object owns at a deterministic time. They solve different problems.
In ASP.NET Core I commonly use `IHttpClientFactory` rather than
constructing and disposing a new client for every request."

------------------------------------------------------------------------

# Day 12 --- Thread Safety, Race Conditions, Span/Memory and SOLID

## 1. Thread safety

Code is thread-safe when it behaves correctly under the concurrent
access patterns it supports.

A shared mutable variable is a common source of concurrency bugs.

``` csharp
counter++;
```

This is not guaranteed to be one indivisible operation. Conceptually it
involves: 1. read 2. add 3. write

Two threads can interleave these steps and lose an update.

## 2. Race condition

A race condition occurs when the result depends on timing or
interleaving of concurrent operations.

``` text
counter = 0

Thread A reads 0
Thread B reads 0
Thread A writes 1
Thread B writes 1

Expected count after two increments: 2
Actual count: 1
```

The example illustrates a lost update.

## 3. lock

`lock` ensures that only one thread at a time can enter the protected
critical section for the same lock object.

``` csharp
private readonly object _sync = new();
private int _counter;

public void Increment()
{
    lock (_sync)
    {
        _counter++;
    }
}
```

Use a private, stable lock object. Do not lock on `this`, a public type,
or a string because other code may lock the same object unexpectedly.

Use `lock` when multiple related operations must be protected as one
critical section.

## 4. Interlocked

`Interlocked` provides atomic operations for specific primitive
operations.

``` csharp
private int _counter;

public void Increment()
{
    Interlocked.Increment(ref _counter);
}
```

For a simple atomic increment, this is a targeted option.

`Interlocked` does not replace `lock` for arbitrary multi-step
invariants. Use `lock` when several related reads/writes must be
coordinated together.

  -----------------------------------------------------------------------------------
  Need                                            Typical tool
  ----------------------------------------------- -----------------------------------
  Atomic                                          `Interlocked`
  increment/decrement/exchange/compare-exchange   

  Protect multiple related state changes          `lock`

  Coordinate async workflows                      Async-compatible coordination
                                                  primitives, such as
                                                  `SemaphoreSlim`, when appropriate
  -----------------------------------------------------------------------------------

## 5. Span`<T>`{=html}

`Span<T>` represents a contiguous region of memory. It can provide
efficient slicing and access without necessarily allocating a new array
or collection.

``` csharp
Span<int> numbers = stackalloc int[5];
numbers[0] = 42;

Span<int> slice = numbers.Slice(0, 2);
```

Important characteristics: - `Span<T>` is a `ref struct`. - It has
restrictions that prevent it from being stored in ordinary heap objects
or used across ordinary async suspension points. - It is useful for
high-performance parsing, buffer processing and slicing. - It does not
automatically make code faster; measure the real workload.

## 6. Memory`<T>`{=html}

`Memory<T>` represents memory that can be stored and passed around more
flexibly than `Span<T>`, including across async boundaries.

``` csharp
Memory<byte> buffer = new byte[1024];

await ReadIntoBufferAsync(buffer);
```

Simple comparison:

  -----------------------------------------------------------------------
  Span`<T>`{=html}                    Memory`<T>`{=html}
  ----------------------------------- -----------------------------------
  `ref struct`                        Regular struct

  Very efficient short-lived view     More flexible lifetime

  Cannot be stored in ordinary heap   Can be stored and used across async
  objects                             boundaries

  Useful for synchronous              Useful when buffers need to flow
  parsing/slicing                     through async APIs
  -----------------------------------------------------------------------

Use the abstraction required by the API and lifetime. Do not introduce
low-level memory types without a measured need.

## 7. SOLID principles

SOLID is a set of design principles, not a checklist that every class
must visibly demonstrate. Apply them when they improve maintainability
and changeability.

### S --- Single Responsibility Principle

A module should have one focused responsibility and one main reason to
change.

A service that handles order rules, database persistence, email and
invoice generation may have too many responsibilities.

A possible separation:

``` text
OrderService
OrderRepository
EmailService
InvoiceService
```

Avoid interpreting SRP as "one method per class." The goal is cohesive
responsibility.

### O --- Open/Closed Principle

Software entities should be open for extension but closed for
unnecessary modification of stable existing logic.

Fragile design:

``` csharp
if (paymentType == "UPI")
{
    // UPI flow
}
else if (paymentType == "CARD")
{
    // Card flow
}
```

A new payment type repeatedly changes the same conditional.

A strategy abstraction may help:

``` csharp
public interface IPaymentProcessor
{
    Task ProcessAsync(decimal amount);
}
```

New implementations can be added behind the contract. Do not add
abstractions for hypothetical future requirements; use them when
variation is real.

### L --- Liskov Substitution Principle

A subtype should honor the behavioral contract of its base type. Code
that works with the base abstraction should not break unexpectedly when
given a valid subtype.

If a derived type overrides a promised operation by throwing
`NotSupportedException`, the abstraction may be wrong.

### I --- Interface Segregation Principle

Clients should not be forced to depend on methods they do not use.

Instead of one giant interface containing unrelated capabilities, split
contracts according to actual client needs.

### D --- Dependency Inversion Principle

High-level policy should depend on abstractions rather than concrete
low-level details.

Weak coupling:

``` csharp
public class OrderService
{
    private readonly SqlOrderRepository _repository =
        new SqlOrderRepository();
}
```

More flexible design:

``` csharp
public class OrderService
{
    private readonly IOrderRepository _repository;

    public OrderService(IOrderRepository repository)
    {
        _repository = repository;
    }
}
```

The business logic depends on a contract rather than a specific storage
implementation.

## 8. DIP vs DI

These are related but not identical:

``` text
DIP = design principle
      Depend on abstractions.

DI  = implementation technique
      Supply dependencies from outside.
```

Constructor injection is one common DI technique. Depending on an
interface rather than a concrete repository is an example of applying
DIP.

## 9. Combining SOLID in a real design

A use-case service can orchestrate abstractions:

``` csharp
public interface IPaymentProcessor
{
    Task ProcessAsync(decimal amount);
}

public interface IOrderRepository
{
    Task SaveAsync(Order order);
}

public interface IEmailService
{
    Task SendConfirmationAsync(Order order);
}

public sealed class OrderService
{
    private readonly IPaymentProcessor _payment;
    private readonly IOrderRepository _repository;
    private readonly IEmailService _email;

    public OrderService(
        IPaymentProcessor payment,
        IOrderRepository repository,
        IEmailService email)
    {
        _payment = payment;
        _repository = repository;
        _email = email;
    }

    public async Task PlaceOrderAsync(Order order)
    {
        await _payment.ProcessAsync(order.Amount);
        await _repository.SaveAsync(order);
        await _email.SendConfirmationAsync(order);
    }
}
```

This illustrates dependency inversion and separation of
responsibilities, but it is not a complete production payment workflow.
A real system must consider transaction boundaries, payment idempotency,
failure after payment but before persistence, and reliable notification
delivery. An outbox/saga-style approach may be appropriate depending on
requirements.

## Product-company design review questions

``` text
Too many reasons to change?
    -> Consider SRP.

New behavior repeatedly modifies stable logic?
    -> Consider OCP or a strategy/polymorphism design.

Subtype cannot honor base contract?
    -> Revisit LSP and the abstraction.

Interface forces irrelevant methods?
    -> Consider ISP.

Business logic directly constructs infrastructure?
    -> Consider DIP and DI.
```

A real design can involve several principles at once. Do not force every
principle into every design.

------------------------------------------------------------------------

# Cross-Day Engineering Lessons

## 1. Value/reference is not stack/heap

Type semantics describe copying and identity behavior. Actual storage
depends on context.

## 2. Async does not mean a new thread

Async/await is an asynchronous control-flow model. Naturally
asynchronous I/O can yield while waiting without dedicating a thread to
the wait.

## 3. Task.Run is not the definition of async

``` text
Task.Run
    -> commonly schedules work on ThreadPool
    -> useful for suitable CPU-bound work

Async I/O
    -> use the API's async method
    -> do not wrap it in Task.Run just to call it async
```

## 4. Cancellation is cooperative

A cancellation token communicates a request to stop. The operation must
observe it.

## 5. GC and Dispose solve different problems

``` text
GC
    -> reclaims unreachable managed memory

Dispose
    -> deterministic cleanup of owned resources
```

## 6. Managed code can still retain memory

Static collections, events, caches and long-lived references can keep
objects reachable.

## 7. lock vs Interlocked

``` text
Simple atomic operation
    -> Interlocked

Multiple related state changes
    -> lock
```

## 8. DI vs DIP

``` text
DIP = principle
DI  = technique
```

------------------------------------------------------------------------

# Interview Answer Framework

When asked a technical question, use this structure:

1.  **Definition:** What is it?
2.  **Problem:** Why do we need it?
3.  **Mechanism:** What happens when it runs?
4.  **Example:** Show a small piece of code.
5.  **Failure mode:** What can go wrong?
6.  **Decision:** When would you use it, and what is the alternative?

Example: "Why use `lock`?"

Weak answer:

> Lock is used for thread safety.

Stronger answer:

> "`lock` protects a critical section so multiple threads cannot execute
> that protected section concurrently using the same lock object. For
> example, `counter++` is a read-modify-write operation and can lose
> updates under concurrency. If several related state changes must be
> protected as one unit, `lock` is appropriate. For a simple atomic
> increment, `Interlocked.Increment` may be more targeted."

------------------------------------------------------------------------

# Master Quick-Revision Map

``` text
C# FUNDAMENTALS
├── Value vs Reference
├── ref / out / in
├── Boxing / Unboxing
├── Classes / Objects
├── Encapsulation / Inheritance / Polymorphism
├── Abstraction
├── Interfaces / Abstract classes
├── Access modifiers
├── static / Fields / Properties
├── var / dynamic
├── Nullable types
├── Structs / Records
├── Equality / GetHashCode
│
├── Collections
│   ├── Array
│   ├── List<T>
│   ├── Dictionary<TKey,TValue>
│   └── HashSet<T>
├── Big-O
├── Generics / Constraints
├── Delegates / Func / Action
├── Events / Subscription lifetime
├── LINQ / Deferred execution
├── IEnumerable<T> / IQueryable<T>
├── Exceptions / Global handling
│
├── Async / Await / Task<T>
├── I/O-bound vs CPU-bound
├── Task.Run / ThreadPool
├── CancellationToken
│
├── GC / Reachability / Generations
├── IDisposable / using / await using
├── HttpClient / IHttpClientFactory
│
├── Thread safety / Race conditions
├── lock / Interlocked
├── Span<T> / Memory<T>
│
└── SOLID
    ├── SRP
    ├── OCP
    ├── LSP
    ├── ISP
    └── DIP
        └── DI
```

# Practical Mastery Checklist

The following are the next validation steps. Do not mark them complete
until the code has been run and the behavior has been checked.

-   [ ] Write small console programs for value/reference copying and
    `ref`/`out`/`in`.
-   [ ] Implement encapsulation and polymorphism in a small domain
    model.
-   [ ] Compare class, struct and record equality behavior.
-   [ ] Implement a frequency counter using `Dictionary`.
-   [ ] Write a generic type and a generic method with constraints.
-   [ ] Build a delegate/event publisher-subscriber example and
    unsubscribe correctly.
-   [ ] Demonstrate LINQ deferred execution and materialization.
-   [ ] Implement an async I/O call with cancellation.
-   [ ] Use `using`/`await using` for owned resources.
-   [ ] Reproduce a race condition and fix it with `Interlocked` or
    `lock`.
-   [ ] Use `Span<T>` for a small parsing/slicing exercise.
-   [ ] Refactor a tightly coupled service to depend on an abstraction.
-   [ ] Explain each solution's trade-offs and failure cases.

# Day 1--12 Completion Status

The original learning track covered these areas conceptually:

-   C# fundamentals and parameter passing
-   OOP and abstraction
-   Access modifiers, static members and properties
-   `var`, `dynamic`, nullable types, structs, records and equality
-   Collections and Big-O
-   Generics and interfaces
-   Delegates and events
-   LINQ and deferred execution
-   Exceptions and error handling
-   Async/await, Task, Task.Run, ThreadPool and cancellation
-   GC, reachability, IDisposable and resource management
-   Thread safety, race conditions, `lock`, `Interlocked`
-   Span/Memory
-   SOLID and DI/DIP

**Important:** Conceptual coverage is not the same as independent
implementation or interview mastery. Use the practical checklist and a
Day 1--12 assessment to identify weak areas before relying on these
topics in interviews.

------------------------------------------------------------------------

# Recommended Day 1--12 Assessment

1.  Concept explanation questions
2.  WHY/design reasoning questions
3.  Code-output prediction
4.  Debugging and bug-fixing exercises
5.  Small implementation tasks
6.  Performance and memory questions
7.  Concurrency and async scenarios
8.  OOP/SOLID design exercises
9.  Product-company interview follow-ups
10. Weak-area revision and mock interview

The target is not to recite definitions. The target is to explain the
behavior, implement it, debug it, and justify the design.
