# C# Interview Guide — All-in-One (From Scratch to Interview-Ready, 2 YOE)

> One file. Simple words first, then an **easy** example, then a **complex / real-world** example.
> Every example that shows output was **compiled and run on .NET 10** before being written here — the output you see is the output you'll get.

---

## How to use this guide

| Marker | Meaning |
|---|---|
| **Why interviewers ask this** | Why the topic matters |
| ⚠️ **Interview trap** | A common wrong answer — avoid it |
| 🎯 **Say this in the interview** | Short, confident model answer to memorise |
| 🧩 **Predict the output** | Puzzle; answer is hidden — think first, then click |
| **Try it yourself** | Exercises with hidden answers |
| **Quick revision** | One-liners to read on interview morning |

**How to practise:** create one scratch project and paste examples into `Program.cs`:

```bash
dotnet new console -n Playground
cd Playground
dotnet run
```

---

## What is expected from a 2-YOE C# developer?

| Area | Junior (0-1 yr) | **You (2 yrs) — expected** | Senior (5+ yrs) |
|---|---|---|---|
| Language basics | Syntax, loops, classes | **Value vs reference, boxing, ref/out, string immutability** | IL-level details |
| OOP | Can define classes | **Explain the 4 pillars with examples, virtual vs new, abstract vs interface, composition** | Domain modelling |
| Collections & LINQ | List, foreach | **Dictionary internals, Big-O, deferred execution, IEnumerable vs IQueryable, GroupBy/Join** | Custom collections |
| Delegates/events | Use lambdas | **Func/Action, closures, events vs delegates, expression trees** | — |
| Async | `await` something | **What await does, deadlocks, async void, WhenAll, cancellation, ConfigureAwait** | Custom schedulers |
| Concurrency | — | **Race conditions, lock, Interlocked, SemaphoreSlim, concurrent collections** | Lock-free design |
| Memory | — | **GC generations, IDisposable/using, common leaks** | Profiling, Span-heavy code |
| Modern C# | — | **Records, pattern matching, nullable reference types, primary ctors** | — |
| Design | — | **SOLID, a few patterns (singleton, factory, strategy, decorator), DI** | Architecture |
| Problem solving | FizzBuzz | **Easy-medium problems with HashSet/Dictionary/Stack, state complexity** | Algorithms depth |

🎯 At 2 YOE, the interviewer checks: *"Do you know **why** it works, **when not** to use it, and have you **hit the problem** in real code?"*
For every topic, prepare: **What is it? Why does it exist? Example? Pitfall you faced?**

---

## The 3-day plan

### Day 1 — Language core (≈ 8 h)
| Time | Sections | Goal |
|---|---|---|
| 1 h | 1. .NET & CLR | Explain C# → IL → JIT; .NET vs .NET Framework |
| 1.5 h | 2. Types & memory | Value vs reference, boxing, const vs readonly, strings |
| 1 h | 3. Methods | ref/out/in, "reference passed by value" trap |
| 2.5 h | 4. OOP + 5. class/struct/record | **Most asked!** Pillars, virtual/override/new, abstract vs interface |
| 1 h | 6. Equality | Equals + GetHashCode contract |
| 1 h | Puzzles from section 20 (1-10) | Check yourself |

### Day 2 — Power features (≈ 8-9 h)
| Time | Sections | Goal |
|---|---|---|
| 1 h | 7. Generics | Constraints, covariance in simple words |
| 1.5 h | 8. Collections | Pick the right collection, Big-O |
| 1 h | 9. Delegates & events | Closures, events vs delegates |
| 2.5 h | 10. LINQ + 11. yield | **Most asked!** Deferred execution + solve the 15 LINQ problems |
| 1 h | 12. Exceptions | throw vs throw ex, filters, best practices |
| 1 h | 19. Coding problems (1-12) | Solve **before** reading answers |

### Day 3 — Advanced + practice (≈ 8 h)
| Time | Sections | Goal |
|---|---|---|
| 2 h | 14. Async/await | **Most asked at 2 YOE!** Deadlocks, WhenAll, cancellation |
| 1 h | 15. Multithreading | Race, lock, SemaphoreSlim, Channel |
| 1 h | 13. GC & IDisposable | Generations, Dispose pattern, leaks |
| 1 h | 16. Modern C# + 17. Reflection | Records, patterns, NRT |
| 1 h | 18. SOLID & patterns | One example per principle |
| 1 h | 19 (rest) + 20 (rest) | Coding + puzzles |
| 1 h | 21. Theory Q&A + 22. Mock | Answer **out loud** with a timer |

**Interview morning:** read only the **Quick revision** blocks + section 22 cheat sheet.

---

## Table of contents

**Part A — Language core**
1. .NET & the CLR — how C# code actually runs
2. Types, Variables & Memory
3. Methods & Parameters
4. OOP — the 4 Pillars in Depth
5. class vs struct vs record vs record struct

**Part B — Power features**
6. Equality, Hashing & Comparison
7. Generics
8. Collections & Data Structures
9. Delegates, Events & Lambdas
10. LINQ in Depth
11. Extension Methods, Iterators & yield

**Part C — Runtime & concurrency**
12. Exception Handling
13. Memory Management, GC & IDisposable
14. Async/Await in Depth
15. Multithreading & Concurrency

**Part D — Modern C#, design & practice**
16. Modern C# Features (C# 7 → 14)
17. Reflection, Attributes & Source Generators
18. SOLID & Design Patterns in C#
19. Coding Problems Interviewers Ask
20. Output-Prediction Puzzles
21. Theory Q&A (80 questions)
22. Mock Interview & Cheat Sheet

> **Related guides you already have:** `SQL-Interview-Book-ALL-IN-ONE.md` (SQL & stored procedures) and `ASPNETCore-Interview-Book-ALL-IN-ONE.md` (ASP.NET Core, EF Core, Web API).

---

# 1. .NET & the CLR — how C# code actually runs

> **Why interviewers ask this:** It shows whether you understand what happens *after* you press F5. Questions like "What is IL?", "What does the JIT do?", "Difference between .NET Framework and .NET 8?" are the classic warm-up — and a shaky answer here makes everything else sound memorized.

## 1.1 C# vs .NET vs CLR vs BCL — four words people mix up

**Simple words:** C# is the *language* you write. .NET is the *platform* that runs it. The CLR is the *engine* inside .NET. The BCL is the *toolbox* of ready-made classes.

**Analogy — a restaurant:**

| Thing | Restaurant analogy | What it really is |
|---|---|---|
| **C#** | The recipe language (English) | A programming language (C# 14 ships with .NET 10) |
| **.NET** | The whole restaurant | Runtime + libraries + SDK + tools (`dotnet` CLI) |
| **CLR** (Common Language Runtime) | The kitchen & chef | Loads code, JIT-compiles it, manages memory (GC), handles exceptions, threads, security, type safety |
| **BCL** (Base Class Library) | Pantry of ready ingredients | `System.String`, `List<T>`, `File`, `HttpClient`, LINQ… |
| **SDK** | Cooking tools | Compiler (Roslyn), `dotnet build/run/test/publish` |

Other languages (F#, VB.NET) also compile to the same IL and run on the same CLR — that's why a C# project can reference an F# library.

```csharp
using System.Runtime.InteropServices;

Console.WriteLine(RuntimeInformation.FrameworkDescription);
Console.WriteLine(typeof(int).FullName);                 // C# 'int' is just an alias
Console.WriteLine(typeof(string).Assembly.GetName().Name);
Console.WriteLine(typeof(List<int>).Namespace);
Console.WriteLine(typeof(int).IsValueType);
Console.WriteLine(typeof(string).IsValueType);
```

Output (the first line depends on your installed runtime):

```text
.NET 10.0.15
System.Int32
System.Private.CoreLib
System.Collections.Generic
True
False
```

`int` is a C# keyword; the real CLR type is `System.Int32`. Same for `string` → `System.String`, `bool` → `System.Boolean`, `long` → `System.Int64`, `object` → `System.Object`.

## 1.2 .NET Framework vs .NET (Core / 5+) vs .NET Standard

| | .NET Framework | .NET (Core 1–3.1, then .NET 5, 6, 7, 8, 9, 10…) | .NET Standard |
|---|---|---|---|
| What is it? | Original 2002 runtime | Modern, open-source rewrite | A **specification** (list of APIs), not a runtime |
| OS | Windows only | Windows, Linux, macOS, containers | — |
| Latest | 4.8.1 (maintenance only, no new features) | .NET 10 (LTS, Nov 2025) | 2.1 (final) |
| Deployment | Installed machine-wide | Side-by-side versions, self-contained, single-file, Native AOT | — |
| Performance | Good | Much faster (Span<T>, tiered JIT, PGO, better GC) | — |
| Use today for | Legacy apps (WebForms, WCF server, WPF on old stacks) | All new development | Libraries that must also run on .NET Framework |

- **LTS vs STS:** even versions (6, 8, 10) are *Long Term Support* (3 years); odd versions (7, 9) are *Standard Term Support* (2 years since .NET 9).
- **.NET Standard 2.0** is the "common denominator" — a library targeting it runs on .NET Framework 4.6.1+ and all modern .NET. For new libraries that don't need .NET Framework, just target `net8.0`/`net10.0`.

🎯 **Say this in the interview:** ".NET Framework is the Windows-only legacy runtime; .NET (formerly .NET Core) is the cross-platform, open-source successor that gets all new features. .NET Standard isn't a runtime — it's a contract of APIs that let one library work on both."

## 1.3 The compilation pipeline: C# → IL → JIT → machine code

**Simple words:** C# is compiled **twice**. First, at build time, the C# compiler (Roslyn) turns your code into **IL** (Intermediate Language — a CPU-independent bytecode). Then, at run time, the CLR's **JIT** (Just-In-Time) compiler turns IL into real machine instructions for *this* CPU, method by method, the first time each method is called.

**Analogy:** You write a book in English (C#). It is translated once into Esperanto (IL) so it can ship anywhere. In each country, a live interpreter (JIT) translates each chapter into the local language the first time someone reads it — and remembers the translation.

```text
  BUILD TIME (your machine / CI)                     RUN TIME (user's machine)
 ┌──────────────┐   Roslyn    ┌───────────────────┐    ┌──────────────────────────────┐
 │ Program.cs   │ ──────────► │ MyApp.dll         │    │            CLR               │
 │ Order.cs     │  (csc)      │  • IL code        │──► │  1. Loader reads assembly    │
 │ ...          │             │  • Metadata       │    │  2. Type safety verification │
 └──────────────┘             │  • Manifest       │    │  3. JIT: IL ──► x64/ARM64    │
                              └───────────────────┘    │     (first call of a method) │
                                                       │  4. Native code cached in    │
                                                       │     memory for next calls    │
                                                       │  5. GC, exceptions, threads  │
                                                       └──────────────┬───────────────┘
                                                                      ▼
                                                                  CPU executes
```

What a tiny method looks like as IL (you can see this with tools like ILSpy or sharplab.io):

```csharp
static int Add(int a, int b) => a + b;
```

```text
// IL (simplified) — a stack machine
ldarg.0     // push a
ldarg.1     // push b
add         // pop both, push a+b
ret         // return top of stack
```

**How it works internally (good to know at 2 YOE):**
- **Tiered compilation** (default since .NET Core 3): a method is first JIT-ed *quickly* with few optimizations (Tier 0). If it's called a lot (≈30 times) it is re-JIT-ed in the background with full optimizations (Tier 1). Hot loops can switch mid-execution via **OSR** (On-Stack Replacement).
- **Dynamic PGO** (on by default since .NET 8): Tier 0 code records what really happens (e.g., which concrete type an interface call usually hits) and Tier 1 uses that to inline/devirtualize. This is why .NET apps "warm up" and get faster.
- An assembly contains **metadata** (all types, methods, fields, signatures) — that's what powers reflection, IntelliSense and `typeof`.

### AOT and ReadyToRun (briefly)

| Mode | What happens | Pros | Cons |
|---|---|---|---|
| **JIT** (default) | IL compiled at run time | Best peak performance (PGO), full reflection | Slower startup |
| **ReadyToRun (R2R)** | IL **plus** pre-compiled native code in the DLL (`<PublishReadyToRun>true</PublishReadyToRun>`) | Faster startup; JIT can still re-optimize hot code | Bigger files |
| **Native AOT** (.NET 7+) | Whole app compiled to a native executable at publish time (`<PublishAot>true</PublishAot>`) | Tiny startup, no JIT, smaller memory — great for containers, CLIs, serverless | No runtime code generation: limited reflection, no `dynamic`, trimming warnings |

## 1.4 Managed vs unmanaged code

- **Managed code** = runs under the CLR: memory is allocated and freed by the GC, array bounds are checked, types are verified. All normal C#.
- **Unmanaged code** = runs directly on the OS: C/C++ DLLs, Win32 APIs, COM. Memory is your problem.
- You cross the boundary with **P/Invoke** (`[DllImport]` / `[LibraryImport]`) or COM interop.
- **Unmanaged resources** (file handles, sockets, DB connections) are *not* freed by the GC automatically → that's why `IDisposable` / `using` exists (covered in the GC part).

```csharp
using System.Runtime.InteropServices;

// Calling a native Windows API from C# (P/Invoke) — Windows only
[DllImport("user32.dll", CharSet = CharSet.Unicode)]
static extern int MessageBox(IntPtr hWnd, string text, string caption, uint type);
```

## 1.5 CTS and CLS

- **CTS (Common Type System):** the rules for *all* types in .NET — every type is either a **value type** or a **reference type**, everything ultimately derives from `System.Object`, single class inheritance, etc. This is why C#'s `int` and VB's `Integer` are the same `System.Int32`.
- **CLS (Common Language Specification):** a *subset* of CTS that every .NET language must support. If you write a public library for any .NET language, stay CLS-compliant (e.g., avoid public `uint` members — VB historically didn't support unsigned types; don't have two public members differing only by case).

```csharp
[assembly: CLSCompliant(true)]   // compiler now warns about non-CLS public APIs
```

## 1.6 Assemblies, namespaces & NuGet

| Concept | Simple words | Example |
|---|---|---|
| **Assembly** | The physical unit of deployment & versioning — a `.dll` or `.exe` with IL + metadata + manifest | `MyCompany.Orders.dll` |
| **Namespace** | A *logical* folder for names to avoid clashes. One assembly can hold many namespaces; one namespace can span many assemblies | `System.Collections.Generic` |
| **NuGet package** | A zip (`.nupkg`) of assemblies + dependency info, downloaded from nuget.org | `Newtonsoft.Json`, `Serilog` |

```csharp
namespace Shop.Orders;          // file-scoped namespace (C# 10+)

public class Order { }          // full name: Shop.Orders.Order
internal class OrderValidator { }   // visible only inside THIS assembly
```

```text
dotnet add package Serilog          # adds <PackageReference Include="Serilog" ... /> to the .csproj
```

- `using System.Text;` imports a namespace; `global using` (C# 10) imports it for the whole project; **implicit usings** (`<ImplicitUsings>enable</ImplicitUsings>`) add common ones like `System`, `System.Linq`, `System.Collections.Generic` — that's why the snippets in this guide can use `List<T>` without a `using`.
- `internal` means "visible inside the same **assembly**", not namespace. (`[InternalsVisibleTo("MyApp.Tests")]` opens it to a test project.)

⚠️ **Interview trap:** "Namespace = assembly?" No. Namespace is logical naming; assembly is the physical file. `System.Collections.Generic.List<T>` lives in `System.Private.CoreLib.dll`, while `System.Collections.Generic.SortedSet<T>` lives in `System.Collections.dll`.

## 1.7 Stack vs heap — the intro

**Simple words:** Your program has two main memory areas.

- **Stack** — a small, super-fast, per-thread "pile of plates". Each method call pushes a *frame* (its parameters and local variables); returning pops it. Cleanup is automatic and instant.
- **Managed heap** — a large shared area where objects created with `new` live. Cleaned up later by the **Garbage Collector (GC)**.

```text
 STACK (thread 1)                 MANAGED HEAP
 ┌──────────────────────┐        ┌────────────────────────────┐
 │ Main()               │        │                            │
 │   int count = 3      │        │   ┌─────────────────────┐  │
 │   Order order ───────┼───────►│   │ Order object        │  │
 ├──────────────────────┤        │   │  Id = 7             │  │
 │ Calculate()          │        │   │  Items ─────────────┼──┼─► List<Item> object
 │   decimal total = 9.5│        │   └─────────────────────┘  │
 └──────────────────────┘        └────────────────────────────┘
```

## 1.8 GC — the intro

The GC finds heap objects that nothing references any more and reclaims their memory. It is **generational** (Gen 0 = new objects, collected often; Gen 1; Gen 2 = long-lived; plus the Large Object Heap for objects ≥ 85,000 bytes) because most objects die young. You don't call `free`/`delete` — but you must still `Dispose` unmanaged resources. Full details (generations, `IDisposable`, finalizers, `using`) are in the memory-management part of this guide.

🎯 **Say this in the interview:** "C# compiles to IL plus metadata inside an assembly. At run time the CLR loads it and the JIT compiles each method to native code on first call, with tiered compilation and PGO optimizing hot paths. The CLR also gives us garbage collection, type safety and exception handling — that's what 'managed code' means."

## Try it yourself

1. Is IL specific to Windows or x64? Why does that matter?
2. Your company has a shared library used by an old .NET Framework 4.8 service and a new .NET 10 API. What should it target?
3. Why is the *first* call to a method slightly slower than later calls?
4. A CLI tool must start in under 50 ms inside a container. Which compilation mode would you consider, and what is the trade-off?
5. Two classes are both named `Logger`. How do you use both in one file?
6. True or false: "The GC will close my database connections for me."

<details><summary>Answer</summary>

1. No. IL is CPU- and OS-independent; the JIT turns it into native code for whatever machine runs it. That's what makes "build once, run on Windows/Linux/macOS, x64/ARM64" possible.
2. **.NET Standard 2.0** (or multi-target: `<TargetFrameworks>netstandard2.0;net10.0</TargetFrameworks>`).
3. The JIT compiles it to machine code on the first call (Tier 0); later calls jump straight to the cached native code.
4. **Native AOT** — no JIT, very fast startup and low memory. Trade-off: limited reflection/`dynamic`, must fix trimming warnings, platform-specific binary, and no dynamic PGO at run time.
5. They must be in different namespaces. Use full names or an alias: `using AppLogger = MyApp.Logging.Logger;`.
6. False. The GC frees *memory*. Connections, file handles, sockets are unmanaged resources → call `Dispose()` (use `using`). A finalizer *might* eventually release them, but at an unpredictable time.

</details>

## Quick revision

- C# = language; .NET = platform; CLR = runtime engine; BCL = standard class library.
- Roslyn compiles C# → IL + metadata in an assembly (`.dll`/`.exe`).
- JIT compiles IL → native code per method on first call; tiered compilation + dynamic PGO optimize hot code.
- ReadyToRun = pre-compiled code inside the DLL for faster startup; Native AOT = fully native exe, no JIT.
- .NET Framework = Windows-only legacy (4.8.1); .NET 5+ = cross-platform, all new features; .NET Standard = API contract.
- Even .NET versions are LTS (8, 10).
- Managed code runs under the CLR with GC & type safety; unmanaged code is native (C/C++, Win32).
- CTS = type rules for all .NET languages; CLS = interoperable subset.
- Assembly = physical deployment unit; namespace = logical grouping; `internal` = assembly-scoped.
- NuGet = package manager; `dotnet add package X`.
- Stack = per-thread method frames; heap = objects managed by the GC.
- `int` is just an alias for `System.Int32`.

# 2. Types, Variables & Memory

> **Why interviewers ask this:** Value vs reference types, boxing, `string` immutability and `const` vs `readonly` are the most-asked C# fundamentals. They reveal whether you understand *what your code does to memory* — the root of many performance bugs and "why did my variable change?" bugs.

## 2.1 Value types vs reference types

**Simple words:**
- A **value type** variable *contains the data itself*. Copying it copies the data.
- A **reference type** variable *contains an address (reference)* pointing to an object on the heap. Copying it copies the address — both variables now point to the **same** object.

**Analogy:** A value type is a **photocopy** of a document — scribble on your copy, mine is unchanged. A reference type is a **Google Docs link** — I send you the link, you edit, I see your edits.

| Value types | Reference types |
|---|---|
| `int`, `long`, `double`, `decimal`, `bool`, `char`, `DateTime`, `Guid`, `TimeSpan`, **enums**, **structs**, `record struct`, tuples (`ValueTuple`) | `class`, `record` (class), `string`, arrays, `object`, delegates, interfaces (the objects behind them), `dynamic` |
| Derive from `System.ValueType` | Derive from `System.Object` directly or indirectly |
| Default value: all-zeros (`0`, `false`, `'\0'`) | Default value: `null` |
| Can't be `null` (unless `Nullable<T>`, e.g. `int?`) | Can be `null` |
| No inheritance (sealed), can implement interfaces | Full inheritance |
| `==` on structs is not defined unless you overload it (records do) | `==` compares references (except `string`, records, overloaded types) |

```csharp
// Value type vs reference type: what gets copied?
var p1 = new PointStruct { X = 1 };
var p2 = p1;          // copies the whole value
p2.X = 99;
Console.WriteLine($"struct: p1.X={p1.X}, p2.X={p2.X}");

var c1 = new PointClass { X = 1 };
var c2 = c1;          // copies the reference (address), not the object
c2.X = 99;
Console.WriteLine($"class:  c1.X={c1.X}, c2.X={c2.X}");
Console.WriteLine($"same object? {ReferenceEquals(c1, c2)}");

struct PointStruct { public int X; }
class PointClass { public int X; }
```

Output:

```text
struct: p1.X=1, p2.X=99
class:  c1.X=99, c2.X=99
same object? True
```

Memory picture after the code runs:

```text
 STACK                                 HEAP
 ┌───────────────┐
 │ p1 │ X = 1    │  ← its own data
 ├───────────────┤
 │ p2 │ X = 99   │  ← its own data
 ├───────────────┤                     ┌──────────────────┐
 │ c1 │ 0x5A10 ──┼────────────────────►│ PointClass       │
 ├───────────────┤               ┌────►│   X = 99         │
 │ c2 │ 0x5A10 ──┼───────────────┘     └──────────────────┘
 └───────────────┘
```

## 2.2 The stack-vs-heap myth

⚠️ **Interview trap:** "Value types live on the stack, reference types live on the heap." This is **only half true**.

The precise rule: **reference-type objects always live on the heap. A value type lives wherever its container lives.**

| Where the value type is declared | Where it lives |
|---|---|
| Local variable / parameter in a normal method | Stack (or a CPU register) |
| Field of a class | **Heap** — inside that class object |
| Element of an array (`int[]`) | **Heap** — arrays are objects |
| Captured by a lambda / used across `await` / inside an iterator | **Heap** — the compiler moves it into a hidden class |
| Boxed (`object o = 5;`) | **Heap** — inside a box |
| Field of a struct | Wherever that struct lives |

```csharp
class Order
{
    public int Quantity;      // int, but it lives on the HEAP inside each Order object
    public decimal Price;     // same
}
```

🎯 **Say this in the interview:** "The real difference is copy semantics: value types are copied by value, reference types by reference. Where they're stored is an implementation detail — a value type is stored inline wherever its container is, so an `int` field of a class is on the heap."

## 2.3 Boxing and unboxing

**Simple words:** **Boxing** = wrapping a value type in an object on the heap so it can be treated as `object` (or an interface). **Unboxing** = taking the value back out. Each box is a heap allocation + copy → GC pressure.

**Analogy:** Putting a single sock in a shipping box so it can travel through the "parcel system" (`object`). Expensive packaging for a tiny thing.

```csharp
int n = 42;
object boxed = n;          // boxing: copy of n goes to the heap
n = 100;                   // changing n does NOT change the box
int unboxed = (int)boxed;  // unboxing: copy back to the stack
Console.WriteLine($"n={n}, boxed={boxed}, unboxed={unboxed}");

object o = 42;
try
{
    long l = (long)o;      // ❌ box holds an int, not a long
}
catch (InvalidCastException)
{
    Console.WriteLine("InvalidCastException: unbox must use the exact type");
}
long ok = (long)(int)o;    // ✅ unbox to int, then convert
Console.WriteLine(ok);

// Two boxes of the same value are different objects
object a = 5, b = 5;
Console.WriteLine(a == b);        // reference comparison!
Console.WriteLine(a.Equals(b));   // value comparison
```

Output:

```text
n=100, boxed=42, unboxed=42
InvalidCastException: unbox must use the exact type
42
False
True
```

**Where boxing hides in real code:**

| Code | Boxes? | Fix |
|---|---|---|
| `ArrayList list; list.Add(5);` | Yes, every add | `List<int>` (generics = no boxing) |
| `object o = 5;` / `string.Format("{0}", 5)` (older APIs) | Yes | Interpolated strings (C# 10+ uses handlers, no boxing) |
| `IComparable c = 5;` (value type → interface) | Yes | Generic constraint `where T : IComparable<T>` |
| `Dictionary<MyStruct, X>` where struct doesn't implement `IEquatable<T>` | Yes, `Equals(object)` on every lookup | Implement `IEquatable<T>` (or use `record struct`) |
| `enumValue.HasFlag(...)` | Not any more (JIT intrinsic since .NET Core 2.1) | — |

🎯 **Say this in the interview:** "Boxing copies a value type into a new heap object so it can be treated as `object` or an interface; unboxing copies it back and must use the exact type. It costs an allocation and GC work, so in hot paths we use generics to avoid it."

## 2.4 Nullable value types and null operators

**Simple words:** Value types can't be `null`. `int?` (short for `Nullable<int>`) is a small struct with `HasValue` + `Value` that adds a "no value" state — perfect for database columns or optional inputs.

| Operator | Name | Meaning |
|---|---|---|
| `a ?? b` | null-coalescing | `a` if not null, else `b` |
| `a ??= b` | null-coalescing assignment (C# 8+) | assign `b` to `a` only if `a` is null |
| `a?.B` | null-conditional | `null` if `a` is null, otherwise `a.B` |
| `a?[i]` | null-conditional index | `null` if `a` is null |
| `a!` | null-forgiving | "trust me, not null" — only silences the compiler warning, no runtime check |

```csharp
int? age = null;
Console.WriteLine(age.HasValue);              // False
Console.WriteLine(age ?? -1);                 // -1   (null-coalescing)
age ??= 30;                                   // assign only if null (C# 8+)
Console.WriteLine(age);                       // 30
Console.WriteLine(age.GetValueOrDefault());   // 30

string? name = null;
Console.WriteLine(name?.Length);              // prints empty line (null)
Console.WriteLine(name?.Length ?? 0);         // 0

Customer? c = new Customer(null);
Console.WriteLine(c?.Address?.City ?? "unknown city");

int? x = null, y = 5;
Console.WriteLine(x + y is null);             // True: null + anything = null
Console.WriteLine(x < y);                     // False
Console.WriteLine(x >= y);                    // False too!

record Address(string City);
record Customer(Address? Address);
```

Output:

```text
False
-1
30
30

0
unknown city
True
False
False
```

⚠️ **Interview trap:** with nullable values, comparisons with `null` are always `false` — so `x < y` and `x >= y` can *both* be false. Don't write `if (!(x < y))` expecting "x >= y".

- `string?` (C# 8 **nullable reference types**) is different: it's only a compiler annotation for warnings; at run time a `string` could always be null. `int?` is a real different type (`Nullable<int>`).
- Accessing `.Value` on an empty `int?` throws `InvalidOperationException`.

## 2.5 `var` vs `dynamic` vs `object`

| | `var` | `object` | `dynamic` |
|---|---|---|---|
| Type decided | At **compile time** by the compiler (inferred) | Compile time: it's `object` | At **run time** |
| Can change type later? | No | It can *hold* anything | Yes |
| IntelliSense / compile errors | Full | Only `object` members | None — errors become runtime exceptions |
| Boxing for value types | No | Yes | Yes |
| Typical use | Everyday locals, LINQ anonymous types | Truly heterogeneous data (rare now) | COM interop, JSON/dynamic languages, reflection shortcuts |

```csharp
var v = 10;          // compile-time type is int
// v = "hello";      // ❌ does not compile: CS0029 (var is still statically typed)

object o = 10;       // static type object; needs a cast to use int members
o = "hello";         // ✅ anything can go in an object
Console.WriteLine(((string)o).Length);

dynamic d = 10;      // type checks happen at RUNTIME
d = "hello";
Console.WriteLine(d.Length);   // resolved at runtime -> 5
try
{
    Console.WriteLine(d.NoSuchMember);
}
catch (Microsoft.CSharp.RuntimeBinder.RuntimeBinderException)
{
    Console.WriteLine("RuntimeBinderException at runtime, not compile time");
}
```

Output:

```text
5
5
RuntimeBinderException at runtime, not compile time
```

🎯 **Say this in the interview:** "`var` is just type inference — the variable is still strongly typed at compile time. `dynamic` skips compile-time checking and binds at run time, so mistakes become `RuntimeBinderException`s."

## 2.6 `const` vs `readonly` vs `static readonly`

**Simple words:** `const` = value written into the code at compile time, never changes. `readonly` = set once (declaration or constructor), per object. `static readonly` = set once for the whole type, at run time.

| | `const` | `readonly` (instance) | `static readonly` |
|---|---|---|---|
| When value is fixed | Compile time | Runtime, in field initializer or **constructor** | Runtime, in initializer or **static constructor** |
| Allowed types | Primitives, `string`, enums, `null` | Any type | Any type |
| Implicitly static? | **Yes** | No — one per instance | Yes |
| Can differ per object? | No | **Yes** | No |
| Stored where | Copied (inlined) into every caller's IL | Field in object | Field of the type |
| Versioning risk | ⚠️ Yes — other assemblies keep the *old* value until recompiled | No | No |
| Example | `const int MaxRetries = 3;` | `readonly Guid _id = Guid.NewGuid();` | `static readonly HttpClient Client = new();` |

```csharp
public class PaymentService
{
    public const int MaxRetries = 3;                                    // truly constant
    public static readonly TimeSpan Timeout = TimeSpan.FromSeconds(30); // not allowed as const
    private readonly ILogger _logger;                                    // set once per instance

    public PaymentService(ILogger logger) => _logger = logger;          // ✅ allowed in ctor
    public void Reset() => _logger = null!;  // ❌ does not compile: CS0191
}
```

Verified compiler errors:
- Assigning a `const` → **CS0131** (left side must be a variable).
- `const DateTime When = DateTime.Now;` → **CS0283** (type `DateTime` cannot be declared const).
- Assigning a `readonly` field outside a constructor → **CS0191**.

⚠️ **Interview trap — const versioning:** Library v1 has `public const int Version = 1;`. Your app uses it. Library v2 changes it to `2` and you swap only the DLL. Your app still sees `1` — the value was copied into your IL. Use `static readonly` for values that might change.

⚠️ **Interview trap — readonly ≠ immutable:** `private readonly List<int> _items = new();` — you can't reassign `_items`, but `_items.Add(5)` works fine. `readonly` protects the *reference*, not the object.

## 2.7 Conversions: implicit, explicit, Parse, TryParse, Convert, checked

**Simple words:**
- **Implicit** conversion: safe, no data loss, happens automatically (`int` → `long`, `int` → `double`, derived → base class).
- **Explicit** conversion (cast): may lose data or fail, you must write `(type)` (`double` → `int`, `long` → `int`, base → derived).
- **Parse**: string → number; throws on bad input. **TryParse**: returns `bool`, never throws — use it for user input. **Convert.ToX**: handles `null` (returns 0) and rounds.

```csharp
using System.Globalization;

int i = 300;
long big = i;                 // implicit: widening, no data loss
byte b = (byte)i;             // explicit: narrowing, data loss!
Console.WriteLine($"byte b = {b}");          // 300 - 256 = 44
double d = 9.99;
Console.WriteLine((int)d);                   // 9 (truncates)
Console.WriteLine(Convert.ToInt32(d));       // 10 (rounds)
Console.WriteLine(Convert.ToInt32(2.5));     // 2 (banker's rounding!)

Console.WriteLine(int.Parse("123"));
Console.WriteLine(int.TryParse("12a", out int r) ? r : -1);
Console.WriteLine(Convert.ToInt32(null as string));  // 0, not exception
try { int.Parse(null!); }
catch (ArgumentNullException) { Console.WriteLine("int.Parse(null) -> ArgumentNullException"); }
try { int.Parse("abc"); }
catch (FormatException) { Console.WriteLine("int.Parse(\"abc\") -> FormatException"); }

// Culture matters for decimals!
Console.WriteLine(double.Parse("1,5", new CultureInfo("de-DE")));          // 1.5 in German
Console.WriteLine(double.Parse("1.5", CultureInfo.InvariantCulture));

int max = int.MaxValue;
int wrapped = unchecked(max + 1);
Console.WriteLine($"unchecked: {wrapped}");
try
{
    int boom = checked(max + 1);
}
catch (OverflowException)
{
    Console.WriteLine("checked: OverflowException");
}
```

Output:

```text
byte b = 44
9
10
2
123
-1
0
int.Parse(null) -> ArgumentNullException
int.Parse("abc") -> FormatException
1.5
1.5
unchecked: -2147483648
checked: OverflowException
```

| Method | Bad format (`"abc"`) | `null` | Overflow | Use when |
|---|---|---|---|---|
| `int.Parse(s)` | `FormatException` | `ArgumentNullException` | `OverflowException` | Input is trusted to be valid |
| `int.TryParse(s, out x)` | returns `false` | returns `false` | returns `false` | **User/external input** (no exceptions = fast) |
| `Convert.ToInt32(s)` | `FormatException` | returns **0** | `OverflowException` | Converting `object`/mixed types, DB values |
| `(int)someDouble` | — | — | silently wraps/undefined unless `checked` | Numeric → numeric when you accept truncation |

- **Overflow:** integer arithmetic is `unchecked` by default — `int.MaxValue + 1` silently wraps to `int.MinValue`. Use `checked(...)`, a `checked { }` block, or `<CheckForOverflowUnderflow>true</CheckForOverflowUnderflow>` for money/counters. (`decimal` always throws on overflow; `double` becomes `Infinity`.)
- `Convert.ToInt32(2.5)` is `2` and `Convert.ToInt32(3.5)` is `4` — **banker's rounding** (round half to even). Use `Math.Round(x, MidpointRounding.AwayFromZero)` for "school" rounding.
- Always pass `CultureInfo.InvariantCulture` when parsing machine data (files, APIs) — `"1.5"` means *fifteen* in some cultures.

## 2.8 Strings: immutability and interning

**Simple words:** A `string` can **never be changed** after creation. Every "change" (`+=`, `Replace`, `ToUpper`, `Trim`) creates a **new** string. It's a reference type that *behaves* like a value: `==` compares content.

**Analogy:** A printed receipt. You can't edit it — you print a new one.

```csharp
string s1 = "hello";
string s2 = s1;
s2 += " world";                 // creates a NEW string; s1 untouched
Console.WriteLine(s1);
Console.WriteLine(s2);

string upper = s1.ToUpper();    // also returns a new string
Console.WriteLine($"{s1} / {upper}");

// Interning: literals are stored once in the intern pool
string a = "dotnet";
string b = "dotnet";
string c = new string("dotnet".ToCharArray());   // built at runtime
Console.WriteLine(ReferenceEquals(a, b));          // True  (same interned literal)
Console.WriteLine(ReferenceEquals(a, c));          // False (different object)
Console.WriteLine(a == c);                         // True  (== compares content for string)
Console.WriteLine(ReferenceEquals(a, string.Intern(c)));  // True
```

Output:

```text
hello
hello world
hello / HELLO
True
False
True
True
```

**Why immutable?** Thread-safe without locks, safe as dictionary keys (hash can't change), safe to share (interning), secure (a validated path can't be modified later).

**Interning:** the CLR keeps an "intern pool" — all identical string *literals* in your program point to one object. Strings built at run time are not interned unless you call `string.Intern`. (Don't intern user data — the pool lives for the whole process.)

## 2.9 `StringBuilder` vs concatenation

**Simple words:** `s += x` in a loop copies the *entire* string every time → O(n²) work and tons of garbage. `StringBuilder` keeps a growable buffer and appends in place → O(n).

```csharp
using System.Diagnostics;
using System.Text;

const int N = 20_000;

long before = GC.GetAllocatedBytesForCurrentThread();
var sw = Stopwatch.StartNew();
string s = "";
for (int i = 0; i < N; i++) s += "x";          // each += copies the whole string
sw.Stop();
long concatBytes = GC.GetAllocatedBytesForCurrentThread() - before;
Console.WriteLine($"string +=      : {sw.ElapsedMilliseconds,4} ms, {concatBytes / 1024 / 1024,4} MB allocated");

before = GC.GetAllocatedBytesForCurrentThread();
sw.Restart();
var sb = new StringBuilder();
for (int i = 0; i < N; i++) sb.Append('x');    // appends into a growable buffer
string result = sb.ToString();
sw.Stop();
long sbBytes = GC.GetAllocatedBytesForCurrentThread() - before;
Console.WriteLine($"StringBuilder  : {sw.ElapsedMilliseconds,4} ms, {sbBytes / 1024,4} KB allocated");
Console.WriteLine(s == result);
```

Sample output (timings vary by machine; the allocation gap is the point):

```text
string +=      :   52 ms,  381 MB allocated
StringBuilder  :    0 ms,   87 KB allocated
True
```

Building a 20,000-char string with `+=` allocated **~381 MB** (1 + 2 + 3 + … + 20,000 chars × 2 bytes); `StringBuilder` allocated ~87 KB.

| Situation | Best choice |
|---|---|
| Joining a few values once: `$"{first} {last}"` | Interpolation / `+` (compiler turns `a + b + c` into one `string.Concat`) |
| Loop with unknown number of appends | `StringBuilder` (set capacity if you know it: `new StringBuilder(4096)`) |
| Joining a collection with a separator | `string.Join(", ", items)` |
| Ultra-hot path, no allocations | `string.Create`, `Span<char>`, `stackalloc` |

⚠️ **Interview trap:** "Always use `StringBuilder`." No — for 2–4 pieces, `StringBuilder` is *slower* (extra object). Use it for loops/many appends.

## 2.10 String comparison, interpolation, verbatim & raw strings

**Rules of thumb:**
- Identifiers, keys, file paths, HTTP headers, config → **`StringComparison.Ordinal`** or **`OrdinalIgnoreCase`** (byte-by-byte, fast, culture-independent).
- Text shown to users and sorted for humans → `CurrentCulture`.
- Never compare with `s1.ToLower() == s2.ToLower()` — allocates two strings and is culture-sensitive.

```csharp
using System.Globalization;

string a = "HELLO", b = "hello";
Console.WriteLine(a == b);                                              // False
Console.WriteLine(string.Equals(a, b, StringComparison.OrdinalIgnoreCase)); // True
Console.WriteLine(a.Equals(b, StringComparison.Ordinal));               // False

// Culture trap: Turkish has dotted/dotless i
var tr = new CultureInfo("tr-TR");
Console.WriteLine("file".ToUpper(tr));                       // FİLE (dotted İ)
Console.WriteLine("file".ToUpper(tr) == "FILE");             // False!
Console.WriteLine(string.Equals("file", "FILE", StringComparison.OrdinalIgnoreCase)); // True

// Dictionary with case-insensitive keys
var headers = new Dictionary<string, string>(StringComparer.OrdinalIgnoreCase)
{
    ["Content-Type"] = "application/json"
};
Console.WriteLine(headers["content-type"]);

// Interpolation with format specifiers & alignment
double price = 1234.5;
var inv = CultureInfo.InvariantCulture;
Console.WriteLine(string.Create(inv, $"[{price,10:F2}] [{price,-10:N1}] [{0.256:P0}]"));
Console.WriteLine(string.Create(inv, $"{new DateTime(2025, 1, 31):yyyy-MM-dd}"));

// Verbatim and raw strings
string path = @"C:\temp\new";                 // no escaping needed
Console.WriteLine(path);
string name = "Asha";
string json = $$"""
    { "name": "{{name}}", "tags": ["a", "b"] }
    """;                                      // raw string (C# 11+)
Console.WriteLine(json);
```

Output:

```text
False
True
False
FİLE
False
True
application/json
[   1234.50] [1,234.5   ] [26 %]
2025-01-31
C:\temp\new
{ "name": "Asha", "tags": ["a", "b"] }
```

| Syntax | Name | Notes |
|---|---|---|
| `"C:\\temp\\new"` | Regular | `\n`, `\t`, `\\`, `\"` escapes |
| `@"C:\temp\new"` | Verbatim | Backslashes literal; `""` for a quote; can span lines |
| `$"Hi {name}"` | Interpolated | `{expr,alignment:format}` |
| `"""...."""` | Raw string literal (C# 11+) | No escaping at all; indentation of closing `"""` is trimmed |
| `$$"""{ "a": {{x}} }"""` | Raw interpolated | Number of `$` = number of braces needed for a hole, so single `{` stays literal (great for JSON) |
| `"abc"u8` | UTF-8 literal (C# 11+) | `ReadOnlySpan<byte>` |

## 2.11 Default values

| Type | `default` |
|---|---|
| `int`, `long`, `double`, `decimal`, … | `0` |
| `bool` | `false` |
| `char` | `'\0'` |
| enum | `0` (even if 0 isn't a named member!) |
| struct | every field set to its default |
| any reference type (`string`, class, array) | `null` |
| `int?` | `null` |

Fields and array elements get defaults automatically. **Local variables do not** — reading an unassigned local is compile error CS0165.

```csharp
int[] scores = new int[3];        // [0, 0, 0]
string[] names = new string[2];   // [null, null]
T GetDefault<T>() => default;     // default literal (C# 7.1+)
```

## 2.12 Enums and `[Flags]`

**Simple words:** An enum gives names to a set of integer constants — `OrderStatus.Shipped` instead of magic number `2`. `[Flags]` enums let one value hold *several* options at once using bits.

**Analogy:** A normal enum is a radio button (one choice). A `[Flags]` enum is a set of checkboxes.

```csharp
var status = OrderStatus.Shipped;
Console.WriteLine(status);                  // Shipped
Console.WriteLine((int)status);             // 2
Console.WriteLine((OrderStatus)99);         // 99  -- no error! enums aren't validated
Console.WriteLine(Enum.IsDefined(typeof(OrderStatus), 99));
Console.WriteLine(Enum.TryParse<OrderStatus>("delivered", ignoreCase: true, out var s) ? s : OrderStatus.Pending);

var perms = Permissions.Read | Permissions.Write;
Console.WriteLine(perms);                                  // Read, Write
Console.WriteLine(perms.HasFlag(Permissions.Write));       // True
Console.WriteLine((perms & Permissions.Delete) != 0);      // False
perms |= Permissions.Delete;                               // add a flag
perms &= ~Permissions.Write;                               // remove a flag
Console.WriteLine(perms);                                  // Read, Delete
Console.WriteLine((int)perms);                             // 5

Console.WriteLine(default(OrderStatus));                   // Pending (0)

enum OrderStatus { Pending, Paid, Shipped, Delivered }

[Flags]
enum Permissions
{
    None   = 0,
    Read   = 1,       // 0001
    Write  = 2,       // 0010
    Delete = 4,       // 0100
    Admin  = Read | Write | Delete   // 0111
}
```

Output:

```text
Shipped
2
99
False
Delivered
Read, Write
True
False
Read, Delete
5
Pending
```

- Underlying type is `int` by default; you can choose another: `enum Small : byte { … }`.
- `[Flags]` values must be powers of two (1, 2, 4, 8 … or `1 << n`), and include `None = 0`.
- ⚠️ **Interview trap:** any integer can be cast to an enum (`(OrderStatus)99` is legal). Validate input from APIs/DBs with `Enum.IsDefined` (or a `switch` with a default case that throws).

## Try it yourself

🧩 **Predict the output** of each line (all verified):

```csharp
string s = "abc";
s.ToUpper();
Console.WriteLine(s);

object x = 1, y = 1;
Console.WriteLine(x == y);
Console.WriteLine(x.Equals(y));

byte b = 255;
b++;
Console.WriteLine(b);

int? n = null;
Console.WriteLine(n + 1 ?? 0);

var f = (Perm)3;
Console.WriteLine(f);
Console.WriteLine((Perm)8);

string t = "Hello";
Console.WriteLine(t.Equals("hello", StringComparison.OrdinalIgnoreCase) ? "same" : "diff");
Console.WriteLine($"{"a" + 1 + 2}|{1 + 2 + "a"}");

[Flags] enum Perm { None = 0, Read = 1, Write = 2, Exec = 4 }
```

1. Write down all 9 output lines.
2. Why does line 1 not print `ABC`?
3. Why is `x == y` false but `x.Equals(y)` true?
4. You store a `DateTime` "app start time" that other assemblies read. `const` or `static readonly`? Why?
5. Rewrite `if (input.ToLower() == "yes")` the right way.
6. An API sends `"status": 7` for your `OrderStatus` enum. What happens on deserialization and how do you guard against it?

<details><summary>Answer</summary>

1. Output:
   ```text
   abc
   False
   True
   0
   0
   Read, Write
   8
   same
   a12|3a
   ```
2. Strings are immutable — `ToUpper()` returns a *new* string that we threw away. Write `s = s.ToUpper();`.
3. Each `object` variable holds a **separate box**. `==` on `object` compares references (two different boxes); `Equals` is virtual and `Int32.Equals` compares values.
   - `byte` 255 + 1 wraps to 0 (unchecked). `n + 1` is `null` (lifted operator) so `?? 0` gives 0. `(Perm)3` = Read|Write. `8` isn't a defined flag combination so it prints the number. `"a" + 1 + 2` is evaluated left to right as string concatenation → `"a12"`; `1 + 2 + "a"` adds the ints first → `"3a"`.
4. `static readonly` — `DateTime` can't be `const` (CS0283), and the value is only known at run time. Also avoids the const-inlining versioning problem.
5. `if (string.Equals(input, "yes", StringComparison.OrdinalIgnoreCase))` — no allocations, culture-safe, null-safe.
6. It becomes `(OrderStatus)7` silently — no exception. Validate with `Enum.IsDefined(typeof(OrderStatus), value)` (or `Enum.IsDefined<OrderStatus>(value)`), or use a `switch` with a throwing default.

</details>

## Quick revision

- Value types hold data; reference types hold a reference to a heap object. Copy semantics is the key difference.
- Value types live where their container lives (a field of a class is on the heap).
- Boxing = value → heap object (`object`/interface); unboxing must use the exact type. Avoid with generics.
- `int?` = `Nullable<int>`; `??`, `??=`, `?.`, `?[]`; null comparisons are always false.
- `var` = compile-time inference; `dynamic` = runtime binding; `object` = base of everything (boxes value types).
- `const` = compile-time, implicitly static, inlined into callers; `readonly` = set in ctor, per instance; `static readonly` = set once per type at run time.
- `readonly` protects the reference, not the object's contents.
- `Parse` throws, `TryParse` returns bool, `Convert` returns 0 for null and uses banker's rounding.
- Integer overflow wraps silently unless `checked`.
- Strings are immutable; literals are interned; `==` on strings compares content.
- Use `StringBuilder` for loops; `+`/interpolation for a few pieces; `string.Join` for collections.
- Compare identifiers with `StringComparison.Ordinal(IgnoreCase)`; never `ToLower() ==`.
- Raw strings `"""…"""` (C# 11) need no escaping; `$$` lets `{` stay literal.
- `[Flags]` enums use powers of two; any int can be cast to an enum — validate input.

# 3. Methods & Parameters

> **Why interviewers ask this:** "What's the difference between `ref` and `out`?" and "If I pass a List to a method and set it to `new List()`, does the caller see it?" are near-guaranteed questions. They test whether you *really* understand references, not just syntax.

## 3.1 Pass by value vs pass by reference

**Simple words:** By default, C# **always passes a copy of the variable** to a method ("pass by value").
- For a value type, the copy is the data → the method can't affect the caller's variable.
- For a reference type, the copy is **the reference** (the address) → the method can modify the *object* the caller sees, but **can't make the caller's variable point to a different object**.

**Analogy:** I give you a **photocopy of my house key** (reference passed by value). You can enter my house and rearrange furniture (mutate the object) — I'll see it. But if you throw away your key copy and get a key to another house (reassign), my key still opens *my* house.

With `ref`, I give you **my actual key ring** — if you swap the key, I now have the new one.

```csharp
var list = new List<int> { 1, 2, 3 };

AddItem(list);
Console.WriteLine(string.Join(",", list));   // 1,2,3,4  - object was mutated

Reassign(list);
Console.WriteLine(string.Join(",", list));   // 1,2,3,4  - caller's variable unchanged!

ReassignByRef(ref list);
Console.WriteLine(string.Join(",", list));   // 100     - caller's variable replaced

static void AddItem(List<int> items) => items.Add(4);

static void Reassign(List<int> items)
{
    items = new List<int> { 100 };   // only the local copy of the reference changes
}

static void ReassignByRef(ref List<int> items)
{
    items = new List<int> { 100 };   // changes the caller's variable itself
}
```

Output:

```text
1,2,3,4
1,2,3,4
100
```

What happens inside `Reassign`:

```text
 BEFORE  items = new ...                AFTER  items = new ...
 STACK              HEAP                STACK              HEAP
 list  ──────┐                          list  ──────────► [1,2,3,4]
             ├────► [1,2,3,4]
 items ──────┘                          items ──────────► [100]   (garbage after return)
 (copy of the reference)
```

With `ref`, `items` is an **alias** to the caller's variable `list` itself — there is no copy:

```text
 list ◄── items (alias)  ──────► [100]
```

| | Value type | Reference type |
|---|---|---|
| Passed normally | Method gets a copy of the data | Method gets a copy of the reference: can mutate object, can't rebind caller's variable |
| Passed with `ref` | Method can change caller's value | Method can mutate object **and** rebind caller's variable |

⚠️ **Interview trap:** "Reference types are passed by reference." ❌ Wrong. *Everything* is passed by value by default; for reference types the **value being copied is a reference**. "Pass by reference" only happens with `ref`/`out`/`in`.

⚠️ **Interview trap — strings:** `string` is a reference type, but immutable, so a method can't mutate it *and* reassigning doesn't affect the caller. It therefore *looks* like a value type when passed.

🎯 **Say this in the interview:** "C# passes arguments by value by default. For reference types that value is the reference, so the method can mutate the shared object but reassigning the parameter doesn't affect the caller. To change the caller's variable itself you need `ref` or `out`."

## 3.2 `ref`, `out`, `in` (and `ref readonly`)

| Modifier | Must be initialized **before** call? | Must be assigned **inside** method? | Method can modify? | Typical use |
|---|---|---|---|---|
| `ref` | ✅ Yes | No | Yes | Swap, modify caller's variable |
| `out` | No | ✅ Yes, on every path | Yes (must) | Return extra values: `TryParse` pattern |
| `in` | ✅ Yes | No | ❌ No (read-only) | Pass **large structs** without copying |
| `ref readonly` (C# 12, params) | ✅ Yes | No | ❌ No | Like `in` but caller must write `ref`/`in` explicitly |

```csharp
int a = 1, b = 2;
Swap(ref a, ref b);
Console.WriteLine($"a={a}, b={b}");

if (TryDivide(10, 0, out int q)) Console.WriteLine(q);
else Console.WriteLine("cannot divide by zero");

var big = new BigMatrix();
Console.WriteLine(Trace(in big));

Console.WriteLine(Sum());                 // 0
Console.WriteLine(Sum(1, 2, 3));          // 6
Console.WriteLine(Sum([4, 5]));           // 9 (collection expression, C# 12)

Console.WriteLine(Greet("Asha"));
Console.WriteLine(Greet("Ravi", greeting: "Hi"));
Console.WriteLine(Greet(punctuation: "?", name: "Mia"));

static void Swap(ref int x, ref int y) => (x, y) = (y, x);

static bool TryDivide(int x, int y, out int result)
{
    if (y == 0) { result = 0; return false; }   // out MUST be assigned on every path
    result = x / y;
    return true;
}

static double Trace(in BigMatrix m)
{
    // m.M11 = 5;   // ❌ does not compile: CS8332 ('in' parameters are read-only)
    return m.M11 + m.M22 + m.M33;
}

static int Sum(params int[] numbers) => numbers.Sum();

static string Greet(string name, string greeting = "Hello", string punctuation = "!")
    => $"{greeting}, {name}{punctuation}";

struct BigMatrix
{
    public double M11 = 1, M12, M13, M21, M22 = 1, M23, M31, M32, M33 = 1;
    public BigMatrix() { }
}
```

Output:

```text
a=2, b=1
cannot divide by zero
3
0
6
9
Hello, Asha!
Hi, Ravi!
Hello, Mia?
```

Notes:
- `ref`/`out` must be written at **both** the declaration and the call site (`Swap(ref a, ref b)`) — it's intentionally visible.
- `out var x` / `out int x` declares the variable inline (C# 7+); `out _` discards it.
- **Restrictions:** `ref`/`out`/`in` parameters can't be used in `async` methods, iterators (`yield`), or captured by lambdas (the alias might outlive the stack frame).
- `in` on a *small* struct or a class buys nothing — and with a *non-readonly* struct, calling a method on an `in` parameter forces a **defensive copy** (see section 5). Use `in` with `readonly struct`s.

## 3.3 `params`, optional and named arguments

- **`params`** lets callers pass any number of arguments (or an array). Must be the **last** parameter, only one per method. Since **C# 13**, `params` works with any collection type: `params ReadOnlySpan<int>`, `params List<T>`, `params IEnumerable<T>` — `params ReadOnlySpan<T>` avoids the array allocation.
- **Optional parameters** have a default value that must be a compile-time constant (`= 10`, `= "x"`, `= null`, `= default`). They must come after required parameters.
- **Named arguments** improve readability (`SendEmail(to: x, isHtml: true)`) and let you skip optional ones / change order.

```csharp
static void Log(string message, params ReadOnlySpan<object> args) { }   // C# 13: no array allocation
Log("User {0} logged in from {1}", "asha", "10.0.0.1");
```

⚠️ **Interview trap — optional parameter versioning:** like `const`, the default value is **copied into the caller's IL** at compile time. If a library changes `retries = 3` to `retries = 5`, callers compiled against the old version still pass `3`. For public APIs, prefer overloads.

## 3.4 Method overloading rules

**Simple words:** Several methods with the **same name** but **different parameter lists** (number, types, order, or `ref`/`out`/`in` modifiers). The compiler picks the **best match at compile time** based on the *static* types of the arguments.

Not enough to overload by: **return type alone**, parameter *names*, or `ref` vs `out` vs `in` only (they differ in metadata but C# treats them as the same signature → CS0663).

Resolution priority (simplified): exact match → implicit numeric widening (choosing the "closest" type) → boxing/reference conversion to base types → `params` expansion.

```csharp
P.Print(5);          // exact match int
P.Print(5L);         // long
P.Print(5.0f);       // no float overload -> widening to double
P.Print('A');        // char -> int (implicit numeric conversion)
P.Print("hi");       // string
P.Print(null);       // most specific reference type: string over object
short s = 3;
P.Print(s);          // short -> int is the "better" conversion than long/double
object o = "hi";
P.Print(o);          // overloads are chosen at COMPILE time by static type

static class P
{
    public static void Print(int x)    => Console.WriteLine($"int {x}");
    public static void Print(long x)   => Console.WriteLine($"long {x}");
    public static void Print(double x) => Console.WriteLine($"double {x}");
    public static void Print(string x) => Console.WriteLine($"string {x ?? "null"}");
    public static void Print(object x) => Console.WriteLine($"object {x}");
}
```

Output:

```text
int 5
long 5
double 5
int 65
string hi
string null
int 3
object hi
```

⚠️ **Interview trap:** `P.Print(o)` where `o` holds a string prints `object hi`. Overload resolution uses the **compile-time (static)** type; only `virtual`/`override` dispatches on the **runtime** type.

⚠️ **Interview trap:** *local functions cannot be overloaded*. If you declare two local functions named `Print` (e.g., in a top-level-statements file), you get **CS0128** ("already defined in this scope") — that's why the example above puts them in a class.

## 3.5 Local functions and static local functions

**Simple words:** A **local function** is a method declared *inside* another method. Use it for helper logic that only makes sense there (recursion helpers, validation before an iterator/async body).

| | Local function | Lambda (`Func<>`) |
|---|---|---|
| Can be recursive easily | ✅ | Awkward |
| Can be generic, use `ref`/`out`/`params` | ✅ | Limited |
| Allocation when capturing | Usually none (captures go into a struct) | Allocates a closure + delegate |
| Can be declared after use | ✅ | ❌ |

A **`static` local function** (C# 8+) **cannot capture** locals or `this` — a compile-time guarantee of no hidden closure allocations and no accidental state sharing.

## 3.6 Expression-bodied members, recursion, tuples & deconstruction, discards

```csharp
Console.WriteLine(Factorial(5));
Console.WriteLine(Fib(40));

var (min, max, avg) = Stats([3, 9, 4, 1]);
Console.WriteLine($"min={min} max={max} avg={avg}");

var result = Stats([10, 20]);
Console.WriteLine(result.Max);          // named tuple elements
Console.WriteLine(result);              // tuples have a nice ToString

var (_, highest, _) = Stats([7, 2]);    // discards: ignore what you don't need
Console.WriteLine(highest);

var p = new Person("Asha", 28);
var (n, age) = p;                       // custom Deconstruct method
Console.WriteLine($"{n} is {age}");

_ = int.TryParse("42", out _);          // discard a return value and an out arg

static long Factorial(int n) => n <= 1 ? 1 : n * Factorial(n - 1);

static long Fib(int n)
{
    var memo = new Dictionary<int, long>();
    return Go(n);

    // local function: can capture 'memo' from the enclosing method
    long Go(int k)
    {
        if (k < 2) return k;
        if (memo.TryGetValue(k, out var v)) return v;
        return memo[k] = Go(k - 1) + Go(k - 2);
    }
}

static (int Min, int Max, double Avg) Stats(int[] values)
{
    return (values.Min(), values.Max(), Average(values));

    // static local function: CANNOT capture locals -> no hidden allocations
    static double Average(int[] v) => (double)v.Sum() / v.Length;
}

class Person(string name, int age)
{
    public string Name { get; } = name;
    public int Age { get; } = age;
    public void Deconstruct(out string name, out int age) => (name, age) = (Name, Age);
}
```

Output:

```text
120
102334155
min=1 max=9 avg=4.25
20
(10, 20, 15)
7
Asha is 28
```

- **Expression-bodied members** (`=>`) work for methods, properties, constructors, indexers, operators: `public int Area => W * H;`.
- **Recursion:** every call adds a stack frame. Deep recursion (≈ tens of thousands of frames) → `StackOverflowException`, which **cannot be caught** and kills the process. C# doesn't guarantee tail-call optimization. For deep data, use a loop + explicit `Stack<T>`. Naive `Fib(40)` makes ~330 million calls; the memoized version above makes ~80.
- **Tuples** `(int Min, int Max)` are `System.ValueTuple` — **value types**, mutable fields, value equality (`(1, 2) == (1, 2)` is true, C# 7.3+). Great for private/internal multi-returns; for public APIs prefer a `record` with a real name.
- **Deconstruction** works on tuples, records (positional), `KeyValuePair` (`foreach (var (key, value) in dict)`) and any type with a `Deconstruct` method.
- **Discards** `_`: ignore out args, tuple parts, return values, or as a catch-all in `switch` patterns.
- Swap without temp: `(a, b) = (b, a);`.

## Try it yourself

🧩 **Predict the output** (verified):

```csharp
string name = "Asha";
Rename(name);
Console.WriteLine(name);

int[] nums = [1, 2, 3];
Change(nums);
Console.WriteLine(string.Join(",", nums));

Console.WriteLine(Count());
Console.WriteLine(Count(null));
Console.WriteLine(Count(null, null));

var (q, r) = DivRem(17, 5);
Console.WriteLine($"{q} r {r}");

static void Rename(string s) => s = "Ravi";
static void Change(int[] a) { a[0] = 99; a = [7, 7, 7]; }
static string Count(params string?[]? items) => items is null ? "null array" : items.Length.ToString();
static (int Quotient, int Remainder) DivRem(int a, int b) => (a / b, a % b);
```

1. Write the 6 output lines.
2. Why does `Count(null)` print `null array` but `Count(null, null)` print `2`?
3. Write a `TryParsePoint(string s, out (int X, int Y) point)` that parses `"3,4"`.
4. Can you overload `int Get()` and `string Get()`? Why?
5. When would you use `in` for a parameter, and when is it pointless?

<details><summary>Answer</summary>

1. Output:
   ```text
   Asha
   99,2,3
   0
   null array
   2
   3 r 2
   ```
   `Rename` reassigns its own copy of the reference. `Change` mutates the shared array (`a[0] = 99`) and then rebinds only its local `a`.
2. A single `null` is convertible to `string?[]` itself, so the compiler passes it as **the array** (normal form). Two `null`s can only be matched by the **expanded** form → an array of 2 nulls.
3. ```csharp
   static bool TryParsePoint(string s, out (int X, int Y) point)
   {
       point = default;
       var parts = s.Split(',');
       if (parts.Length != 2 ||
           !int.TryParse(parts[0], out int x) ||
           !int.TryParse(parts[1], out int y)) return false;
       point = (x, y);
       return true;
   }
   ```
4. No. Return type is not part of the signature for overload resolution → CS0111 (member already defined).
5. Use `in` for **large `readonly struct`s** (e.g., > 16–24 bytes) passed in hot paths to avoid copying. Pointless (or harmful) for small structs like `int`/`DateTime`, for classes (already a reference), and for non-readonly structs (defensive copies).

</details>

## Quick revision

- Everything is passed **by value** by default; for reference types the copied value is the reference.
- Mutating a passed object is visible to the caller; reassigning the parameter is not (unless `ref`).
- `ref` = must be initialized before; `out` = must be assigned inside; `in` = read-only by reference.
- `ref`/`out`/`in` not allowed in `async`, iterators, or lambdas capturing them.
- `params` must be last; C# 13 allows `params Span<T>`/collections.
- Optional defaults are baked into callers (versioning trap), must be compile-time constants.
- Overloads differ by parameter list, not return type; chosen at compile time from static types.
- Local functions can't be overloaded; `static` local functions can't capture.
- Tuples are mutable value types with value equality; deconstruct with `var (a, b) = ...`.
- `_` discards out args, tuple elements, results.
- `StackOverflowException` can't be caught — use iteration for deep recursion.

# 4. OOP — the 4 Pillars in Depth

> **Why interviewers ask this:** Every C# interview has OOP questions: "explain the 4 pillars with examples", "abstract class vs interface", "`new` vs `override`", "constructor execution order". Interviewers want *real* examples from code you'd write at work — not textbook definitions.

**The 4 pillars in one line each:**

| Pillar | Simple words | Real-life analogy | C# tools |
|---|---|---|---|
| **Encapsulation** | Hide internal data; expose safe operations | ATM: you press buttons, can't touch the cash drawer | `private` fields, properties, methods |
| **Abstraction** | Show *what* it does, hide *how* | Car pedal: "accelerate", not "inject fuel at 3 ms" | interfaces, abstract classes |
| **Inheritance** | Reuse & specialize an existing type (IS-A) | A `Dog` is an `Animal` | `class Dog : Animal` |
| **Polymorphism** | One call, many behaviors | "Pay" works for card, UPI, cash | overloading, `virtual`/`override`, interfaces |

## 4.1 Class and object

- A **class** is a blueprint (the house plan). An **object** (instance) is a house built from it with `new`. Many objects, one class.
- Each object has its own copy of **instance fields**; **static** members belong to the class itself (one copy).

## 4.2 Constructors

| Kind | Purpose | Example |
|---|---|---|
| **Default** (parameterless) | Compiler adds one **only if you write no constructor** | `new Order()` |
| **Parameterized** | Force required data at creation | `new Order(id, customer)` |
| **Primary constructor** (C# 12 for classes) | Parameters on the class declaration, available in the whole body | `class Svc(ILogger log) { … }` |
| **Static** | Initialize static data; runs **once**, automatically, before first use; no parameters, no access modifier | `static Config() { … }` |
| **Private** | Prevent outside `new` — singletons, factory methods, static-only classes | `private Settings() { }` |
| **Copy** (by convention) | Create a copy of another instance | `public Point(Point other)` |

**Chaining:** `: this(...)` calls another constructor of the **same** class (avoid duplicate code); `: base(...)` calls the **parent** constructor (required if the base has no parameterless one).

```csharp
public class Employee : Person
{
    public Employee(string name) : this(name, "Unassigned") { }       // chain to sibling ctor
    public Employee(string name, string dept) : base(name)            // chain to parent ctor
    {
        Dept = dept;
    }
    public string Dept { get; }
}

public class Person(string name)            // primary constructor (C# 12)
{
    public string Name { get; } = name;
}
```

⚠️ **Interview trap:** a primary-constructor parameter in a **class** is *not* a property — it's a captured parameter (no public access, and it's mutable). In a **record**, positional parameters *do* become public properties.

## 4.3 Object initialization ORDER (classic interview question)

```csharp
Console.WriteLine("--- first new Derived() ---");
var d1 = new Derived();
Console.WriteLine("--- second new Derived() ---");
var d2 = new Derived();

static class Log
{
    public static int Write(string msg) { Console.WriteLine(msg); return 0; }
}

class Base
{
    private static int s_b = Log.Write("Base static field");
    private int _b = Log.Write("Base instance field");

    static Base() => Log.Write("Base static ctor");
    public Base()
    {
        Log.Write("Base ctor body");
        Describe();                      // ⚠️ virtual call from a constructor
    }
    protected virtual void Describe() => Log.Write("Base.Describe");
}

class Derived : Base
{
    private static int s_d = Log.Write("Derived static field");
    private int _d = Log.Write("Derived instance field");
    private readonly string _name = "set by field initializer";

    static Derived() => Log.Write("Derived static ctor");
    public Derived() : base()
    {
        Log.Write("Derived ctor body");
    }
    protected override void Describe() => Log.Write($"Derived.Describe sees _name = '{_name}'");
}
```

Output:

```text
--- first new Derived() ---
Derived static field
Derived static ctor
Derived instance field
Base static field
Base static ctor
Base instance field
Base ctor body
Derived.Describe sees _name = 'set by field initializer'
Derived ctor body
--- second new Derived() ---
Derived instance field
Base instance field
Base ctor body
Derived.Describe sees _name = 'set by field initializer'
Derived ctor body
```

**How to remember it (C# specifically):**

```text
 new Derived()
   1. Derived static field initializers ┐ only the FIRST time the type is used
   2. Derived static constructor        ┘
   3. Derived INSTANCE field initializers   ← C# runs these BEFORE calling base()
   4. ── call base() ──►
        5. Base static fields + static ctor (first time only)
        6. Base instance field initializers
        7. Base constructor body
   8. Derived constructor body
```

- Field initializers run **derived → base**; constructor **bodies** run **base → derived**.
- Static parts run **once per type**, lazily, before first access (exact timing for types *without* a static constructor can be earlier — `beforefieldinit`).
- This differs from Java, where derived field initializers run *after* the base constructor.

⚠️ **Interview trap — virtual call in constructor:** the base constructor called the **overridden** `Describe()` *before* the derived constructor body ran. Here it worked only because `_name` was set by a field initializer (step 3). If `_name` were assigned in the `Derived` constructor body, `Describe` would see `null`. Rule: **don't call virtual methods from constructors.**

## 4.4 Encapsulation with properties

**Simple words:** Keep fields `private`; expose **properties** — methods disguised as fields — so you can validate, compute, or make things read-only.

| Property kind | Syntax | Use |
|---|---|---|
| Auto-property | `public string Name { get; set; }` | Simple data, compiler creates the backing field |
| Read-only auto | `public string Id { get; }` | Set only in constructor |
| Private setter | `public int Count { get; private set; }` | Class changes it, outsiders read |
| Init-only (C# 9) | `public string Sku { get; init; }` | Set in object initializer, then immutable |
| `required` (C# 11) | `public required string Name { get; init; }` | Compiler forces callers to set it |
| Computed | `public decimal Total => Price * Qty;` | No storage, calculated each time |
| Full property | `get => _x; set { validate; _x = value; }` | Validation, change notification |
| `field` keyword (C# 14) | `set => field = value.Trim();` | Validation without declaring a backing field |

```csharp
var acc = new BankAccount("ACC-1", 100m);
acc.Deposit(50m);
Console.WriteLine($"{acc.Number}: {acc.Balance} ({acc.Status})");
try { acc.Withdraw(500m); }
catch (InvalidOperationException ex) { Console.WriteLine(ex.Message); }

var p = new Person { FirstName = "Asha", LastName = "Rao", Age = 28 };
Console.WriteLine(p.FullName);
// p.FirstName = "X";     // ❌ does not compile: CS8852 (init-only property)
try { p.Age = -5; }
catch (ArgumentOutOfRangeException ex) { Console.WriteLine(ex.ParamName + " rejected"); }

class BankAccount
{
    private decimal _balance;                       // backing field (hidden state)

    public BankAccount(string number, decimal opening)
    {
        Number = number;
        _balance = opening;
    }

    public string Number { get; }                   // get-only auto property: set in ctor only
    public decimal Balance => _balance;             // computed, read-only from outside
    public string Status => _balance >= 1000 ? "Gold" : "Standard";

    public void Deposit(decimal amount)
    {
        ArgumentOutOfRangeException.ThrowIfNegativeOrZero(amount);
        _balance += amount;
    }

    public void Withdraw(decimal amount)
    {
        if (amount > _balance) throw new InvalidOperationException("Insufficient funds");
        _balance -= amount;
    }
}

class Person
{
    public required string FirstName { get; init; }      // C# 11 required + C# 9 init
    public required string LastName { get; init; }
    public string FullName => $"{FirstName} {LastName}";

    private int _age;
    public int Age
    {
        get => _age;
        set
        {
            ArgumentOutOfRangeException.ThrowIfNegative(value, nameof(Age));
            _age = value;
        }
    }

    // C# 14: 'field' keyword - validation without declaring a backing field
    public string Email
    {
        get;
        set => field = value?.Trim().ToLowerInvariant() ?? "";
    } = "";
}
```

Output:

```text
ACC-1: 150 (Standard)
Insufficient funds
Asha Rao
Age rejected
```

Why not public fields? A property lets you add validation, logging, lazy loading, or change notification **later without breaking callers** (changing a field to a property is a binary-breaking change), and properties work with data binding/serializers/interfaces.

🎯 **Say this in the interview:** "Encapsulation means the object protects its own invariants. In `BankAccount`, the balance can only change through `Deposit`/`Withdraw`, which validate — nobody can set `Balance = -1000` from outside."

## 4.5 Access modifiers

| Modifier | Same class | Derived class (same assembly) | Other class (same assembly) | Derived class (other assembly) | Other class (other assembly) |
|---|---|---|---|---|---|
| `public` | ✅ | ✅ | ✅ | ✅ | ✅ |
| `protected internal` | ✅ | ✅ | ✅ | ✅ | ❌ |
| `internal` | ✅ | ✅ | ✅ | ❌ | ❌ |
| `protected` | ✅ | ✅ | ❌ | ✅ | ❌ |
| `private protected` (C# 7.2) | ✅ | ✅ | ❌ | ❌ | ❌ |
| `private` | ✅ | ❌ | ❌ | ❌ | ❌ |
| `file` (C# 11, types only) | visible only inside the same source file | | | | |

- `protected internal` = protected **OR** internal (wider). `private protected` = protected **AND** internal (narrower).
- Defaults: class members → `private`; top-level types → `internal`; interface members → `public`.

## 4.6 Inheritance

**Simple words:** A derived class gets all members of its base class and can add or specialize behavior. C# allows **single class inheritance** (one base class) but **multiple interfaces**. Every class ultimately inherits from `System.Object` (`ToString`, `Equals`, `GetHashCode`, `GetType`).

- `base.Method()` calls the parent's version; `base(...)` calls the parent constructor.
- `sealed class` = cannot be inherited (`class Sub : Sealed` → **CS0509**). `sealed override` = stop further overriding. Sealing also lets the JIT devirtualize calls → slightly faster.
- Two base classes (`class Mi : A, B` where both are classes) → **CS1721**.
- Constructors are **not** inherited.

## 4.7 Polymorphism: compile-time vs runtime

| | Compile-time (static) | Runtime (dynamic) |
|---|---|---|
| Mechanism | Method **overloading**, operator overloading, generics | `virtual` + `override`, abstract members, interfaces |
| Decided by | Compiler, using **static** (declared) types | CLR at run time, using the **actual object type** (via the v-table) |
| Example | `Add(int,int)` vs `Add(double,double)` | `Shape s = new Circle(); s.Area();` |

```csharp
// Compile-time vs runtime polymorphism together
Calculator calc = new ScientificCalculator();
Console.WriteLine(calc.Add(2, 3));          // overload chosen at compile time (int,int)
Console.WriteLine(calc.Add(2.5, 3));        // overload (double,double)
Console.WriteLine(calc.Describe());         // override chosen at runtime

// Composition over inheritance
var checkout = new Checkout(new PercentageDiscount(10), new EmailNotifier());
checkout.Complete(200m);
var checkout2 = new Checkout(new NoDiscount(), new SmsNotifier());
checkout2.Complete(200m);

class Calculator
{
    public int Add(int a, int b) => a + b;
    public double Add(double a, double b) => a + b;
    public virtual string Describe() => "Basic calculator";
}

sealed class ScientificCalculator : Calculator
{
    public override string Describe() => "Scientific calculator (" + base.Describe() + " ++)";
}

interface IDiscount { decimal Apply(decimal amount); }
interface INotifier { void Notify(string message); }

class PercentageDiscount(decimal percent) : IDiscount
{
    public decimal Apply(decimal amount) => amount - amount * percent / 100;
}
class NoDiscount : IDiscount { public decimal Apply(decimal amount) => amount; }
class EmailNotifier : INotifier { public void Notify(string m) => Console.WriteLine($"Email: {m}"); }
class SmsNotifier : INotifier { public void Notify(string m) => Console.WriteLine($"SMS: {m}"); }

class Checkout(IDiscount discount, INotifier notifier)    // HAS-A, not IS-A
{
    public void Complete(decimal amount)
    {
        var total = discount.Apply(amount);
        notifier.Notify($"Order placed, total {total:0.00}");
    }
}
```

Output:

```text
5
5.5
Scientific calculator (Basic calculator ++)
Email: Order placed, total 180.00
SMS: Order placed, total 200.00
```

**How runtime polymorphism works internally:** each object header points to its type's **method table**, which contains a **v-table** — an array of method addresses. `override` *replaces* the slot of the base virtual method; a call to `calc.Describe()` looks up the slot at run time → it finds `ScientificCalculator.Describe`. A `new` method gets a **new slot** instead, so calls through the base type still hit the old slot.

## 4.8 Method hiding (`new`) vs overriding (`override`) — the classic puzzle

🧩 **Predict the output:**

```csharp
Animal a = new Dog();
Dog d = new Dog();

a.Speak();    // virtual + override -> runtime type wins
a.Eat();      // 'new' hides -> compile-time type wins
d.Eat();
((Animal)d).Eat();

Animal p = new Puppy();
p.Speak();
((Dog)p).Speak();
((Puppy)p).Speak();

class Animal
{
    public virtual void Speak() => Console.WriteLine("Animal speaks");
    public void Eat() => Console.WriteLine("Animal eats");
}

class Dog : Animal
{
    public override void Speak() => Console.WriteLine("Dog barks");
    public new void Eat() => Console.WriteLine("Dog eats");
}

class Puppy : Dog
{
    public new virtual void Speak() => Console.WriteLine("Puppy yips");  // starts a NEW virtual chain
}
```

<details><summary>Answer</summary>

```text
Dog barks
Animal eats
Dog eats
Animal eats
Dog barks
Dog barks
Puppy yips
```

- `override` → the **runtime** type decides (`a` is really a `Dog`).
- `new` → the **compile-time** (variable) type decides (`a` is declared `Animal`, so `Animal.Eat`).
- `Puppy.Speak` is `new virtual`: it *hides* `Dog.Speak` and starts a separate chain. Through an `Animal` or `Dog` variable, the old slot (Dog's override) is used. Only a `Puppy`-typed variable sees "Puppy yips".

</details>

| | `override` | `new` (hiding) |
|---|---|---|
| Requires base member to be | `virtual`, `abstract`, or `override` | Anything (non-virtual too) |
| Call through base-type variable | **Derived** version | **Base** version |
| v-table | Replaces the slot | Adds a new slot / no slot |
| Use when | You want polymorphism | Rare: base class you don't control added a member with your name |

If you hide without writing `new`, the compiler gives warning **CS0108** — `new` just says "I meant it".

🎯 **Say this in the interview:** "`override` participates in virtual dispatch, so the runtime type decides. `new` hides the member, so the declared type of the variable decides. Hiding breaks polymorphism, so I only use it when I have no other choice."

## 4.9 Abstract classes and interfaces

**Abstract class** = a *partially built* base class. It can't be instantiated (`new Shape()` → **CS0144**), can have fields, constructors, implemented methods, and `abstract` members that derived classes **must** implement.

**Interface** = a *contract* (a "can-do" capability: `IDisposable`, `IComparable<T>`, `IEnumerable<T>`). A class can implement **many** interfaces.

- **Explicit implementation** (`void IFileStore.Save()`) — member is only callable through the interface. Use it to resolve name clashes or hide rarely used members.
- **Default interface methods** (C# 8+, .NET Core 3+) — interface members can have a body, so you can add a method to a published interface without breaking implementers.
- Interfaces can also have `static abstract` members (C# 11) — used by generic math (`INumber<T>`).

```csharp
var doc = new Document();
// doc.Save();               // ❌ does not compile: CS1061 (explicit members are hidden on the class)
((IFileStore)doc).Save();
((ICloudStore)doc).Save();

ILogger logger = new ConsoleLogger();
logger.LogError("Disk full");   // default interface method (C# 8+)

Shape[] shapes = [new Circle(1), new Rectangle(2, 3)];
foreach (var s in shapes) Console.WriteLine(s.Describe());

interface IFileStore  { void Save(); }
interface ICloudStore { void Save(); }

class Document : IFileStore, ICloudStore
{
    void IFileStore.Save()  => Console.WriteLine("Saved to disk");
    void ICloudStore.Save() => Console.WriteLine("Uploaded to cloud");
}

interface ILogger
{
    void Log(string level, string message);
    void LogError(string message) => Log("ERROR", message);   // has a body!
}

class ConsoleLogger : ILogger
{
    public void Log(string level, string message) => Console.WriteLine($"[{level}] {message}");
}

abstract class Shape
{
    public abstract double Area();                        // MUST be implemented
    public virtual string Name => GetType().Name;         // MAY be overridden
    public string Describe() => $"{Name} with area {Area():F2}";  // shared code
}

class Circle(double r) : Shape
{
    public override double Area() => Math.PI * r * r;
}

class Rectangle(double w, double h) : Shape
{
    public override double Area() => w * h;
    public override string Name => "Rect";
}
```

Output:

```text
Saved to disk
Uploaded to cloud
[ERROR] Disk full
Circle with area 3.14
Rect with area 6.00
```

⚠️ **Interview trap:** a default interface method is **not** inherited by the class — `new ConsoleLogger().LogError("x")` doesn't compile; you must call it through an `ILogger` variable.

### Abstract class vs interface

| | Abstract class | Interface |
|---|---|---|
| Relationship | IS-A ("a `Circle` is a `Shape`") | CAN-DO ("is `IDisposable`") |
| Multiple? | One base class only | Implement many |
| Fields / state | ✅ Instance fields | ❌ No instance fields (static only) |
| Constructors | ✅ | ❌ |
| Implemented members | ✅ | ✅ since C# 8 (default methods), but no state |
| Access modifiers on members | Any | Public by default (C# 8+ allows others) |
| Adding a member later | Add a virtual method with a body → safe | Add with a default implementation → safe, otherwise breaks implementers |
| Use for | Sharing code + state among closely related classes | Contracts, DI, mocking in unit tests, unrelated types sharing a capability |

🎯 **Say this in the interview:** "I use an interface to define a contract — it's what I inject and mock. I use an abstract class when related classes share real code and state, like a template method. Often both: an interface for consumers, plus an abstract base class as a convenient default implementation."

## 4.10 Composition over inheritance

**Simple words:** Instead of *being* something (inheritance), an object *has* helpers that do the work (composition). In the `Checkout` example above, discount and notification behaviors are plugged in — no `EmailPercentageDiscountCheckout` subclass explosion.

| Inheritance problem | Composition answer |
|---|---|
| Deep hierarchies are fragile (change base → break children) | Small, focused classes |
| Behavior fixed at compile time | Swap behaviors at run time / via DI |
| Combinations explode (n × m subclasses) | Combine n + m components |
| Hard to unit test | Inject fakes/mocks of interfaces |

Use inheritance for genuine IS-A relationships with shared behavior; use composition by default.

## 4.11 Upcasting, downcasting, `is`, `as`, pattern matching

- **Upcast** (derived → base): implicit and always safe.
- **Downcast** (base → derived): explicit; throws `InvalidCastException` if wrong.
- `as`: returns `null` instead of throwing (reference/nullable types only).
- `is` with a pattern: test **and** cast in one step — the modern preferred way.

```csharp
object[] things = [new Dog("Rex"), new Cat(), "hello", 42, null!];

foreach (var t in things)
{
    // 'as' returns null on failure (reference/nullable types only)
    var dog = t as Dog;
    Console.Write(dog is null ? "not a dog | " : $"dog {dog.Name} | ");

    // 'is' with pattern: test + cast in one step
    if (t is Animal { } animal) Console.Write($"animal {animal.GetType().Name} | ");

    // switch expression with type patterns
    Console.WriteLine(t switch
    {
        Dog { Name.Length: > 2 } x => $"long-named dog {x.Name}",
        Animal                      => "some animal",
        string s                    => $"string of {s.Length}",
        int n when n > 10           => "big int",
        null                        => "null!",
        _                           => "unknown"
    });
}

Animal up = new Dog("Bo");          // upcast: implicit, always safe
Dog down = (Dog)up;                 // downcast: explicit, may throw
try { Cat bad = (Cat)up; }
catch (InvalidCastException) { Console.WriteLine("InvalidCastException on bad downcast"); }

class Animal { }
class Dog(string name) : Animal { public string Name { get; } = name; }
class Cat : Animal { }
```

Output:

```text
dog Rex | animal Dog | long-named dog Rex
not a dog | animal Cat | some animal
not a dog | string of 5
not a dog | big int
not a dog | null!
InvalidCastException on bad downcast
```

| | `(T)x` cast | `x as T` | `x is T t` |
|---|---|---|---|
| On failure | Throws `InvalidCastException` | Returns `null` | Returns `false` |
| Works with value types | ✅ | Only `Nullable<T>` | ✅ |
| Use when | You're *sure* of the type | Legacy code / null-check after | **Preferred**: test + cast |

## 4.12 Static classes & members, partial classes, nested classes

```csharp
Console.WriteLine(Config.Version);
Console.WriteLine(MathUtil.Square(7));
Console.WriteLine(Counter.Next());
Console.WriteLine(Counter.Next());
var s = Settings.Instance;
Console.WriteLine(ReferenceEquals(s, Settings.Instance));
var o = new Outer.Inner();
Console.WriteLine(o.Hello());
var r = new Report { Title = "Q3" };
Console.WriteLine(r.Render());

static class MathUtil                          // cannot be instantiated or inherited
{
    public static int Square(int x) => x * x;
}

static class Config
{
    public const string Version = "1.0";         // compile-time constant, baked into callers
    public static readonly DateTime StartedAt = DateTime.UtcNow;  // runtime value, set once
}

class Counter
{
    private static int s_count;                  // ONE copy shared by all
    public static int Next() => ++s_count;
}

sealed class Settings
{
    public static Settings Instance { get; } = new Settings();
    private Settings() { }                       // private ctor: nobody else can 'new' it
}

class Outer
{
    private static string Secret => "nested classes can see private members of Outer";
    public class Inner { public string Hello() => Secret; }
}

partial class Report { public string Title { get; set; } = ""; }
partial class Report { public string Render() => $"Report: {Title}"; }
```

Output:

```text
1.0
49
1
2
True
nested classes can see private members of Outer
Report: Q3
```

- **Static class**: only static members, can't be instantiated/inherited (it's `abstract sealed` in IL). Used for helpers and **extension methods**. Static state is shared across all threads — mutable static state needs locking (`Interlocked.Increment` for counters).
- **Static members** in a normal class: shared by all instances; can't use `this`.
- **Partial classes/methods**: one class split across files — used by code generators (WinForms designer, source generators, EF scaffolding). All parts compile into one type. C# 13/14 also allow partial properties/indexers/constructors/events.
- **Nested classes**: a class inside another; can access the outer class's `private` members. Good for implementation details (e.g., a private enumerator, a builder).

## 4.13 Indexers and operator overloading

**Indexer** = lets your object be used like an array: `grid[1, 2]`. **Operator overloading** = define `+`, `==`, `>`, conversions for your type — great for value-like types (money, vectors, dates).

```csharp
var a = new Money(10.50m, "USD");
var b = new Money(2.25m, "USD");
Console.WriteLine(a + b);
Console.WriteLine(a * 3);
Console.WriteLine(a > b);
Console.WriteLine(a == new Money(10.50m, "USD"));
try { _ = a + new Money(1, "EUR"); }
catch (InvalidOperationException ex) { Console.WriteLine(ex.Message); }

decimal raw = (decimal)a;            // explicit conversion operator
Console.WriteLine(raw);

var grid = new Grid(3, 3);
grid[1, 2] = 'X';                    // 2D indexer
grid["C1"] = 'O';                    // string indexer (overloaded)
Console.WriteLine(grid);

readonly record struct Money(decimal Amount, string Currency)
{
    public static Money operator +(Money x, Money y)
    {
        if (x.Currency != y.Currency) throw new InvalidOperationException("Currency mismatch");
        return x with { Amount = x.Amount + y.Amount };
    }
    public static Money operator *(Money x, decimal factor) => x with { Amount = x.Amount * factor };
    public static bool operator >(Money x, Money y) => x.Amount > y.Amount;
    public static bool operator <(Money x, Money y) => x.Amount < y.Amount;
    public static explicit operator decimal(Money m) => m.Amount;
    public override string ToString() => $"{Amount:0.00} {Currency}";
}

class Grid(int rows, int cols)
{
    private readonly char[,] _cells = new char[rows, cols];

    public char this[int r, int c]
    {
        get => _cells[r, c] == '\0' ? '.' : _cells[r, c];
        set => _cells[r, c] = value;
    }

    public char this[string cell]                     // "C1" -> row 2, col 0
    {
        get => this[cell[0] - 'A', cell[1] - '1'];
        set => this[cell[0] - 'A', cell[1] - '1'] = value;
    }

    public override string ToString()
    {
        var sb = new System.Text.StringBuilder();
        for (int r = 0; r < rows; r++)
        {
            for (int c = 0; c < cols; c++) sb.Append(this[r, c]);
            if (r < rows - 1) sb.AppendLine();
        }
        return sb.ToString();
    }
}
```

Output:

```text
12.75 USD
31.50 USD
True
True
Currency mismatch
10.50
...
..X
O..
```

Rules: operators are `public static`; some must be overloaded **in pairs** (`==`/`!=`, `<`/`>`, `<=`/`>=`) — forgetting the pair is a compile error (CS0216). If you overload `==`, also override `Equals` and `GetHashCode` (records do all three for you — that's why `a == new Money(...)` is `True`). You can't overload `=`, `&&`, `||`, `?:`, `new`.

## Try it yourself

🧩 **Exercise 1 — predict the output** (verified):

```csharp
A obj = new C();
obj.M();
obj.N();
((B)obj).N();

Console.WriteLine(Lazy.Value);
Console.WriteLine("after first access");
Console.WriteLine(Lazy.Value);

class A
{
    public virtual void M() => Console.WriteLine("A.M");
    public virtual void N() => Console.WriteLine("A.N");
}
class B : A
{
    public override void M() => Console.WriteLine("B.M");
    public new virtual void N() => Console.WriteLine("B.N");
}
class C : B
{
    public override void M() => Console.WriteLine("C.M");
    public override void N() => Console.WriteLine("C.N");
}

static class Lazy
{
    static Lazy() => Console.WriteLine("static ctor runs once");
    public static int Value = 42;
}
```

2. Design `Notification` types for Email, SMS and Push where each has a different `Send()`, sharing a `CreatedAt` timestamp and a `Log()` helper. Abstract class, interface, or both?
3. Why does this not compile, and how do you fix it? `public class Repo { public Repo(string cs) { } } public class UserRepo : Repo { }`
4. What's the difference between `protected internal` and `private protected`?
5. Name 3 ways to stop a class from being instantiated with `new` from outside.

<details><summary>Answer</summary>

1. Output:
   ```text
   C.M
   A.N
   C.N
   static ctor runs once
   42
   after first access
   42
   ```
   `M` is one override chain (A→B→C), so the runtime type `C` wins. `B.N` is `new virtual` — a *new* chain; `C.N` overrides `B.N`, not `A.N`. Through an `A` variable, `A.N`'s slot was never overridden → `A.N`. Through a `B` variable → `C.N`. The static constructor runs once, just before first access.
2. Both: `interface INotification { Task SendAsync(); }` for consumers/DI/mocking, plus `abstract class NotificationBase : INotification` holding `CreatedAt` and `Log()`, with `abstract Task SendAsync()`; `EmailNotification : NotificationBase` etc.
3. `UserRepo` gets an implicit parameterless constructor that calls `base()`, but `Repo` has no parameterless constructor → error **CS7036**. Fix: `public UserRepo(string cs) : base(cs) { }`.
4. `protected internal` = accessible from the same assembly **or** derived classes anywhere (union). `private protected` = only derived classes **in the same assembly** (intersection).
5. `private` constructor (singleton/factory), `static class`, `abstract class` (can still be instantiated via a derived class).

</details>

## Quick revision

- 4 pillars: encapsulation (protect state), abstraction (hide how), inheritance (IS-A reuse), polymorphism (one call, many behaviors).
- Default constructor exists only if you write none; chain with `: this(...)` / `: base(...)`.
- Init order: derived static → derived instance fields → base static → base fields → base ctor body → derived ctor body.
- Never call virtual methods from constructors.
- Properties > public fields: validation, `init`, `required`, computed, C# 14 `field`.
- `protected internal` = OR; `private protected` = AND; members default to `private`, types to `internal`.
- Single class inheritance, multiple interfaces; `sealed` stops inheritance.
- Overloading = compile-time; `virtual`/`override` = runtime via v-table.
- `override` → runtime type wins; `new` → declared type wins.
- Abstract class = shared code + state; interface = contract (default methods since C# 8, no state).
- Prefer composition over inheritance.
- Use `is T x` pattern matching instead of `as` + null check or blind casts.
- Static class = `abstract sealed`; partial = one type, many files; nested classes see outer privates.
- Overload operators in pairs; also override `Equals`/`GetHashCode`.

# 5. class vs struct vs record vs record struct

> **Why interviewers ask this:** Since C# 9/10, "When would you use a record?" and "class vs struct?" are standard questions. They check that you understand equality, copying, immutability and performance — and can pick the right tool for DTOs, value objects and entities.

## 5.1 The big comparison table

| | `class` | `struct` | `record` (= `record class`, C# 9) | `record struct` (C# 10) |
|---|---|---|---|---|
| Kind | Reference type | Value type | Reference type | Value type |
| Assignment copies | Reference | Whole value | Reference | Whole value |
| `==` by default | Reference equality | ❌ Not defined (CS0019) unless overloaded | **Value** equality (all properties) | **Value** equality |
| `Equals()` by default | Reference | Value (via reflection — slow!) | Value (generated, fast) | Value (generated, fast) |
| `ToString()` | Type name | Type name | `Person { Name = Asha, Age = 28 }` | Same style |
| `with` expression | ❌ | ✅ (C# 10) | ✅ | ✅ |
| Inheritance | ✅ | ❌ (interfaces only) | ✅ (records from records only) | ❌ |
| Can be `null` | ✅ | ❌ (`T?` needed) | ✅ | ❌ |
| Positional syntax `(string Name)` | Primary ctor only (C# 12) — params are *not* properties | Same as class | Generates **`init`** properties + `Deconstruct` | Generates **mutable** `{ get; set; }` properties (use `readonly record struct` for `init`) |
| Default mutability | Mutable | Mutable | Immutable (positional) | Mutable (positional) |
| Typical use | Entities, services, anything with identity/behavior | Small, short-lived values (`Point`, `Money`) | DTOs, API requests/responses, value objects, events, messages | Small value objects with value semantics (keys, coordinates) |

## 5.2 When to use which

**Simple words:**
- **class** → "This thing has an *identity*." Two customers with the same name are still two different customers. Services, entities, things with lots of behavior or mutable state.
- **record** → "This thing *is its data*." Two `Address`es with the same fields **are** the same address. DTOs, configuration, messages, events.
- **struct** → "This is a small *value* like a number." Microsoft's guideline: logically a single value, **≤ 16 bytes**, immutable, not boxed often, short-lived.
- **record struct** → a struct with free value equality, `ToString` and `with`. Prefer `readonly record struct`.

**Analogy:** A **class** is a *person* (two "Asha Rao"s are still different people). A **record** is a *banknote design* (two ₹500 notes are equally ₹500). A **struct** is the *number written on a sticky note* — copy it freely.

🎯 **Say this in the interview:** "I use classes for entities and services where identity matters, records for immutable data like DTOs and value objects since they give value equality, `with` and a readable `ToString` for free, and structs or `readonly record struct`s only for small, immutable values where avoiding heap allocations matters."

## 5.3 Records: value equality, `with`, deconstruction, inheritance

```csharp
var c1 = new PersonClass("Asha", 28);
var c2 = new PersonClass("Asha", 28);
var r1 = new PersonRecord("Asha", 28);
var r2 = new PersonRecord("Asha", 28);

Console.WriteLine(c1 == c2);           // False: reference equality
Console.WriteLine(r1 == r2);           // True : value equality
Console.WriteLine(ReferenceEquals(r1, r2));
Console.WriteLine(r1);                 // compiler-generated ToString

var older = r1 with { Age = 29 };      // non-destructive mutation
Console.WriteLine(older);
Console.WriteLine(r1.Age);             // original unchanged

var (name, age) = r1;                  // positional records deconstruct
Console.WriteLine($"{name}/{age}");

var set = new HashSet<PersonRecord> { r1, r2 };
Console.WriteLine(set.Count);          // 1 - same hash & equal

// Inheritance + equality: EqualityContract makes types matter
Person p = new Person("Ravi");
Person e = new Employee("Ravi", "IT");
Console.WriteLine(p == e);             // False: different runtime types
Console.WriteLine(e);

// Shallow immutability trap
var team = new Team("Core", new List<string> { "Asha" });
team.Members.Add("Ravi");              // the list itself is still mutable!
Console.WriteLine(string.Join(",", team.Members));

var t2 = team with { Name = "Core2" };
t2.Members.Add("Mia");                 // 'with' is a SHALLOW copy
Console.WriteLine(team.Members.Count);

class PersonClass(string name, int age) { public string Name = name; public int Age = age; }
record PersonRecord(string Name, int Age);
record Person(string Name);
record Employee(string Name, string Dept) : Person(Name);
record Team(string Name, List<string> Members);
```

Output:

```text
False
True
False
PersonRecord { Name = Asha, Age = 28 }
PersonRecord { Name = Asha, Age = 29 }
28
Asha/28
1
False
Employee { Name = Ravi, Dept = IT }
Asha,Ravi
3
```

**What the compiler generates for `record PersonRecord(string Name, int Age);`** (simplified):

```csharp
class PersonRecord : IEquatable<PersonRecord>
{
    public PersonRecord(string Name, int Age) { this.Name = Name; this.Age = Age; }
    public string Name { get; init; }
    public int Age { get; init; }

    protected virtual Type EqualityContract => typeof(PersonRecord);
    public virtual bool Equals(PersonRecord? other) =>
        other is not null && EqualityContract == other.EqualityContract &&
        EqualityComparer<string>.Default.Equals(Name, other.Name) &&
        EqualityComparer<int>.Default.Equals(Age, other.Age);
    public override bool Equals(object? obj) => Equals(obj as PersonRecord);
    public override int GetHashCode() => HashCode.Combine(EqualityContract, Name, Age);
    public static bool operator ==(PersonRecord? a, PersonRecord? b) => a?.Equals(b) ?? b is null;
    public static bool operator !=(PersonRecord? a, PersonRecord? b) => !(a == b);
    public override string ToString() => $"PersonRecord {{ Name = {Name}, Age = {Age} }}";
    public void Deconstruct(out string Name, out int Age) => (Name, Age) = (this.Name, this.Age);
    public virtual PersonRecord <Clone>$() => new PersonRecord(this);   // used by 'with'
    protected PersonRecord(PersonRecord original) { Name = original.Name; Age = original.Age; }
}
```

Key points:
- **`with`** = call the hidden `Clone` (copy constructor), then set the listed `init` properties. It's a **shallow** copy — reference-type members (like a `List`) are shared.
- **Equality compares each field with its own `Equals`.** A `List<T>` or array member compares by **reference** → two records with equal-looking lists are *not* equal (see exercise). Use `ImmutableArray<T>`/custom equality if you need deep comparison.
- **Inheritance:** a record can inherit only from a record. `EqualityContract` (the runtime type) is part of equality, so a `Person` and an `Employee` with the same `Name` are never equal.
- Records can also be written with normal property syntax: `public record Order { public required int Id { get; init; } }`.
- You can add methods, override `ToString`, or customize `Equals(T? other)` / `PrintMembers`.

⚠️ **Interview trap — EF Core entities:** don't make EF entities records. Entities have **identity** (an Id) and are tracked/mutated; value equality and `with` copies confuse change tracking. Records are great for DTOs and value objects.

⚠️ **Interview trap — "records are immutable":** only *positional* record classes get `init`-only properties. You can declare `public string Name { get; set; }` in a record and it's mutable. And `record struct` positional properties are mutable unless it's `readonly record struct`.

## 5.4 Struct pitfalls

```csharp
// Pitfall 1: mutable struct inside a collection
var list = new List<Counter> { new Counter() };
// list[0].Count++;        // ❌ does not compile: CS1612 (list[0] returns a COPY)
var tmp = list[0]; tmp.Count++; list[0] = tmp;     // ✅ copy, modify, write back
Console.WriteLine(list[0].Count);

var arr = new[] { new Counter() };
arr[0].Count++;                                    // arrays give direct access - works
Console.WriteLine(arr[0].Count);

// Pitfall 2: mutating through a readonly field -> defensive copy
var h = new Holder();
h.Increment();
h.Increment();
Console.WriteLine(h.Value);                        // 0 !

// Pitfall 3: struct in foreach is a copy (iteration variable is read-only)
foreach (var c in arr)
{
    // c.Count = 5;          // ❌ does not compile: CS1654 (cannot modify members of 'c')
}

// readonly struct: compiler guarantees immutability, no defensive copies
var p = new Point3(1, 2, 3);
Console.WriteLine(p.Length);
Console.WriteLine(p with { Z = 0 });

Console.WriteLine(default(Counter).Count);        // structs are never null: default = all zeros

struct Counter
{
    public int Count;
    public void Inc() => Count++;
}

class Holder
{
    private readonly Counter _counter = new Counter();
    public void Increment() => _counter.Inc();    // operates on a hidden copy!
    public int Value => _counter.Count;
}

readonly record struct Point3(double X, double Y, double Z)
{
    public double Length => Math.Sqrt(X * X + Y * Y + Z * Z);
}
```

Output:

```text
1
1
0
3.7416573867739413
Point3 { X = 1, Y = 2, Z = 0, Length = 2.23606797749979 }
0
```

| Pitfall | What happens | Fix |
|---|---|---|
| **Mutable structs** | Copies are made silently (assignment, return from method/property/indexer, `foreach`), so you mutate a copy and the change is lost | Make structs **immutable** (`readonly struct`) |
| **Defensive copies** | Calling a non-readonly method on a `readonly` field or `in` parameter → compiler copies the struct first to protect the original (`Holder.Value` stays 0) | `readonly struct`, or mark members `readonly` (C# 8) |
| **Large structs** | Every assignment/parameter copies all bytes — a 200-byte struct passed around is slower than a class reference | Keep ≤ 16 bytes, or use `in`/`ref`, or use a class |
| **Boxing** | Struct used as `object`/interface or in non-generic collections gets boxed, and the box is a *separate copy* | Generics, implement `IEquatable<T>` |
| **Default `Equals`** | `ValueType.Equals` may use reflection → slow, and `GetHashCode` can be poor | Implement `IEquatable<T>` or use `record struct` |
| **`default(T)` / parameterless ctor** | `default` and array elements skip your constructor → fields are zero; you can't prevent an "invalid" default | Design so all-zeros is a valid value |

**`ref struct`** (C# 7.2, e.g. `Span<T>`): a struct that can **only live on the stack** — can't be boxed, can't be a field of a class, can't be captured by lambdas, couldn't be used across `await` (C# 13 relaxes some rules: allowed in async/iterators as long as it's not used across `await`/`yield`; C# 13 also lets ref structs implement interfaces). This is what makes `Span<T>` safe for slicing stack memory.

**Struct constructors:** since C# 10, structs can have explicit parameterless constructors and field initializers; since C# 11 you don't have to assign every field (unassigned fields are zeroed automatically).

## 5.5 Positional records, inheritance and a real-world example

```csharp
// API contract (DTOs) — immutable, value-equal, log-friendly
public record CreateOrderRequest(string CustomerId, IReadOnlyList<OrderLine> Lines);
public record OrderLine(string Sku, int Quantity, decimal UnitPrice);

// Domain events with inheritance
public abstract record OrderEvent(Guid OrderId, DateTimeOffset At);
public sealed record OrderPlaced(Guid OrderId, DateTimeOffset At, decimal Total) : OrderEvent(OrderId, At);
public sealed record OrderCancelled(Guid OrderId, DateTimeOffset At, string Reason) : OrderEvent(OrderId, At);

// Value object with validation in a positional record
public readonly record struct Email
{
    public string Value { get; }
    public Email(string value)
    {
        if (!value.Contains('@')) throw new ArgumentException("Invalid email", nameof(value));
        Value = value.Trim().ToLowerInvariant();
    }
    public override string ToString() => Value;
}

// Entity — identity matters, mutable through behavior → class
public class Order(Guid id)
{
    public Guid Id { get; } = id;
    private readonly List<OrderLine> _lines = [];
    public IReadOnlyList<OrderLine> Lines => _lines;
    public void Add(OrderLine line) => _lines.Add(line);
}
```

Pattern matching on records reads naturally:

```csharp
string Describe(OrderEvent e) => e switch
{
    OrderPlaced { Total: > 1000 } p => $"Big order {p.OrderId}",
    OrderPlaced p                   => $"Order {p.OrderId}",
    OrderCancelled(_, _, var reason) => $"Cancelled: {reason}",   // positional pattern
    _ => "Unknown"
};
```

## Try it yourself

🧩 **Exercise 1 — predict the output** (verified):

```csharp
var p = new P { X = 1 };
Bump(p);
Console.WriteLine(p.X);

var a = new Tagged("x", [1, 2]);
var b = new Tagged("x", [1, 2]);
Console.WriteLine(a == b);

var r1 = new Pt(1, 2);
var r2 = r1 with { };
Console.WriteLine($"{r1 == r2} {ReferenceEquals(r1, r2)}");

var s1 = new SPt(1, 2);
var s2 = new SPt(1, 2);
Console.WriteLine(s1.Equals(s2));

static void Bump(P v) => v.X++;

struct P { public int X; }
record Tagged(string Name, int[] Values);
record Pt(int X, int Y);
struct SPt(int x, int y) { public int X = x; public int Y = y; }
```

2. You need a dictionary key made of `(TenantId, UserId)`. Which type would you choose and why?
3. Can `s1 == s2` be used for the plain `struct SPt` above?
4. Your teammate made a 120-byte mutable struct `OrderSnapshot` and passes it to 10 methods. What problems can arise and what would you suggest?
5. Should an EF Core `Customer` entity be a record? Why / why not?

<details><summary>Answer</summary>

1. Output:
   ```text
   1
   False
   True False
   True
   ```
   `Bump` mutates a **copy** of the struct. `Tagged` records compare the `int[]` by **reference** (two different arrays) → `False`. `with { }` creates a **new** object that is value-equal. Struct `Equals` compares fields → `True`.
2. A `readonly record struct TenantUser(Guid TenantId, Guid UserId)` (or a tuple `(Guid, Guid)`): small, immutable, value equality and a good `GetHashCode` generated, no heap allocation per key.
3. No — `==` isn't defined for a plain struct (compile error **CS0019**). Overload `==`/`!=` (plus `Equals`/`GetHashCode`), or use a `record struct`.
4. Every pass/assignment copies 120 bytes (slow), mutations get lost on copies, and defensive copies appear with `readonly`/`in`. Make it a `class` (or `record` if it's data), or at least a `readonly struct` passed with `in`.
5. No. Entities have identity (`Id`) and are mutated and tracked by EF; record value equality and `with` copies conflict with change tracking. Use a class; use records for DTOs/value objects.

</details>

## Quick revision

- class = reference + identity; struct = value + copy; record = reference + value equality; record struct = value + generated equality.
- Records give you `Equals`, `GetHashCode`, `==`, `ToString`, `Deconstruct`, `with` for free.
- Positional `record` → `init` properties; positional `record struct` → mutable unless `readonly record struct`.
- `with` is a shallow copy; collection members compare by reference.
- Record equality includes the runtime type (`EqualityContract`).
- Records inherit only from records; structs can't inherit at all.
- Structs: ≤ 16 bytes, immutable, short-lived; prefer `readonly struct`.
- Mutable struct + collection/readonly field/foreach = lost updates or compile errors (CS1612, CS1654).
- Defensive copies happen on `readonly` fields and `in` params of non-readonly structs.
- `ref struct` (`Span<T>`) is stack-only: no boxing, no class fields, no lambda capture.
- Plain structs have no `==` (CS0019); default `ValueType.Equals` can be slow.
- Don't use records for EF Core entities.

---

# 6. Equality, Hashing & Comparison

> **Why interviewers ask this:** Almost every real bug with `Dictionary`, `HashSet`, `Distinct()` or caching comes down to "what does *equal* mean for this type?". Interviewers use `==` vs `Equals`, and "why override `GetHashCode`?" to check whether you understand value vs reference semantics and how hash-based collections really work.

## 6.1 Three ways to ask "are these equal?"

**Simple words:** C# has two different questions:

- **Reference equality** – "Are these two variables pointing at the *same object* in memory?" (Same house?)
- **Value equality** – "Do these two objects *contain the same data*?" (Two different houses built from the same blueprint, same paint, same furniture?)

| Tool | What it checks | Can a type change it? |
|---|---|---|
| `ReferenceEquals(a, b)` | Always "same object?" | ❌ Never. Always reference. |
| `a.Equals(b)` | Whatever the type decides (virtual method) | ✅ Override `Equals` |
| `a == b` | Whatever the **compile-time type's** operator says | ✅ Overload `operator ==` |

Default behaviour by kind of type:

| Type kind | `==` default | `Equals` default |
|---|---|---|
| `class` | Reference | Reference |
| `struct` | ❌ Not defined (compile error) | Value (field by field, via reflection = slow) |
| `string` | **Value** (overloaded) | Value |
| `record` / `record struct` | **Value** (compiler-generated) | Value (compiler-generated) |
| Primitives (`int`, `double`…) | Value | Value |

**Easy example — the whole picture in one program:**

```csharp
int a = 5, b = 5;
Console.WriteLine($"int    ==     : {a == b}");
Console.WriteLine($"int    Equals : {a.Equals(b)}");

var c1 = new PersonClass("Ana");
var c2 = new PersonClass("Ana");
Console.WriteLine($"class  ==     : {c1 == c2}");
Console.WriteLine($"class  Equals : {c1.Equals(c2)}");
Console.WriteLine($"class  RefEq  : {ReferenceEquals(c1, c1)}");

string s1 = "hello";
string s2 = new string("hello".ToCharArray());   // a different string object, same text
Console.WriteLine($"string ==     : {s1 == s2}");
Console.WriteLine($"string RefEq  : {ReferenceEquals(s1, s2)}");
object o1 = s1, o2 = s2;
Console.WriteLine($"object ==     : {o1 == o2}");     // static type is object -> reference comparison!
Console.WriteLine($"object Equals : {o1.Equals(o2)}"); // virtual call -> string.Equals runs

var r1 = new PersonRecord("Ana");
var r2 = new PersonRecord("Ana");
Console.WriteLine($"record ==     : {r1 == r2}");
Console.WriteLine($"record RefEq  : {ReferenceEquals(r1, r2)}");

class PersonClass(string name) { public string Name { get; } = name; }
record PersonRecord(string Name);
```

Output:

```text
int    ==     : True
int    Equals : True
class  ==     : False
class  Equals : False
class  RefEq  : True
string ==     : True
string RefEq  : False
object ==     : False
object Equals : True
record ==     : True
record RefEq  : False
```

⚠️ **Interview trap:** `object o1 = s1, o2 = s2; o1 == o2` is `False` even though both are `"hello"`. Operators are **static** and chosen at **compile time** from the *declared* type (`object` → reference `==`). `Equals` is **virtual** and chosen at **run time** from the *actual* type (`string`). If only *one* side is typed `object` and the other `string`, the compiler warns you (*CS0252/CS0253: possible unintended reference comparison*); when both are `object`, you get no warning at all.

Structs don't get `==` for free:

```csharp
var p = new Point(1, 2);
var q = new Point(1, 2);
Console.WriteLine(p.Equals(q)); // True  (field-by-field, may use reflection = slow)
Console.WriteLine(p == q);      // ❌ does not compile: CS0019 (no == operator defined for the struct)

struct Point(int x, int y) { public int X = x; public int Y = y; }
```

🎯 **Say this in the interview:** "`ReferenceEquals` is always identity. `Equals` is a virtual method a type overrides to define value equality. `==` is a static operator resolved at compile time from the declared type — strings and records overload it for value equality, plain classes don't."

🧩 **Predict the output:**

```csharp
object x = 42;
object y = 42;
Console.WriteLine(x == y);
Console.WriteLine(x.Equals(y));

string s = "abc";
string t = "ab" + "c";          // compile-time constant
Console.WriteLine((object)s == (object)t);
```

<details><summary>Answer</summary>

```text
False
True
True
```

- `x == y` → `False`: each `42` is boxed into a **separate** object, and `object ==` compares references.
- `x.Equals(y)` → `True`: virtual `Int32.Equals` compares the values.
- The last one is `True` because `"ab" + "c"` is a compile-time constant, folded to `"abc"`, and all string literals are **interned** (stored once). Don't rely on that in real code.

</details>

## 6.2 Why you must override `Equals` AND `GetHashCode` together

**Real-life analogy:** a `Dictionary`/`HashSet` is a big library with numbered shelves (buckets). `GetHashCode()` tells the librarian **which shelf** to go to; `Equals()` is used to check the books **on that shelf** one by one. If two "equal" books give different shelf numbers, the librarian looks on the wrong shelf and says "not found".

The **contract**:

1. If `a.Equals(b)` is `true` ⇒ `a.GetHashCode() == b.GetHashCode()` **must** be true.
2. The reverse is NOT required: equal hash codes can belong to different objects (a *collision*) — that's fine, `Equals` sorts it out.
3. The hash code must not change while the object is inside a hash-based collection.

**Runnable demo — what breaks if you only override `Equals`:**

```csharp
var bad = new HashSet<BadPoint> { new BadPoint(1, 2) };
Console.WriteLine($"Equals says equal?       {new BadPoint(1, 2).Equals(new BadPoint(1, 2))}");
Console.WriteLine($"HashSet contains?        {bad.Contains(new BadPoint(1, 2))}");

var good = new HashSet<GoodPoint> { new GoodPoint(1, 2) };
Console.WriteLine($"Fixed HashSet contains?  {good.Contains(new GoodPoint(1, 2))}");

// ❌ Overrides Equals but NOT GetHashCode (compiler warns CS0659)
class BadPoint(int x, int y)
{
    public int X { get; } = x;
    public int Y { get; } = y;
    public override bool Equals(object? obj) => obj is BadPoint p && p.X == X && p.Y == Y;
}

// ✅ Overrides both
class GoodPoint(int x, int y)
{
    public int X { get; } = x;
    public int Y { get; } = y;
    public override bool Equals(object? obj) => obj is GoodPoint p && p.X == X && p.Y == Y;
    public override int GetHashCode() => HashCode.Combine(X, Y);
}
```

Output:

```text
Equals says equal?       True
HashSet contains?        False
Fixed HashSet contains?  True
```

`BadPoint` inherits `object.GetHashCode()`, which is based on object identity. Two "equal" `BadPoint`s almost always land in different buckets, so `Contains` never even calls your `Equals`.

### The complete, correct pattern (`IEquatable<T>` + `HashCode.Combine`)

```csharp
var a = new Money(10.00m, "USD");
var b = new Money(10.00m, "usd");
var c = new Money(25m, "EUR");

Console.WriteLine(a == b);
Console.WriteLine(a.Equals((object)b));
Console.WriteLine(a != c);
Console.WriteLine(a.GetHashCode() == b.GetHashCode());

var prices = new Dictionary<Money, string> { [a] = "Coffee" };
Console.WriteLine(prices[b]);      // found, because b is "equal" to a

public sealed class Money : IEquatable<Money>
{
    public decimal Amount { get; }
    public string Currency { get; }

    public Money(decimal amount, string currency) =>
        (Amount, Currency) = (amount, currency.ToUpperInvariant());

    // 1. Strongly-typed Equals (no boxing, no casting) — from IEquatable<T>
    public bool Equals(Money? other) =>
        other is not null && Amount == other.Amount && Currency == other.Currency;

    // 2. object.Equals — delegate to the typed one
    public override bool Equals(object? obj) => Equals(obj as Money);

    // 3. GetHashCode — use EXACTLY the same fields as Equals
    public override int GetHashCode() => HashCode.Combine(Amount, Currency);

    // 4. Operators — so == behaves like Equals
    public static bool operator ==(Money? left, Money? right) =>
        left is null ? right is null : left.Equals(right);
    public static bool operator !=(Money? left, Money? right) => !(left == right);
}
```

Output:

```text
True
True
True
True
Coffee
```

Checklist for a hand-written equality type:

| Step | Why |
|---|---|
| Implement `IEquatable<T>.Equals(T?)` | Typed, fast, no boxing for structs; used by `List.Contains`, `Dictionary`, `EqualityComparer<T>.Default` |
| Override `Equals(object?)` | So non-generic code (and `object` variables) agree |
| Override `GetHashCode()` with **the same fields** | Contract rule #1 |
| Use `HashCode.Combine(...)` | Good distribution, handles nulls, no manual `* 31 ^ ...` math |
| Overload `==` / `!=` | So `==` doesn't silently stay reference equality |
| Prefer **immutable** fields | Contract rule #3 |
| `sealed` class (or compare `GetType()`) | Avoids asymmetric equality between base and derived types |

💡 For most data types just use a **`record`** — the compiler writes all of the above for you (`Equals`, `GetHashCode`, `==`, `!=`, `IEquatable<T>`).

### The mutable key trap

```csharp
var key = new MutableKey { Id = 1 };
var dict = new Dictionary<MutableKey, string> { [key] = "first" };

key.Id = 2;    // 💥 mutate the key AFTER inserting it

Console.WriteLine(dict.ContainsKey(key));                        // looks in bucket for hash(2)
Console.WriteLine(dict.ContainsKey(new MutableKey { Id = 1 }));  // right bucket, but Equals fails
Console.WriteLine(dict.Count);                                   // the entry is "lost" but still there

class MutableKey
{
    public int Id { get; set; }
    public override bool Equals(object? o) => o is MutableKey k && k.Id == Id;
    public override int GetHashCode() => Id.GetHashCode();
}
```

Output:

```text
False
False
1
```

⚠️ **Interview trap:** Never mutate a field that participates in `GetHashCode` while the object is a key in a `Dictionary`/`HashSet`. The entry becomes unreachable (a silent memory leak + logic bug). Make keys immutable (`record`, `init`, `readonly`).

🎯 **Say this in the interview:** "Hash collections first use `GetHashCode` to pick a bucket, then `Equals` to confirm. If I override `Equals` but not `GetHashCode`, equal objects land in different buckets and lookups fail. So I always override both, using the same fields, usually with `HashCode.Combine`, and keep those fields immutable."

## 6.3 Ordering: `IComparable<T>` vs `IComparer<T>`

**Simple words:**

- `IComparable<T>` = the object knows its own **natural order** ("I know how I compare to others"). Implemented *inside* the type. Example: numbers, dates, strings.
- `IComparer<T>` = an **external judge** that compares two objects. Lets you have *many* orders (by name, by salary, by date) without touching the type.

Both return an `int`: **negative** → first comes before second, **0** → same position, **positive** → first comes after.

| | `IComparable<T>` | `IComparer<T>` | `Comparison<T>` delegate |
|---|---|---|---|
| Where | Inside the type | Separate class | Lambda |
| Method | `CompareTo(T other)` | `Compare(T x, T y)` | `(x, y) => int` |
| How many orders | One "natural" order | As many as you like | As many as you like |
| Used by | `List.Sort()`, `OrderBy(x => x)`, `SortedSet<T>` default | `Sort(comparer)`, `SortedSet(comparer)`, `OrderBy(k, comparer)` | `List.Sort(comparison)`, `Comparer<T>.Create` |

```csharp
var team = new List<Employee>
{
    new("Zara", 90_000), new("Adam", 120_000), new("Mia", 90_000)
};

team.Sort();                                       // uses IComparable<Employee> (by Salary)
Console.WriteLine(string.Join(", ", team));

team.Sort(new NameComparer());                     // uses an IComparer<Employee>
Console.WriteLine(string.Join(", ", team));

team.Sort((x, y) => y.Salary.CompareTo(x.Salary)); // Comparison<T> lambda, descending
Console.WriteLine(string.Join(", ", team));

var bySalaryThenName = Comparer<Employee>.Create((x, y) =>
{
    int bySalary = x.Salary.CompareTo(y.Salary);
    return bySalary != 0 ? bySalary : string.Compare(x.Name, y.Name, StringComparison.Ordinal);
});
var sorted = new SortedSet<Employee>(team, bySalaryThenName);
Console.WriteLine(string.Join(", ", sorted));

record Employee(string Name, decimal Salary) : IComparable<Employee>
{
    // negative = this comes first, 0 = same position, positive = this comes after
    public int CompareTo(Employee? other) => other is null ? 1 : Salary.CompareTo(other.Salary);
    public override string ToString() => $"{Name}({Salary / 1000}k)";
}

class NameComparer : IComparer<Employee>
{
    public int Compare(Employee? x, Employee? y) =>
        string.Compare(x?.Name, y?.Name, StringComparison.Ordinal);
}
```

Output:

```text
Zara(90k), Mia(90k), Adam(120k)
Adam(120k), Mia(90k), Zara(90k)
Adam(120k), Mia(90k), Zara(90k)
Mia(90k), Zara(90k), Adam(120k)
```

⚠️ **Interview trap:** `List<T>.Sort()` / `Array.Sort()` are **unstable** (introsort): equal elements may swap places. LINQ `OrderBy` is **stable** — equal elements keep their original order. If you need "sort by salary, keep insertion order for ties", use `OrderBy` or add a tie-breaker.

⚠️ **Interview trap:** Don't write comparers as `return x.Salary - y.Salary;` with `int`s — it overflows for large/negative values. Use `x.CompareTo(y)`.

## 6.4 `IEqualityComparer<T>` and `EqualityComparer<T>.Default`

`IEqualityComparer<T>` is the "external judge" for **equality** (`Equals` + `GetHashCode`), just like `IComparer<T>` is for ordering. You pass it to `Dictionary`, `HashSet`, `Distinct`, `GroupBy`, `ToDictionary`, etc.

`EqualityComparer<T>.Default` picks the best built-in strategy: uses `IEquatable<T>` if implemented, otherwise `object.Equals`. Generic collections use it internally.

```csharp
string[] names = ["Asha", "asha", "Bilal", "ASHA"];
Console.WriteLine(string.Join(", ", names.Distinct()));
Console.WriteLine(string.Join(", ", names.Distinct(StringComparer.OrdinalIgnoreCase)));

var stock = new Dictionary<string, int>(StringComparer.OrdinalIgnoreCase) { ["apple"] = 5 };
Console.WriteLine(stock["APPLE"]);

// Custom comparer: two products are "the same" if their Sku matches (Name ignored)
Product[] products = [new("A1", "Pen"), new("A1", "Blue Pen"), new("B2", "Book")];
Console.WriteLine(string.Join(", ", products.Distinct(new SkuComparer())));

Console.WriteLine(EqualityComparer<string>.Default.Equals("x", "x"));

record Product(string Sku, string Name);

class SkuComparer : IEqualityComparer<Product>
{
    public bool Equals(Product? x, Product? y) => x?.Sku == y?.Sku;
    public int GetHashCode(Product p) => p.Sku.GetHashCode();
}
```

Output:

```text
Asha, asha, Bilal, ASHA
Asha, Bilal
5
Product { Sku = A1, Name = Pen }, Product { Sku = B2, Name = Book }
True
```

🎯 **Say this in the interview:** "When I can't or shouldn't change a type's equality — like case-insensitive string keys — I pass an `IEqualityComparer<T>`, e.g. `StringComparer.OrdinalIgnoreCase`, into the `Dictionary` or `Distinct` call."

## Try it yourself

1. Write a class `Isbn` (wraps a string) that is equal **case-insensitively** and works correctly in a `HashSet`.
2. Why does `new List<int>{1,2} == new List<int>{1,2}` return `False`? How do you compare the contents?
3. Sort `["pear","fig","apple","kiwi","date"]` by length, then alphabetically — once with LINQ, once with `List.Sort`.
4. What happens if `GetHashCode()` always returns `42`? Is it *correct*? Is it *fast*?
5. Are these equal? `record Team(string Name, List<string> Members);` `new Team("A", ["x"]) == new Team("A", ["x"])`

<details><summary>Answer</summary>

**1.** Normalize in the constructor, then use it in both `Equals` and `GetHashCode`:

```csharp
var books = new HashSet<Isbn> { new("978-0134494166") };
Console.WriteLine(books.Contains(new Isbn("978-0134494166")));
Console.WriteLine(new Isbn("abc-x") == new Isbn("ABC-X"));

public sealed class Isbn(string value) : IEquatable<Isbn>
{
    public string Value { get; } = value.ToUpperInvariant();
    public bool Equals(Isbn? other) => other is not null && Value == other.Value;
    public override bool Equals(object? obj) => Equals(obj as Isbn);
    public override int GetHashCode() => Value.GetHashCode();
    public static bool operator ==(Isbn? a, Isbn? b) => a is null ? b is null : a.Equals(b);
    public static bool operator !=(Isbn? a, Isbn? b) => !(a == b);
}
```

```text
True
True
```

**2.** `List<T>` is a class and doesn't override `Equals`/`==`, so both compare references.

```csharp
var a = new List<int> { 1, 2 };
var b = new List<int> { 1, 2 };
Console.WriteLine(a == b);
Console.WriteLine(a.Equals(b));
Console.WriteLine(a.SequenceEqual(b));
```

```text
False
False
True
```

**3.**

```csharp
string[] words = ["pear", "fig", "apple", "kiwi", "date"];
Console.WriteLine(string.Join(",", words.OrderBy(w => w.Length).ThenBy(w => w, StringComparer.Ordinal)));

var list = words.ToList();
list.Sort((x, y) => x.Length != y.Length ? x.Length.CompareTo(y.Length) : string.CompareOrdinal(x, y));
Console.WriteLine(string.Join(",", list));
```

```text
fig,date,kiwi,pear,apple
fig,date,kiwi,pear,apple
```

**4.** It's **correct** (equal objects still have equal hashes) but every item lands in the same bucket, so `Dictionary` degrades from O(1) to **O(n)** per lookup — a linked-list scan.

**5.** `False`. The record's generated `Equals` compares each property with `EqualityComparer<T>.Default`, and for `List<string>` that's **reference** equality.

```csharp
var t1 = new Team("A", ["x"]);
var t2 = new Team("A", ["x"]);
Console.WriteLine(t1 == t2);

record Team(string Name, List<string> Members);
```

```text
False
```

</details>

## Quick revision

- `ReferenceEquals` = same object, always; can't be overridden.
- `Equals` = virtual, run-time type decides; `==` = static, compile-time type decides.
- `string` and `record` give value equality for both `==` and `Equals`.
- Structs: `Equals` works (slow reflection by default), `==` doesn't compile unless you define it.
- Override `Equals` ⇒ override `GetHashCode` (compiler warns CS0659).
- Equal objects **must** have equal hash codes; equal hash codes don't imply equality.
- Use `HashCode.Combine(a, b, c)` over hand-rolled hash math.
- Implement `IEquatable<T>` for typed, boxing-free equality.
- Never mutate a key's hashed fields while it's inside a `Dictionary`/`HashSet`.
- `IComparable<T>` = natural order inside the type; `IComparer<T>` = external, many orders.
- `List.Sort` is unstable; `OrderBy` is stable.
- `IEqualityComparer<T>` (e.g. `StringComparer.OrdinalIgnoreCase`) customizes equality for collections and LINQ.

# 7. Generics

> **Why interviewers ask this:** Generics are everywhere — `List<T>`, `Task<T>`, `IRepository<T>`, DI registrations. Interviewers check that you know *why* they exist (type safety, no boxing), can write constraints, and can explain covariance/contravariance without hand-waving.

## 7.1 Why generics exist

**Simple words:** A generic is a **template with a blank** (`T`) that you fill in with a real type later. Write the code once, use it for `int`, `string`, `Customer`…

**Real-life analogy:** A *labelled* storage box. An old `ArrayList` is a box labelled "stuff" — you can throw anything in, and you must check what you pulled out. A `List<int>` is a box labelled "ONLY integers" — the label is checked at the door (compile time).

Before generics (.NET 1.x), collections stored `object`:

```csharp
using System.Collections;

var old = new ArrayList { 1, 2, "three" };   // anything goes — no type safety
try
{
    int sum = 0;
    foreach (object o in old) sum += (int)o;  // unboxing cast on every item
}
catch (InvalidCastException ex) { Console.WriteLine($"Runtime crash: {ex.GetType().Name}"); }

var modern = new List<int> { 1, 2 };
// modern.Add("three");                       // compile-time error instead of a runtime crash
int total = 0;
foreach (int n in modern) total += n;          // no cast, no boxing
Console.WriteLine($"Total = {total}");
```

Output:

```text
Runtime crash: InvalidCastException
Total = 3
```

And with generics the bad line doesn't even compile:

```csharp
var modern = new List<int> { 1, 2 };
modern.Add("three");   // ❌ does not compile: CS1503 (cannot convert from 'string' to 'int')
```

| | `ArrayList` (object-based) | `List<T>` (generic) |
|---|---|---|
| Type safety | ❌ Runtime `InvalidCastException` | ✅ Compile-time error |
| Value types | **Boxed** on add, **unboxed** on read (heap allocation each time) | Stored directly, no boxing |
| Casting | Needed everywhere | None |
| Performance | Slower + more GC pressure | Fast |

🎯 **Say this in the interview:** "Generics give compile-time type safety and avoid boxing for value types, so I write one implementation that works for any type without casts. The runtime creates specialized code per value type and shares one implementation for all reference types."

## 7.2 Generic methods, classes and interfaces

```csharp
int x = 1, y = 2;
Swap(ref x, ref y);
Console.WriteLine($"x={x}, y={y}");

Console.WriteLine(Max(3, 7));            // T inferred as int
Console.WriteLine(Max("apple", "pear")); // T inferred as string

var box = new Box<string>("hi");
Console.WriteLine(box);

var repo = new InMemoryRepository<Customer>();
repo.Add(new Customer { Id = 1, Name = "Contoso" });
Console.WriteLine(repo.GetById(1)?.Name);
Console.WriteLine(repo.CreateBlank().Name);

static void Swap<T>(ref T a, ref T b) => (a, b) = (b, a);
static T Max<T>(T a, T b) where T : IComparable<T> => a.CompareTo(b) >= 0 ? a : b;

public class Box<T>(T value)
{
    public T Value { get; } = value;
    public override string ToString() => $"Box<{typeof(T).Name}>({Value})";
}

public interface IEntity { int Id { get; } }

public interface IRepository<T> where T : IEntity
{
    void Add(T item);
    T? GetById(int id);
}

public class InMemoryRepository<T> : IRepository<T> where T : class, IEntity, new()
{
    private readonly Dictionary<int, T> _store = new();
    public void Add(T item) => _store[item.Id] = item;
    public T? GetById(int id) => _store.GetValueOrDefault(id);
    public T CreateBlank() => new T();   // allowed only because of the new() constraint
}

public class Customer : IEntity
{
    public int Id { get; set; }
    public string Name { get; set; } = "(unnamed)";
}
```

Output:

```text
x=2, y=1
7
pear
Box<String>(hi)
Contoso
(unnamed)
```

Notes:

- **Type inference:** `Max(3, 7)` — the compiler figures out `T = int` from the arguments. You rarely write `Max<int>(3, 7)`.
- Type parameter names: `T` for one, `TKey`/`TValue`/`TResult` when there are several.
- A generic class can **implement** a generic interface (`InMemoryRepository<T> : IRepository<T>`) and **add stricter constraints**.

## 7.3 Constraints — telling the compiler what `T` can do

Without constraints, `T` could be *anything*, so you can only use `object` members on it. Constraints unlock more.

| Constraint | Meaning | Unlocks / typical use |
|---|---|---|
| `where T : class` | Reference type | `T` can be `null`; use `as`, compare to `null` |
| `where T : class?` | Reference type, nullable allowed | Nullable-aware APIs |
| `where T : struct` | Non-nullable value type | `T?` means `Nullable<T>`; no boxing guarantees |
| `where T : notnull` | Any non-nullable type | Dictionary keys (`TKey : notnull`) |
| `where T : unmanaged` | Value type with no references inside (int, double, simple structs) | `Span<T>`, pointers, `stackalloc`, binary serialization |
| `where T : new()` | Has a public parameterless ctor | `new T()` — must be **last** in the list |
| `where T : SomeBaseClass` | Is/derives from that class | Call base members |
| `where T : ISomeInterface` | Implements that interface | Call interface members (`CompareTo`, `Id`) |
| `where T : U` | `T` is/derives from another type parameter `U` | Relating two type params |
| `where T : allows ref struct` (C# 13) | `T` may be a `ref struct` like `Span<T>` | High-performance generic APIs |

Combining: `where T : class, IEntity, new()` → order is *class/struct* first, then base class, then interfaces, then `new()`.

```csharp
static T Create<T>() => new T();   // ❌ does not compile: CS0304 (add 'where T : new()')
```

⚠️ **Interview trap:** You **can't** add constraints for operators like `+` with classic constraints — `where T : int` is illegal. Since C# 11 the answer is **generic math** (`INumber<T>`, below).

## 7.4 `default(T)`

`default(T)` = "the zero value for whatever `T` is": `0`, `false`, `null`, or a struct with all fields zeroed. Since C# 7.1 you can just write `default` when the type is obvious.

```csharp
Console.WriteLine($"int      : {Show<int>()}");
Console.WriteLine($"bool     : {Show<bool>()}");
Console.WriteLine($"double   : {Show<double>()}");
Console.WriteLine($"(int,int): {Show<(int, int)>()}");
Console.WriteLine($"string   : {Show<string>()}");
Console.WriteLine($"int?     : {Show<int?>()}");

static string Show<T>() => default(T)?.ToString() ?? "null";
```

Output:

```text
int      : 0
bool     : False
double   : 0
(int,int): (0, 0)
string   : null
int?     : null
```

⚠️ **Interview trap:** For an **unconstrained** `T`, `T?` does **not** mean `Nullable<T>`. In `TValue? Get()` with `TValue = int`, the return type is just `int` and "missing" comes back as `0`, not `null`. Only with `where T : struct` does `T?` become `Nullable<T>`.

## 7.5 Generic static fields = one copy per closed type (a free cache)

`Cache<int>` and `Cache<string>` are **different types** at runtime, each with **its own** static fields and its own static constructor run. This is a well-known zero-cost caching trick (used inside `EqualityComparer<T>.Default`, `Array.Empty<T>()`, etc.).

```csharp
Console.WriteLine(TypeInfoCache<int>.Description);
Console.WriteLine(TypeInfoCache<string>.Description);
Console.WriteLine(TypeInfoCache<int>.Description);   // cached: no second "computing"

static class TypeInfoCache<T>
{
    public static readonly string Description;

    static TypeInfoCache()   // runs ONCE per closed type: once for int, once for string
    {
        Console.WriteLine($"  computing for {typeof(T).Name}...");
        Description = $"{typeof(T).FullName}, value type: {typeof(T).IsValueType}";
    }
}
```

Output:

```text
  computing for Int32...
System.Int32, value type: True
  computing for String...
System.String, value type: False
System.Int32, value type: True
```

🧩 **Predict the output:**

```csharp
Counter<int>.Count++;
Counter<int>.Count++;
Counter<string>.Count++;
Console.WriteLine($"{Counter<int>.Count} {Counter<string>.Count} {Counter<double>.Count}");

static class Counter<T> { public static int Count; }
```

<details><summary>Answer</summary>

```text
2 1 0
```

Each closed type (`Counter<int>`, `Counter<string>`, `Counter<double>`) has its **own** `Count` field.

</details>

## 7.6 Covariance (`out`) and contravariance (`in`) — in simple words

**The question:** a `Dog` *is an* `Animal`. Is a *"box of Dogs"* also a *"box of Animals"*? It depends on what you can **do** with the box.

- **Covariance (`out T`)** — the type only **gives `T` out** (produces/returns it). A *shelter that hands out Dogs* can be used where you need *something that hands out Animals*. ✅ Safe: whatever comes out is a Dog, which is an Animal.
  → `IEnumerable<out T>`, `IReadOnlyList<out T>`, `Func<out TResult>`. (Note: `Task<T>` is a *class*, so it is **not** variant — only interfaces and delegates can be.)
- **Contravariance (`in T`)** — the type only **takes `T` in** (consumes it). A *vet who can treat ANY Animal* can be used where you need *a vet for Dogs*. ✅ Safe: every Dog you hand in is an Animal.
  → `Action<in T>`, `IComparer<in T>`, `IEqualityComparer<in T>`, `Func<in T, out TResult>`.
- **Invariant** — `T` goes both in and out (`List<T>` has `Add(T)` *and* `T this[int]`). No conversion allowed.

```csharp
IEnumerable<string> names = new List<string> { "Ana", "Ben" };
IEnumerable<object> objects = names;             // ✅ covariance (IEnumerable<out T>)
Console.WriteLine(string.Join(",", objects));

Action<object> printAnything = o => Console.WriteLine($"Printing {o}");
Action<string> printString = printAnything;      // ✅ contravariance (Action<in T>)
printString("hello");

Func<string> makeString = () => "made";
Func<object> makeObject = makeString;            // ✅ Func<out TResult>
Console.WriteLine(makeObject());

IProducer<Animal> producer = new DogShelter();   // custom covariant interface
Console.WriteLine(producer.Produce().Sound());

IConsumer<Dog> vet = new AnimalVet();            // custom contravariant interface
vet.Treat(new Dog());

// Arrays are covariant too — but NOT type-safe!
object[] arr = new string[2];
try { arr[0] = 42; }
catch (ArrayTypeMismatchException) { Console.WriteLine("ArrayTypeMismatchException!"); }

class Animal { public virtual string Sound() => "..."; }
class Dog : Animal { public override string Sound() => "Woof"; }

interface IProducer<out T> { T Produce(); }        // T only comes OUT
interface IConsumer<in T> { void Treat(T item); }  // T only goes IN

class DogShelter : IProducer<Dog> { public Dog Produce() => new Dog(); }
class AnimalVet : IConsumer<Animal>
{
    public void Treat(Animal a) => Console.WriteLine($"Treating a {a.GetType().Name}");
}
```

Output:

```text
Ana,Ben
Printing hello
made
Woof
Treating a Dog
ArrayTypeMismatchException!
```

And the invariant case:

```csharp
List<object> objects = new List<string>();   // ❌ does not compile: CS0029 (List<T> is invariant)
```

Why is `List<string>` → `List<object>` forbidden? Because then you could call `objects.Add(42)` and put an `int` into a list of strings. Arrays *did* allow this in .NET 1.0 (before generics), which is why the array line above throws `ArrayTypeMismatchException` at **run time** instead of failing at compile time.

| | Keyword | Direction | Allowed conversion | Examples |
|---|---|---|---|---|
| Covariance | `out` | T only returned | `I<Derived>` → `I<Base>` | `IEnumerable<T>`, `IReadOnlyCollection<T>`, `Func<TResult>` |
| Contravariance | `in` | T only accepted | `I<Base>` → `I<Derived>` | `Action<T>`, `IComparer<T>`, `IEqualityComparer<T>` |
| Invariance | none | in and out | none | `List<T>`, `IList<T>`, `Dictionary<K,V>` |

🎯 **Say this in the interview:** "Covariance (`out`) lets me use a more derived type argument where a base one is expected, which is safe when the type only *produces* T, like `IEnumerable<T>`. Contravariance (`in`) is the reverse, safe when the type only *consumes* T, like `Action<T>` or `IComparer<T>`. It works only on interfaces and delegates, and only for reference types."

⚠️ **Interview trap:** Variance doesn't apply to value types: `IEnumerable<int>` → `IEnumerable<object>` does **not** compile (CS0266) — it would need boxing of every element.

## 7.7 Generic math (C# 11 / .NET 7+) — brief

Interfaces can now have **static abstract members** (like `T.Zero` and `operator +`). `INumber<T>` bundles them, so you can finally write one `Sum` for all numeric types.

```csharp
using System.Numerics;

Console.WriteLine(SumAll([1, 2, 3]));
Console.WriteLine(SumAll([1.5, 2.25]));
Console.WriteLine(SumAll([10m, 20.5m]));
Console.WriteLine(Average([2, 4, 9]));

static T SumAll<T>(IEnumerable<T> items) where T : INumber<T>
{
    T total = T.Zero;                 // static abstract member on the interface
    foreach (var item in items) total += item;   // operator + comes from INumber<T>
    return total;
}

static double Average<T>(IReadOnlyList<T> items) where T : INumber<T> =>
    double.CreateChecked(SumAll(items)) / items.Count;
```

Output:

```text
6
3.75
30.5
5
```

## Try it yourself

1. Write `Pair<TFirst, TSecond>` with a `Swap()` method that returns a `Pair<TSecond, TFirst>`.
2. Write `MinOf<T>(IReadOnlyList<T> items)` that works for `int` and `string`. Which constraint do you need?
3. Why can't `IList<T>` be declared `IList<out T>`?
4. Which constraint lets you write `new T()`? Which one lets you use `T` as a `Dictionary` key without a nullable warning?
5. Write a `SimpleCache<TKey, TValue>` with `GetOrAdd(key, factory)` that calls the factory only once per key.

<details><summary>Answer</summary>

**1.**

```csharp
var p = new Pair<int, string>(1, "one");
Console.WriteLine(p.Swap());

public record Pair<TFirst, TSecond>(TFirst First, TSecond Second)
{
    public Pair<TSecond, TFirst> Swap() => new(Second, First);
}
```

```text
Pair { First = one, Second = 1 }
```

**2.** `where T : IComparable<T>` so you can call `CompareTo`.

```csharp
Console.WriteLine(MinOf([5, 2, 9]));
Console.WriteLine(MinOf(["pear", "apple"]));

static T MinOf<T>(IReadOnlyList<T> items) where T : IComparable<T>
{
    if (items.Count == 0) throw new ArgumentException("Empty list", nameof(items));
    T min = items[0];
    foreach (var item in items)
        if (item.CompareTo(min) < 0) min = item;
    return min;
}
```

```text
2
apple
```

**3.** `IList<T>` has `Add(T item)` and `this[int] { set; }` — `T` goes **in**, so making it covariant would allow putting an `Animal` into a list of `Dog`s. Only output-only interfaces like `IReadOnlyList<out T>` can be covariant.

**4.** `new()` for `new T()`; `notnull` for dictionary keys (`Dictionary<TKey, TValue> where TKey : notnull`).

**5.**

```csharp
var cache = new SimpleCache<int, string>();
Console.WriteLine(cache.GetOrAdd(1, k => { Console.WriteLine("  computing"); return $"v{k}"; }));
Console.WriteLine(cache.GetOrAdd(1, k => "never called"));

public class SimpleCache<TKey, TValue> where TKey : notnull
{
    private readonly Dictionary<TKey, TValue> _items = new();

    public TValue GetOrAdd(TKey key, Func<TKey, TValue> factory)
    {
        if (!_items.TryGetValue(key, out var value))
        {
            value = factory(key);
            _items[key] = value;
        }
        return value;
    }
}
```

```text
  computing
v1
v1
```

</details>

## Quick revision

- Generics = type parameters → compile-time safety + no boxing for value types.
- `ArrayList` boxes `int`s and fails at run time; `List<int>` fails at compile time.
- Type inference usually lets you skip `<int>` on generic method calls.
- Constraints: `class`, `struct`, `notnull`, `unmanaged`, `new()`, base class, interface, `allows ref struct` (C# 13).
- `new()` must be the last constraint; `class`/`struct` must be first.
- `default(T)` = zero/false/null; unconstrained `T?` is **not** `Nullable<T>`.
- Each closed generic type has its own static fields → per-type cache.
- `out` = covariant (producer), `in` = contravariant (consumer); interfaces & delegates only.
- `List<T>` is invariant; arrays are (unsafely) covariant → `ArrayTypeMismatchException`.
- Variance doesn't work for value-type arguments.
- C# 11 generic math: `where T : INumber<T>` gives `T.Zero`, `+`, `*`, etc.

# 8. Collections & Data Structures

> **Why interviewers ask this:** Picking the wrong collection is the #1 hidden performance bug in business code (`List.Contains` inside a loop = O(n²)). Interviewers want to hear Big-O, *how* `List<T>` and `Dictionary<TKey,TValue>` work inside, and which collection you'd pick for a given scenario.

## 8.1 Array vs `List<T>`

**Simple words:**

- **Array (`T[]`)** — a fixed row of lockers. Size decided at creation, can't grow.
- **`List<T>`** — a row of lockers that **moves to a bigger row** when it's full. Internally it *is* an array (`_items`) plus a `Count`.

How `List<T>` grows: capacity starts at 0, becomes 4 on first `Add`, then **doubles** (4 → 8 → 16 → 32…). Each growth = allocate a new array + copy all items (O(n)), but because it doubles, `Add` is **amortized O(1)**.

```csharp
var list = new List<int>();
int lastCapacity = -1;
for (int i = 0; i < 20; i++)
{
    if (list.Capacity != lastCapacity)
    {
        Console.WriteLine($"Count={list.Count,2}  Capacity={list.Capacity}");
        lastCapacity = list.Capacity;
    }
    list.Add(i);
}
Console.WriteLine($"Count={list.Count,2}  Capacity={list.Capacity}");

var presized = new List<int>(1000);   // you know the size -> no resize + copy
Console.WriteLine($"Presized capacity: {presized.Capacity}");
list.TrimExcess();
Console.WriteLine($"After TrimExcess: {list.Capacity}");
```

Output:

```text
Count= 0  Capacity=0
Count= 1  Capacity=4
Count= 5  Capacity=8
Count= 9  Capacity=16
Count=17  Capacity=32
Count=20  Capacity=32
Presized capacity: 1000
After TrimExcess: 20
```

| | Array `T[]` | `List<T>` |
|---|---|---|
| Size | Fixed | Grows automatically |
| Memory | Exactly `n` items | `Capacity` ≥ `Count` (some spare) |
| Index access | O(1) | O(1) |
| Add at end | ❌ (must create a new array) | Amortized O(1) |
| Insert/remove in middle | ❌ manual | O(n) — shifts items |
| Multi-dimensional | ✅ `int[,]`, jagged `int[][]` | ❌ (use `List<List<T>>`) |
| Best for | Fixed-size data, hot loops, interop, `Span<T>` | Most everyday "list of things" |

💡 If you know roughly how many items you'll add, pass the **capacity** (`new List<T>(1000)`) to avoid repeated resize-and-copy.

## 8.2 `Dictionary<TKey, TValue>` — how it really works

**Real-life analogy:** a coat check. You hand in a coat (value) and get a numbered token (key). To find the coat, the attendant doesn't search all coats — they compute the hook number from your token and walk straight there.

Inside (simplified):

```text
Add("Asha", 29)
  1. hash = "Asha".GetHashCode()          e.g. 1_734_918_222
  2. bucket = hash % buckets.Length       e.g. 1_734_918_222 % 7 = 3
  3. store entry {hash, key, value, next} in the entries array;
     buckets[3] points to that entry

Lookup "Asha"
  1. same hash -> same bucket 3
  2. walk the (usually tiny) chain in bucket 3, comparing stored hash first,
     then key.Equals(...) -> found
```

- **Collision** = two different keys land in the same bucket. .NET uses **chaining**: entries in a bucket are linked via a `next` index. Lookups then compare several keys.
- **Resize**: when the entries array is full, the dictionary grows to the next **prime** size larger than 2× and **re-hashes** everything (O(n), but rare → amortized O(1) add).
- **Average** O(1) for add/lookup/remove; **worst case** O(n) if every key collides (bad `GetHashCode`).
- Strings use randomized hashing per process, so `"Asha".GetHashCode()` differs between runs — never persist hash codes.

```csharp
var ages = new Dictionary<string, int>
{
    ["Asha"] = 29,
    ["Bilal"] = 31
};

ages["Chen"] = 26;                               // indexer: add OR overwrite
Console.WriteLine(ages.TryAdd("Asha", 99));      // False — key exists, nothing changes
try { ages.Add("Asha", 99); }
catch (ArgumentException) { Console.WriteLine("Add throws on duplicate key"); }

try { _ = ages["Zed"]; }
catch (KeyNotFoundException) { Console.WriteLine("Indexer throws on missing key"); }

if (ages.TryGetValue("Bilal", out int bilalAge))  // ONE lookup instead of ContainsKey + indexer
    Console.WriteLine($"Bilal is {bilalAge}");

Console.WriteLine(ages.GetValueOrDefault("Zed", -1));

foreach (var (name, age) in ages)                 // KeyValuePair deconstruction
    Console.WriteLine($"{name}: {age}");
```

Output:

```text
False
Add throws on duplicate key
Indexer throws on missing key
Bilal is 31
-1
Asha: 29
Bilal: 31
Chen: 26
```

⚠️ **Interview trap:** `Dictionary` **does not guarantee order**. It *looks* like insertion order until you remove items and the free slots get reused. Need order? Use `SortedDictionary` (by key) or `OrderedDictionary<TKey,TValue>` (.NET 9+, insertion order).

⚠️ **Interview trap:** `if (dict.ContainsKey(k)) x = dict[k];` does **two** lookups. Use `TryGetValue` (one lookup).

## 8.3 `HashSet<T>`, `SortedDictionary`, `SortedList`, `SortedSet`

- **`HashSet<T>`** — a dictionary with only keys. "Have I seen this?" in O(1). Set algebra: `UnionWith`, `IntersectWith`, `ExceptWith`, `IsSubsetOf`.
- **`SortedDictionary<K,V>`** — a **red-black tree**; always sorted by key; O(log n) everything.
- **`SortedList<K,V>`** — two **sorted arrays** (keys, values); O(log n) lookup by binary search, but O(n) insert (shifting). Less memory; index access (`Keys[0]`).
- **`SortedSet<T>`** — sorted, unique; `Min`, `Max`, `GetViewBetween` for range queries.

```csharp
var sd = new SortedDictionary<string, int> { ["pear"] = 3, ["apple"] = 5, ["mango"] = 1 };
Console.WriteLine(string.Join(", ", sd));                     // always sorted by key

var sl = new SortedList<int, string> { [30] = "thirty", [10] = "ten", [20] = "twenty" };
Console.WriteLine($"{sl.Keys[0]} -> {sl.Values[0]}");         // index access (it's two sorted arrays)

var ss = new SortedSet<int> { 5, 1, 9, 1, 3 };
Console.WriteLine(string.Join(", ", ss));                     // sorted + unique
Console.WriteLine($"Min={ss.Min}, Max={ss.Max}, Between 2..6: {string.Join(",", ss.GetViewBetween(2, 6))}");

var hs = new HashSet<int> { 1, 2, 3 };
Console.WriteLine(hs.Add(2));                                 // False — already there
hs.IntersectWith([2, 3, 4]);                                  // set algebra, in place
Console.WriteLine(string.Join(", ", hs));
```

Output:

```text
[apple, 5], [mango, 1], [pear, 3]
10 -> ten
1, 3, 5, 9
Min=1, Max=9, Between 2..6: 3,5
False
2, 3
```

| | `SortedDictionary` | `SortedList` |
|---|---|---|
| Structure | Red-black tree | Two sorted arrays |
| Insert/remove | O(log n) | O(n) (shifting) |
| Lookup | O(log n) | O(log n) |
| Memory | More (node objects) | Less |
| Index access | ❌ | ✅ `Keys[i]`, `Values[i]` |
| Best when | Many inserts in random order | Build once / sorted input, then mostly read |

## 8.4 `Queue<T>`, `Stack<T>`, `LinkedList<T>`, `PriorityQueue<TElement,TPriority>`

| Collection | Analogy | Order | Key operations (all O(1) unless noted) |
|---|---|---|---|
| `Queue<T>` | Ticket counter line | FIFO | `Enqueue`, `Dequeue`, `Peek` |
| `Stack<T>` | Stack of plates / browser Back | LIFO | `Push`, `Pop`, `Peek` |
| `LinkedList<T>` | Train cars coupled together | Doubly linked | `AddFirst/Last/Before/After`, `Remove(node)`; `Find` is O(n); no index |
| `PriorityQueue<E,P>` (.NET 6+) | Hospital ER triage | Smallest priority first (min-heap) | `Enqueue`/`Dequeue` O(log n), `Peek` O(1) |

```csharp
var queue = new Queue<string>();           // FIFO — ticket counter
queue.Enqueue("A"); queue.Enqueue("B"); queue.Enqueue("C");
Console.WriteLine($"Dequeue: {queue.Dequeue()}, Peek: {queue.Peek()}, Count: {queue.Count}");

var stack = new Stack<string>();           // LIFO — stack of plates
stack.Push("A"); stack.Push("B"); stack.Push("C");
Console.WriteLine($"Pop: {stack.Pop()}, Peek: {stack.Peek()}, Count: {stack.Count}");

var linked = new LinkedList<int>([1, 2, 4]);
var node2 = linked.Find(2)!;               // Find is O(n)...
linked.AddAfter(node2, 3);                 // ...but insert next to a known node is O(1)
linked.AddFirst(0);
Console.WriteLine(string.Join(" <-> ", linked));

var pq = new PriorityQueue<string, int>(); // .NET 6+: SMALLEST priority value comes out first
pq.Enqueue("Low-priority email", 5);
pq.Enqueue("Production outage", 1);
pq.Enqueue("Code review", 3);
while (pq.TryDequeue(out var task, out var priority))
    Console.WriteLine($"[{priority}] {task}");
```

Output:

```text
Dequeue: A, Peek: B, Count: 2
Pop: C, Peek: B, Count: 2
0 <-> 1 <-> 2 <-> 3 <-> 4
[1] Production outage
[3] Code review
[5] Low-priority email
```

🧩 **Predict the output:**

```csharp
var stack = new Stack<int>([1, 2, 3]);
Console.WriteLine(string.Join(",", stack));

var queue = new Queue<int>([1, 2, 3]);
Console.WriteLine(string.Join(",", queue));
```

<details><summary>Answer</summary>

```text
3,2,1
1,2,3
```

The `Stack<T>(IEnumerable<T>)` constructor **pushes** 1, 2, 3 in order, so 3 is on top and enumeration goes top → bottom. The queue enumerates front → back.

</details>

⚠️ **Interview trap:** `PriorityQueue` is **not** stable (equal priorities come out in no guaranteed order), doesn't support "update priority", and its enumeration order is **not** sorted. For a max-heap, use `Comparer<int>.Create((a, b) => b.CompareTo(a))`.

## 8.5 Concurrent collections

**Simple words:** normal collections assume **one thread at a time**. Two threads calling `Dictionary.Add` together can corrupt its internal arrays (lost items, exceptions, even infinite loops). `System.Collections.Concurrent` types are safe to use from many threads without your own `lock`.

| Type | Use for |
|---|---|
| `ConcurrentDictionary<K,V>` | Shared caches, counters. Atomic `GetOrAdd`, `AddOrUpdate`, `TryRemove` |
| `ConcurrentQueue<T>` | Thread-safe FIFO (producer/consumer) |
| `ConcurrentStack<T>` | Thread-safe LIFO |
| `ConcurrentBag<T>` | Unordered; fastest when the same thread adds and takes |
| `BlockingCollection<T>` | Producer/consumer with blocking + bounded capacity (older; `System.Threading.Channels` is the modern choice) |

```csharp
using System.Collections.Concurrent;

string[] words = Enumerable.Repeat(new[] { "red", "green", "blue", "red" }, 10_000)
                           .SelectMany(w => w).ToArray();

var counts = new ConcurrentDictionary<string, int>();
Parallel.ForEach(words, w => counts.AddOrUpdate(w, 1, (_, old) => old + 1));  // atomic per key
foreach (var kv in counts.OrderBy(k => k.Key))
    Console.WriteLine($"{kv.Key}: {kv.Value}");

var cache = new ConcurrentDictionary<int, string>();
Console.WriteLine(cache.GetOrAdd(42, key => $"computed {key}"));

var bag = new ConcurrentBag<int>();
Parallel.For(0, 100, i => bag.Add(i));
Console.WriteLine($"Bag count: {bag.Count}");

var q = new ConcurrentQueue<int>();
q.Enqueue(1); q.Enqueue(2);
Console.WriteLine(q.TryDequeue(out int first) ? $"Dequeued {first}" : "empty");
```

Output:

```text
blue: 10000
green: 10000
red: 20000
computed 42
Bag count: 100
Dequeued 1
```

⚠️ **Interview trap:** `GetOrAdd(key, factory)` is thread-safe for the **dictionary**, but the `factory` **can run more than once** for the same key under contention (only one result is stored). If the factory is expensive or has side effects, store `Lazy<T>` values: `GetOrAdd(key, k => new Lazy<T>(() => Create(k))).Value`.

⚠️ **Interview trap:** `if (!cd.ContainsKey(k)) cd[k] = v;` is still a **race** (check-then-act). Use the atomic methods.

## 8.6 Immutable, frozen and read-only collections

| Kind | Example | Can *anyone* change it? | Notes |
|---|---|---|---|
| Read-only **view** | `IReadOnlyList<T>`, `list.AsReadOnly()` | Yes — the owner of the underlying list | Just hides mutating methods from the consumer |
| Immutable | `ImmutableList<T>`, `ImmutableDictionary` | No — "changes" return a **new** collection | Shares structure internally; thread-safe; good for snapshots/undo |
| Frozen (.NET 8+) | `FrozenDictionary`, `FrozenSet` | No | Expensive to build, **fastest** reads; ideal for config/lookup tables built at startup |

```csharp
using System.Collections.Frozen;
using System.Collections.Immutable;

var v1 = ImmutableList.Create(1, 2, 3);
var v2 = v1.Add(4);                               // returns a NEW list; v1 is untouched
Console.WriteLine($"v1: {string.Join(",", v1)} | v2: {string.Join(",", v2)}");

FrozenDictionary<string, string> countries = new Dictionary<string, string>
{
    ["IN"] = "India",
    ["US"] = "United States"
}.ToFrozenDictionary();                           // .NET 8+: slow to build, fastest to read
Console.WriteLine(countries["IN"]);

var backing = new List<int> { 1, 2 };
IReadOnlyList<int> view = backing.AsReadOnly();   // read-only VIEW, not a copy
backing.Add(3);
Console.WriteLine($"Read-only view sees: {string.Join(",", view)}");
```

Output:

```text
v1: 1,2,3 | v2: 1,2,3,4
India
Read-only view sees: 1,2,3
```

🎯 **Say this in the interview:** "I expose `IReadOnlyList<T>` or `IReadOnlyCollection<T>` from my APIs so callers can't mutate my internal state. If the data must never change — even by me — I use immutable collections, and for lookup tables built once at startup I use `FrozenDictionary` in .NET 8+."

## 8.7 The interface hierarchy

```text
IEnumerable<T>                 -> can be foreach'ed (GetEnumerator). That's all.
 ├─ IReadOnlyCollection<T>     -> + Count
 │   ├─ IReadOnlyList<T>       -> + this[int] (get only)
 │   └─ IReadOnlySet<T>        (.NET 5+)
 └─ ICollection<T>             -> + Count, Add, Remove, Clear, Contains, CopyTo
     ├─ IList<T>               -> + this[int] get/set, IndexOf, Insert, RemoveAt
     ├─ ISet<T>                -> + UnionWith, IntersectWith, ...
     └─ IDictionary<K,V>       (ICollection<KeyValuePair<K,V>>) -> + this[key], TryGetValue
```

`List<T>` implements all of `IList<T>`, `IReadOnlyList<T>`, `ICollection<T>`, `IEnumerable<T>`.

**Which one should a method accept / return?**

- **Accept** the *least* you need: only looping? take `IEnumerable<T>`. Need `Count` + index? `IReadOnlyList<T>`.
- **Return** something *useful but safe*: `IReadOnlyList<T>` / `IReadOnlyCollection<T>` (materialized), or `IEnumerable<T>` when it's lazy/streamed.

## 8.8 Big-O cheat sheet

| Operation | `T[]` | `List<T>` | `LinkedList<T>` | `Dictionary` / `HashSet` | `SortedDictionary` / `SortedSet` | `SortedList` | `Queue` / `Stack` | `PriorityQueue` |
|---|---|---|---|---|---|---|---|---|
| Access by index | O(1) | O(1) | O(n) | — | — | O(1) | — | — |
| Search by value/key | O(n) | O(n) | O(n) | **O(1)** avg | O(log n) | O(log n) | O(n) | O(n) |
| Add at end / insert | — | O(1)* | O(1) | O(1)* | O(log n) | O(n) | O(1)* | O(log n) |
| Insert/remove in middle | — | O(n) | O(1) with node | — | — | — | — | — |
| Remove by key/value | — | O(n) | O(n) (O(1) by node) | O(1) | O(log n) | O(n) | — | — |
| Min / Max | O(n) | O(n) | O(n) | O(n) | O(log n) | O(1) | — | O(1) peek |
| Kept sorted? | No | No | No | No | **Yes** | **Yes** | No | Heap order |

\* amortized — occasional O(n) resize.

## 8.9 Choosing the right collection

| Scenario | Pick | Why |
|---|---|---|
| Ordered list of items, read by index, append | `List<T>` | Default choice |
| Fixed-size buffer / hot numeric loop | `T[]` / `Span<T>` | No overhead |
| Look up customer by ID | `Dictionary<int, Customer>` | O(1) by key |
| "Have I already processed this ID?" | `HashSet<T>` | O(1) contains, no duplicates |
| Leaderboard always sorted by score | `SortedSet<T>` / `SortedDictionary` | Sorted + O(log n) updates |
| Background job processing in arrival order | `Queue<T>` / `Channel<T>` | FIFO |
| Undo/redo, expression parsing, DFS | `Stack<T>` | LIFO |
| Task scheduler by urgency, Dijkstra, top-K | `PriorityQueue<TElement,TPriority>` | Heap |
| LRU cache | `Dictionary` + `LinkedList` | O(1) lookup + O(1) move-to-front |
| Cache shared by many threads | `ConcurrentDictionary` | Atomic ops |
| Country codes loaded once at startup | `FrozenDictionary` | Fastest reads |
| Return data that callers must not modify | `IReadOnlyList<T>` / immutable | Encapsulation |
| One key → many values | `ILookup` / `Dictionary<K, List<V>>` | Grouping |

## 8.10 Trap: modifying a collection while iterating

```csharp
var nums = new List<int> { 1, 2, 3, 4, 5, 6 };
try
{
    foreach (var n in nums)
        if (n % 2 == 0) nums.Remove(n);
}
catch (InvalidOperationException ex) { Console.WriteLine(ex.Message); }

nums = [1, 2, 3, 4, 5, 6];
nums.RemoveAll(n => n % 2 == 0);                  // ✅ fix 1: built-in, O(n)
Console.WriteLine(string.Join(",", nums));

nums = [1, 2, 3, 4, 5, 6];
for (int i = nums.Count - 1; i >= 0; i--)         // ✅ fix 2: index loop, backwards
    if (nums[i] % 2 == 0) nums.RemoveAt(i);
Console.WriteLine(string.Join(",", nums));

nums = [1, 2, 3, 4, 5, 6];
foreach (var n in nums.ToList())                  // ✅ fix 3: iterate over a copy
    if (n % 2 == 0) nums.Remove(n);
Console.WriteLine(string.Join(",", nums));

var dict = new Dictionary<string, int> { ["a"] = 1, ["b"] = 2, ["c"] = 3 };
foreach (var key in dict.Keys)
    if (key == "b") dict.Remove(key);             // allowed for Dictionary since .NET Core 3.0
Console.WriteLine(string.Join(",", dict.Keys));
```

Output:

```text
Collection was modified; enumeration operation may not execute.
1,3,5
1,3,5
1,3,5
a,c
```

**Why:** `List<T>` keeps a private `_version` counter that every `Add`/`Remove` increments. The enumerator remembers the version when it started and throws if it changes. (Since .NET Core 3.0, `Dictionary.Remove` and overwriting an existing key's value during enumeration are explicitly allowed; adding new keys still throws.)

## 8.11 `Span<T>`, `Memory<T>` and `ArrayPool<T>` (intro)

**Simple words:** `Span<T>` is a **window** onto memory you already have (an array, part of a string, stack memory). Slicing it creates **no copies and no garbage**. Think of a magnifying glass over part of a page — you can read (and write) through it, but you didn't photocopy the page.

- `Span<T>` / `ReadOnlySpan<T>` — a `ref struct`: lives **only on the stack**, so it can't be a class field, can't be boxed, can't cross `await`.
- `Memory<T>` — the heap-friendly cousin; store it in fields or use across `await`, then call `.Span` to work with it.
- `ArrayPool<T>.Shared` — **borrow** a large array and **return** it, instead of allocating a new one per request.

```csharp
using System.Buffers;

int[] numbers = [10, 20, 30, 40, 50];
Span<int> middle = numbers.AsSpan(1, 3);    // a window over the array — no copy
middle[0] = 99;                             // writes into the ORIGINAL array
Console.WriteLine(string.Join(",", numbers));

ReadOnlySpan<char> date = "2024-11-26";
int year = int.Parse(date[..4]);            // slice a string without Substring allocations
int month = int.Parse(date.Slice(5, 2));
Console.WriteLine($"Year={year}, Month={month}");

Span<byte> buffer = stackalloc byte[4];     // memory on the stack — no GC at all
buffer.Fill(7);
Console.WriteLine(string.Join(",", buffer.ToArray()));

byte[] rented = ArrayPool<byte>.Shared.Rent(1000);   // borrow a big buffer
try { Console.WriteLine($"Asked for 1000, got at least 1000: {rented.Length >= 1000}"); }
finally { ArrayPool<byte>.Shared.Return(rented); }   // ...and give it back

Memory<int> mem = numbers.AsMemory(2);      // Memory<T> may live on the heap (fields, async)
Console.WriteLine(mem.Span[0]);
```

Output:

```text
10,99,30,40,50
Year=2024, Month=11
7,7,7,7
Asked for 1000, got at least 1000: True
30
```

⚠️ **Interview trap:** `ArrayPool.Rent(1000)` may return a **bigger** array (e.g. 1024) that may contain **old data**. Always track your own length and never use the array after `Return`.

## Try it yourself

1. Check if a string of brackets like `"{[()]}"` is balanced. Which collection?
2. Return the **top K most frequent** numbers in an array.
3. Implement an **LRU cache** with O(1) `Get` and `Put`.
4. Remove duplicates from `[3, 1, 3, 2, 1, 5]` **keeping the first-seen order**.
5. When can a `Dictionary` lookup become O(n)?

<details><summary>Answer</summary>

**1.** A `Stack<char>`: push openers, pop and match on closers.

```csharp
foreach (var s in new[] { "{[()]}", "([)]", "((", "" })
    Console.WriteLine($"\"{s}\" -> {IsBalanced(s)}");

static bool IsBalanced(string s)
{
    var pairs = new Dictionary<char, char> { [')'] = '(', [']'] = '[', ['}'] = '{' };
    var stack = new Stack<char>();
    foreach (char c in s)
    {
        if (c is '(' or '[' or '{') stack.Push(c);
        else if (pairs.TryGetValue(c, out char open))
        {
            if (stack.Count == 0 || stack.Pop() != open) return false;
        }
    }
    return stack.Count == 0;
}
```

```text
"{[()]}" -> True
"([)]" -> False
"((" -> False
"" -> True
```

**2.** Count with a `Dictionary`, keep a **min-heap of size K** with `PriorityQueue` → O(n log k).

```csharp
int[] nums = [1, 1, 1, 2, 2, 3, 4, 4, 4, 4];
Console.WriteLine(string.Join(",", TopK(nums, 2)));

static List<int> TopK(int[] nums, int k)
{
    var freq = new Dictionary<int, int>();
    foreach (var n in nums) freq[n] = freq.GetValueOrDefault(n) + 1;

    var heap = new PriorityQueue<int, int>();     // min-heap keyed by frequency, size <= k
    foreach (var (num, count) in freq)
    {
        heap.Enqueue(num, count);
        if (heap.Count > k) heap.Dequeue();       // throw away the least frequent
    }

    var result = new List<int>();
    while (heap.Count > 0) result.Add(heap.Dequeue());
    result.Reverse();                             // most frequent first
    return result;
}
```

```text
4,1
```

**3.** `Dictionary<key, LinkedListNode>` for O(1) lookup + `LinkedList` to keep recency order.

```csharp
var lru = new LruCache<string, int>(capacity: 2);
lru.Put("a", 1);
lru.Put("b", 2);
lru.Get("a");            // "a" becomes most recently used
lru.Put("c", 3);         // full -> evicts least recently used = "b"
Console.WriteLine($"{lru.Get("a")} {lru.Get("b")} {lru.Get("c")}");

class LruCache<TKey, TValue>(int capacity) where TKey : notnull
{
    private readonly Dictionary<TKey, LinkedListNode<(TKey Key, TValue Value)>> _map = new();
    private readonly LinkedList<(TKey Key, TValue Value)> _order = new();   // front = most recent

    public TValue? Get(TKey key)
    {
        if (!_map.TryGetValue(key, out var node)) return default;
        _order.Remove(node);        // O(1): we hold the node
        _order.AddFirst(node);
        return node.Value.Value;
    }

    public void Put(TKey key, TValue value)
    {
        if (_map.TryGetValue(key, out var existing)) _order.Remove(existing);
        else if (_map.Count == capacity)
        {
            var oldest = _order.Last!;
            _order.RemoveLast();
            _map.Remove(oldest.Value.Key);
        }
        _map[key] = _order.AddFirst((key, value));
    }
}
```

```text
1 0 3
```

(`Get("b")` returns `0` — `default(int)` — because `TValue?` on an unconstrained generic is not `Nullable<int>`; see 7.4.)

**4.** `HashSet.Add` returns `false` for an item it already has:

```csharp
int[] input = [3, 1, 3, 2, 1, 5];
var seen = new HashSet<int>();
var result = new List<int>();
foreach (var n in input)
    if (seen.Add(n)) result.Add(n);      // Add returns false for duplicates
Console.WriteLine(string.Join(",", result));
Console.WriteLine(string.Join(",", input.Distinct()));
```

```text
3,1,2,5
3,1,2,5
```

**5.** When many keys produce the **same hash code** (bad `GetHashCode`, e.g. returns a constant, or an attacker crafting colliding keys). Then all keys share one bucket and each lookup scans the chain → O(n).

</details>

## Quick revision

- `List<T>` = array + count; capacity 0 → 4 → 8 → 16 (doubling); `Add` amortized O(1).
- Pre-size lists/dictionaries when you know the count.
- `Dictionary`: hash → bucket → chain; O(1) average, O(n) worst; resizes to a prime ≥ 2×.
- `Dictionary` order is not guaranteed; use `TryGetValue` instead of `ContainsKey` + indexer.
- `HashSet<T>` for uniqueness / fast "contains".
- `SortedDictionary` = tree (fast inserts); `SortedList` = arrays (less memory, index access).
- `Queue` FIFO, `Stack` LIFO, `PriorityQueue` min-heap (.NET 6+), `LinkedList` O(1) insert at a node.
- Use `Concurrent*` collections across threads; `GetOrAdd`'s factory may run twice.
- `IReadOnlyList<T>` is a view; `ImmutableList<T>` never changes; `FrozenDictionary` (.NET 8) = fastest reads.
- Accept `IEnumerable<T>`, return `IReadOnlyList<T>` (or `IEnumerable<T>` if streaming).
- Removing from a `List` inside `foreach` throws — use `RemoveAll`, reverse `for`, or iterate a copy.
- `Span<T>` = zero-copy window, stack-only; `Memory<T>` for heap/async; `ArrayPool` to reuse buffers.

# 9. Delegates, Events & Lambdas

> **Why interviewers ask this:** Delegates and lambdas power LINQ, ASP.NET Core middleware, callbacks, `Task.Run`, and every UI/event system. Interviewers love the closure-in-a-loop puzzle, "event vs delegate", and "how can events cause memory leaks?".

## 9.1 What is a delegate?

**Simple words:** a delegate is a **type-safe variable that holds a method**. You can store it, pass it to another method, and call it later.

**Real-life analogy:** a **TV remote**. The remote (delegate) doesn't know how the TV works — it just holds "the thing to call when the button is pressed". You can reprogram it for a different TV (re-assign it), as long as the new TV accepts the same signals (same **signature**). Unlike a C function pointer, the compiler checks that the signature matches — hence *type-safe*.

```csharp
MathOp op = Add;                    // point the delegate at a method
Console.WriteLine(op(3, 4));
op = Multiply;                      // re-point it at another method
Console.WriteLine(op.Invoke(3, 4)); // op(...) is shorthand for op.Invoke(...)

Console.WriteLine(Apply(10, 5, Add));              // pass a method as an argument
Console.WriteLine(Apply(10, 5, (a, b) => a - b));  // or a lambda

MathOp all = Add;
all += Multiply;                    // multicast: invocation list now has 2 methods
Console.WriteLine($"Multicast returns: {all(2, 5)}");
Console.WriteLine($"Methods in list: {all.GetInvocationList().Length}");

static int Add(int a, int b) { Console.WriteLine("  Add called"); return a + b; }
static int Multiply(int a, int b) { Console.WriteLine("  Multiply called"); return a * b; }
static int Apply(int a, int b, MathOp operation) => operation(a, b);

delegate int MathOp(int a, int b);   // the "shape" of method it can point to
```

Output:

```text
  Add called
7
  Multiply called
12
  Add called
15
5
  Add called
  Multiply called
Multicast returns: 10
Methods in list: 2
```

Key facts:

- A delegate declaration (`delegate int MathOp(int a, int b);`) creates a **class** deriving from `System.MulticastDelegate`.
- `+=` builds an **invocation list** (multicast). Calling it calls **every** method in order.
- For a multicast delegate with a return value, you get **only the last method's return value**; the others are lost. Use `GetInvocationList()` if you need each result.
- If one handler in the list throws, the remaining handlers **don't run**.

## 9.2 `Func`, `Action`, `Predicate` — built-in delegate types

You almost never need to declare your own delegate type anymore.

| Type | Signature | Returns | Example |
|---|---|---|---|
| `Action` | `() => void` | nothing | `Action log = () => Console.WriteLine("hi");` |
| `Action<T1..T16>` | `(T1, …) => void` | nothing | `Action<string> greet = n => …;` |
| `Func<TResult>` | `() => TResult` | `TResult` | `Func<int> dice = () => 4;` |
| `Func<T1..T16, TResult>` | `(T1, …) => TResult` | **last** type argument | `Func<int, int, int> add = (a, b) => a + b;` |
| `Predicate<T>` | `T => bool` | `bool` | `Predicate<int> isEven = n => n % 2 == 0;` (used by `List.FindAll`, `RemoveAll`, `Exists`) |

```csharp
Func<int, int, int> add = (a, b) => a + b;                           // LAST type arg = return type
Action<string> greet = name => Console.WriteLine($"Hello, {name}!"); // returns void
Predicate<int> isEven = n => n % 2 == 0;                             // returns bool
Func<int> dice = () => 4;                                            // no input, returns int

greet("Asha");
Console.WriteLine(add(2, 3));
Console.WriteLine(isEven(10));
Console.WriteLine(dice());

var evens = new List<int> { 1, 2, 3, 4 }.FindAll(isEven);   // List<T> APIs use Predicate<T>
Console.WriteLine(string.Join(",", evens));

Func<int, int> square1 = delegate (int x) { return x * x; }; // anonymous method (C# 2)
Func<int, int> square2 = x => x * x;                          // lambda (C# 3+)
Console.WriteLine($"{square1(5)} {square2(5)}");

var parse = (string s) => int.Parse(s);   // C# 10+: natural type -> Func<string, int>
Console.WriteLine(parse("42") + 1);

Func<int, int> twice = static x => x * 2; // C# 9+: static lambda can't capture variables
Console.WriteLine(twice(21));
```

Output:

```text
Hello, Asha!
5
True
4
2,4
25 25
43
42
```

⚠️ **Interview trap:** `Predicate<int>` and `Func<int, bool>` have the same shape but are **different types** — you can't assign one to the other directly (see exercise 4).

## 9.3 Closures and the captured loop variable trap

**Simple words:** when a lambda uses a variable from outside (`counter`, `i`), it **captures the variable itself, not a copy of its value**. The compiler moves that variable into a hidden class ("display class") so it can live as long as the lambda does.

```csharp
var actions = new List<Action>();

for (int i = 0; i < 3; i++)
    actions.Add(() => Console.Write($"{i} "));    // all lambdas share ONE variable i
foreach (var a in actions) a();
Console.WriteLine(" <- for loop");

actions.Clear();
foreach (var n in new[] { 0, 1, 2 })
    actions.Add(() => Console.Write($"{n} "));    // C# 5+: a fresh n per iteration
foreach (var a in actions) a();
Console.WriteLine(" <- foreach loop");

actions.Clear();
for (int i = 0; i < 3; i++)
{
    int copy = i;                                 // ✅ fix: new variable each iteration
    actions.Add(() => Console.Write($"{copy} "));
}
foreach (var a in actions) a();
Console.WriteLine(" <- for loop with copy");

int counter = 0;
Func<int> next = () => ++counter;                 // closure captures the VARIABLE, not the value
next(); next();
Console.WriteLine($"counter = {counter}");
```

Output:

```text
3 3 3  <- for loop
0 1 2  <- foreach loop
0 1 2  <- for loop with copy
counter = 2
```

Why `3 3 3`? A `for` loop declares **one** `i` for the whole loop. All three lambdas share it, and by the time they run, the loop has finished and `i == 3`.

Since **C# 5**, `foreach` creates a **new** iteration variable for each pass, so each lambda captures its own `n` → `0 1 2`. (Before C# 5, `foreach` also printed `2 2 2` — a famous breaking change.) `for` was **not** changed.

⚠️ **Interview trap:** The same bug appears with `Task.Run(() => Process(i))` inside a `for` loop — tasks may all see the final value. Copy to a local first.

🎯 **Say this in the interview:** "Lambdas capture variables, not values. A `for` loop has one shared variable, so closures created in it all see the final value; `foreach` gets a fresh variable per iteration since C# 5. The fix is to copy the loop variable into a local inside the loop body."

## 9.4 Events — delegates with a safety cover

**Simple words:** an **event** is a delegate field wrapped so that **outsiders can only subscribe (`+=`) or unsubscribe (`-=`)**. Only the owning class can **raise** it or reset it.

**Real-life analogy:** a **YouTube channel**. Viewers can *subscribe* and *unsubscribe*. Only the channel owner can *publish* a video (raise the event). With a plain public delegate, any viewer could publish videos in your name or wipe the subscriber list (`= null`).

**Publisher / subscriber example** with the standard `EventHandler<TEventArgs>` pattern:

```csharp
var shop = new OrderService();
var email = new EmailNotifier();
var stock = new InventoryUpdater();

shop.OrderPlaced += email.OnOrderPlaced;          // subscribe
shop.OrderPlaced += stock.OnOrderPlaced;
shop.OrderPlaced += (_, e) => Console.WriteLine($"  [log]   order {e.OrderId} total {e.Total}");

shop.PlaceOrder(101, 250m);

shop.OrderPlaced -= email.OnOrderPlaced;          // unsubscribe
shop.PlaceOrder(102, 75m);

public class OrderPlacedEventArgs(int orderId, decimal total) : EventArgs
{
    public int OrderId { get; } = orderId;
    public decimal Total { get; } = total;
}

public class OrderService                          // the PUBLISHER
{
    public event EventHandler<OrderPlacedEventArgs>? OrderPlaced;

    public void PlaceOrder(int id, decimal total)
    {
        Console.WriteLine($"Placing order {id}...");
        OnOrderPlaced(new OrderPlacedEventArgs(id, total));
    }

    // Convention: protected virtual On<EventName> so derived classes can hook in
    protected virtual void OnOrderPlaced(OrderPlacedEventArgs e) => OrderPlaced?.Invoke(this, e);
}

public class EmailNotifier                         // SUBSCRIBERS
{
    public void OnOrderPlaced(object? sender, OrderPlacedEventArgs e) =>
        Console.WriteLine($"  [email] confirmation for order {e.OrderId}");
}

public class InventoryUpdater
{
    public void OnOrderPlaced(object? sender, OrderPlacedEventArgs e) =>
        Console.WriteLine($"  [stock] reserving items for order {e.OrderId}");
}
```

Output:

```text
Placing order 101...
  [email] confirmation for order 101
  [stock] reserving items for order 101
  [log]   order 101 total 250
Placing order 102...
  [stock] reserving items for order 102
  [log]   order 102 total 75
```

What the `event` keyword blocks from outside the class:

```csharp
var p = new Publisher();
p.Changed?.Invoke(p, EventArgs.Empty);  // ❌ does not compile: CS0070
p.Changed = null;                       // ❌ does not compile: CS0070

class Publisher { public event EventHandler? Changed; }
```

| | Public delegate field | `event` |
|---|---|---|
| Outsider `+=` / `-=` | ✅ | ✅ |
| Outsider invokes it | ✅ 😬 | ❌ CS0070 |
| Outsider assigns `= null` / `= handler` (wipes others) | ✅ 😬 | ❌ CS0070 |
| Can be in an interface | As a property | ✅ |
| Custom `add`/`remove` accessors | ❌ | ✅ |

Conventions (what reviewers expect):

- Signature `void Handler(object? sender, TEventArgs e)`; use `EventHandler` or `EventHandler<TEventArgs>`.
- Raise via `protected virtual void OnXxx(...)` so subclasses can extend.
- Use `?.Invoke(...)` — copies the delegate to a temp first, so it's thread-safe against someone unsubscribing between the null check and the call.
- Event names: `OrderPlaced` (past) / `Closing` (before) — not `OnOrderPlaced`.

## 9.5 Memory leaks from events

**The problem:** `publisher.Event += subscriber.Handler` stores a reference **to the subscriber** inside the publisher's invocation list. If the publisher lives long (a singleton, a static event, the main window) and the subscriber is "closed" but never unsubscribes, the GC **can't collect it** — the publisher still references it.

```csharp
using System.Runtime.CompilerServices;

var publisher = new Publisher();     // long-lived (e.g. a singleton service)

WeakReference leaky = CreateScreen(publisher, unsubscribe: false);
WeakReference clean = CreateScreen(publisher, unsubscribe: true);

GC.Collect();
GC.WaitForPendingFinalizers();
GC.Collect();

Console.WriteLine($"Forgot to unsubscribe -> still alive? {leaky.IsAlive}");
Console.WriteLine($"Unsubscribed          -> still alive? {clean.IsAlive}");
publisher.Raise();                   // the "closed" leaky screen still reacts!

[MethodImpl(MethodImplOptions.NoInlining)]
static WeakReference CreateScreen(Publisher p, bool unsubscribe)
{
    var screen = new Screen(unsubscribe ? "clean" : "leaky");
    p.SomethingHappened += screen.OnSomething;      // publisher now holds a reference to screen
    if (unsubscribe) p.SomethingHappened -= screen.OnSomething;
    return new WeakReference(screen);               // "screen closed" — we drop our reference
}

class Publisher
{
    public event EventHandler? SomethingHappened;
    public void Raise() => SomethingHappened?.Invoke(this, EventArgs.Empty);
}

class Screen(string name)
{
    public void OnSomething(object? s, EventArgs e) => Console.WriteLine($"{name} screen got the event");
}
```

Output:

```text
Forgot to unsubscribe -> still alive? True
Unsubscribed          -> still alive? False
leaky screen got the event
```

Fixes:

1. **Unsubscribe** (`-=`) when the subscriber is done — typically in `Dispose()`.
2. Make the subscriber's lifetime ≤ the publisher's.
3. Weak event patterns (`WeakReference`, WPF's `WeakEventManager`) when you can't control lifetimes.
4. Avoid **static** events, or be extra careful with them — they live forever.

⚠️ **Interview trap:** You **can't unsubscribe an anonymous lambda** unless you kept a reference to it: `x.Evt -= (s, e) => …` creates a *new* delegate that doesn't match. Store it in a variable first.

🧩 **Predict the output:**

```csharp
Func<int> f = () => 1;
f += () => 2;
f += () => 3;
Console.WriteLine(f());

f -= () => 3;     // removes...?
Console.WriteLine(f.GetInvocationList().Length);
```

<details><summary>Answer</summary>

```text
3
3
```

`f()` returns `3` (last method's return value). The `-=` uses a **different** lambda instance (a different compiler-generated method), so nothing is removed and the list still has 3 entries.

</details>

## 9.6 Lambdas vs expression trees (`Expression<Func<...>>`)

The **same lambda text** can become two very different things depending on the target type:

- `Func<int, bool>` → compiled **code** (IL). You can only run it.
- `Expression<Func<int, bool>>` → a **data structure** (a tree of nodes: parameter, `>=`, constant) describing the code. You can inspect it, rewrite it, translate it into another language (SQL!), or compile it at run time.

**Real-life analogy:** `Func` is a **cooked dish** — you can eat it. `Expression` is the **recipe** — you can read it, translate it to French, or cook it later.

```csharp
using System.Linq.Expressions;

Func<int, bool> isAdultFunc = age => age >= 18;               // compiled code (IL)
Expression<Func<int, bool>> isAdultExpr = age => age >= 18;   // DATA: a tree describing the code

Console.WriteLine(isAdultFunc(20));
Console.WriteLine(isAdultExpr);                               // the code, as text!
var body = (BinaryExpression)isAdultExpr.Body;
Console.WriteLine($"NodeType={body.NodeType}, Left={body.Left}, Right={body.Right}");

Func<int, bool> compiled = isAdultExpr.Compile();             // tree -> runnable delegate
Console.WriteLine(compiled(15));

// Build a tree by hand: x => x * 2 + 1
var x = Expression.Parameter(typeof(int), "x");
var tree = Expression.Lambda<Func<int, int>>(
    Expression.Add(Expression.Multiply(x, Expression.Constant(2)), Expression.Constant(1)), x);
Console.WriteLine($"{tree}  => f(5) = {tree.Compile()(5)}");
```

Output:

```text
True
age => (age >= 18)
NodeType=GreaterThanOrEqual, Left=age, Right=18
False
x => ((x * 2) + 1)  => f(5) = 11
```

This is how **EF Core** works: `db.Users.Where(u => u.Age >= 18)` takes an `Expression<Func<User, bool>>`, walks the tree and generates `WHERE [u].[Age] >= 18`. A `Func` would be a black box it couldn't translate.

Limitations — only *expression* lambdas can become trees:

```csharp
using System.Linq.Expressions;

Expression<Func<int, bool>> bad = age => { return age >= 18; };  // ❌ does not compile: CS0834
```

| | Lambda → `Func<...>` | Lambda → `Expression<Func<...>>` |
|---|---|---|
| What you get | Executable delegate | Data (tree of `Expression` nodes) |
| Run it | `f(x)` | `.Compile()` first (slow — cache it) |
| Inspect/translate | ❌ | ✅ |
| Statement bodies `{ ... }` | ✅ | ❌ CS0834 |
| Used by | LINQ to Objects (`IEnumerable`) | LINQ providers (`IQueryable`: EF Core), mocking (Moq `Setup`), validation libs |

🎯 **Say this in the interview:** "A lambda assigned to `Func` compiles to code; assigned to `Expression<Func>` it compiles to an expression tree — an object model of the code. `IQueryable` providers like EF Core read that tree and translate it to SQL, which is why some C# methods can't be used inside EF queries."

## Try it yourself

1. Write a generic `Retry<T>(Func<T> action, int maxAttempts, Action<Exception>? onError)` helper.
2. Fix the bug: `for (int i = 0; i < 5; i++) tasks.Add(Task.Run(() => Save(i)));`
3. Build a `Thermometer` class with a `TemperatureChanged` event that fires only when the change is more than 1 degree.
4. Can you assign a `Predicate<int>` to a `Func<int, bool>`? How do you convert?
5. Write `Compose(f, g)` that returns a function computing `g(f(x))`.

<details><summary>Answer</summary>

**1.**

```csharp
int attempts = 0;
var result = Retry(() =>
{
    attempts++;
    if (attempts < 3) throw new TimeoutException($"attempt {attempts} failed");
    return "success";
}, maxAttempts: 5, onError: ex => Console.WriteLine($"  {ex.Message}"));
Console.WriteLine($"{result} after {attempts} attempts");

static T Retry<T>(Func<T> action, int maxAttempts, Action<Exception>? onError = null)
{
    for (int i = 1; ; i++)
    {
        try { return action(); }
        catch (Exception ex) when (i < maxAttempts)   // last attempt: let it bubble up
        {
            onError?.Invoke(ex);
        }
    }
}
```

```text
  attempt 1 failed
  attempt 2 failed
success after 3 attempts
```

**2.** Copy the loop variable: `for (int i = 0; i < 5; i++) { int id = i; tasks.Add(Task.Run(() => Save(id))); }` — or use `foreach (var i in Enumerable.Range(0, 5))`.

**3.**

```csharp
var t = new Thermometer();
t.TemperatureChanged += (_, e) => Console.WriteLine($"Changed {e.Old} -> {e.New}");
foreach (var reading in new[] { 20.0, 20.5, 22.0, 22.4, 19.0 })
    t.Report(reading);

public record TemperatureChangedEventArgs(double Old, double New);  // EventArgs base is optional

public class Thermometer
{
    private double? _last;
    public event EventHandler<TemperatureChangedEventArgs>? TemperatureChanged;

    public void Report(double value)
    {
        if (_last is double old)
        {
            if (Math.Abs(value - old) <= 1) return;             // ignore small changes
            TemperatureChanged?.Invoke(this, new(old, value));
        }
        _last = value;
    }
}
```

```text
Changed 20 -> 22
Changed 22 -> 19
```

**4.** Not directly — different delegate types, even with identical signatures:

```csharp
Predicate<int> isEven = n => n % 2 == 0;
Func<int, bool> f = isEven;   // ❌ does not compile: CS0029
```

Convert via the `Invoke` method group or by wrapping:

```csharp
Predicate<int> isEven = n => n % 2 == 0;
Func<int, bool> f1 = isEven.Invoke;                 // method group conversion
Func<int, bool> f2 = new Func<int, bool>(isEven);   // wrap it
Console.WriteLine($"{f1(4)} {f2(5)}");
```

```text
True False
```

**5.**

```csharp
Func<int, int> addOne = x => x + 1;
Func<int, int> square = x => x * x;
var addThenSquare = Compose(addOne, square);
Console.WriteLine(addThenSquare(3));   // (3 + 1)^2

static Func<T, TResult> Compose<T, TMid, TResult>(Func<T, TMid> first, Func<TMid, TResult> second)
    => x => second(first(x));
```

```text
16
```

</details>

## Quick revision

- Delegate = type-safe reference to a method (or several); it's a class deriving from `MulticastDelegate`.
- `Func<…, TResult>` returns (last type arg); `Action<…>` returns void; `Predicate<T>` returns bool.
- Multicast: all handlers run in order; you get only the **last** return value; an exception stops the rest.
- Lambdas capture **variables**, not values (closures live in a compiler-generated class).
- `for` loop shares one variable → `3 3 3`; `foreach` gets a fresh one per iteration since C# 5.
- `static` lambdas (C# 9) can't capture → no hidden allocations.
- `event` = delegate that outsiders can only `+=`/`-=`; only the owner can invoke or reset it (CS0070).
- Standard pattern: `EventHandler<TEventArgs>`, `protected virtual OnXxx`, `?.Invoke(this, e)`.
- Forgetting `-=` on a long-lived publisher keeps subscribers alive → memory leak.
- You can't unsubscribe an inline lambda you didn't store.
- `Expression<Func<>>` = code as data; EF Core translates it to SQL; statement lambdas aren't allowed (CS0834).

# 10. LINQ in Depth

> **Why interviewers ask this:** LINQ is used in virtually every C# codebase, and live-coding rounds are full of "group employees by department", "second highest salary", "left join". Interviewers also probe the *concepts* — deferred execution, multiple enumeration, `IEnumerable` vs `IQueryable` — because misunderstanding them causes real production bugs (N+1 queries, loading whole tables into memory).

## 10.0 Sample data used in this whole section

Add this file to your console project **once** (e.g. `SampleData.cs`). The `global using static` line lets every snippet below simply write `Employees` and `Departments`.

```csharp
global using static SampleData;

public record Employee(int Id, string Name, string Dept, decimal Salary, DateTime JoinDate);
public record Department(string Name, string Location);

public static class SampleData
{
    public static readonly List<Employee> Employees =
    [
        new(1,  "Asha",  "Engineering", 120_000m, new DateTime(2021, 3, 15)),
        new(2,  "Bilal", "Engineering",  95_000m, new DateTime(2022, 7, 1)),
        new(3,  "Chen",  "Sales",        70_000m, new DateTime(2020, 1, 10)),
        new(4,  "Diya",  "Engineering", 120_000m, new DateTime(2023, 2, 20)),
        new(5,  "Ethan", "HR",           60_000m, new DateTime(2021, 11, 5)),
        new(6,  "Farah", "Sales",        85_000m, new DateTime(2022, 7, 18)),
        new(7,  "Gopal", "Engineering", 105_000m, new DateTime(2024, 1, 8)),
        new(8,  "Hana",  "HR",           65_000m, new DateTime(2023, 9, 30)),
        new(9,  "Ivan",  "Sales",        70_000m, new DateTime(2024, 3, 12)),
        new(10, "Asha",  "Sales",        72_000m, new DateTime(2022, 2, 14)),   // same name, different person
    ];

    public static readonly List<Department> Departments =
    [
        new("Engineering", "Bengaluru"),
        new("Sales",       "Mumbai"),
        new("HR",          "Pune"),
        new("Marketing",   "Delhi"),   // no employees -> useful for LEFT JOIN
    ];
}
```

## 10.1 What is LINQ?

**Simple words:** LINQ (**L**anguage **IN**tegrated **Q**uery) lets you **query any collection with the same set of methods** — filter, sort, group, join — like SQL, but in C#, type-checked by the compiler.

**Real-life analogy:** a **universal remote** that works with any TV brand. The same `Where`/`Select`/`OrderBy` buttons work on a `List`, an array, XML, a database (EF Core), etc.

Under the hood, LINQ is just **extension methods** on `IEnumerable<T>` (in `System.Linq.Enumerable`) and `IQueryable<T>` (in `System.Linq.Queryable`) that take **lambdas**.

### Query syntax vs method syntax

```csharp
// Method syntax (most common in real code)
var richMethod = Employees
    .Where(e => e.Salary >= 100_000)
    .OrderBy(e => e.Name)
    .Select(e => e.Name);

// Query syntax (SQL-like) — the compiler turns it into the SAME method calls
var richQuery = from e in Employees
                where e.Salary >= 100_000
                orderby e.Name
                select e.Name;

Console.WriteLine(string.Join(", ", richMethod));
Console.WriteLine(string.Join(", ", richQuery));
```

Output:

```text
Asha, Diya, Gopal
Asha, Diya, Gopal
```

| | Query syntax | Method syntax |
|---|---|---|
| Looks like | SQL (`from … where … select`) | Chained method calls |
| Coverage | Subset (`where, select, orderby, group, join, let, into`) | **Everything** (`Take`, `Distinct`, `Count`, `Chunk`, …) |
| Readability wins | Joins, `let`, multiple `from` | Most other cases |
| Compiles to | Method syntax | — |

You can mix them: `(from e in Employees where … select e).Count()`.

## 10.2 Deferred vs immediate execution

**Simple words:** most LINQ operators **don't do anything when you write them**. They build a *recipe*. The work happens only when you **enumerate** the result (`foreach`, `ToList()`, `Count()`, `First()`…) — and it happens **again every time** you enumerate.

**Real-life analogy:** a **saved Google search**. Saving the search doesn't fetch anything. Every time you click it, it searches *the current* web — new pages show up.

```csharp
var numbers = new List<int> { 1, 2, 3 };

var query = numbers.Where(n =>
{
    Console.WriteLine($"  checking {n}");
    return n > 1;
});
Console.WriteLine("Query defined (nothing has run yet)");

numbers.Add(4);                         // change the source AFTER defining the query

Console.WriteLine("First enumeration:");
foreach (var n in query) Console.WriteLine($"  -> {n}");

Console.WriteLine("Count() runs the whole pipeline AGAIN:");
Console.WriteLine($"  count = {query.Count()}");

var live = numbers.Where(n => n > 1);
var snapshot = live.ToList();           // immediate execution: runs now, stores the results
numbers.Add(5);
Console.WriteLine($"Snapshot has {snapshot.Count} items; live query now has {live.Count()}");
```

Output:

```text
Query defined (nothing has run yet)
First enumeration:
  checking 1
  checking 2
  -> 2
  checking 3
  -> 3
  checking 4
  -> 4
Count() runs the whole pipeline AGAIN:
  checking 1
  checking 2
  checking 3
  checking 4
  count = 3
Snapshot has 3 items; live query now has 4
```

Notice three things: (1) nothing ran when the query was defined, (2) `4` was included even though it was added *after* the query was defined, (3) `Count()` re-ran the predicate for every element.

| Deferred (lazy, returns a sequence) | Immediate (runs now) |
|---|---|
| `Where`, `Select`, `SelectMany`, `OrderBy`, `ThenBy`, `GroupBy`*, `Join`, `Skip`, `Take`, `Distinct`, `Concat`, `Zip`, `Chunk`, `Reverse`* | `ToList`, `ToArray`, `ToDictionary`, `ToHashSet`, `ToLookup`, `Count`, `Sum`, `Min`, `Max`, `Average`, `Aggregate`, `First`, `Single`, `Last`, `ElementAt`, `Any`, `All`, `Contains`, `foreach` |

\* `OrderBy`, `GroupBy`, `Reverse` are deferred, but when enumeration starts they must read the **whole** source before yielding the first item ("non-streaming").

### The multiple enumeration trap

```csharp
int dbCalls = 0;
IEnumerable<Employee> GetEmployeesFromDb()
{
    dbCalls++;                          // imagine an expensive DB/HTTP call here
    foreach (var e in Employees) yield return e;
}

var engineers = GetEmployeesFromDb().Where(e => e.Dept == "Engineering");
if (engineers.Any())
    Console.WriteLine($"{engineers.Count()} engineers, top: {engineers.MaxBy(e => e.Salary)!.Name}");
Console.WriteLine($"Source enumerated {dbCalls} times");

dbCalls = 0;
var engineerList = GetEmployeesFromDb().Where(e => e.Dept == "Engineering").ToList(); // once
if (engineerList.Count > 0)
    Console.WriteLine($"{engineerList.Count} engineers, top: {engineerList.MaxBy(e => e.Salary)!.Name}");
Console.WriteLine($"Source enumerated {dbCalls} times");
```

Output:

```text
4 engineers, top: Asha
Source enumerated 3 times
4 engineers, top: Asha
Source enumerated 1 times
```

With a database or HTTP-backed `IEnumerable`, that's **three round trips** instead of one — or worse, three *different* answers if the data changes in between. Your IDE/analyzers warn about "possible multiple enumeration".

⚠️ **Interview trap:** Don't "fix" it by calling `.ToList()` everywhere either — materializing a huge or infinite sequence wastes memory. Materialize **once**, when you know you'll need the data more than once.

🎯 **Say this in the interview:** "Most LINQ operators use deferred execution — they build a pipeline that runs only when enumerated, and re-runs on every enumeration. Operators like `ToList`, `Count` or `First` execute immediately. If I need a result more than once, I materialize it once with `ToList()` to avoid repeated work."

🧩 **Predict the output:**

```csharp
int threshold = 2;
var q = new[] { 1, 2, 3, 4 }.Where(n => n > threshold);
threshold = 3;
Console.WriteLine(string.Join(",", q));
```

<details><summary>Answer</summary>

```text
4
```

The lambda captures the *variable* `threshold`; the query runs at `string.Join`, when `threshold` is already `3`.

</details>

## 10.3 The operators you must know

### Filtering, projection, ordering, flattening

```csharp
// Where + Select (with index) into an anonymous type
var recent = Employees
    .Where(e => e.JoinDate.Year >= 2023)
    .Select((e, i) => new { No = i + 1, e.Name, e.JoinDate.Year });
foreach (var r in recent) Console.WriteLine(r);

// OrderBy + ThenByDescending + ThenBy (multi-level sort)
var sorted = Employees.OrderBy(e => e.Dept).ThenByDescending(e => e.Salary).ThenBy(e => e.Name)
                      .Select(e => $"{e.Dept}/{e.Name}/{e.Salary}");
Console.WriteLine(string.Join(" | ", sorted));

// SelectMany: one-to-many, then flatten
var teams = new Dictionary<string, string[]>
{
    ["Alpha"] = ["Asha", "Bilal"],
    ["Beta"]  = ["Chen"]
};
var memberships = teams.SelectMany(t => t.Value, (team, member) => $"{member}@{team.Key}");
Console.WriteLine(string.Join(", ", memberships));
```

Output:

```text
{ No = 1, Name = Diya, Year = 2023 }
{ No = 2, Name = Gopal, Year = 2024 }
{ No = 3, Name = Hana, Year = 2023 }
{ No = 4, Name = Ivan, Year = 2024 }
Engineering/Asha/120000 | Engineering/Diya/120000 | Engineering/Gopal/105000 | Engineering/Bilal/95000 | HR/Hana/65000 | HR/Ethan/60000 | Sales/Farah/85000 | Sales/Asha/72000 | Sales/Chen/70000 | Sales/Ivan/70000
Asha@Alpha, Bilal@Alpha, Chen@Beta
```

- `Select` = 1 input → 1 output (a *map*). `SelectMany` = 1 input → *many* outputs, flattened into one sequence.
- `OrderBy(...).ThenBy(...)` for multi-level sorts. **Never** `OrderBy(...).OrderBy(...)` — see the puzzle below.
- .NET 7+: `Order()` / `OrderDescending()` sort by the element itself.

🧩 **Predict the output:**

```csharp
var first = Employees.OrderBy(e => e.Dept).OrderBy(e => e.Salary).First();
Console.WriteLine($"{first.Name} {first.Dept} {first.Salary}");
```

<details><summary>Answer</summary>

```text
Ethan HR 60000
```

The second `OrderBy` **replaces** the primary sort — the list is now sorted by `Salary` only (Dept order survives only as a tiebreaker, because `OrderBy` is stable). To sort by Dept *then* Salary, use `OrderBy(e => e.Dept).ThenBy(e => e.Salary)`.

</details>

### Grouping and joining

```csharp
// GroupBy: key -> bucket of items
foreach (var g in Employees.GroupBy(e => e.Dept))
    Console.WriteLine($"{g.Key}: {string.Join(", ", g.Select(e => e.Name))}");

// GroupBy with a result selector
var stats = Employees.GroupBy(
    e => e.Dept,
    (dept, emps) => new { Dept = dept, Count = emps.Count(), Avg = emps.Average(e => e.Salary) });
foreach (var s in stats) Console.WriteLine($"{s.Dept,-12} count={s.Count} avg={s.Avg:0}");

// Join = INNER join (Marketing has no employees -> not in output)
var withCity = Employees.Join(Departments, e => e.Dept, d => d.Name, (e, d) => $"{e.Name}-{d.Location}");
Console.WriteLine(string.Join(", ", withCity.Take(4)));

// GroupJoin = each department + ITS employees (Marketing gets an empty group)
var perDept = Departments.GroupJoin(Employees, d => d.Name, e => e.Dept,
                                    (d, emps) => $"{d.Name}={emps.Count()}");
Console.WriteLine(string.Join(", ", perDept));
```

Output:

```text
Engineering: Asha, Bilal, Diya, Gopal
Sales: Chen, Farah, Ivan, Asha
HR: Ethan, Hana
Engineering  count=4 avg=110000
Sales        count=4 avg=74250
HR           count=2 avg=62500
Asha-Bengaluru, Bilal-Bengaluru, Chen-Mumbai, Diya-Bengaluru
Engineering=4, Sales=4, HR=2, Marketing=0
```

- `GroupBy` returns `IEnumerable<IGrouping<TKey, TElement>>` — each group **is** a sequence with a `.Key`. Groups appear in order of first occurrence.
- `Join` = SQL **INNER JOIN** (unmatched rows disappear).
- `GroupJoin` = "each left item + a collection of its matches" — the building block for **LEFT JOIN** (problem 7).

### Set operations

```csharp
int[] a = [1, 2, 2, 3, 4];
int[] b = [3, 4, 5];

Console.WriteLine($"Distinct : {string.Join(",", a.Distinct())}");
Console.WriteLine($"Union    : {string.Join(",", a.Union(b))}");
Console.WriteLine($"Intersect: {string.Join(",", a.Intersect(b))}");
Console.WriteLine($"Except   : {string.Join(",", a.Except(b))}");
Console.WriteLine($"Concat   : {string.Join(",", a.Concat(b))}");

// .NET 6+: *By variants compare using a key
var firstPerDept = Employees.DistinctBy(e => e.Dept).Select(e => e.Name);
Console.WriteLine($"DistinctBy(Dept): {string.Join(",", firstPerDept)}");
```

Output:

```text
Distinct : 1,2,3,4
Union    : 1,2,3,4,5
Intersect: 3,4
Except   : 1,2
Concat   : 1,2,2,3,4,3,4,5
DistinctBy(Dept): Asha,Chen,Ethan
```

`Union`, `Intersect`, `Except`, `Distinct` remove duplicates (they use hashing → O(n + m)). `Concat` keeps everything. .NET 6 added `DistinctBy`, `UnionBy`, `IntersectBy`, `ExceptBy` that compare by a key.

### Partitioning, zipping, aggregating, quantifiers

```csharp
int[] nums = [5, 3, 8, 1, 9, 2];

Console.WriteLine($"Skip(2).Take(3): {string.Join(",", nums.Skip(2).Take(3))}");
Console.WriteLine($"TakeWhile(<8)  : {string.Join(",", nums.TakeWhile(n => n < 8))}");
Console.WriteLine($"Take(^2..)     : {string.Join(",", nums.Take(^2..))}");
Console.WriteLine($"Chunk(4)       : {string.Join(" ", nums.Chunk(4).Select(c => $"[{string.Join(",", c)}]"))}");

string[] names = ["Asha", "Bilal", "Chen"];
int[] scores = [90, 75, 88];
Console.WriteLine($"Zip            : {string.Join(", ", names.Zip(scores, (n, s) => $"{n}:{s}"))}");

Console.WriteLine($"Aggregate (product): {nums.Aggregate((acc, n) => acc * n)}");
Console.WriteLine($"Aggregate (seeded) : {names.Aggregate("Team:", (acc, n) => acc + " " + n)}");

Console.WriteLine($"Any(>8)={nums.Any(n => n > 8)} All(>0)={nums.All(n => n > 0)} Count(even)={nums.Count(n => n % 2 == 0)}");
Console.WriteLine($"Sum={nums.Sum()} Min={nums.Min()} Max={nums.Max()} Avg={nums.Average():0.00}");

var topEarner = Employees.MaxBy(e => e.Salary);   // .NET 6+: returns the ELEMENT
var maxSalary = Employees.Max(e => e.Salary);     // returns the VALUE
Console.WriteLine($"MaxBy -> {topEarner!.Name}, Max -> {maxSalary}");
Console.WriteLine($"MinBy(JoinDate) -> {Employees.MinBy(e => e.JoinDate)!.Name}");
```

Output:

```text
Skip(2).Take(3): 8,1,9
TakeWhile(<8)  : 5,3
Take(^2..)     : 9,2
Chunk(4)       : [5,3,8,1] [9,2]
Zip            : Asha:90, Bilal:75, Chen:88
Aggregate (product): 2160
Aggregate (seeded) : Team: Asha Bilal Chen
Any(>8)=True All(>0)=True Count(even)=2
Sum=28 Min=1 Max=9 Avg=4.67
MaxBy -> Asha, Max -> 120000
MinBy(JoinDate) -> Chen
```

- `Chunk(n)` (.NET 6+) splits into arrays of size `n` — great for batching DB writes or API calls.
- `Aggregate` = general "fold/reduce". Without a seed, the first element is the starting accumulator.
- `MinBy`/`MaxBy` (.NET 6+) return the **element**; `Min`/`Max` return the **value**. On ties, the **first** one wins.
- `Any()` stops at the first match; `Count() > 0` may walk the entire sequence. Prefer `Any()`.

### First / FirstOrDefault / Single / SingleOrDefault / Last

| Method | 0 matches | 1 match | 2+ matches | Use when |
|---|---|---|---|---|
| `First()` | ❌ throws | ✅ item | ✅ first item | At least one must exist |
| `FirstOrDefault()` | `default` (`null`/`0`) | ✅ item | ✅ first item | May not exist |
| `Single()` | ❌ throws | ✅ item | ❌ throws | Exactly one must exist (e.g. by primary key) |
| `SingleOrDefault()` | `default` | ✅ item | ❌ **throws** | Zero or one allowed; more is a bug |
| `Last()` / `LastOrDefault()` | like First, from the end | | | — |

```csharp
int[] nums = [1, 2, 3, 2];
int[] empty = [];

Console.WriteLine(nums.First(n => n == 2));            // first match
Console.WriteLine(empty.FirstOrDefault());             // default(int) = 0
Console.WriteLine(empty.FirstOrDefault(-1));           // .NET 6+: custom default
Console.WriteLine(nums.Single(n => n == 3));           // exactly one match
Try(() => empty.First());
Try(() => nums.Single(n => n == 2));                   // two matches!
Try(() => nums.SingleOrDefault(n => n == 2));          // ...OrDefault still throws on > 1
Console.WriteLine(nums.SingleOrDefault(n => n == 9));  // zero matches -> default

static void Try(Func<int> f)
{
    try { f(); }
    catch (InvalidOperationException ex) { Console.WriteLine($"InvalidOperationException: {ex.Message}"); }
}
```

Output:

```text
2
0
-1
3
InvalidOperationException: Sequence contains no elements
InvalidOperationException: Sequence contains more than one matching element
InvalidOperationException: Sequence contains more than one matching element
0
```

⚠️ **Interview trap:** `SingleOrDefault` still **throws** on 2+ matches. And `Single` must scan the **whole** sequence to prove there's no second match, so it's slower than `First`.

## 10.4 `IEnumerable<T>` vs `IQueryable<T>`

**Simple words:**

- `IEnumerable<T>` = "I'll pull items **into memory** and run your C# lambdas on them one by one." (LINQ to Objects)
- `IQueryable<T>` = "Give me your query as an **expression tree**; my provider (EF Core) will **translate** it into something else (SQL) and run it **at the source**."

```csharp
IEnumerable<Employee> inMemory = Employees;
IQueryable<Employee> queryable = Employees.AsQueryable();

var q1 = inMemory.Where(e => e.Salary > 100_000);    // lambda compiled to Func<Employee,bool>
var q2 = queryable.Where(e => e.Salary > 100_000);   // lambda captured as Expression<Func<...>>

Console.WriteLine(q1.GetType().Name);                // an in-memory iterator object
Console.WriteLine(q2.Expression);                    // a TREE a provider (EF Core) can turn into SQL
Console.WriteLine($"{q1.Count()} {q2.Count()}");
```

Output (the iterator's type name is an internal detail and varies between .NET versions):

```text
SizeOptIListWhereIterator`1
System.Collections.Generic.List`1[Employee].Where(e => (e.Salary > 100000))
3 3
```

With EF Core, the same query becomes SQL like this (illustrative):

```sql
SELECT [e].[Id], [e].[Name], [e].[Dept], [e].[Salary], [e].[JoinDate]
FROM [Employees] AS [e]
WHERE [e].[Salary] > 100000.0
```

| | `IEnumerable<T>` | `IQueryable<T>` |
|---|---|---|
| Namespace / methods | `System.Linq.Enumerable` | `System.Linq.Queryable` |
| Lambdas become | `Func<T, bool>` (compiled code) | `Expression<Func<T, bool>>` (data) |
| Where it runs | In your process, in memory | At the source (database, OData, …) |
| Filtering a 1M-row table | Loads 1M rows, filters in C# 😱 | `WHERE` clause in SQL, returns few rows |
| Any C# method in lambdas | ✅ | ❌ only what the provider can translate |
| Inherits | — | `IQueryable<T> : IEnumerable<T>` |

⚠️ **Interview trap:** Calling `.AsEnumerable()` or `.ToList()` too early switches to in-memory LINQ — everything after it runs in C#, after downloading all rows:

```csharp
// ❌ downloads the WHOLE table, then filters in memory
var bad = db.Employees.ToList().Where(e => e.Salary > 100_000);
// ✅ filter in SQL, then download only matching rows
var good = db.Employees.Where(e => e.Salary > 100_000).ToList();
```

Similarly, a repository method returning `IEnumerable<Employee>` from a `DbSet` makes callers' `.Where(...)` run in memory.

🎯 **Say this in the interview:** "`IEnumerable` runs LINQ in memory with compiled delegates. `IQueryable` captures the query as an expression tree so a provider like EF Core can translate it into SQL and run it in the database. I keep queries as `IQueryable` until filtering, paging and projection are done, then materialize."

## 10.5 `ToList` vs `ToArray` vs `ToDictionary` vs `ToLookup`

```csharp
var byId = Employees.ToDictionary(e => e.Id);              // key must be unique
Console.WriteLine(byId[3].Name);

try { Employees.ToDictionary(e => e.Name); }               // "Asha" appears twice!
catch (ArgumentException ex) { Console.WriteLine(ex.Message); }

ILookup<string, Employee> byName = Employees.ToLookup(e => e.Name);  // key -> MANY values
Console.WriteLine($"Ashas: {byName["Asha"].Count()}, Zeds: {byName["Zed"].Count()}"); // never throws

HashSet<string> deptSet = Employees.Select(e => e.Dept).ToHashSet();
Console.WriteLine(string.Join(",", deptSet));

Employee[] arr = Employees.ToArray();
List<Employee> list = Employees.ToList();
Console.WriteLine($"{arr.Length} {list.Count}");
```

Output:

```text
Chen
An item with the same key has already been added. Key: Asha
Ashas: 2, Zeds: 0
Engineering,Sales,HR
10 10
```

| Method | Result | Duplicate keys | Missing key | Typical use |
|---|---|---|---|---|
| `ToList()` | `List<T>` | — | — | Need to add/remove later, or general materialization |
| `ToArray()` | `T[]` | — | — | Fixed snapshot; slightly leaner |
| `ToHashSet()` | `HashSet<T>` | Removed | — | Fast `Contains` |
| `ToDictionary(k)` | `Dictionary<K,T>` | ❌ throws `ArgumentException` | Indexer throws | Unique key → one item |
| `ToLookup(k)` | `ILookup<K,T>` (immutable) | ✅ grouped | Returns **empty** sequence | Key → many items |

`ToLookup` is like `GroupBy`, but **immediate** and indexable by key.

## 10.6 Writing your own LINQ-style operators with `yield`

LINQ operators are just extension methods + iterators. You can write your own:

```csharp
var highPaid = Employees.WhereIf(true, e => e.Salary > 100_000).Select(e => e.Name);
Console.WriteLine(string.Join(",", highPaid));

foreach (var (item, index) in new[] { "a", "b", "c" }.WithIndex())
    Console.Write($"{index}:{item} ");
Console.WriteLine();

Console.WriteLine(string.Join(",", Enumerable.Range(1, 10).MyWhere(n => n % 2 == 0)));

public static class MyLinqExtensions
{
    public static IEnumerable<T> MyWhere<T>(this IEnumerable<T> source, Func<T, bool> predicate)
    {
        ArgumentNullException.ThrowIfNull(source);     // runs immediately (eager validation)
        ArgumentNullException.ThrowIfNull(predicate);
        return Iterator();

        IEnumerable<T> Iterator()                      // runs lazily
        {
            foreach (var item in source)
                if (predicate(item)) yield return item;
        }
    }

    public static IEnumerable<T> WhereIf<T>(this IEnumerable<T> source, bool condition, Func<T, bool> predicate)
        => condition ? source.Where(predicate) : source;

    public static IEnumerable<(T Item, int Index)> WithIndex<T>(this IEnumerable<T> source)
    {
        int i = 0;
        foreach (var item in source) yield return (item, i++);
    }
}
```

Output:

```text
Asha,Diya,Gopal
0:a 1:b 2:c 
2,4,6,8,10
```

The *wrapper + local iterator* pattern in `MyWhere` is how the real BCL does it: argument checks run **immediately** at the call site, while the loop runs **lazily** (section 11.4 explains why). .NET 9+ ships a built-in `Index()` operator equivalent to `WithIndex`.

## 10.7 Fifteen classic LINQ interview problems

All use the sample data from 10.0.

### Problem 1 — Second highest salary

```csharp
var secondHighest = Employees
    .Select(e => e.Salary)
    .Distinct()                       // 120000 appears twice — must not count it twice
    .OrderByDescending(s => s)
    .Skip(1)
    .FirstOrDefault();                // 0 if there is no second salary
Console.WriteLine(secondHighest);

var who = Employees.Where(e => e.Salary == secondHighest).Select(e => e.Name);
Console.WriteLine(string.Join(", ", who));
```

```text
105000
Gopal
```

⚠️ Without `Distinct()`, you'd get `120000` again (Asha and Diya share it).

### Problem 2 — Count (and total) of employees per department

```csharp
var summary = Employees
    .GroupBy(e => e.Dept)
    .Select(g => new { Dept = g.Key, Count = g.Count(), Total = g.Sum(e => e.Salary) })
    .OrderByDescending(x => x.Count).ThenBy(x => x.Dept);

foreach (var s in summary)
    Console.WriteLine($"{s.Dept,-12} {s.Count}  total={s.Total}");
```

```text
Engineering  4  total=440000
Sales        4  total=297000
HR           2  total=125000
```

### Problem 3 — Find duplicates

```csharp
var dupNames = Employees.GroupBy(e => e.Name)
                        .Where(g => g.Count() > 1)
                        .Select(g => $"{g.Key} x{g.Count()}");
Console.WriteLine(string.Join(", ", dupNames));

int[] nums = [4, 1, 4, 7, 1, 4];
Console.WriteLine(string.Join(",", nums.GroupBy(n => n).Where(g => g.Count() > 1).Select(g => g.Key)));

var seen = new HashSet<int>();         // single-pass alternative
Console.WriteLine(string.Join(",", nums.Where(n => !seen.Add(n)).Distinct()));
```

```text
Asha x2
4,1
4,1
```

⚠️ The `HashSet` version relies on a side effect inside a lambda — fine for a one-shot query, but it gives wrong results if the query is enumerated twice.

### Problem 4 — Word frequency (top 3)

```csharp
var text = "the quick brown fox jumps over the lazy dog The fox";

var top3 = text.ToLowerInvariant()
    .Split(' ', StringSplitOptions.RemoveEmptyEntries)
    .GroupBy(w => w)
    .Select(g => (Word: g.Key, Count: g.Count()))
    .OrderByDescending(x => x.Count).ThenBy(x => x.Word)
    .Take(3);
foreach (var (word, count) in top3) Console.WriteLine($"{word}: {count}");

// .NET 9+: CountBy = GroupBy + Count in one step
foreach (var (word, count) in text.ToLowerInvariant().Split(' ').CountBy(w => w).Where(kv => kv.Value > 1))
    Console.WriteLine($"{word} -> {count}");
```

```text
the: 3
fox: 2
brown: 1
the -> 3
fox -> 2
```

### Problem 5 — Top N per group (top 2 earners per department)

```csharp
var top2PerDept = Employees
    .GroupBy(e => e.Dept)
    .SelectMany(g => g.OrderByDescending(e => e.Salary).ThenBy(e => e.Name).Take(2));

foreach (var e in top2PerDept)
    Console.WriteLine($"{e.Dept,-12} {e.Name,-6} {e.Salary}");
```

```text
Engineering  Asha   120000
Engineering  Diya   120000
Sales        Farah  85000
Sales        Asha   72000
HR           Hana   65000
HR           Ethan  60000
```

### Problem 6 — Flatten nested lists

```csharp
List<List<int>> nested = [[1, 2], [3], [], [4, 5, 6]];
Console.WriteLine(string.Join(",", nested.SelectMany(x => x)));

// Any depth -> recursion + yield
object[] deep = [1, new object[] { 2, new object[] { 3, 4 } }, 5];
Console.WriteLine(string.Join(",", Flatten(deep)));

static IEnumerable<int> Flatten(IEnumerable<object> items)
{
    foreach (var item in items)
    {
        if (item is IEnumerable<object> inner)
            foreach (var x in Flatten(inner)) yield return x;
        else
            yield return (int)item;
    }
}
```

```text
1,2,3,4,5,6
1,2,3,4,5
```

### Problem 7 — LEFT JOIN (departments with or without employees)

```csharp
var leftJoin =
    from d in Departments
    join e in Employees on d.Name equals e.Dept into deptEmps
    from e in deptEmps.DefaultIfEmpty()               // no match -> one null
    select new { Dept = d.Name, Employee = e?.Name ?? "(none)" };

foreach (var row in leftJoin.Where(r => r.Dept is "HR" or "Marketing"))
    Console.WriteLine($"{row.Dept,-10} {row.Employee}");

// Same thing in method syntax
var leftJoin2 = Departments
    .GroupJoin(Employees, d => d.Name, e => e.Dept, (d, emps) => new { d, emps })
    .SelectMany(x => x.emps.DefaultIfEmpty(),
                (x, e) => new { Dept = x.d.Name, Employee = e?.Name ?? "(none)" });
Console.WriteLine($"Rows: {leftJoin2.Count()}");

// .NET 10+: built-in LeftJoin operator
var leftJoin3 = Departments.LeftJoin(Employees, d => d.Name, e => e.Dept,
                                     (d, e) => $"{d.Name}:{e?.Name ?? "(none)"}");
Console.WriteLine(leftJoin3.Last());
```

```text
HR         Ethan
HR         Hana
Marketing  (none)
Rows: 11
Marketing:(none)
```

The recipe: `join … into group` + `from x in group.DefaultIfEmpty()`. `DefaultIfEmpty()` turns an empty group into a single `null`, so the left row survives. **.NET 10** adds `LeftJoin`/`RightJoin` operators directly.

### Problem 8 — Pagination

```csharp
int pageSize = 3;
for (int page = 1; page <= 2; page++)
{
    var items = Employees
        .OrderBy(e => e.Id)                       // ALWAYS order before paging
        .Skip((page - 1) * pageSize)
        .Take(pageSize)
        .Select(e => e.Name);
    Console.WriteLine($"Page {page}: {string.Join(", ", items)}");
}
int totalPages = (int)Math.Ceiling(Employees.Count / (double)pageSize);
Console.WriteLine($"Total pages: {totalPages}");
```

```text
Page 1: Asha, Bilal, Chen
Page 2: Diya, Ethan, Farah
Total pages: 4
```

⚠️ Paging without `OrderBy` gives unpredictable pages in SQL (EF Core even warns you). Deep `Skip` on big tables is slow — keyset paging (`Where(e => e.Id > lastSeenId).Take(n)`) scales better.

### Problem 9 — Group by year / month and pivot

```csharp
foreach (var g in Employees.GroupBy(e => e.JoinDate.Year).OrderBy(g => g.Key))
    Console.WriteLine($"{g.Key}: {g.Count()} hire(s) [{string.Join(", ", g.Select(e => e.Name))}]");

// Group by Year + Month (composite key)
var sameMonth = Employees
    .GroupBy(e => new { e.JoinDate.Year, e.JoinDate.Month })
    .Where(g => g.Count() > 1)
    .Select(g => $"{g.Key.Year}-{g.Key.Month:00}: {string.Join(", ", g.Select(e => e.Name))}");
Console.WriteLine($"Same month: {string.Join("; ", sameMonth)}");

// Pivot: rows = Dept, columns = Year, cell = headcount
int[] years = Employees.Select(e => e.JoinDate.Year).Distinct().Order().ToArray();
Console.WriteLine($"{"Dept",-12}" + string.Concat(years.Select(y => $"{y,6}")));
foreach (var g in Employees.GroupBy(e => e.Dept))
    Console.WriteLine($"{g.Key,-12}" + string.Concat(years.Select(y => $"{g.Count(e => e.JoinDate.Year == y),6}")));
```

```text
2020: 1 hire(s) [Chen]
2021: 2 hire(s) [Asha, Ethan]
2022: 3 hire(s) [Bilal, Farah, Asha]
2023: 2 hire(s) [Diya, Hana]
2024: 2 hire(s) [Gopal, Ivan]
Same month: 2022-07: Bilal, Farah
Dept          2020  2021  2022  2023  2024
Engineering      0     1     1     1     1
Sales            1     0     2     0     1
HR               0     1     0     1     0
```

Anonymous types (`new { Year, Month }`) have value equality, so they work as composite `GroupBy` keys. A tuple `(e.JoinDate.Year, e.JoinDate.Month)` works too.

### Problem 10 — Highest paid employee in each department

```csharp
foreach (var e in Employees.GroupBy(e => e.Dept).Select(g => g.MaxBy(e => e.Salary)!))
    Console.WriteLine($"{e.Dept}: {e.Name} ({e.Salary})");

// Ties: return ALL top earners per department
var allTop = Employees
    .GroupBy(e => e.Dept)
    .Select(g => new { Dept = g.Key, Max = g.Max(e => e.Salary), Members = g })
    .Select(x => $"{x.Dept}: {string.Join(" & ", x.Members.Where(e => e.Salary == x.Max).Select(e => e.Name))}");
Console.WriteLine(string.Join(" | ", allTop));
```

```text
Engineering: Asha (120000)
Sales: Farah (85000)
HR: Hana (65000)
Engineering: Asha & Diya | Sales: Farah | HR: Hana
```

### Problem 11 — Departments whose average salary is above X (SQL `HAVING`)

```csharp
var richDepts = Employees
    .GroupBy(e => e.Dept)
    .Select(g => new { Dept = g.Key, Avg = g.Average(e => e.Salary) })
    .Where(x => x.Avg > 70_000)                     // like SQL HAVING
    .OrderByDescending(x => x.Avg);

foreach (var d in richDepts) Console.WriteLine($"{d.Dept}: {d.Avg:0.##}");
```

```text
Engineering: 110000
Sales: 74250
```

### Problem 12 — Employees earning more than their department's average

```csharp
var aboveAvg =
    from e in Employees
    group e by e.Dept into g
    let avg = g.Average(x => x.Salary)              // 'let' = a variable inside the query
    from emp in g
    where emp.Salary > avg
    select $"{emp.Name} ({emp.Dept}: {emp.Salary} > {avg:0})";

foreach (var line in aboveAvg) Console.WriteLine(line);
```

```text
Asha (Engineering: 120000 > 110000)
Diya (Engineering: 120000 > 110000)
Farah (Sales: 85000 > 74250)
Hana (HR: 65000 > 62500)
```

### Problem 13 — First non-repeating character & character frequency

```csharp
string s = "swiss cheese";

char? firstUnique = s.Where(char.IsLetter)
                     .GroupBy(c => c)               // groups keep first-seen order
                     .FirstOrDefault(g => g.Count() == 1)?.Key;
Console.WriteLine($"First non-repeating: {firstUnique}");

var freq = s.Where(c => c != ' ')
            .GroupBy(c => c)
            .OrderByDescending(g => g.Count())      // OrderBy is STABLE: ties keep original order
            .Select(g => $"{g.Key}={g.Count()}");
Console.WriteLine(string.Join(" ", freq));
```

```text
First non-repeating: w
s=4 e=3 w=1 i=1 c=1 h=1
```

### Problem 14 — Missing numbers in a sequence

```csharp
int[] received = [1, 2, 4, 7, 8, 10];
var missing = Enumerable.Range(1, received.Max()).Except(received);
Console.WriteLine(string.Join(",", missing));

// Exactly ONE missing from 1..n? Use the sum formula: O(n), no extra memory
int[] oneMissing = [1, 2, 3, 5];
int n = oneMissing.Length + 1;
Console.WriteLine(n * (n + 1) / 2 - oneMissing.Sum());
```

```text
3,5,6,9
4
```

### Problem 15 — Group anagrams

```csharp
string[] words = ["listen", "silent", "enlist", "google", "gogole", "cat", "act", "dog"];

var anagramGroups = words
    .GroupBy(w => new string(w.Order().ToArray()))   // key = letters sorted: "eilnst"
    .Where(g => g.Count() > 1)
    .Select(g => $"[{string.Join(", ", g)}]");

Console.WriteLine(string.Join(" ", anagramGroups));
```

```text
[listen, silent, enlist] [google, gogole] [cat, act]
```

## Try it yourself

1. List employees who joined in **2022**, highest salary first, as `Name:Salary`.
2. Which department has the **highest total salary**?
3. For each department print `Dept: name1, name2, …` with names sorted alphabetically.
4. Find groups of employees who have **exactly the same salary**.
5. Produce a **dense rank** by salary (equal salaries share a rank, no gaps).
6. Why is `if (list.Where(x => x.IsActive).Count() > 0)` worse than `if (list.Any(x => x.IsActive))`?

<details><summary>Answer</summary>

**1.**

```csharp
var joined2022 = Employees
    .Where(e => e.JoinDate.Year == 2022)
    .OrderByDescending(e => e.Salary)
    .Select(e => $"{e.Name}:{e.Salary}");
Console.WriteLine(string.Join(", ", joined2022));
```

```text
Bilal:95000, Farah:85000, Asha:72000
```

**2.**

```csharp
var top = Employees
    .GroupBy(e => e.Dept)
    .Select(g => new { Dept = g.Key, Total = g.Sum(e => e.Salary) })
    .MaxBy(x => x.Total)!;
Console.WriteLine($"{top.Dept} {top.Total}");
```

```text
Engineering 440000
```

**3.**

```csharp
var lines = Employees
    .GroupBy(e => e.Dept)
    .Select(g => $"{g.Key}: {string.Join(", ", g.Select(e => e.Name).Order())}");
foreach (var line in lines) Console.WriteLine(line);
```

```text
Engineering: Asha, Bilal, Diya, Gopal
Sales: Asha, Chen, Farah, Ivan
HR: Ethan, Hana
```

**4.**

```csharp
var sameSalary = Employees
    .GroupBy(e => e.Salary)
    .Where(g => g.Count() > 1)
    .Select(g => $"{g.Key}: {string.Join(" & ", g.Select(e => e.Name))}");
Console.WriteLine(string.Join(" | ", sameSalary));
```

```text
120000: Asha & Diya | 70000: Chen & Ivan
```

**5.**

```csharp
var rankOf = Employees
    .Select(e => e.Salary).Distinct().OrderDescending()
    .Select((salary, i) => (salary, rank: i + 1))
    .ToDictionary(x => x.salary, x => x.rank);       // O(1) rank lookup

var ranked = Employees
    .OrderByDescending(e => e.Salary).ThenBy(e => e.Name)
    .Select(e => $"{rankOf[e.Salary]}.{e.Name}");
Console.WriteLine(string.Join(" ", ranked));
```

```text
1.Asha 1.Diya 2.Gopal 3.Bilal 4.Farah 5.Asha 6.Chen 6.Ivan 7.Hana 8.Ethan
```

**6.** `Count()` must walk **all** matching elements (or, in EF Core, run `SELECT COUNT(*)`), whereas `Any()` stops at the **first** match (`SELECT CASE WHEN EXISTS(...)`). `Any(predicate)` is also shorter and states the intent.

</details>

## Quick revision

- LINQ = extension methods on `IEnumerable<T>`/`IQueryable<T>` + lambdas; query syntax compiles to method syntax.
- Most operators are **deferred**; `ToList`/`Count`/`First`/`Sum`/`foreach` execute.
- Deferred queries re-run on every enumeration and see the **current** source and captured variables.
- Multiple enumeration = repeated work (or repeated DB calls) → materialize once when reused.
- `Select` = map 1→1; `SelectMany` = 1→many, flattened.
- `OrderBy().ThenBy()` — a second `OrderBy` replaces the first; `OrderBy` is stable.
- `Join` = inner join; `GroupJoin` + `DefaultIfEmpty` = left join (.NET 10: `LeftJoin`).
- `First` throws on empty; `Single` throws on 0 or 2+; `SingleOrDefault` still throws on 2+.
- `MaxBy`/`MinBy` return elements; `Max`/`Min` return values; `Chunk`, `DistinctBy` are .NET 6+.
- `IQueryable` builds expression trees → SQL at the source; `IEnumerable` runs in memory.
- `ToDictionary` throws on duplicate keys; `ToLookup` groups them and never throws on missing keys.
- Prefer `Any()` over `Count() > 0`; always `OrderBy` before `Skip`/`Take`.

# 11. Extension Methods, Iterators & `yield`

> **Why interviewers ask this:** All of LINQ is built from these two features. Interviewers ask "how does an extension method work?", "what does `yield return` compile into?", and "why didn't my iterator throw an exception?" to see whether you understand what the compiler does for you.

## 11.1 Extension methods

**Simple words:** an extension method lets you **add a method to an existing type without changing it or inheriting from it** — even `string`, `int`, or an interface. It's really a **static method** that the compiler lets you call *as if* it were an instance method.

**Real-life analogy:** a **phone case with a built-in card holder**. You didn't open up the phone or buy a new model — you clipped something on, and now `phone.HoldCards()` "works".

Rules:

1. Declared in a **static, non-generic, top-level** class.
2. The method is `static`, and the first parameter has `this` before its type: `static bool IsWeekend(this DateTime d)`.
3. Its namespace must be imported (`using`) at the call site.
4. It can only use the target's **public** (or accessible) members — no access to `private` fields.
5. If the type already has an **instance method with the same signature, the instance method wins** — the extension is silently ignored.

```csharp
string? nothing = null;
Console.WriteLine(nothing.IsNullOrBlank());               // no NullReferenceException!
Console.WriteLine("Hello, interview world".Truncate(10));
Console.WriteLine(StringExtensions.Truncate("Hello, interview world", 5)); // what the compiler emits

int[] data = [3, 8, 1, 8];
Console.WriteLine(data.SecondLargest());
Console.WriteLine(new DateTime(2024, 11, 30).IsWeekend());

public static class StringExtensions
{
    public static bool IsNullOrBlank(this string? s) => string.IsNullOrWhiteSpace(s);

    public static string Truncate(this string s, int max) =>
        s.Length <= max ? s : s[..max] + "...";
}

public static class CollectionExtensions
{
    public static T SecondLargest<T>(this IEnumerable<T> source) where T : IComparable<T> =>
        source.Distinct().OrderDescending().Skip(1).First();

    public static bool IsWeekend(this DateTime d) =>
        d.DayOfWeek is DayOfWeek.Saturday or DayOfWeek.Sunday;
}
```

Output:

```text
True
Hello, int...
Hello...
3
True
```

**How it compiles:** `"Hello".Truncate(10)` is rewritten to `StringExtensions.Truncate("Hello", 10)`. That's why calling an extension on `null` doesn't throw by itself — no virtual dispatch happens; `null` is just passed as the first argument.

🧩 **Predict the output:**

```csharp
var p = new Printer();
p.Print();
p.Describe();

class Printer { public void Print() => Console.WriteLine("Instance Print"); }

static class PrinterExtensions
{
    public static void Print(this Printer p) => Console.WriteLine("Extension Print");
    public static void Describe(this Printer p) => Console.WriteLine("Extension Describe");
}
```

<details><summary>Answer</summary>

```text
Instance Print
Extension Describe
```

Instance members always take priority. The extension `Print` is only considered when no applicable instance method exists — so it's unreachable via `p.Print()` (you'd have to call `PrinterExtensions.Print(p)`).

</details>

Real-world uses: all of LINQ (`System.Linq.Enumerable`), ASP.NET Core registration (`services.AddControllers()`, `app.UseRouting()`), fluent builders, and domain helpers on types you don't own.

⚠️ **Interview trap:** Extension methods are resolved **statically** at compile time — they are not virtual and can't be overridden. And they can't add **state** (fields) to a type.

### C# 14 extension members (.NET 10)

C# 14 adds **extension blocks**: besides methods, you can now declare extension **properties** and **static** extension members (callable on the type itself).

```csharp
Console.WriteLine("".IsEmpty);
Console.WriteLine("abc".IsEmpty);
Console.WriteLine(string.Repeat("ab", 3));      // static extension member: called on the TYPE
Console.WriteLine(new[] { 1, 2, 3 }.Middle);

public static class ModernExtensions
{
    extension(string s)                          // C# 14: receiver declared once for the block
    {
        public bool IsEmpty => s.Length == 0;    // extension PROPERTY
    }

    extension(string)                            // no receiver name -> static members
    {
        public static string Repeat(string value, int times) =>
            string.Concat(Enumerable.Repeat(value, times));
    }

    extension<T>(T[] array)                      // generic extension block
    {
        public T Middle => array[array.Length / 2];
    }
}
```

Output:

```text
True
False
ababab
2
```

Classic `this`-parameter extension methods still work and are binary-compatible; extension blocks are the new, more general syntax.

🎯 **Say this in the interview:** "An extension method is a static method in a static class whose first parameter is marked `this`. The compiler rewrites `obj.Method()` to `StaticClass.Method(obj)`, so it adds no state, isn't virtual, and loses to an instance method with the same signature. LINQ is built entirely from them. C# 14 extends the idea to properties and static members via `extension` blocks."

## 11.2 `IEnumerable` / `IEnumerator` by hand vs `yield return`

**Simple words:**

- `IEnumerable<T>` = "something you **can loop over**" — it has one method, `GetEnumerator()`.
- `IEnumerator<T>` = the **cursor** doing the looping: `MoveNext()` (go to next, returns `false` at the end), `Current` (the item under the cursor), `Dispose()`.

**Real-life analogy:** a **book** (`IEnumerable`) and a **bookmark** (`IEnumerator`). Many readers can read the same book at once, each with their own bookmark.

`foreach` is just compiler sugar for `GetEnumerator()` + `while (MoveNext())` + `Dispose()`. Writing an enumerator by hand takes a whole class; `yield return` makes the compiler write it for you:

```csharp
foreach (var n in new CountdownManual(3)) Console.Write($"{n} ");
Console.WriteLine("<- manual IEnumerator");

foreach (var n in Countdown(3)) Console.Write($"{n} ");
Console.WriteLine("<- yield return");

// What foreach really compiles to:
using (IEnumerator<int> it = Countdown(2).GetEnumerator())
{
    while (it.MoveNext()) Console.Write($"{it.Current} ");
}
Console.WriteLine("<- hand-written foreach");

static IEnumerable<int> Countdown(int from)
{
    for (int i = from; i >= 1; i--) yield return i;
}

class CountdownManual(int from) : IEnumerable<int>
{
    public IEnumerator<int> GetEnumerator() => new Enumerator(from);
    System.Collections.IEnumerator System.Collections.IEnumerable.GetEnumerator() => GetEnumerator();

    private sealed class Enumerator(int start) : IEnumerator<int>
    {
        private readonly int _start = start;
        private int _current = start + 1;      // "before the first element"

        public int Current => _current;
        object System.Collections.IEnumerator.Current => Current;
        public bool MoveNext() => --_current >= 1;
        public void Reset() => _current = _start + 1;
        public void Dispose() { }
    }
}
```

Output:

```text
3 2 1 <- manual IEnumerator
3 2 1 <- yield return
2 1 <- hand-written foreach
```

| | Hand-written `IEnumerator<T>` | `yield return` iterator |
|---|---|---|
| Code | A class with state fields, `MoveNext`, `Current`, `Reset`, `Dispose` | A normal-looking method |
| State tracking | You do it | Compiler-generated state machine |
| `Reset()` | You implement it | Throws `NotSupportedException` |
| When to use | Rare: max performance struct enumerators (like `List<T>.Enumerator`) | Almost always |

## 11.3 `yield return`, `yield break`, laziness and infinite sequences

- `yield return x;` — "hand `x` to the caller and **pause** here. Continue from this exact line when they ask for the next item."
- `yield break;` — "the sequence is over" (like `return` for iterators).
- Iterators are **lazy**: nothing runs until the first `MoveNext()`, and only as much runs as the consumer asks for. This makes **infinite sequences** possible.
- If the consumer stops early (`break`, `First()`, `Take()`), `foreach` calls `Dispose()`, which runs your `finally` blocks.

```csharp
foreach (var n in Numbers())
{
    Console.WriteLine($"  got {n}");
    if (n == 2) break;                  // stop early: rest of the iterator never runs
}

Console.WriteLine(string.Join(",", Fibonacci().Take(10)));   // infinite, made finite by Take
Console.WriteLine(Fibonacci().First(f => f >= 100));
Console.WriteLine(string.Join(",", ReadUntilStop(["a", "b", "STOP", "c"])));

static IEnumerable<int> Numbers()
{
    try
    {
        Console.WriteLine("  [start]");
        yield return 1;
        Console.WriteLine("  [resumed after 1]");
        yield return 2;
        Console.WriteLine("  [resumed after 2]");    // never printed
        yield return 3;
    }
    finally { Console.WriteLine("  [finally: cleanup]"); }   // runs on break via Dispose()
}

static IEnumerable<long> Fibonacci()
{
    long a = 0, b = 1;
    while (true)
    {
        yield return a;
        (a, b) = (b, a + b);
    }
}

static IEnumerable<string> ReadUntilStop(IEnumerable<string> lines)
{
    foreach (var line in lines)
    {
        if (line == "STOP") yield break;     // end the sequence early
        yield return line;
    }
}
```

Output:

```text
  [start]
  got 1
  [resumed after 1]
  got 2
  [finally: cleanup]
0,1,1,2,3,5,8,13,21,34
144
a,b
```

Iterator restrictions: an iterator method must return `IEnumerable<T>`, `IEnumerator<T>` (or the non-generic versions / `IAsyncEnumerable<T>`); it can't have `ref`/`out` parameters; and `yield return` can't appear inside a `catch` block or a `try` that has a `catch` (it *can* be in `try`/`finally`).

### Trap: argument validation in an iterator is lazy too

```csharp
var seq = EveryOther<int>(null!);            // no exception here!
Console.WriteLine("Sequence created...");
try { seq.ToList(); }
catch (ArgumentNullException) { Console.WriteLine("...exception only when enumerated"); }

static IEnumerable<T> EveryOther<T>(IEnumerable<T> source)
{
    ArgumentNullException.ThrowIfNull(source);   // part of the lazy state machine!
    int i = 0;
    foreach (var item in source)
        if (i++ % 2 == 0) yield return item;
}
```

Output:

```text
Sequence created...
...exception only when enumerated
```

The **entire** method body — including the null check — becomes part of `MoveNext()`, so the bad call is only discovered much later, far away from the bug. Fix: split into a non-iterator wrapper that validates + a private/local iterator (exactly what `MyWhere` does in 10.6). The BCL's LINQ operators do the same.

## 11.4 How `yield` compiles into a state machine (simplified)

For

```csharp
static IEnumerable<int> Countdown(int from)
{
    for (int i = from; i >= 1; i--) yield return i;
}
```

the compiler generates roughly this (simplified, **not** the exact generated code — illustration only):

```csharp
static IEnumerable<int> Countdown(int from) => new CountdownIterator(from);

sealed class CountdownIterator(int from) : IEnumerable<int>, IEnumerator<int>
{
    private int _state = 0;     // 0 = not started, 1 = paused after a yield, -1 = finished
    private int _i;             // local variables become FIELDS so they survive between calls
    public int Current { get; private set; }

    public bool MoveNext()
    {
        switch (_state)
        {
            case 0:                       // first call: start of the method body
                _i = from;
                goto loopCheck;
            case 1:                       // resumed: continue right after 'yield return'
                _i--;
                goto loopCheck;
            default:
                return false;
        }
    loopCheck:
        if (_i >= 1)
        {
            Current = _i;                 // 'yield return i'
            _state = 1;                   // remember where we paused
            return true;
        }
        _state = -1;                      // end of method = end of sequence
        return false;
    }

    public IEnumerator<int> GetEnumerator() => this;   // real code creates a fresh copy if already used
    System.Collections.IEnumerator System.Collections.IEnumerable.GetEnumerator() => this;
    object System.Collections.IEnumerator.Current => Current;
    public void Reset() => throw new NotSupportedException();
    public void Dispose() => _state = -1;              // also runs 'finally' blocks in real code
}
```

Key ideas to explain:

1. Calling `Countdown(3)` runs **none** of your code — it just creates the state machine object.
2. Local variables and parameters become **fields**, so their values survive between `MoveNext()` calls.
3. Each `yield return` becomes "set `Current`, save `_state`, `return true`"; the `switch` jumps back to the right spot next time.
4. `async`/`await` uses the very same state-machine idea (with `MoveNext` driven by task continuations).

🎯 **Say this in the interview:** "`yield return` makes the compiler generate a class implementing `IEnumerable<T>` and `IEnumerator<T>`. My locals become fields, and `MoveNext` is a switch on a state number that resumes right after the last `yield`. That's why iterators are lazy, why the body doesn't run until enumeration, and why `finally` blocks run on `Dispose`."

## 11.5 `IAsyncEnumerable<T>` (C# 8+) — streaming with `await`

When each item needs an `await` (paging through an API, reading a DB cursor, streaming from gRPC), combine `async` and `yield return`, and consume with `await foreach`:

```csharp
await foreach (var price in StreamPricesAsync())
    Console.WriteLine($"Price: {price}");

static async IAsyncEnumerable<decimal> StreamPricesAsync()
{
    decimal[] prices = [101.5m, 102.25m, 99.75m];
    foreach (var p in prices)
    {
        await Task.Delay(100);     // e.g. waiting on a network call
        yield return p;
    }
}
```

Output:

```text
Price: 101.5
Price: 102.25
Price: 99.75
```

EF Core's `AsAsyncEnumerable()`, ASP.NET Core streaming responses and `Channel<T>.Reader.ReadAllAsync()` all use it. Pass a `CancellationToken` with `[EnumeratorCancellation]` on the parameter to support cancellation.

## Try it yourself

1. Write a generic extension `IsBetween(min, max)` that works for `int`, `char` and `DateOnly`.
2. Write a `Pairwise()` iterator that yields `(previous, current)` tuples, and use it to print the differences between consecutive squares `[1, 4, 9, 16]`.
3. Write an **infinite** prime number generator and take the first 10.
4. What's the output?

   ```csharp
   var seq = Greet();
   Console.WriteLine("A");
   foreach (var _ in seq) { }
   Console.WriteLine("C");

   static IEnumerable<int> Greet() { Console.WriteLine("B"); yield break; }
   ```

5. Write a `ForEach((item, index) => …)` extension for `IEnumerable<T>`. Why isn't `ForEach` built into LINQ?

<details><summary>Answer</summary>

**1.**

```csharp
Console.WriteLine(5.IsBetween(1, 10));
Console.WriteLine('m'.IsBetween('a', 'f'));
Console.WriteLine(new DateOnly(2024, 6, 1).IsBetween(new(2024, 1, 1), new(2024, 12, 31)));

public static class ComparableExtensions
{
    public static bool IsBetween<T>(this T value, T min, T max) where T : IComparable<T> =>
        value.CompareTo(min) >= 0 && value.CompareTo(max) <= 0;
}
```

```text
True
False
True
```

**2.**

```csharp
int[] squares = [1, 4, 9, 16];
Console.WriteLine(string.Join(" ", squares.Pairwise().Select(p => p.Current - p.Previous)));

public static class SeqExtensions
{
    public static IEnumerable<(T Previous, T Current)> Pairwise<T>(this IEnumerable<T> source)
    {
        using var e = source.GetEnumerator();
        if (!e.MoveNext()) yield break;
        T prev = e.Current;
        while (e.MoveNext())
        {
            yield return (prev, e.Current);
            prev = e.Current;
        }
    }
}
```

```text
3 5 7
```

**3.**

```csharp
Console.WriteLine(string.Join(",", Primes().Take(10)));

static IEnumerable<int> Primes()
{
    for (int n = 2; ; n++)
    {
        bool isPrime = true;
        for (int d = 2; d * d <= n; d++)
            if (n % d == 0) { isPrime = false; break; }
        if (isPrime) yield return n;
    }
}
```

```text
2,3,5,7,11,13,17,19,23,29
```

**4.** `B` is printed only when enumeration begins — calling `Greet()` runs nothing:

```text
A
B
C
```

**5.**

```csharp
new[] { "x", "y" }.ForEach((s, i) => Console.WriteLine($"{i}: {s}"));

public static class EnumerableExtensions
{
    public static void ForEach<T>(this IEnumerable<T> source, Action<T, int> action)
    {
        int i = 0;
        foreach (var item in source) action(item, i++);
    }
}
```

```text
0: x
1: y
```

LINQ operators are meant to be **side-effect-free queries** that return new sequences; a `ForEach` exists only for side effects, and a plain `foreach` loop is clearer (and supports `break`, `await`, etc.). `List<T>.ForEach` exists, but many style guides discourage it.

</details>

## Quick revision

- Extension method = static method in a static class, first parameter marked `this`.
- `obj.Ext()` compiles to `StaticClass.Ext(obj)`; works on `null`, isn't virtual, can't add state.
- Instance methods beat extension methods with the same signature.
- C# 14 (.NET 10) `extension(T x) { … }` blocks add extension properties and static members.
- `IEnumerable<T>` = can be looped (`GetEnumerator`); `IEnumerator<T>` = the cursor (`MoveNext`, `Current`).
- `foreach` = `GetEnumerator` + `while (MoveNext())` + `Dispose` in a `finally`.
- `yield return` pauses and resumes; `yield break` ends the sequence.
- Iterators are lazy → infinite sequences are fine with `Take`/`First`.
- Breaking out of `foreach` calls `Dispose()`, which runs the iterator's `finally` blocks.
- Validation inside an iterator is deferred — use a wrapper + local iterator.
- The compiler turns iterators into a state machine class (locals → fields, `switch` on state).
- `IAsyncEnumerable<T>` + `await foreach` = async streaming (C# 8+).

---

# 12. Exception Handling

> **Why interviewers ask this:** Every production bug eventually shows up as an exception. Interviewers want to know if you can (1) read the flow of `try/catch/finally` without guessing, (2) preserve stack traces so bugs can be diagnosed, and (3) decide *when* an exception is the right tool versus a `TryXxx` / Result return. `throw` vs `throw ex` is asked in almost every .NET interview.

## 12.1 What is an exception? (simple words)

An **exception** is an object that says *"something went wrong and I can't continue normally"*. When it is thrown, the runtime **stops executing the current code** and walks *up* the call stack looking for a `catch` that matches. If nobody catches it, the process crashes.

🏠 **Analogy:** You are cooking (method A) following a recipe your mom gave you (method B called A). You discover there is no gas. You can't fix that yourself, so you *shout* ("throw"). Whoever in the house knows how to handle "no gas" (a matching `catch`) responds. Whatever happens, you *turn off the stove* before leaving the kitchen (`finally`).

```
Main()  ──calls──►  PlaceOrder()  ──calls──►  SaveToDb()   💥 throw
  ▲ catch here?          ▲ catch here?            │
  └──────────────────────┴──── the exception bubbles UP ◄┘
```

## 12.2 `try / catch / finally` flow

| Block | Runs when | Typical use |
|---|---|---|
| `try` | Always (first) | Code that might fail |
| `catch (T ex)` | Only if an exception of type `T` (or derived) is thrown inside `try` | Recover, log, translate |
| `finally` | **Always** – success, exception, `return`, `break`, `continue` | Release resources, reset state |

**Easy example**

```csharp
try
{
    int[] arr = new int[2];
    arr[5] = 1;                          // throws IndexOutOfRangeException
    Console.WriteLine("not printed");
}
catch (IndexOutOfRangeException ex)
{
    Console.WriteLine($"Caught: {ex.Message}");
}
finally
{
    Console.WriteLine("Cleanup always runs");
}
```

### `finally` with `return` — the classic trick question

```csharp
Console.WriteLine(Demo());
Console.WriteLine(string.Join(",", ListDemo()));

static int Demo()
{
    int x = 1;
    try
    {
        Console.WriteLine("try");
        return x;          // value 1 is captured NOW
    }
    finally
    {
        x = 99;            // changes the variable, not the captured return value
        Console.WriteLine("finally");
    }
}

static List<int> ListDemo()
{
    var list = new List<int> { 1 };
    try { return list; }           // reference captured
    finally { list.Add(2); }       // same object is mutated -> caller sees 2
}
```

Output (verified):
```
try
finally
1
1,2
```

Why? `return x;` copies the **value** into a hidden slot, then `finally` runs. Changing `x` afterwards does nothing. For a reference type the hidden slot holds the **reference**, so mutating the object *is* visible.

⚠️ **Interview trap:** You **cannot** `return` from inside a `finally` block in C# (error CS0157). So "which return wins?" questions from Java don't apply.

⚠️ **Interview trap:** `finally` does **not** run if the process is killed (`Environment.FailFast`, `StackOverflowException`, task manager kill, power loss).

🧩 **Predict the output:**

```csharp
Console.WriteLine(F());

static string F()
{
    try
    {
        Console.WriteLine("A");
        throw new Exception("boom");
    }
    catch (Exception)
    {
        Console.WriteLine("B");
        return "C";
    }
    finally
    {
        Console.WriteLine("D");
    }
}
```

<details><summary>Answer</summary>

```
A
B
D
C
```
`catch` decides the return value "C", then `finally` prints "D", and only *after* that does the caller print "C".
</details>

## 12.3 Exception hierarchy

```
System.Object
└── System.Exception
    ├── System.SystemException            (thrown by the runtime)
    │   ├── NullReferenceException
    │   ├── IndexOutOfRangeException
    │   ├── InvalidCastException
    │   ├── InvalidOperationException
    │   │   └── ObjectDisposedException
    │   ├── ArgumentException
    │   │   ├── ArgumentNullException
    │   │   └── ArgumentOutOfRangeException
    │   ├── FormatException
    │   ├── ArithmeticException
    │   │   └── DivideByZeroException / OverflowException
    │   ├── IOException → FileNotFoundException, DirectoryNotFoundException
    │   ├── OperationCanceledException → TaskCanceledException
    │   ├── OutOfMemoryException, StackOverflowException (cannot be caught)
    │   └── ...
    ├── AggregateException                 (many exceptions in one, from tasks)
    ├── HttpRequestException
    └── ApplicationException               (legacy – don't derive from it)
```

| Exception | Throw it when… |
|---|---|
| `ArgumentNullException` | A required parameter is `null` (`ArgumentNullException.ThrowIfNull(x)`) |
| `ArgumentOutOfRangeException` | Value outside allowed range (`ThrowIfNegative`, `ThrowIfZero`… .NET 8+) |
| `ArgumentException` | Argument is invalid for other reasons |
| `InvalidOperationException` | Object is in the wrong *state* ("Order already shipped") |
| `NotSupportedException` | Operation not supported ("read-only collection") |
| `NotImplementedException` | Placeholder only – never ship it |
| `ObjectDisposedException` | Used after `Dispose()` |
| `KeyNotFoundException` | Dictionary indexer with missing key |
| `TimeoutException` | Operation timed out |

### Catch order: specific → general

The first matching `catch` wins, so specific types must come first. The compiler protects you:

```csharp
try
{
    int.Parse("x");
}
catch (Exception ex) { }
catch (FormatException ex) { }   // ❌ does not compile: CS0160
```

`CS0160: A previous catch clause already catches all exceptions of this or of a super type ('Exception')`.

## 12.4 `throw` vs `throw ex` vs `throw new X(..., inner)`

| Syntax | Stack trace | Use when |
|---|---|---|
| `throw;` | ✅ Preserved (points to the original line) | Log & rethrow, or partial handling |
| `throw ex;` | ❌ **Reset** – starts at this line; original location lost | Practically never |
| `throw new XException("context", ex);` | ✅ Original kept in `InnerException` | Add business context / translate layers |
| `ExceptionDispatchInfo.Capture(ex).Throw();` | ✅ Preserved, even on another thread | Advanced: rethrow a stored exception later |

**Runnable demo**

```csharp
try
{
    Service.ThrowEx();
}
catch (Exception e) { Console.WriteLine("--- throw ex ---\n" + e.StackTrace); }

try
{
    Service.ThrowOnly();
}
catch (Exception e) { Console.WriteLine("--- throw; ---\n" + e.StackTrace); }

try
{
    Service.Wrap();
}
catch (Exception e)
{
    Console.WriteLine("--- wrap ---");
    Console.WriteLine($"{e.GetType().Name}: {e.Message}");
    Console.WriteLine($"Inner: {e.InnerException?.GetType().Name}: {e.InnerException?.Message}");
}

static class Service
{
    static void Deep() => throw new InvalidOperationException("DB is down");

    public static void ThrowEx()
    {
        try { Deep(); }
        catch (Exception ex) { throw ex; }   // ❌ resets stack trace
    }

    public static void ThrowOnly()
    {
        try { Deep(); }
        catch (Exception) { throw; }         // ✅ keeps original stack trace
    }

    public static void Wrap()
    {
        try { Deep(); }
        catch (Exception ex) { throw new ApplicationException("Could not load order 42", ex); }
    }
}
```

Output (verified, paths shortened):
```
--- throw ex ---
   at Service.ThrowEx() in ...\Program.cs:line 31
   at Program.<Main>$(String[] args) in ...\Program.cs:line 3
--- throw; ---
   at Service.Deep() in ...\Program.cs:line 26
   at Service.ThrowOnly() in ...\Program.cs:line 36
   at Program.<Main>$(String[] args) in ...\Program.cs:line 9
--- wrap ---
ApplicationException: Could not load order 42
Inner: InvalidOperationException: DB is down
```

Notice: with `throw ex` the frame `Service.Deep()` — the **real** culprit — has disappeared. In production that means hours of extra debugging.

🎯 **Say this in the interview:** "`throw;` rethrows the same exception and keeps the original stack trace. `throw ex;` resets the stack trace to the current line, so we lose where it really failed. If I need to add context I wrap it: `throw new MyException("context", ex)` so the original is preserved as `InnerException`."

## 12.5 Exception filters (`when`) — C# 6

A filter is a boolean condition evaluated **before** the stack unwinds. If it returns `false`, the `catch` is skipped as if it didn't exist.

🏠 **Analogy:** A receptionist who only accepts parcels addressed to "HR"; everything else keeps moving down the corridor untouched.

**Easy:**
```csharp
catch (HttpRequestException ex) when (ex.StatusCode == HttpStatusCode.NotFound) { ... }
```

**Complex — filtering by status code + the "log without catching" trick:**

```csharp
using System.Net;

foreach (var code in new[] { HttpStatusCode.NotFound, HttpStatusCode.ServiceUnavailable, HttpStatusCode.BadRequest })
{
    try
    {
        try
        {
            throw new HttpRequestException("call failed", null, code);
        }
        catch (Exception ex) when (Log(ex))      // never catches, just logs
        {
        }
        catch (HttpRequestException ex) when (ex.StatusCode == HttpStatusCode.NotFound)
        {
            Console.WriteLine("  -> 404: return null");
        }
        catch (HttpRequestException ex) when (ex.StatusCode == HttpStatusCode.ServiceUnavailable)
        {
            Console.WriteLine("  -> 503: retry later");
        }
    }
    catch (HttpRequestException ex)
    {
        Console.WriteLine($"  -> outer caught {(int)ex.StatusCode!}");
    }
}

static bool Log(Exception ex)
{
    Console.WriteLine($"LOG: {ex.Message} ({(ex as HttpRequestException)?.StatusCode})");
    return false;   // false = "I don't want to catch it"
}
```

Output (verified):
```
LOG: call failed (NotFound)
  -> 404: return null
LOG: call failed (ServiceUnavailable)
  -> 503: retry later
LOG: call failed (BadRequest)
  -> outer caught 400
```

| `catch { if (!cond) throw; }` | `catch when (cond)` |
|---|---|
| Stack is unwound, then rethrown | Stack is **not** unwound if `false` |
| Debugger/crash dump shows the rethrow point | Crash dump shows the original throw point with locals intact |
| More code | Cleaner, declarative |

## 12.6 Custom exceptions — when & how

**When:** callers need to *catch it specifically* or need *extra data* (e.g. `Shortfall`). If no one will ever catch it differently, use a built-in type (`InvalidOperationException`, `ArgumentException`).

**How (checklist):**
1. Name ends with `Exception`.
2. Derive from `Exception` (not `ApplicationException`).
3. Provide the 3 standard constructors: `()`, `(string)`, `(string, Exception)`.
4. Add read-only properties for extra data.
5. (Binary serialization constructors are obsolete since .NET 8 – skip them.)

```csharp
var account = new BankAccount(100m);
try
{
    account.Withdraw(250m);
}
catch (InsufficientFundsException ex)
{
    Console.WriteLine(ex.Message);
    Console.WriteLine($"Short by {ex.Shortfall}");
}

public class InsufficientFundsException : Exception
{
    public decimal Balance { get; }
    public decimal Requested { get; }
    public decimal Shortfall => Requested - Balance;

    public InsufficientFundsException() { }
    public InsufficientFundsException(string message) : base(message) { }
    public InsufficientFundsException(string message, Exception inner) : base(message, inner) { }

    public InsufficientFundsException(decimal balance, decimal requested)
        : base($"Cannot withdraw {requested} - balance is only {balance}.")
    {
        Balance = balance;
        Requested = requested;
    }
}

public class BankAccount(decimal balance)
{
    public decimal Balance { get; private set; } = balance;

    public void Withdraw(decimal amount)
    {
        ArgumentOutOfRangeException.ThrowIfNegativeOrZero(amount);   // .NET 8+ guard helper
        if (amount > Balance) throw new InsufficientFundsException(Balance, amount);
        Balance -= amount;
    }
}
```

Output (verified):
```
Cannot withdraw 250 - balance is only 100.
Short by 150
```

## 12.7 Exceptions vs `TryXxx` vs Result pattern

🏠 **Analogy:** An exception is a **fire alarm** – loud, expensive, for unexpected events. A `TryXxx`/Result return is a **"sorry, out of stock" note** – a normal, expected outcome.

| Approach | Good for | Cost | Example |
|---|---|---|---|
| Exception | Truly *exceptional* / bug / infrastructure failure | High (stack capture, unwinding) | DB down, null argument |
| `TryXxx(out T)` | Expected failure, single value, no reason needed | ~free | `int.TryParse`, `dict.TryGetValue` |
| `Result<T>` | Expected failure **with a reason**, business validation | Small object/struct | "Email already exists" |

```csharp
using System.Globalization;
CultureInfo.CurrentCulture = CultureInfo.InvariantCulture;

// TryXxx pattern
if (int.TryParse("12x", out int n)) Console.WriteLine(n);
else Console.WriteLine("Not a number");

// Result pattern
var r1 = UserService.Register("bob@x.com");
var r2 = UserService.Register("oops");
foreach (var r in new[] { r1, r2 })
    Console.WriteLine(r.IsSuccess ? $"OK: id={r.Value}" : $"FAIL: {r.Error}");

public readonly record struct Result<T>(bool IsSuccess, T? Value, string? Error)
{
    public static Result<T> Ok(T value) => new(true, value, null);
    public static Result<T> Fail(string error) => new(false, default, error);
}

public static class UserService
{
    static int _nextId = 1;
    public static Result<int> Register(string email) =>
        email.Contains('@')
            ? Result<int>.Ok(_nextId++)
            : Result<int>.Fail($"'{email}' is not a valid email");
}
```

Output (verified):
```
Not a number
OK: id=1
FAIL: 'oops' is not a valid email
```

### Performance cost — runnable

```csharp
using System.Diagnostics;

const int N = 100_000;
string[] inputs = Enumerable.Range(0, N).Select(i => i % 2 == 0 ? "123" : "abc").ToArray();

// warm-up
foreach (var s in inputs.Take(100)) { int.TryParse(s, out _); try { int.Parse(s); } catch (FormatException) { } }

var sw = Stopwatch.StartNew();
int ok1 = 0;
foreach (var s in inputs)
{
    try { int.Parse(s); ok1++; }
    catch (FormatException) { }
}
Console.WriteLine($"Parse + catch : {sw.ElapsedMilliseconds,5} ms");

sw.Restart();
int ok2 = 0;
foreach (var s in inputs)
    if (int.TryParse(s, out _)) ok2++;
Console.WriteLine($"TryParse      : {sw.ElapsedMilliseconds,5} ms");
```

Representative output (your numbers will differ):
```
Parse + catch :   189 ms
TryParse      :     3 ms
```

50,000 thrown exceptions cost ~60× more. A `try` block that **doesn't** throw is almost free — the cost is in *throwing*.

🎯 **Say this in the interview:** "Exceptions are for exceptional situations. For expected failures like user input or 'not found' I use `TryXxx` or a Result type, because throwing captures a stack trace and unwinds the stack, which is orders of magnitude slower than returning a bool."

## 12.8 `AggregateException` and tasks

Tasks can fail with **several** exceptions at once (e.g. `Task.WhenAll`). They are stored in an `AggregateException`.

| How you wait | What you get |
|---|---|
| `await task` | The **first** inner exception, unwrapped (nice `catch` blocks) |
| `task.Wait()` / `task.Result` | `AggregateException` wrapping all of them |
| `task.Exception` | `AggregateException` with **all** inner exceptions |
| `task.GetAwaiter().GetResult()` | First exception unwrapped (but still blocking!) |

```csharp
Task t1 = Task.Run(() => throw new InvalidOperationException("A failed"));
Task t2 = Task.Run(() => throw new ArgumentException("B failed"));
Task all = Task.WhenAll(t1, t2);

// 1) await -> only the FIRST exception is thrown
try { await all; }
catch (Exception ex) { Console.WriteLine($"await caught: {ex.GetType().Name} - {ex.Message}"); }

// 2) the Task still holds ALL of them
Console.WriteLine($"all.Exception has {all.Exception!.InnerExceptions.Count} inner exceptions:");
foreach (var inner in all.Exception.InnerExceptions)
    Console.WriteLine($"  {inner.GetType().Name}: {inner.Message}");

// 3) .Wait() / .Result -> throws AggregateException
try { all.Wait(); }
catch (AggregateException ae)
{
    Console.WriteLine($".Wait() caught {ae.GetType().Name}");
    ae.Flatten().Handle(e => { Console.WriteLine($"  handled {e.GetType().Name}"); return true; });
}
```

Representative output (which exception is "first" can vary between runs):
```
await caught: ArgumentException - B failed
all.Exception has 2 inner exceptions:
  ArgumentException: B failed
  InvalidOperationException: A failed
.Wait() caught AggregateException
  handled ArgumentException
  handled InvalidOperationException
```

⚠️ **Interview trap:** `try { await Task.WhenAll(...) } catch (Exception ex)` silently "loses" all but one exception. Inspect `whenAllTask.Exception.InnerExceptions` if you need all of them (see §14.10).

## 12.9 Global (last-chance) handlers

| Handler | Fires when | Can it prevent the crash? |
|---|---|---|
| `AppDomain.CurrentDomain.UnhandledException` | Any unhandled exception on any thread | ❌ No – log & flush only |
| `TaskScheduler.UnobservedTaskException` | A faulted `Task` nobody observed is garbage-collected | ✅ Yes (since .NET 4.5 it doesn't crash by default; `e.SetObserved()`) |
| ASP.NET Core `UseExceptionHandler` / `IExceptionHandler` (.NET 8) | Unhandled exception in a request | ✅ Returns a 500/ProblemDetails |
| WPF `DispatcherUnhandledException`, WinForms `Application.ThreadException` | UI-thread exceptions | ✅ `e.Handled = true` |

```csharp
using System.Runtime.CompilerServices;

TaskScheduler.UnobservedTaskException += (_, e) =>
{
    Console.WriteLine($"Unobserved: {e.Exception.InnerException?.Message}");
    e.SetObserved();   // mark as handled
};

FireAndForget();
await Task.Delay(200);           // let the task fail
GC.Collect();                    // the faulted Task is garbage...
GC.WaitForPendingFinalizers();   // ...its finalizer raises the event
Console.WriteLine("App still alive");

[MethodImpl(MethodImplOptions.NoInlining)]
static void FireAndForget() => _ = Task.Run(() => throw new InvalidOperationException("nobody awaited me"));
```

Output (verified):
```
Unobserved: nobody awaited me
App still alive
```

## 12.10 Best practices checklist

1. ✅ Catch **specific** exceptions you can actually handle; let the rest bubble.
2. ✅ Use `throw;` to rethrow – never `throw ex;`.
3. ✅ Wrap with context: `throw new OrderException("Order 42 failed", ex);`.
4. ✅ Validate arguments early with guard helpers: `ArgumentNullException.ThrowIfNull(x)`, `ArgumentException.ThrowIfNullOrEmpty(s)`.
5. ✅ Use `using`/`finally` for cleanup, not `catch`.
6. ✅ Log **once**, at the boundary (controller/middleware/job), not at every layer ("log-and-rethrow" spam).
7. ✅ Use `TryXxx`/Result for expected failures; exceptions for exceptional ones.
8. ✅ Use filters (`when`) instead of catch-check-rethrow.
9. ❌ Never swallow: `catch { }` hides bugs.
10. ❌ Don't use exceptions for control flow (loops, validation of user input).
11. ❌ Don't catch `NullReferenceException`/`IndexOutOfRangeException` – fix the bug.
12. ❌ Don't throw `Exception` or `SystemException` directly – use a meaningful type.
13. ✅ In async code, let `OperationCanceledException` propagate (it's not an error).

## Try it yourself

1. What prints? `try { return 1; } finally { Console.WriteLine("F"); }` called via `Console.WriteLine(M());`
2. Write a filter that catches `IOException` only when the message contains "locked".
3. Why does `catch (Exception) {}` followed by `catch (IOException) {}` fail to compile?
4. You have a method `User? FindUser(int id)`. Should "user not found" throw? Justify.
5. A colleague writes `catch (Exception ex) { _log.Error(ex); throw ex; }`. Give two problems.
6. How do you see *all* exceptions from `await Task.WhenAll(a, b, c)`?

<details><summary>Answer</summary>

1. `F` then `1` – `finally` runs before the caller receives the value.
2. `catch (IOException ex) when (ex.Message.Contains("locked")) { ... }`
3. CS0160 – the earlier `catch (Exception)` already handles `IOException`, so the second is unreachable. Put specific types first.
4. No – "not found" is an expected outcome. Return `null` (or `TryFindUser(id, out user)` / `Result<User>`). Throw only if the caller's contract guarantees the user exists (e.g. `GetUser` → `KeyNotFoundException`).
5. (a) `throw ex` resets the stack trace; use `throw;`. (b) Logging at every layer produces duplicate logs; log at the boundary or use a `when (Log(ex))` filter.
6. Keep the `Task` returned by `WhenAll`, `try { await whenAll; } catch { foreach (var e in whenAll.Exception!.InnerExceptions) ... }`.
</details>

## Quick revision

- Exception = object that unwinds the stack until a matching `catch` is found.
- `finally` always runs (except process kill / FailFast / StackOverflow).
- `return` value is captured before `finally`; mutations to reference objects are still visible.
- Catch specific → general; otherwise CS0160.
- `throw;` keeps stack trace; `throw ex;` resets it; wrap with `inner` for context.
- `when` filters run before unwinding; `false` = skip this catch.
- Custom exception: suffix `Exception`, derive from `Exception`, 3 constructors, extra properties.
- Exceptions are expensive to *throw*; `try` itself is cheap.
- Expected failures → `TryXxx` / Result pattern.
- `await` throws the first inner exception; `.Wait()/.Result` throw `AggregateException`.
- `AppDomain.UnhandledException` = log-only last chance; `TaskScheduler.UnobservedTaskException` = unobserved faulted tasks.
- Never `catch {}` silently; log once at the boundary.

# 13. Memory Management, GC & IDisposable

> **Why interviewers ask this:** "C# has a garbage collector, so I never think about memory" is a red flag. Interviewers want to see that you know *what* the GC cleans up (managed memory) and *what it doesn't* (files, sockets, DB connections, unmanaged handles), that you use `using` correctly, and that you can spot the classic managed memory leaks (events, static caches, timers).

## 13.1 Stack vs managed heap (quick recap)

| | Stack | Managed heap |
|---|---|---|
| Stores | Local value-type variables, references (the "address"), method frames | Objects of reference types (`class`, arrays, strings, boxed values) |
| Lifetime | Freed automatically when the method returns | Freed by the **GC** when no longer reachable |
| Speed | Extremely fast (move a pointer) | Allocation fast (bump pointer), collection costs time |
| Size | Small (~1 MB per thread) | Large (GBs) |

```csharp
void M()
{
    int age = 30;                  // 'age' lives on the stack
    var p = new Person("Ann");     // 'p' (reference) on stack, Person object on heap
    Point pt = new Point(1, 2);    // struct: stored inline on the stack
}                                  // stack frame gone; Person becomes garbage
```

⚠️ **Interview trap:** "Value types always live on the stack" is **wrong**. A struct that is a *field of a class*, an *array element*, a *captured lambda variable* or *boxed* lives on the heap. Correct: "value types are stored **inline** where they are declared".

### struct vs class allocation

| | `struct` (value type) | `class` (reference type) |
|---|---|---|
| Allocation | Inline (stack/inside parent) – no GC pressure | Heap – GC must track & collect it |
| Copy semantics | Whole value copied on assignment | Reference copied |
| Good for | Small (≤16 bytes guideline), immutable data: `Point`, `Money`, `DateTime` | Entities, behaviour, large data |
| Pitfall | Boxing (cast to `object`/interface) allocates! Large structs = expensive copies | Many tiny short-lived objects = GC work |

## 13.2 What does the Garbage Collector do? (simple words)

🏠 **Analogy:** The heap is a **restaurant**. Objects are customers at tables. The GC is the **waiter** who periodically checks which tables are abandoned (no one references them), clears them, and pushes the remaining customers together so there's one big free area at the end (compaction).

The GC answers one question: **"Which objects can my program still reach?"** Everything else is garbage.

### Mark & compact in 3 steps

```
Roots: static fields, local variables on stacks, CPU registers, GC handles
            │
1. MARK     ▼  follow every reference from the roots, mark reachable objects
   Heap: [A✔][B ][C✔][D ][E✔][F ]

2. SWEEP    unmarked objects (B, D, F) are garbage

3. COMPACT  slide live objects together & update all references
   Heap: [A][C][E][..........free..........]
                  ▲ next allocation pointer ("bump pointer" = very fast new)
```

- A **cycle** (A → B → A) with no root reference is still collected – .NET does **not** use reference counting.
- During compaction, managed threads are briefly **paused** ("stop the world"), which is why allocation-heavy code can cause latency spikes.

## 13.3 Generations 0 / 1 / 2 + LOH + POH

**Generational hypothesis:** *most objects die young* (temporary strings, DTOs, LINQ iterators). So the GC checks young objects often and old objects rarely.

| Area | Contains | Collected |
|---|---|---|
| **Gen 0** | Brand-new objects | Very often, very fast (ms or less) |
| **Gen 1** | Survived one Gen 0 GC – "buffer" between young & old | Often |
| **Gen 2** | Long-lived objects (caches, singletons, static data) | Rarely – "full GC", most expensive |
| **LOH** (Large Object Heap) | Objects **≥ 85,000 bytes** (big arrays/strings) | Only with Gen 2; **not compacted** by default → fragmentation |
| **POH** (Pinned Object Heap, .NET 5+) | Objects allocated as pinned (`GC.AllocateArray(..., pinned: true)`) for native interop | With Gen 2; never moved |

🏠 **Analogy:** Gen 0 = your desk (cleaned every hour), Gen 1 = drawer (cleaned daily), Gen 2 = attic (cleaned once a year). The LOH is the garage for furniture too big to move around.

```csharp
var obj = new object();
Console.WriteLine($"New object      -> gen {GC.GetGeneration(obj)}");
GC.Collect();
Console.WriteLine($"After 1 GC      -> gen {GC.GetGeneration(obj)}");
GC.Collect();
Console.WriteLine($"After 2 GCs     -> gen {GC.GetGeneration(obj)}");
GC.Collect();
Console.WriteLine($"After 3 GCs     -> gen {GC.GetGeneration(obj)} (max is {GC.MaxGeneration})");

var small = new byte[84_000];
var big = new byte[85_000];
Console.WriteLine($"84,000 bytes    -> gen {GC.GetGeneration(small)}");
Console.WriteLine($"85,000 bytes    -> gen {GC.GetGeneration(big)} (LOH is reported as gen 2)");

var pinned = GC.AllocateArray<byte>(1000, pinned: true);
Console.WriteLine($"Pinned (POH)    -> gen {GC.GetGeneration(pinned)}");

Console.WriteLine($"Server GC? {System.Runtime.GCSettings.IsServerGC}");
GC.KeepAlive(obj);
```

Output (verified on .NET 10, console app):
```
New object      -> gen 0
After 1 GC      -> gen 1
After 2 GCs     -> gen 2
After 3 GCs     -> gen 2 (max is 2)
84,000 bytes    -> gen 0
85,000 bytes    -> gen 2 (LOH is reported as gen 2)
Pinned (POH)    -> gen 2
Server GC? False
```

### When does the GC run?
1. Gen 0 budget is exhausted (most common – triggered by *allocations*).
2. The OS signals low memory.
3. `GC.Collect()` is called (avoid in production code – almost always makes things worse).
4. Container memory limit is approached (GC is container-aware).

⚠️ **Interview trap:** The GC is **non-deterministic** – you can't know *when* an object will be collected. That's exactly why we need `IDisposable` for resources that must be released *now*.

### Workstation vs Server GC (brief)

| | Workstation GC | Server GC |
|---|---|---|
| Default for | Console, desktop apps | ASP.NET Core apps |
| Heaps | One | One per CPU core |
| GC threads | One | One per core, run in parallel |
| Trade-off | Lower memory, less throughput | Higher throughput, more memory |
| Concurrent/background GC | On by default (Gen 2 collected in background) | On by default |

Configure with `<ServerGarbageCollection>true</ServerGarbageCollection>` in the `.csproj`. .NET 8+ also has **DATAS** (dynamic adaptation), enabled by default for Server GC in .NET 9+, which shrinks heap count for small workloads.

🎯 **Say this in the interview:** "The GC is generational: new objects go to Gen 0, survivors are promoted to Gen 1 and Gen 2, because most objects die young. It marks everything reachable from roots, then compacts the survivors. Objects of 85 KB or more go to the Large Object Heap, which is collected with Gen 2 and not compacted by default."

## 13.4 `IDisposable` & `using`

The GC frees **memory**. It does **not** know that your object holds a file handle, socket, DB connection or lock. `IDisposable.Dispose()` is the "release it NOW" button.

🏠 **Analogy:** Returning a library book. The GC is the library eventually noticing a book is missing; `Dispose()` is you returning it on time so the next person can borrow it.

### Forms of `using`

```csharp
// 1) using statement (classic) – disposed at the closing brace
using (var conn = new SqlConnection(cs))
{
    conn.Open();
}

// 2) using declaration (C# 8) – disposed at end of the enclosing scope
using var file = File.OpenRead("data.txt");

// 3) await using (C# 8) – calls DisposeAsync for IAsyncDisposable
await using var stream = new FileStream("log.txt", FileMode.Append);
```

`using` is compiler sugar for:
```csharp
var conn = new SqlConnection(cs);
try { conn.Open(); }
finally { conn?.Dispose(); }
```

**Runnable: disposal order**

```csharp
await RunAsync();

static async Task RunAsync()
{
    using (var a = new Res("A"))
    {
        Console.WriteLine("inside using block");
    }                                        // A disposed here

    using var b = new Res("B");              // C# 8 using declaration
    using var c = new Res("C");
    await using var d = new AsyncRes("D");   // IAsyncDisposable
    Console.WriteLine("end of method body");
}                                            // disposed in REVERSE order: D, C, B

class Res(string name) : IDisposable
{
    public void Dispose() => Console.WriteLine($"Dispose {name}");
}

class AsyncRes(string name) : IAsyncDisposable
{
    public async ValueTask DisposeAsync()
    {
        await Task.Delay(10);   // e.g. flush a buffer to the network
        Console.WriteLine($"DisposeAsync {name}");
    }
}
```

Output (verified):
```
inside using block
Dispose A
end of method body
DisposeAsync D
Dispose C
Dispose B
```

| `IDisposable` | `IAsyncDisposable` (.NET Core 3.0+) |
|---|---|
| `void Dispose()` | `ValueTask DisposeAsync()` |
| `using` | `await using` |
| Cleanup is quick / synchronous | Cleanup does I/O (flush to network/disk) – e.g. `Utf8JsonWriter`, `DbConnection`, `Stream` |

## 13.5 The full Dispose pattern (finalizer + `SuppressFinalize`)

A **finalizer** (`~ClassName()`) is a safety net the GC runs *sometime* after the object becomes unreachable – only needed if your class **directly owns an unmanaged resource** (raw `IntPtr` handle, native memory).

```csharp
using System.Runtime.InteropServices;

Make(dispose: true);
Make(dispose: false);
GC.Collect();
GC.WaitForPendingFinalizers();
Console.WriteLine("done");

static void Make(bool dispose)
{
    var r = new NativeBuffer(dispose ? "disposed-properly" : "forgotten");
    if (dispose) r.Dispose();
}

public class NativeBuffer : IDisposable
{
    private IntPtr _buffer;                 // unmanaged memory
    private readonly MemoryStream _stream;  // managed IDisposable
    private readonly string _name;
    private bool _disposed;

    public NativeBuffer(string name)
    {
        _name = name;
        _buffer = Marshal.AllocHGlobal(1024);
        _stream = new MemoryStream();
    }

    public void Dispose()
    {
        Dispose(disposing: true);
        GC.SuppressFinalize(this);          // finalizer no longer needed
    }

    protected virtual void Dispose(bool disposing)
    {
        if (_disposed) return;
        if (disposing)
        {
            _stream.Dispose();              // managed: only when called from Dispose()
        }
        Marshal.FreeHGlobal(_buffer);       // unmanaged: always
        _buffer = IntPtr.Zero;
        _disposed = true;
        Console.WriteLine($"{_name}: Dispose(disposing: {disposing})");
    }

    ~NativeBuffer() => Dispose(disposing: false);   // safety net

    public void Write(byte b)
    {
        ObjectDisposedException.ThrowIf(_disposed, this);
        Marshal.WriteByte(_buffer, b);
    }
}
```

Output (verified):
```
disposed-properly: Dispose(disposing: True)
forgotten: Dispose(disposing: False)
done
```

The "forgotten" object was still cleaned up – by the finalizer, at an unpredictable time.

**Why `disposing`?** When the finalizer runs, other managed objects you reference (like `_stream`) may *already be finalized*, so you must touch only unmanaged state. When `Dispose()` is called by user code, everything is still alive, so you clean both.

**Why `GC.SuppressFinalize(this)`?** Objects with a finalizer survive an extra GC (they go to the *finalization queue*, are finalized on a dedicated thread, then collected next time → promoted to an older generation). If we've already cleaned up, `SuppressFinalize` removes the object from that queue so it dies cheaply.

### Prefer `SafeHandle` (modern advice)

Instead of writing a finalizer yourself, wrap the native handle in a `SafeHandle` subclass (e.g. `SafeFileHandle`, or your own `SafeHandleZeroOrMinusOneIsInvalid`). It already has a reliable, critical finalizer. Then your class only needs the simple pattern:

```csharp
public sealed class MyFile : IDisposable
{
    private readonly SafeFileHandle _handle;
    public MyFile(string path) => _handle = File.OpenHandle(path);   // .NET 6+
    public void Dispose() => _handle.Dispose();                        // no finalizer needed
}
```

| | Finalizer (`~T()`) | `Dispose()` |
|---|---|---|
| Called by | GC (finalizer thread) | Your code / `using` |
| When | Non-deterministic (maybe never if process exits abruptly) | Deterministic – exactly when you say |
| Cleans | Unmanaged resources only | Managed + unmanaged |
| Performance | Costly (object survives extra GC) | Cheap |
| Needed when | Class **directly** owns unmanaged handle (rare; prefer SafeHandle) | Class owns *anything* disposable |

⚠️ **Interview trap:** "A finalizer is the same as a C++ destructor." No – C++ destructors are deterministic; .NET finalizers are not. The deterministic equivalent is `Dispose` + `using`.

⚠️ **Interview trap:** `Dispose` must be **idempotent** (safe to call twice) and should never throw.

🎯 **Say this in the interview:** "The GC only manages memory, so anything holding OS resources implements `IDisposable` and I release it with `using`. A finalizer is just a non-deterministic safety net for unmanaged handles; in the full pattern, `Dispose()` calls `Dispose(true)` and `GC.SuppressFinalize(this)` so the object doesn't pay the finalization cost. In modern code I wrap handles in `SafeHandle` instead of writing finalizers."

## 13.6 Memory leaks in managed code

A "leak" in .NET = objects that are **still reachable** (so the GC can't collect them) but that you no longer need.

| Leak source | Why it leaks | Fix |
|---|---|---|
| **Event handlers** | Publisher's delegate holds a reference to the subscriber | Unsubscribe (`-=`) in `Dispose`, or weak events |
| **Static collections / caches** | Statics are GC roots – live forever | Bounded cache (`MemoryCache` with size/expiry), remove entries |
| **Timers** | A running `System.Threading.Timer`/`System.Timers.Timer` keeps its callback target alive | `Dispose()` the timer |
| **Closures captured by long-lived delegates** | Lambda captures `this` or a big object | Capture only what you need |
| **Undisposed `IDisposable`** | Native memory/handles not freed (e.g. `HttpClient` per request, `Bitmap`) | `using`; reuse `HttpClient` via `IHttpClientFactory` |
| **`async` tasks never completing** | State machine + locals stay alive | Timeouts / cancellation |

**Runnable: the event-handler leak**

```csharp
using System.Runtime.CompilerServices;

var publisher = new Publisher();
WeakReference weak = Subscribe(publisher, unsubscribe: false);
GC.Collect(); GC.WaitForPendingFinalizers(); GC.Collect();
Console.WriteLine($"Forgot to unsubscribe -> subscriber alive? {weak.IsAlive}");

var publisher2 = new Publisher();
WeakReference weak2 = Subscribe(publisher2, unsubscribe: true);
GC.Collect(); GC.WaitForPendingFinalizers(); GC.Collect();
Console.WriteLine($"Unsubscribed          -> subscriber alive? {weak2.IsAlive}");
GC.KeepAlive(publisher); GC.KeepAlive(publisher2);

[MethodImpl(MethodImplOptions.NoInlining)]
static WeakReference Subscribe(Publisher p, bool unsubscribe)
{
    var s = new Subscriber(p);
    if (unsubscribe) s.Unsubscribe();
    return new WeakReference(s);
}

public class Publisher
{
    public event EventHandler? PriceChanged;
    public void Raise() => PriceChanged?.Invoke(this, EventArgs.Empty);
}

public class Subscriber
{
    private readonly Publisher _p;
    private readonly byte[] _bigData = new byte[10_000_000];
    public Subscriber(Publisher p) { _p = p; _p.PriceChanged += OnPrice; }
    public void Unsubscribe() => _p.PriceChanged -= OnPrice;
    private void OnPrice(object? s, EventArgs e) { }
}
```

Output (verified, Release build):
```
Forgot to unsubscribe -> subscriber alive? True
Unsubscribed          -> subscriber alive? False
```

The 10 MB subscriber stays alive as long as the publisher lives, purely because of `+=`. Imagine a long-lived singleton publisher and thousands of short-lived view-models subscribing – classic WPF/Blazor leak.

**Static cache leak (and the fix):**

```csharp
// ❌ grows forever – statics are GC roots
public static class ReportCache
{
    private static readonly Dictionary<int, byte[]> _cache = new();
    public static byte[] Get(int id) => _cache.TryGetValue(id, out var r) ? r : _cache[id] = Load(id);
}

// ✅ bounded + expiring (Microsoft.Extensions.Caching.Memory)
var cache = new MemoryCache(new MemoryCacheOptions { SizeLimit = 100 });
cache.Set(id, report, new MemoryCacheEntryOptions { Size = 1, SlidingExpiration = TimeSpan.FromMinutes(5) });
```

**Timer leak:** `new System.Threading.Timer(_ => this.Refresh(), null, 0, 1000);` – if nobody disposes it, the timer (rooted by the runtime) keeps `this` alive and keeps firing forever. Store it in a field and `Dispose()` it.

## 13.7 `WeakReference` — "I'd like it, but don't keep it alive for me"

A weak reference lets you point to an object **without** preventing the GC from collecting it. Useful for caches of expensive-but-recreatable data and weak event patterns.

```csharp
using System.Runtime.CompilerServices;

var cache = new WeakCache();
Fill(cache);
Console.WriteLine($"Before GC: {Check(cache)}");
GC.Collect();
Console.WriteLine($"After GC : {Check(cache)}");

[MethodImpl(MethodImplOptions.NoInlining)]
static void Fill(WeakCache c) => c.Add("report", new byte[50_000_000]);

[MethodImpl(MethodImplOptions.NoInlining)]
static string Check(WeakCache c) => c.TryGet("report", out _) ? "hit" : "miss (GC reclaimed it)";

class WeakCache
{
    private readonly Dictionary<string, WeakReference<byte[]>> _items = new();
    public void Add(string key, byte[] value) => _items[key] = new WeakReference<byte[]>(value);
    public bool TryGet(string key, out byte[]? value)
    {
        value = null;
        return _items.TryGetValue(key, out var w) && w.TryGetTarget(out value);
    }
}
```

Output (verified, Release build):
```
Before GC: hit
After GC : miss (GC reclaimed it)
```

⚠️ Results with weak references/`IsAlive` can differ in **Debug** builds, because the JIT extends local lifetimes for the debugger.

## 13.8 Low-allocation code: `Span<T>` & `stackalloc` (brief)

`Span<T>` is a **view** over contiguous memory (array, stack memory, part of a string) – no copying, no allocation. It's a `ref struct`, so it can only live on the stack (can't be a field of a class, can't be used across `await`).

🏠 **Analogy:** Instead of photocopying pages 10–20 of a book (`Substring`/`Split` allocate new strings), you put two bookmarks in it (`Span` slice).

```csharp
string csv = "10,20,30,40";

// Classic: Split allocates a string[] + 4 strings
int sum1 = csv.Split(',').Sum(int.Parse);

// Span: zero heap allocations
int sum2 = 0;
ReadOnlySpan<char> span = csv;
foreach (Range r in span.Split(','))           // .NET 8+ MemoryExtensions.Split
    sum2 += int.Parse(span[r]);

Console.WriteLine($"{sum1} {sum2}");

// stackalloc: small temporary buffer on the stack
Span<int> squares = stackalloc int[5];
for (int i = 0; i < squares.Length; i++) squares[i] = i * i;
Console.WriteLine(string.Join(" ", squares.ToArray()));

long before = GC.GetAllocatedBytesForCurrentThread();
int sum3 = 0;
foreach (Range r in span.Split(',')) sum3 += int.Parse(span[r]);
long after = GC.GetAllocatedBytesForCurrentThread();
Console.WriteLine($"Span parse allocated {after - before} bytes");

before = GC.GetAllocatedBytesForCurrentThread();
int sum4 = csv.Split(',').Sum(int.Parse);
after = GC.GetAllocatedBytesForCurrentThread();
Console.WriteLine($"Split parse allocated {after - before} bytes");
```

Output (verified on .NET 10; byte count may differ by runtime version):
```
100 100
0 1 4 9 16
Span parse allocated 0 bytes
Split parse allocated 216 bytes
```

| Type | Where can it live | Use across `await`? |
|---|---|---|
| `Span<T>` / `ReadOnlySpan<T>` | Stack only (`ref struct`) | ❌ |
| `Memory<T>` / `ReadOnlyMemory<T>` | Anywhere (heap-friendly) | ✅ |
| `stackalloc` | Stack – keep it small (≤ ~1 KB) or you risk `StackOverflowException` | ❌ |
| `ArrayPool<T>.Shared.Rent/Return` | Reuse big arrays instead of allocating | ✅ |

🧩 **Predict the output:**

```csharp
using var a = new Res("A");
using (var b = new Res("B")) { Console.WriteLine("body"); }
Console.WriteLine("end");
// class Res(string n) : IDisposable { public void Dispose() => Console.WriteLine($"Dispose {n}"); }
```

<details><summary>Answer</summary>

```
body
Dispose B
end
Dispose A
```
`B` (using statement) is disposed at its closing brace; `A` (using declaration) at the end of the enclosing scope – here, the end of the program.
</details>

## Try it yourself

1. An object has no references but holds an open `FileStream` field. When is the file closed if you never call `Dispose`?
2. Why does an object with a finalizer survive longer than one without?
3. Your ASP.NET app's memory grows steadily. You see a `static List<Order>` that orders are added to for "audit". Diagnose & fix.
4. Is `new byte[100_000]` allocated in Gen 0? Why does it matter?
5. Rewrite `var parts = line.Split(';'); var id = int.Parse(parts[0]);` to avoid allocations.
6. When should a class implement a finalizer?

<details><summary>Answer</summary>

1. Only when the GC eventually collects it and `FileStream`'s internal `SafeFileHandle` finalizer runs – at an unknown time (maybe never before process exit). Meanwhile the file stays locked. Always `using`.
2. On collection it is put on the finalization queue (kept alive), the finalizer thread runs `~T()`, and only the *next* GC frees it – so it's promoted to an older generation. `GC.SuppressFinalize` avoids this.
3. Static fields are GC roots, so every order is kept forever (managed leak). Persist audit records to a DB/log, or use a bounded/expiring cache.
4. No – ≥ 85,000 bytes goes to the LOH (collected only with Gen 2, not compacted by default). Repeatedly allocating large temporary arrays causes Gen 2 GCs and fragmentation → use `ArrayPool<T>`.
5. `ReadOnlySpan<char> s = line; int idx = s.IndexOf(';'); int id = int.Parse(idx < 0 ? s : s[..idx]);`
6. Only if it *directly* owns an unmanaged resource and you can't use a `SafeHandle`. Almost never in application code.
</details>

## Quick revision

- GC manages **memory** only; `IDisposable` manages **resources**.
- GC = mark reachable from roots → sweep → compact; cycles are collected.
- Gen 0 → Gen 1 → Gen 2; most objects die young; Gen 2 = full GC.
- ≥ 85,000 bytes → LOH (Gen 2, not compacted by default); POH for pinned arrays (.NET 5+).
- Workstation GC for desktop/console; Server GC (per-core heaps) for ASP.NET Core.
- `using` = `try/finally { Dispose(); }`; using declarations dispose at scope end in reverse order.
- `await using` → `IAsyncDisposable.DisposeAsync()`.
- Dispose pattern: `Dispose()` → `Dispose(true)` + `GC.SuppressFinalize(this)`; finalizer → `Dispose(false)`.
- Prefer `SafeHandle` over writing finalizers.
- Managed leaks: events, statics, timers, closures, undisposed objects.
- `WeakReference<T>` lets the GC collect the target.
- `Span<T>`/`stackalloc` = zero-allocation slicing; `Span` can't cross `await` – use `Memory<T>`.

# 14. Async/Await in Depth

> **Why interviewers ask this:** Almost every modern .NET app (ASP.NET Core APIs, EF Core, HttpClient, Azure SDKs) is async. Misusing it causes deadlocks, thread-pool starvation, lost exceptions and slow APIs. Interviewers check whether you know that `await` **frees the thread** rather than "runs on a new thread", and whether you can spot `.Result`, `async void` and sequential awaits that should be `WhenAll`.

## 14.1 Synchronous vs asynchronous vs concurrent vs parallel — the kitchen analogy

🍳 You must make **tea, toast and eggs**.

| Style | Kitchen | Code |
|---|---|---|
| **Synchronous** | One cook. Puts kettle on and **stares at it** until it boils, then toast, then eggs. | Blocking calls one after another |
| **Asynchronous** | One cook. Puts kettle on, **while it heats** puts bread in toaster, then cracks eggs. Reacts when each "ding" happens. | `await` – thread is free while waiting for I/O |
| **Concurrent** | Several tasks are *in progress* during the same period (may be interleaved by one cook). | Many tasks alive at once |
| **Parallel** | **Several cooks** literally working at the same instant on different CPU cores. | `Parallel.For`, `Task.Run` × N, PLINQ |

- Asynchrony is about **not waiting idly** (I/O).
- Parallelism is about **using more CPU cores** (computation).
- Concurrency is the umbrella term: more than one thing in progress.

## 14.2 Thread vs ThreadPool vs Task

| | `Thread` | `ThreadPool` | `Task` |
|---|---|---|---|
| What | An OS thread you create & own | A shared pool of reusable worker threads | A *promise* of a future result (may or may not use a thread) |
| Cost | ~1 MB stack + OS creation time | Cheap – reused | Very cheap object |
| Return value | ❌ (manual) | ❌ | ✅ `Task<T>` |
| Exceptions | Crash the process if unhandled | Crash | Stored in the task, rethrown on `await` |
| Cancellation, continuation, composition | Manual | ❌ | ✅ `CancellationToken`, `await`, `WhenAll` |
| Use for | Long-running dedicated work (rare) | Indirectly | **Default choice** |

⚠️ **Interview trap:** "A Task is a thread." No. `Task.Delay(1000)` or `HttpClient.GetAsync` uses **no thread** while waiting – it's a timer / OS I/O completion. Only `Task.Run` explicitly queues work to a thread-pool thread.

## 14.3 I/O-bound vs CPU-bound

| | I/O-bound | CPU-bound |
|---|---|---|
| Waiting on | Network, disk, DB | Processor (calculations, image resize, parsing huge JSON) |
| Use | `await` the async API directly (`await http.GetAsync()`) | `await Task.Run(() => Compute())` (in UI apps) or `Parallel` |
| Threads used while waiting | **Zero** | One (or more) busy thread |

```csharp
// I/O-bound ✅
string html = await httpClient.GetStringAsync(url);

// CPU-bound in a UI app ✅ (keeps UI responsive)
var hash = await Task.Run(() => ComputeHash(bigFile));

// ❌ Wrapping I/O in Task.Run wastes a thread that just blocks
var html2 = await Task.Run(() => httpClient.GetStringAsync(url).Result);
```

⚠️ In **ASP.NET Core**, don't `Task.Run` CPU work inside a request just to "make it async" – you're simply moving work from one pool thread to another, adding overhead.

## 14.4 What `await` really does

`async` lets you use `await`; it does **not** make the method run on another thread.

When execution hits `await someTask`:
1. If `someTask` is already complete → just continue synchronously (no thread switch).
2. Otherwise → register **"the rest of this method"** (the *continuation*) on the task, and **return** to the caller (returning an incomplete `Task`). The thread is now free to do other work.
3. When the task completes, the continuation is scheduled – on the captured `SynchronizationContext` (UI thread) if there is one, otherwise on a thread-pool thread.

The compiler turns your method into a **state machine** (a struct implementing `IAsyncStateMachine` with a `MoveNext()` method and a `state` field: "-1 = running, 0 = waiting at first await, …").

```
Thread T1 ──[ code before await ]──► hits await (I/O started) ──► returns to caller, T1 free for other requests
                                                                       │
                              ... no thread is waiting – the OS/network does the work ...
                                                                       │
Thread T5 ◄──────────────────────── I/O completes: continuation queued ┘
           ──[ code after await ]──► completes the Task
```

```csharp
Console.WriteLine($"1. Before await  - thread {Environment.CurrentManagedThreadId}");
string data = await DownloadAsync();
Console.WriteLine($"4. After await   - thread {Environment.CurrentManagedThreadId}  ({data})");

static async Task<string> DownloadAsync()
{
    Console.WriteLine($"2. Start download - thread {Environment.CurrentManagedThreadId}");
    await Task.Delay(500);   // simulated network I/O: NO thread is blocked here
    Console.WriteLine($"3. Download done  - thread {Environment.CurrentManagedThreadId}");
    return "42 KB";
}
```

Representative output (thread IDs will differ):
```
1. Before await  - thread 2
2. Start download - thread 2
3. Download done  - thread 5
4. After await   - thread 5  (42 KB)
```

Code *before* the first `await` runs synchronously on the caller's thread. A console app has no `SynchronizationContext`, so the continuation resumes on a thread-pool thread.

🎯 **Say this in the interview:** "`await` doesn't create a thread. If the task isn't finished, the compiler-generated state machine registers the rest of the method as a continuation and returns, freeing the thread. When the I/O completes, the continuation is scheduled on the captured context or the thread pool. That's why async scales: during the wait, no thread is blocked."

🧩 **Predict the output:**

```csharp
Console.WriteLine("A");
Task t = PrintAsync();
Console.WriteLine("C");
await t;
Console.WriteLine("E");

static async Task PrintAsync()
{
    Console.WriteLine("B");
    await Task.Delay(100);
    Console.WriteLine("D");
}
```

<details><summary>Answer</summary>

```
A
B
C
D
E
```
"B" prints synchronously (code before the first `await` runs on the caller's thread). At `await Task.Delay` the method returns to the caller, which prints "C", then waits for `t`.
</details>

🧩 **Predict the output:**

```csharp
Console.WriteLine("1");
Task t = NoAwaitAsync();
Console.WriteLine("3");
await t;

static async Task NoAwaitAsync()
{
    await Task.CompletedTask;     // already complete -> continues synchronously
    Console.WriteLine("2");
}
```

<details><summary>Answer</summary>

```
1
2
3
```
Awaiting an already-completed task doesn't yield, so the whole method runs synchronously before returning.
</details>

## 14.5 Async return types

| Return type | Use when | Notes |
|---|---|---|
| `Task` | Async method with no result | The default |
| `Task<T>` | Async method returning `T` | The default |
| `ValueTask` / `ValueTask<T>` | Hot paths that **often complete synchronously** (e.g. cached value) | Avoids a `Task` allocation. ⚠️ Await it **only once**; don't call `.Result` before completion; don't await concurrently |
| `void` | **Only** event handlers (`button.Click += async (s, e) => ...`) | Can't be awaited; exceptions crash the process |
| `IAsyncEnumerable<T>` | Async streams (§14.12) | `await foreach` |

### Why `async void` is dangerous — runnable

```csharp
AppDomain.CurrentDomain.UnhandledException += (_, e) =>
    Console.WriteLine($"UnhandledException: {((Exception)e.ExceptionObject).Message} - process is dying");

try
{
    Bad();
}
catch
{
    Console.WriteLine("caught?");   // never runs
}
Console.WriteLine("Main continues...");
Thread.Sleep(3000);   // give the pool thread time to blow up

static async void Bad()
{
    await Task.Yield();
    throw new InvalidOperationException("boom from async void");
}
```

Output (verified – the process **crashes**):
```
Main continues...
UnhandledException: boom from async void - process is dying
Unhandled exception. System.InvalidOperationException: boom from async void
   at Program.<<Main>$>g__Bad|0_1() in ...\Program.cs:line 18
   ...
```

The caller's `try/catch` is useless because there's no `Task` to carry the exception back. Also: the caller can't know when it finished, and unit tests can't await it.

```csharp
// ❌
public async void SaveAsync() { ... }
// ✅
public async Task SaveAsync() { ... }
```

## 14.6 Deadlocks with `.Result` / `.Wait()` (sync-over-async)

> Explained, not run (a console app has no `SynchronizationContext`, so it wouldn't deadlock there).

```csharp
// Classic ASP.NET (.NET Framework) / WPF / WinForms button handler
public string GetData()
{
    return GetDataAsync().Result;      // ❌ blocks the UI/request thread
}

public async Task<string> GetDataAsync()
{
    await Task.Delay(1000);            // captures the UI SynchronizationContext
    return "data";                     // ...wants to resume on the UI thread
}
```

```
UI thread: GetData() ──► .Result ──► BLOCKED waiting for task ─────────┐
                                                                       │  waits for
Timer fires: continuation must run on UI thread ──► UI thread busy ◄───┘  each other = DEADLOCK 💀
```

1. UI thread calls `.Result` and blocks.
2. `Task.Delay` completes; the continuation is posted to the UI context.
3. UI thread is blocked waiting → continuation can never run → task never completes.

**Fixes (in order of preference):**
1. **Async all the way**: `public async Task<string> GetData() => await GetDataAsync();`
2. Use `ConfigureAwait(false)` inside library code (continuation doesn't need the UI thread).
3. Last resort in `Main`/legacy code: `Task.Run(() => GetDataAsync()).GetAwaiter().GetResult()`.

### Thread-pool starvation (ASP.NET Core)

ASP.NET Core has **no** `SynchronizationContext`, so `.Result` won't deadlock – but each blocked request holds a pool thread hostage. Under load the pool runs out of threads, injects new ones slowly (~1–2 per second), and requests queue up: latency explodes while CPU is low. That's **thread-pool starvation** – the #1 symptom of sync-over-async in services.

## 14.7 `ConfigureAwait(false)` — when it matters

`await task.ConfigureAwait(false)` = "I don't need to come back to the original context; any thread-pool thread is fine."

| Code type | Use `ConfigureAwait(false)`? |
|---|---|
| General-purpose **library** / NuGet package | ✅ Yes, on every await (callers might block on a UI thread) |
| UI app code (WPF/WinForms/MAUI) that touches UI after the await | ❌ No – you need the UI thread |
| ASP.NET Core app code | Not needed (no SynchronizationContext) – harmless either way |
| Console app | Not needed |

```csharp
public async Task<Config> LoadConfigAsync(string path)          // library code
{
    string json = await File.ReadAllTextAsync(path).ConfigureAwait(false);
    return JsonSerializer.Deserialize<Config>(json)!;            // runs on pool thread – fine
}
```

⚠️ **Interview trap:** `ConfigureAwait(false)` does *not* mean "run the task on another thread". It only affects **where the continuation resumes**. And if the task is already complete, the code continues on the same thread anyway.

## 14.8 `Task.WhenAll` vs sequential awaits

🍳 Sequential = boil kettle, wait, *then* start toaster. WhenAll = start everything, then wait once.

```csharp
using System.Diagnostics;

var sw = Stopwatch.StartNew();
await GetUserAsync(); await GetOrdersAsync(); await GetCartAsync();
Console.WriteLine($"Sequential: {sw.ElapsedMilliseconds} ms");

sw.Restart();
Task<string> u = GetUserAsync(), o = GetOrdersAsync(), c = GetCartAsync(); // all 3 started
await Task.WhenAll(u, o, c);
Console.WriteLine($"WhenAll   : {sw.ElapsedMilliseconds} ms -> {u.Result}, {o.Result}, {c.Result}");

// C# shortcut when all tasks return the same type:
string[] results = await Task.WhenAll(GetUserAsync(), GetOrdersAsync(), GetCartAsync());
Console.WriteLine(string.Join(" | ", results));

static async Task<string> GetUserAsync()   { await Task.Delay(1000); return "user"; }
static async Task<string> GetOrdersAsync() { await Task.Delay(1000); return "orders"; }
static async Task<string> GetCartAsync()   { await Task.Delay(1000); return "cart"; }
```

Representative output (your numbers will differ slightly):
```
Sequential: 3029 ms
WhenAll   : 1001 ms -> user, orders, cart
user | orders | cart
```

Using `.Result` **after** `await Task.WhenAll` is safe – the tasks are already complete, so nothing blocks.

⚠️ **Interview trap:** Only use `WhenAll` for **independent** operations. Also, EF Core's `DbContext` is **not** thread-safe – you can't run two queries on the same context concurrently with `WhenAll`.

## 14.9 `Task.WhenAny` + timeouts

```csharp
// 1) Classic WhenAny timeout
Task<string> slow = SlowServiceAsync(3000);
Task winner = await Task.WhenAny(slow, Task.Delay(1000));
Console.WriteLine(winner == slow ? $"Got: {slow.Result}" : "Timed out (but slow task is STILL running!)");

// 2) .NET 6+: WaitAsync
try
{
    string r = await SlowServiceAsync(3000).WaitAsync(TimeSpan.FromSeconds(1));
}
catch (TimeoutException) { Console.WriteLine("WaitAsync -> TimeoutException"); }

// 3) Fastest mirror wins
var mirrors = new[] { SlowServiceAsync(300, "EU"), SlowServiceAsync(100, "US"), SlowServiceAsync(200, "ASIA") };
Task<string> first = await Task.WhenAny(mirrors);
Console.WriteLine($"Fastest mirror: {await first}");

static async Task<string> SlowServiceAsync(int ms, string name = "slow")
{
    await Task.Delay(ms);
    return name;
}
```

Output (verified):
```
Timed out (but slow task is STILL running!)
WaitAsync -> TimeoutException
Fastest mirror: US
```

⚠️ `WhenAny`/`WaitAsync` stop **waiting**; they don't stop the **work**. To actually stop it, pass a `CancellationToken` (next section).

## 14.10 Cancellation: `CancellationToken` & `CancellationTokenSource`

🏠 **Analogy:** The `CancellationTokenSource` is the **remote control**; the `CancellationToken` is the **TV receiver** you hand to workers. Cancellation is **cooperative** – the worker must check the token or pass it on.

```csharp
using var cts = new CancellationTokenSource();
cts.CancelAfter(TimeSpan.FromMilliseconds(350));     // auto-cancel after 350 ms

try
{
    await ProcessFilesAsync(cts.Token);
}
catch (OperationCanceledException)
{
    Console.WriteLine("Cancelled cleanly!");
}

// Linked tokens: cancel if the USER cancels OR our own timeout fires
using var userCts = new CancellationTokenSource();
using var timeoutCts = new CancellationTokenSource(TimeSpan.FromMilliseconds(150));
using var linked = CancellationTokenSource.CreateLinkedTokenSource(userCts.Token, timeoutCts.Token);
try
{
    await Task.Delay(5000, linked.Token);
}
catch (OperationCanceledException)
{
    Console.WriteLine($"Linked cancelled. By timeout? {timeoutCts.IsCancellationRequested}, by user? {userCts.IsCancellationRequested}");
}

static async Task ProcessFilesAsync(CancellationToken ct)
{
    for (int i = 1; i <= 10; i++)
    {
        ct.ThrowIfCancellationRequested();   // cooperative check
        Console.WriteLine($"Processing file {i}");
        await Task.Delay(100, ct);           // pass the token down!
    }
}
```

Representative output (timing-based):
```
Processing file 1
Processing file 2
Processing file 3
Processing file 4
Cancelled cleanly!
Linked cancelled. By timeout? True, by user? False
```

**Real-world (ASP.NET Core):** the framework gives you `HttpContext.RequestAborted`; just add a parameter:

```csharp
app.MapGet("/report", async (ReportService svc, CancellationToken ct) => await svc.BuildAsync(ct));
```
If the browser disconnects, the token fires and your DB query/HTTP calls stop – saving server resources.

Rules of thumb:
- Accept `CancellationToken ct = default` as the **last** parameter of public async methods.
- Pass it to every async call you make.
- `OperationCanceledException` is not an error – don't log it as one.
- Dispose `CancellationTokenSource` (it may hold a timer).

## 14.11 Exception handling with `WhenAll`

```csharp
Task all = Task.WhenAll(CallA(), CallB(), CallC());
try
{
    await all;
}
catch
{
    foreach (var ex in all.Exception!.InnerExceptions)   // ALL failures, not just the first
        logger.LogError(ex, "Call failed");
    throw;
}
```

See the runnable demo in §12.8: `await` surfaces only the first exception; `task.Exception.InnerExceptions` has all of them.

## 14.12 Async streams: `IAsyncEnumerable<T>` + `await foreach` (C# 8)

`Task<List<T>>` = wait for *all* items, then get them at once. `IAsyncEnumerable<T>` = get items **one by one as they arrive** (paging APIs, DB cursors, streaming responses).

```csharp
await foreach (var order in GetOrdersAsync())
    Console.WriteLine($"Got order {order} at {DateTime.Now:ss.fff}");

using var cts = new CancellationTokenSource(250);
try
{
    await foreach (var o in GetOrdersAsync().WithCancellation(cts.Token))
        Console.WriteLine($"[cancellable] {o}");
}
catch (OperationCanceledException) { Console.WriteLine("stream cancelled"); }

static async IAsyncEnumerable<int> GetOrdersAsync(
    [System.Runtime.CompilerServices.EnumeratorCancellation] CancellationToken ct = default)
{
    for (int page = 1; page <= 3; page++)
    {
        await Task.Delay(200, ct);          // fetch a page from the DB/API
        yield return page * 100;            // hand items over one by one
    }
}
```

Representative output (timestamps will differ – note ~200 ms gaps):
```
Got order 100 at 41.882
Got order 200 at 42.110
Got order 300 at 42.315
[cancellable] 100
stream cancelled
```

EF Core: `await foreach (var p in db.Products.AsAsyncEnumerable())`. .NET 10 adds built-in LINQ for `IAsyncEnumerable` (`System.Linq.AsyncEnumerable`).

## 14.13 `Task.Delay` vs `Thread.Sleep`

| | `Thread.Sleep(ms)` | `await Task.Delay(ms)` |
|---|---|---|
| Blocks thread? | ✅ Yes – thread does nothing | ❌ No – a timer; thread is free |
| Cancellable? | ❌ | ✅ `Task.Delay(ms, ct)` |
| Use in async code | ❌ Never | ✅ |

```csharp
using System.Diagnostics;

var sw = Stopwatch.StartNew();
var tasks = Enumerable.Range(0, 4).Select(_ => Task.Run(() => Thread.Sleep(1000)));
await Task.WhenAll(tasks);
Console.WriteLine($"Thread.Sleep x4 on pool: {sw.ElapsedMilliseconds} ms (4 threads blocked)");

sw.Restart();
await Task.WhenAll(Enumerable.Range(0, 1000).Select(_ => Task.Delay(1000)));
Console.WriteLine($"Task.Delay x1000      : {sw.ElapsedMilliseconds} ms (0 threads blocked)");
```

Representative output:
```
Thread.Sleep x4 on pool: 1015 ms (4 threads blocked)
Task.Delay x1000      : 1014 ms (0 threads blocked)
```

1,000 concurrent `Task.Delay`s cost no threads. 1,000 `Thread.Sleep`s would need 1,000 threads (and would take far longer while the pool slowly grows).

## 14.14 Fire-and-forget pitfalls

```csharp
_ = SendEmailAsync(user);   // "fire and forget"
```

Problems: exceptions are unobserved (silently lost), no retry, the work dies if the app restarts, and in ASP.NET Core scoped services (`DbContext`) are disposed when the request ends → `ObjectDisposedException`.

✅ Better: queue the work to a `Channel<T>` consumed by a `BackgroundService` (see §15.9), or use a durable queue (Azure Service Bus, Hangfire). If you must fire-and-forget, wrap the body in `try/catch` and log.

## 14.15 Async lambdas

```csharp
Func<Task> f = async () => await Task.Delay(10);                  // ✅ returns Task
Func<int, Task<int>> g = async x => { await Task.Delay(10); return x * 2; };

// ⚠️ Action-typed parameter + async lambda = async void!
list.ForEach(async item => await SaveAsync(item));    // ❌ not awaited, exceptions crash
foreach (var item in list) await SaveAsync(item);     // ✅
await Parallel.ForEachAsync(list, async (item, ct) => await SaveAsync(item));   // ✅ concurrent
```

⚠️ **Interview trap:** `List<T>.ForEach` takes an `Action<T>`; an async lambda passed to it becomes `async void`. Same with `new Thread(async () => ...)` and `Timer` callbacks.

## 14.16 Sync-over-async & async-over-sync

| Anti-pattern | Example | Why bad | Fix |
|---|---|---|---|
| **Sync-over-async** | `GetAsync().Result`, `.Wait()` | Deadlocks (UI/classic ASP.NET), thread-pool starvation | Async all the way |
| **Async-over-sync** | `public Task<int> CalcAsync() => Task.Run(() => Calc());` in a library | Fake async; hides the cost; wastes a thread in servers | Expose sync `Calc()`; let the caller decide to `Task.Run` |

## 14.17 Common anti-patterns cheat sheet

| ❌ Anti-pattern | ✅ Do instead |
|---|---|
| `async void` methods | `async Task` (void only for event handlers) |
| `.Result` / `.Wait()` / `GetAwaiter().GetResult()` | `await` |
| Sequential awaits of independent work | `Task.WhenAll` |
| `Thread.Sleep` in async code | `await Task.Delay` |
| Ignoring `CancellationToken` | Accept & pass it through |
| `Task.Run` around async I/O | `await` the I/O directly |
| `return await` everywhere *(minor)* | Fine for readability; required inside `try/using` |
| `async` lambda passed to `Action` | Use `Func<Task>` overloads / `foreach` + `await` |
| Fire-and-forget with no error handling | Background queue / `BackgroundService` |
| `await`ing a `ValueTask` twice | Await once or call `.AsTask()` |
| `lock` around an `await` (CS1996) | `SemaphoreSlim.WaitAsync` |

## Try it yourself

1. Is this I/O- or CPU-bound and how would you write it? (a) reading a 2 GB file and computing SHA-256, (b) calling 5 REST APIs.
2. Why can't you write `lock (x) { await Foo(); }`?
3. A WPF button handler calls `var s = LoadAsync().Result;` and the UI freezes forever. Explain and fix.
4. Rewrite to run in ~1 s instead of ~3 s: `var a = await A(); var b = await B(); var c = await C();` (each 1 s, independent).
5. Add a 2-second timeout to `await client.GetStringAsync(url)` that actually cancels the HTTP request.
6. What's wrong with `items.ForEach(async i => await SaveAsync(i));`?

<details><summary>Answer</summary>

1. (a) Reading is I/O, hashing is CPU – use `await SHA256.HashDataAsync(stream)` (async I/O) or `Task.Run` in a UI app. (b) Pure I/O – start all 5 calls and `await Task.WhenAll(...)`.
2. Compiler error CS1996 – a `Monitor` lock is owned by a thread, and after `await` you may resume on a different thread. Use `SemaphoreSlim(1,1)` with `await sem.WaitAsync()` / `Release()` in `finally`.
3. Deadlock: `.Result` blocks the UI thread; `LoadAsync`'s continuation needs the UI thread. Make the handler `async void Button_Click(...)` (event handler is the allowed case) and `var s = await LoadAsync();`.
4. `var ta = A(); var tb = B(); var tc = C(); await Task.WhenAll(ta, tb, tc); var (a, b, c) = (ta.Result, tb.Result, tc.Result);`
5. `using var cts = new CancellationTokenSource(TimeSpan.FromSeconds(2)); var s = await client.GetStringAsync(url, cts.Token);`
6. `ForEach` takes `Action<T>`, so the lambda is `async void`: nothing is awaited, the method returns before saving finishes, and exceptions crash the process. Use `foreach (...) await SaveAsync(i);` or `Task.WhenAll(items.Select(SaveAsync))`.
</details>

## Quick revision

- Async = don't wait idly (I/O); parallel = more cores (CPU).
- `Task` ≠ thread; I/O tasks use no thread while waiting.
- `async` enables `await`; it doesn't create threads.
- `await` = register continuation + return; compiler builds a state machine.
- Code before the first `await` runs synchronously.
- Return `Task`/`Task<T>`; `ValueTask` for hot, often-synchronous paths; `async void` only for event handlers.
- `.Result`/`.Wait()` → deadlocks with a SynchronizationContext, starvation in ASP.NET Core.
- `ConfigureAwait(false)` in libraries; unnecessary in ASP.NET Core apps.
- Independent awaits → `Task.WhenAll`; first-wins/timeout → `WhenAny` / `WaitAsync`.
- Cancellation is cooperative: pass `CancellationToken` everywhere; `CancelAfter`, linked tokens.
- `await WhenAll` throws the first exception; `task.Exception.InnerExceptions` has all.
- `IAsyncEnumerable<T>` + `await foreach` streams items; `Task.Delay` not `Thread.Sleep`.

# 15. Multithreading & Concurrency

> **Why interviewers ask this:** Web servers handle many requests at once, background services process queues in parallel, and caches are shared between threads. Interviewers want to know if you can recognise a **race condition**, pick the right tool (`lock`, `Interlocked`, `SemaphoreSlim`, concurrent collections, `Channel<T>`), and avoid **deadlocks**. "Make this singleton/counter/cache thread-safe" is a very common live-coding question.

## 15.1 Process vs thread

| | Process | Thread |
|---|---|---|
| What | A running program with its **own memory space** | A path of execution **inside** a process |
| Memory | Isolated from other processes | **Shares** heap & statics with other threads of the same process |
| Cost to create | Heavy | Lighter (~1 MB stack) |
| Crash impact | Other processes survive | An unhandled exception on any thread kills the whole process |
| Communication | IPC (pipes, sockets, files) | Shared memory (→ needs synchronization!) |

🏠 **Analogy:** A process is a **house**; threads are **people living in it**. They share the kitchen (heap). If two people grab the same pan at the same time without talking, you get a mess – a race condition.

## 15.2 Creating threads

```csharp
// 1) Raw thread – rarely needed (long-running dedicated work, custom priority/apartment)
var t = new Thread(() => Console.WriteLine($"Worker on thread {Environment.CurrentManagedThreadId}"))
{
    IsBackground = true      // background threads don't keep the process alive
};
t.Start();
t.Join();                    // wait for it to finish

// 2) Thread pool via Task.Run – the normal way
await Task.Run(() => Console.WriteLine("pool thread"));

// 3) Long-running dedicated thread via Task
await Task.Factory.StartNew(() => { /* loop for hours */ }, TaskCreationOptions.LongRunning);
```

| Foreground thread (default `new Thread`) | Background thread (`IsBackground = true`, all pool threads) |
|---|---|
| Process waits for it before exiting | Killed when the last foreground thread ends |

## 15.3 Race conditions — runnable

`counter++` looks like one step but is actually **three**: read → add 1 → write. Two threads can read the same old value and both write `old + 1` – one increment is lost.

```
Thread A: read 5 ───────► add → 6 ─► write 6
Thread B:    read 5 ─► add → 6 ──────────────► write 6     (expected 7!)
```

```csharp
int counter = 0;
var threads = new Thread[4];
for (int t = 0; t < threads.Length; t++)
{
    threads[t] = new Thread(() =>
    {
        for (int i = 0; i < 1_000_000; i++)
            counter++;               // read -> add -> write: NOT atomic
    });
    threads[t].Start();
}
foreach (var th in threads) th.Join();
Console.WriteLine($"Expected 4000000, got {counter}");
```

Representative output (different on every run – that's the point):
```
Expected 4000000, got 1588199
```

## 15.4 `lock`, `Monitor`, `Interlocked`

### `lock` (= `Monitor.Enter/Exit` in a `try/finally`)

Only **one thread at a time** may be inside a `lock` block for the same lock object. Others wait.

🏠 **Analogy:** A single-occupancy bathroom with one key. Whoever holds the key goes in; everyone else queues.

```csharp
lock (_gate) { _balance -= amount; }

// compiler generates (roughly):
bool taken = false;
try { Monitor.Enter(_gate, ref taken); _balance -= amount; }
finally { if (taken) Monitor.Exit(_gate); }
```

**What to lock on:**

| ✅ Lock on | ❌ Never lock on | Why |
|---|---|---|
| `private readonly object _gate = new();` | `this` | Outside code can lock your object too → surprise deadlocks |
| `private readonly Lock _gate = new();` (C# 13 / .NET 9+) | `typeof(MyClass)` | Globally visible |
| | string literals (`lock ("x")`) | Interned – shared across the whole process |
| | value types (`lock (count)`) | Doesn't compile: CS0185 |

```csharp
int count = 0;
lock (count) { count++; }   // ❌ does not compile: CS0185 'int' is not a reference type as required by the lock statement
```

### `Interlocked` — atomic operations on single variables

Hardware-level atomic instructions for `int`/`long`/references: `Increment`, `Decrement`, `Add`, `Exchange`, `CompareExchange`. No blocking, very fast – but only for **one** variable.

**Runnable: comparing fixes**

```csharp
using System.Diagnostics;

const int Threads = 4, Loops = 1_000_000;

int unsafeCount = 0, lockedCount = 0, interlockedCount = 0, newLockCount = 0;
object gate = new();
Lock newGate = new();      // C# 13 / .NET 9+ dedicated lock type

Run("No sync    ", () => unsafeCount++);
Run("lock       ", () => { lock (gate) lockedCount++; });
Run("Lock (C#13)", () => { lock (newGate) newLockCount++; });
Run("Interlocked", () => Interlocked.Increment(ref interlockedCount));

Console.WriteLine($"{unsafeCount} {lockedCount} {newLockCount} {interlockedCount}");

void Run(string name, Action body)
{
    var sw = Stopwatch.StartNew();
    Parallel.For(0, Threads, _ => { for (int i = 0; i < Loops; i++) body(); });
    Console.WriteLine($"{name}: {sw.ElapsedMilliseconds,4} ms");
}
```

Representative output (your numbers will differ; the unsynchronised count changes every run):
```
No sync    :   12 ms
lock       :  515 ms
Lock (C#13):  640 ms
Interlocked:  135 ms
1063667 4000000 4000000 4000000
```

Takeaways: no sync is fast but **wrong**; `Interlocked` is the cheapest correct option for a single counter; `lock` is needed when several fields must change together (e.g. debit one account *and* credit another). Under heavy contention like this micro-benchmark, both lock types are slow – the real win is to reduce sharing.

> **C# 13 / .NET 9 `System.Threading.Lock`:** a dedicated lock type. `lock (myLock)` on it uses the faster `Lock.EnterScope()` API instead of `Monitor`, and the intent is clearer. Don't cast it to `object` and lock on that (compiler warning CS9216 – it falls back to `Monitor`).

⚠️ **Interview trap:** You cannot `await` inside a `lock` (CS1996). For async code, use `SemaphoreSlim`.

## 15.5 Deadlocks

A **deadlock** = two (or more) threads each hold a lock and wait for the lock the other one holds. Nobody ever proceeds.

🏠 **Analogy:** Two people in a narrow corridor, each politely waiting for the other to step aside – forever.

The demo below uses `Monitor.TryEnter` with a timeout so it **detects** the deadlock instead of hanging forever (a plain `lock` would hang):

```csharp
object lockA = new(), lockB = new();

var t1 = new Thread(() => Transfer("T1", lockA, lockB));
var t2 = new Thread(() => Transfer("T2", lockB, lockA));   // opposite order!
t1.Start(); t2.Start(); t1.Join(); t2.Join();

static void Transfer(string name, object first, object second)
{
    lock (first)
    {
        Console.WriteLine($"{name} took first lock");
        Thread.Sleep(100);   // make the bad timing certain
        if (Monitor.TryEnter(second, TimeSpan.FromSeconds(1)))   // a plain lock(second) would hang forever
        {
            try { Console.WriteLine($"{name} took second lock"); }
            finally { Monitor.Exit(second); }
        }
        else
        {
            Console.WriteLine($"{name} DEADLOCK detected - gave up after 1s");
        }
    }
}
```

Output (verified; the order of T1/T2 lines may swap):
```
T1 took first lock
T2 took first lock
T1 DEADLOCK detected - gave up after 1s
T2 DEADLOCK detected - gave up after 1s
```

**Prevention:**
1. **Consistent lock ordering** – always acquire locks in the same global order (e.g. by account ID: `var (first, second) = a.Id < b.Id ? (a, b) : (b, a);`).
2. Hold locks for as short a time as possible; never call unknown code (events, callbacks, virtual methods) while holding a lock.
3. Use timeouts (`Monitor.TryEnter`, `SemaphoreSlim.WaitAsync(timeout)`).
4. Prefer lock-free designs: immutable data, `Interlocked`, concurrent collections, channels.
5. Async: never block on async code (`.Result`) – see §14.6.

## 15.6 `SemaphoreSlim` — "at most N at a time" (async-friendly)

🏠 **Analogy:** A nightclub bouncer with a counter: max 3 people inside. When one leaves (`Release`), the next one enters.

**Real-world: throttling concurrent downloads**

```csharp
using var throttle = new SemaphoreSlim(3);     // at most 3 at a time
int running = 0, maxSeen = 0;

var urls = Enumerable.Range(1, 10).Select(i => $"https://example.com/file{i}");
var tasks = urls.Select(async url =>
{
    await throttle.WaitAsync();
    try
    {
        int now = Interlocked.Increment(ref running);
        InterlockedMax(ref maxSeen, now);
        await Task.Delay(200);                 // pretend: await http.GetStringAsync(url)
        return url.Length;
    }
    finally
    {
        Interlocked.Decrement(ref running);
        throttle.Release();
    }
});

int[] sizes = await Task.WhenAll(tasks);
Console.WriteLine($"Downloaded {sizes.Length} files, max concurrent = {maxSeen}");

static void InterlockedMax(ref int target, int value)
{
    int current;
    while ((current = Volatile.Read(ref target)) < value &&
           Interlocked.CompareExchange(ref target, value, current) != current) { }
}
```

Output (verified):
```
Downloaded 10 files, max concurrent = 3
```

`SemaphoreSlim(1, 1)` is the standard **async lock**:

```csharp
private readonly SemaphoreSlim _mutex = new(1, 1);

public async Task RefreshTokenAsync()
{
    await _mutex.WaitAsync();
    try { _token = await _auth.GetTokenAsync(); }   // await allowed here ✅
    finally { _mutex.Release(); }                    // ALWAYS in finally
}
```

### Brief: `Mutex`, `ReaderWriterLockSlim`, `volatile`

- **`Mutex`** – like `lock` but can be **named** and works **across processes** (e.g. "only one instance of my app may run"). Much slower (kernel object). Thread-affine – must be released by the owning thread.
  ```csharp
  using var m = new Mutex(initiallyOwned: false, name: "Global\\MyApp-SingleInstance");
  if (!m.WaitOne(0)) { Console.WriteLine("Already running"); return; }
  ```
- **`ReaderWriterLockSlim`** – many **readers** at once *or* one **writer**. Good for read-heavy data that is rarely updated (config, lookup tables).
  ```csharp
  _rw.EnterReadLock();  try { return _map[key]; } finally { _rw.ExitReadLock(); }
  _rw.EnterWriteLock(); try { _map[key] = v; }    finally { _rw.ExitWriteLock(); }
  ```
- **`volatile`** – tells the compiler/CPU not to cache or reorder reads/writes of a field, so other threads see the latest value. Good for a simple `bool _stop` flag. It does **not** make `count++` atomic. Prefer `Volatile.Read/Write`, `Interlocked` or `lock` when in doubt.

## 15.7 Thread-safe collections

| Collection | Use for |
|---|---|
| `ConcurrentDictionary<K,V>` | Shared cache / lookup with concurrent reads & writes |
| `ConcurrentQueue<T>` / `ConcurrentStack<T>` | Lock-free FIFO/LIFO |
| `ConcurrentBag<T>` | Unordered collection where the same threads add & take |
| `BlockingCollection<T>` | Classic (sync) producer/consumer with blocking `Take()` and bounded capacity |
| `Channel<T>` | **Modern async** producer/consumer (`await WriteAsync` / `await foreach ReadAllAsync`) |
| `ImmutableList<T>`, `ImmutableDictionary<K,V>`, `FrozenDictionary` (.NET 8) | Read-mostly data – share freely, "modify" by creating a new copy |

⚠️ **Interview trap:** A normal `Dictionary`/`List` is **not** thread-safe for concurrent writes – concurrent `Add`s can corrupt its internal state (lost items, exceptions, even infinite loops).

### `ConcurrentDictionary.GetOrAdd` pitfall — runnable

`GetOrAdd(key, factory)` is thread-safe for the *dictionary*, but the **factory can run several times** concurrently (only one result is stored). Bad if the factory is expensive or has side effects. Fix: store `Lazy<T>`.

```csharp
using System.Collections.Concurrent;

var dict = new ConcurrentDictionary<string, string>();
int factoryCalls = 0;

Parallel.For(0, 8, _ =>
    dict.GetOrAdd("config", key =>
    {
        Interlocked.Increment(ref factoryCalls);
        Thread.Sleep(100);           // expensive: e.g. load from DB
        return "loaded";
    }));
Console.WriteLine($"GetOrAdd        : factory ran {factoryCalls} time(s), entries = {dict.Count}");

var lazyDict = new ConcurrentDictionary<string, Lazy<string>>();
int lazyCalls = 0;
Parallel.For(0, 8, i =>
{
    string v = lazyDict.GetOrAdd("config", key => new Lazy<string>(() =>
    {
        Interlocked.Increment(ref lazyCalls);
        Thread.Sleep(100);
        return "loaded";
    })).Value;
});
Console.WriteLine($"GetOrAdd + Lazy : factory ran {lazyCalls} time(s)");

// AddOrUpdate for atomic counters per key
var hits = new ConcurrentDictionary<string, int>();
Parallel.For(0, 1000, i => hits.AddOrUpdate(i % 2 == 0 ? "even" : "odd", 1, (_, old) => old + 1));
Console.WriteLine($"even={hits["even"]}, odd={hits["odd"]}");
```

Representative output (the first number depends on your CPU core count):
```
GetOrAdd        : factory ran 8 time(s), entries = 1
GetOrAdd + Lazy : factory ran 1 time(s)
even=500, odd=500
```

⚠️ **Interview trap:** `if (!dict.ContainsKey(k)) dict[k] = v;` on a `ConcurrentDictionary` is **check-then-act** – still a race. Use the atomic methods `GetOrAdd`, `TryAdd`, `AddOrUpdate`, `TryUpdate`, `TryRemove`.

### `BlockingCollection<T>` (sync producer/consumer)

```csharp
using var queue = new BlockingCollection<int>(boundedCapacity: 100);
var consumer = Task.Run(() => { foreach (var item in queue.GetConsumingEnumerable()) Process(item); });
for (int i = 0; i < 1000; i++) queue.Add(i);   // blocks if 100 items are waiting
queue.CompleteAdding();
consumer.Wait();
```

It **blocks threads** while waiting – fine for dedicated worker threads, but in async code prefer `Channel<T>`.

## 15.8 `Channel<T>` — async producer/consumer (runnable)

🏠 **Analogy:** A conveyor belt in a sushi restaurant. The chef (producer) puts plates on; customers (consumers) take them off. A **bounded** belt has limited space – if it's full, the chef must wait (*back-pressure*).

```csharp
using System.Threading.Channels;

var channel = Channel.CreateBounded<int>(new BoundedChannelOptions(capacity: 2)
{
    FullMode = BoundedChannelFullMode.Wait     // producer waits when full = back-pressure
});

var producer = Task.Run(async () =>
{
    for (int i = 1; i <= 5; i++)
    {
        await channel.Writer.WriteAsync(i);
        Console.WriteLine($"Produced {i}");
    }
    channel.Writer.Complete();                 // "no more items"
});

var consumer = Task.Run(async () =>
{
    await foreach (int item in channel.Reader.ReadAllAsync())
    {
        await Task.Delay(100);                 // slow consumer
        Console.WriteLine($"    Consumed {item}");
    }
});

await Task.WhenAll(producer, consumer);
Console.WriteLine("All done");
```

Representative output (interleaving may vary slightly):
```
Produced 1
Produced 2
Produced 3
    Consumed 1
Produced 4
    Consumed 2
Produced 5
    Consumed 3
    Consumed 4
    Consumed 5
All done
```

The producer runs ahead only until the belt is full, then is paced by the slow consumer. In ASP.NET Core, a controller writes jobs to a singleton `Channel<T>` and a `BackgroundService` reads them – the safe replacement for fire-and-forget.

## 15.9 `Parallel.For` / `ForEach` / `ForEachAsync` and PLINQ

| API | For | Notes |
|---|---|---|
| `Parallel.For` / `Parallel.ForEach` | **CPU-bound** loops over in-memory data | Blocks the calling thread until done; uses pool threads |
| `Parallel.ForEachAsync` (.NET 6+) | **I/O-bound** async work with a concurrency limit | `MaxDegreeOfParallelism` (default = processor count) |
| PLINQ `.AsParallel()` | CPU-heavy LINQ queries | `.AsOrdered()` to keep order; `.WithDegreeOfParallelism(n)` |
| `Task.WhenAll` | A known set of async operations | No built-in throttling (add `SemaphoreSlim`) |

```csharp
using System.Collections.Concurrent;
using System.Diagnostics;

// Parallel.For - CPU-bound
long total = 0;
Parallel.For(1, 1001, i => Interlocked.Add(ref total, i));
Console.WriteLine($"Sum 1..1000 = {total}");

// Parallel.ForEachAsync (.NET 6+) - I/O-bound with a concurrency limit
var sw = Stopwatch.StartNew();
var results = new ConcurrentBag<string>();
await Parallel.ForEachAsync(Enumerable.Range(1, 8),
    new ParallelOptions { MaxDegreeOfParallelism = 4 },
    async (id, ct) =>
    {
        await Task.Delay(250, ct);             // e.g. call an API
        results.Add($"item{id}");
    });
Console.WriteLine($"ForEachAsync: {results.Count} items in ~{sw.ElapsedMilliseconds} ms (8 items / 4 at a time x 250ms)");

// PLINQ
var primes = Enumerable.Range(2, 200_000)
    .AsParallel()
    .Where(IsPrime)
    .Count();
Console.WriteLine($"Primes below 200002: {primes}");

var ordered = Enumerable.Range(1, 10).AsParallel().AsOrdered().Select(x => x * x).Take(5);
Console.WriteLine(string.Join(",", ordered));

static bool IsPrime(int n)
{
    if (n < 2) return false;
    for (int i = 2; i * i <= n; i++) if (n % i == 0) return false;
    return true;
}
```

Representative output (timing will differ):
```
Sum 1..1000 = 500500
ForEachAsync: 8 items in ~529 ms (8 items / 4 at a time x 250ms)
Primes below 200002: 17984
1,4,9,16,25
```

⚠️ **Interview trap:** `Parallel.ForEach(items, async item => await ...)` – the lambda becomes `async void`; `ForEach` returns immediately. Use `Parallel.ForEachAsync`.

⚠️ Parallelism isn't free: for tiny loop bodies, the scheduling overhead makes `Parallel.For` **slower** than a plain `for`. Measure.

## 15.10 Thread-safe singleton with `Lazy<T>`

```csharp
Parallel.For(0, 5, _ => Console.WriteLine(AppConfig.Instance.GetHashCode() == AppConfig.Instance.GetHashCode()));
Console.WriteLine($"Constructor ran {AppConfig.CtorCount} time(s)");

public sealed class AppConfig
{
    private static readonly Lazy<AppConfig> _instance = new(() => new AppConfig());  // thread-safe by default
    public static AppConfig Instance => _instance.Value;
    public static int CtorCount;

    private AppConfig()
    {
        Interlocked.Increment(ref CtorCount);
        Thread.Sleep(100);   // expensive init
    }
}
```

Output (verified):
```
True
True
True
True
True
Constructor ran 1 time(s)
```

| Singleton style | Thread-safe? | Lazy? |
|---|---|---|
| `if (_i == null) _i = new X();` | ❌ two threads can both create | ✅ |
| `lock` + double-checked locking | ✅ (easy to get wrong) | ✅ |
| `static readonly X Instance = new();` | ✅ (CLR guarantees type init once) | Mostly (at first type access) |
| `Lazy<X>` | ✅ (`LazyThreadSafetyMode.ExecutionAndPublication`) | ✅ |
| DI `services.AddSingleton<X>()` | ✅ | ✅ – the modern default |

## 15.11 `ThreadLocal<T>` vs `AsyncLocal<T>` (brief)

- **`ThreadLocal<T>`** / `[ThreadStatic]`: each **thread** gets its own copy. Great for per-thread caches (e.g. a `Random` per thread). Broken across `await`, because the continuation may run on another thread.
- **`AsyncLocal<T>`**: value flows with the **async call chain** (the `ExecutionContext`) – across awaits and thread hops. Used for correlation IDs, `Activity.Current`, `IHttpContextAccessor`.

```csharp
var threadLocal = new ThreadLocal<string>(() => "default");
var asyncLocal = new AsyncLocal<string>();

threadLocal.Value = "request-42";
asyncLocal.Value = "request-42";

await Task.Delay(100);   // continuation may run on another thread

Console.WriteLine($"ThreadLocal after await: {threadLocal.Value}");
Console.WriteLine($"AsyncLocal  after await: {asyncLocal.Value}");
```

Representative output (if the continuation happens to resume on the same thread, ThreadLocal would still show "request-42"):
```
ThreadLocal after await: default
AsyncLocal  after await: request-42
```

## 15.12 Choosing the right tool

| Tool | Protects | Async-friendly? | Cost | Typical use |
|---|---|---|---|---|
| `Interlocked` | One `int`/`long`/reference | ✅ (non-blocking) | Lowest | Counters, flags, ID generators |
| `lock` / `Lock` (C# 13) | A block of code / several fields together | ❌ no `await` inside | Low (uncontended) | Updating related state atomically |
| `SemaphoreSlim` | N concurrent entries (1 = async mutex) | ✅ `WaitAsync` | Low-medium | Throttling, async critical sections |
| `ReaderWriterLockSlim` | Many readers / one writer | ❌ | Medium | Read-heavy shared data |
| `Mutex` | Cross-process exclusion | ❌ | High (kernel) | Single app instance |
| Concurrent collections | The collection's own operations | ✅ | Low | Shared caches, queues |
| `Channel<T>` | Producer/consumer hand-off | ✅ | Low | Background work queues, pipelines |
| Immutable / frozen collections | Nothing to protect – never mutated | ✅ | Copy on change | Config, lookup tables |

🎯 **Say this in the interview:** "First I try not to share mutable state at all. If I must: a single counter → `Interlocked`; a few fields changed together → `lock` on a private object; anything with `await` inside → `SemaphoreSlim(1,1)`; shared maps → `ConcurrentDictionary` using its atomic methods; producer/consumer → `Channel<T>`. To avoid deadlocks, I acquire locks in a consistent order and never block on async code."

🧩 **Predict the output:**

```csharp
var list = new List<int>();
Parallel.For(0, 10_000, i => list.Add(i));
Console.WriteLine(list.Count);
```

<details><summary>Answer</summary>

Unpredictable: usually **less than 10000**, and sometimes an exception (`IndexOutOfRangeException` / "Destination array was not long enough") because `List<T>.Add` is not thread-safe – two threads write into the same slot or race during internal array resizing. Fix: `ConcurrentBag<int>`, a `lock`, or PLINQ `Enumerable.Range(0, 10_000).AsParallel().ToList()`.
</details>

## Try it yourself

1. Make this thread-safe in the cheapest way: `private int _requests; public void OnRequest() => _requests++;`
2. Two methods: `Transfer(a, b)` locks `a` then `b`. Thread 1 calls `Transfer(x, y)`, thread 2 calls `Transfer(y, x)`. What can happen, and how do you fix it?
3. Limit an app to at most 5 concurrent calls to a rate-limited API, in async code.
4. Why is `lock (this)` discouraged?
5. Write a thread-safe, lazily-created singleton in one line (no DI).
6. You need an in-process background queue in ASP.NET Core where controllers enqueue emails and one worker sends them. Which type do you use?

<details><summary>Answer</summary>

1. `public void OnRequest() => Interlocked.Increment(ref _requests);`
2. Deadlock – each thread holds one lock and waits for the other. Fix by acquiring locks in a consistent order (e.g. lower account ID first), using `Monitor.TryEnter` with a timeout, or a single coarser lock.
3. `private readonly SemaphoreSlim _sem = new(5);` then `await _sem.WaitAsync(ct); try { await CallApiAsync(); } finally { _sem.Release(); }` – or `Parallel.ForEachAsync` with `MaxDegreeOfParallelism = 5`.
4. `this` is publicly reachable; any external code can lock the same object and cause contention or deadlocks you can't see. Lock on a private readonly object.
5. `private static readonly Lazy<Service> _inst = new(() => new Service()); public static Service Instance => _inst.Value;`
6. A singleton `Channel<EmailMessage>` (bounded for back-pressure) written by controllers and read with `await foreach (... ReadAllAsync(ct))` in a `BackgroundService`.
</details>

## Quick revision

- Process = isolated memory; threads in a process share the heap → need synchronization.
- `x++` is read-modify-write → race condition without sync.
- `lock` = `Monitor.Enter/Exit` in try/finally; lock on a `private readonly object` (or C# 13 `Lock`).
- Never lock on `this`, `typeof(T)`, strings or value types (CS0185).
- No `await` inside `lock` (CS1996) → `SemaphoreSlim(1,1)`.
- `Interlocked` = fastest atomic ops for a single variable.
- Deadlock = circular wait; prevent with consistent lock ordering, timeouts, short critical sections.
- `SemaphoreSlim(n)` throttles concurrency; `Mutex` is cross-process; `ReaderWriterLockSlim` for read-heavy data.
- `volatile` gives visibility, not atomicity.
- `ConcurrentDictionary.GetOrAdd` factory may run multiple times → wrap in `Lazy<T>`.
- `Channel<T>` = async producer/consumer with back-pressure; `BlockingCollection<T>` = sync version.
- `Parallel.For/ForEach` for CPU work, `Parallel.ForEachAsync` for throttled async I/O, PLINQ for parallel queries.
- `Lazy<T>` gives a thread-safe lazy singleton; `AsyncLocal<T>` flows across awaits, `ThreadLocal<T>` doesn't.

---

# 16. Modern C# Features (C# 7 → 14)

> **Why interviewers ask this:** They want to know if you write *today's* C#, not 2012 C#. Using pattern matching, records, nullable reference types and switch expressions naturally in a live-coding round is an instant "this person keeps up" signal. Questions like "what's new in C# 12?" or "what does `!` do after a variable?" are very common.

Think of C# versions like phone OS updates: the old apps (code) still run, but every update adds shortcuts that make everyday tasks shorter and safer. You don't need to memorize every feature — you need to **recognize** them, **explain** them in one sentence, and **use** the popular ones fluently.

## 16.1 Version table — the headline features

| C# | .NET (year) | Headline features you should be able to name |
|---|---|---|
| 7.0–7.3 | .NET Fx 4.7 / Core 2.x (2017–18) | Tuples & deconstruction, `out var`, pattern matching (`is int n`, `case` with types), local functions, discards `_`, `ref` locals/returns, throw expressions, digit separators `1_000`, `async Main` (7.1), `default` literal, `in` params, `readonly struct`, `ref struct`, `Span<T>` (7.2), `private protected` |
| 8.0 | .NET Core 3.x (2019) | **Nullable reference types**, **switch expressions**, property/tuple/positional patterns, `using` declarations, async streams (`IAsyncEnumerable`, `await foreach`), ranges & indices (`^1`, `1..3`), default interface methods, `??=`, static local functions, readonly members |
| 9.0 | .NET 5 (2020) | **Records**, **`init`** setters, **top-level statements**, relational & logical patterns (`>`, `and`, `or`, `not`), **target-typed `new()`**, covariant returns, static lambdas, native ints (`nint`) |
| 10 | .NET 6 (2021) | **Global usings** & implicit usings, **file-scoped namespaces**, `record struct`, extended property patterns (`{ A.B: 1 }`), lambda natural types, `const` interpolated strings |
| 11 | .NET 7 (2022) | **Raw string literals** `"""`, **list patterns** `[1, .., var last]`, **`required` members**, generic math (static abstract interface members), `file`-local types, UTF-8 literals `"abc"u8`, generic attributes |
| 12 | .NET 8 (2023, LTS) | **Primary constructors** for classes/structs, **collection expressions** `[1, 2, ..other]`, default lambda parameters, `using` alias for any type, inline arrays, `ref readonly` params |
| 13 | .NET 9 (2024) | `params` collections (`params ReadOnlySpan<T>`), new `System.Threading.Lock` type, `\e` escape, partial properties, `ref`/`unsafe` in iterators & async, `allows ref struct`, `^` index in object initializers |
| 14 | .NET 10 (2025, LTS) | **`field` keyword** in properties, **extension members** (`extension(T x) { ... }` blocks: extension properties & static members), null-conditional assignment `a?.B = x`, `nameof(List<>)`, implicit span conversions, lambda params with modifiers without types, partial constructors/events, user-defined compound assignment (`+=`) |

🎯 **Say this in the interview:** "Each C# version ships with a .NET version. I'm on .NET 8/10 so I use C# 12–14: records for DTOs, pattern matching and switch expressions instead of if-chains, nullable reference types to catch nulls at compile time, primary constructors for DI, and collection expressions."

> All examples below are top-level-statement console programs. Paste into `Program.cs` of a `dotnet new console` (.NET 10) project — or, with the .NET 10 SDK, save as `demo.cs` and run `dotnet run demo.cs`. (Nullable and implicit usings are on by default in new projects.)

## 16.2 Tuples & deconstruction (C# 7)

**Simple words:** a tuple is a lightweight, unnamed "bag" of a few values — perfect for returning 2–3 things from a method without creating a class. `ValueTuple` is a **struct** (no heap allocation). Element names (`Min`, `Max`) are compile-time sugar only — at runtime they're `Item1`, `Item2`.

```csharp
(int Min, int Max) MinMax(int[] a) => (a.Min(), a.Max());

var r = MinMax([3, 9, 1, 7]);
Console.WriteLine($"{r.Min}..{r.Max}");

var (lo, hi) = MinMax([5, 2]);          // deconstruction
Console.WriteLine(lo + hi);

int x = 1, y = 2;
(x, y) = (y, x);                         // swap without temp
Console.WriteLine($"x={x}, y={y}");

Console.WriteLine((1, "a") == (1, "a")); // tuple equality (C# 7.3)
var person = (Name: "Ann", Age: 30);
Console.WriteLine(person);
var (name, _) = person;                  // discard what you don't need
Console.WriteLine(name);
```

```text
1..9
7
x=2, y=1
True
(Ann, 30)
Ann
```

**Deconstruct your own types** by adding a `Deconstruct` method (records get one automatically):

```csharp
var (w, h) = new Size(1920, 1080);
Console.WriteLine($"{w}x{h}");

class Size(int width, int height)
{
    public void Deconstruct(out int w, out int h) => (w, h) = (width, height);
}
```

```text
1920x1080
```

⚠️ **Interview trap:** Tuples are great for *private/internal* helpers. For public APIs, prefer a `record` — tuple names are not part of the type and are easy to mix up.

## 16.3 Pattern matching — the whole family

**Simple words:** a *pattern* is a question you ask a value: "Are you an `int`? Are you greater than 10? Do you have a `Length` of 0? Are you an array starting with 1?" If the answer is yes, you can also **pull values out** in the same step.

| Pattern | Version | Example | Means |
|---|---|---|---|
| Type / declaration | 7 | `o is string s` | is it a string? if so, call it `s` |
| Constant | 7 | `x is null`, `x is 42` | equals this constant |
| `var` | 7 | `x is var v` | always matches, captures |
| Discard | 8 | `_` | matches anything |
| Property | 8 | `p is { Age: > 18, Name: "Ann" }` | check members |
| Positional | 8 | `pt is Point(0, 0)` | uses `Deconstruct` |
| Tuple | 8 | `(a, b) is (1, 2)` | match several values together |
| Relational | 9 | `x is > 0 and < 10` | `<`, `<=`, `>`, `>=` |
| Logical | 9 | `not null`, `A or B`, `A and B` | combine patterns |
| Extended property | 10 | `{ Address.City: "Pune" }` | nested member shortcut |
| List | 11 | `[1, .., var last]` | arrays/lists by shape; `..` = slice |

### Easy example — relational + logical patterns

```csharp
foreach (var s in new[] { 95, 80, 50, 10, 120 })
    Console.Write(Grade(s) + " ");
Console.WriteLine();
Console.WriteLine(IsLetter('q') + " " + IsLetter('7'));

static string Grade(int score) => score switch
{
    < 0 or > 100 => "invalid",
    >= 90 => "A",
    >= 75 => "B",
    >= 50 => "C",
    _ => "F"
};

static bool IsLetter(char c) => c is (>= 'a' and <= 'z') or (>= 'A' and <= 'Z');
```

```text
A B C F invalid 
True False
```

### Complex example — one switch, every kind of pattern

```csharp
object?[] items = [42, -7, "hello", "", null, 3.5, new Point(0, 0), new Point(3, 4), new[] { 1, 2, 3 }, 'x'];
foreach (var item in items)
    Console.WriteLine(Describe(item));

static string Describe(object? o) => o switch
{
    null                          => "null",
    int n when n < 0              => $"negative int {n}",          // type pattern + guard
    int n                         => $"int {n}",
    string { Length: 0 }          => "empty string",               // property pattern
    string s                      => $"string '{s}' ({s.Length} chars)",
    double d and > 0 and < 10     => $"small double {d}",          // relational + logical
    Point(0, 0)                   => "origin",                     // positional pattern
    Point { X: var x, Y: var y }  => $"point at distance {Math.Sqrt(x * x + y * y)}",
    int[] and [1, .., var last]   => $"int[] starting with 1, ending with {last}", // list pattern
    _                             => $"something else: {o.GetType().Name}"
};

record Point(int X, int Y);
```

```text
int 42
negative int -7
string 'hello' (5 chars)
empty string
null
small double 3.5
origin
point at distance 5
int[] starting with 1, ending with 3
something else: Char
```

### List patterns (C# 11)

```csharp
int[][] arrays = [[], [1], [1, 2], [1, 2, 3, 4], [9, 9, 9]];
foreach (var a in arrays)
    Console.WriteLine(a switch
    {
        [] => "empty",
        [var only] => $"one element: {only}",
        [var first, var second] => $"two: {first},{second}",
        [1, .. var middle, var last] => $"starts with 1, middle=[{string.Join(",", middle)}], last={last}",
        [_, _, ..] => "3+ elements, not starting with 1"
    });
```

```text
empty
one element: 1
two: 1,2
starts with 1, middle=[2,3], last=4
3+ elements, not starting with 1
```

⚠️ **Interview trap:** Order matters — the compiler picks the **first** matching arm. If an earlier arm already covers a later one, you get error **CS8510** ("pattern has already been handled"). And if your switch *expression* isn't exhaustive you get warning **CS8509** and a `SwitchExpressionException` at runtime for unhandled values.

```csharp
static string Size(int n) => n switch
{
    > 0 => "positive",
    > 10 => "big",       // ❌ does not compile: CS8510 (already handled by '> 0')
    _ => "other"
};
```

## 16.4 Switch expressions (C# 8)

**Simple words:** the old `switch` *statement* runs code; the new `switch` *expression* **returns a value**. No `case`, no `break`, arms are `pattern => result`, and `_` is the default.

```csharp
Console.WriteLine(Winner("rock", "scissors"));
Console.WriteLine(Winner("rock", "paper"));
Console.WriteLine(Winner("paper", "paper"));
Console.WriteLine(Shipping(new Order("IN", 450m)));
Console.WriteLine(Shipping(new Order("US", 20m)));

// tuple pattern: switch on two values at once
static string Winner(string a, string b) => (a, b) switch
{
    _ when a == b => "draw",
    ("rock", "scissors") or ("scissors", "paper") or ("paper", "rock") => "player 1",
    _ => "player 2"
};

static decimal Shipping(Order o) => o switch
{
    { Country: "IN", Total: >= 400 } => 0m,
    { Country: "IN" } => 40m,
    { Total: > 100 } => 10m,
    _ => 25m
};

record Order(string Country, decimal Total);
```

```text
player 1
player 2
draw
0
25
```

## 16.5 Nullable reference types (C# 8)

**Simple words:** by default, *every* reference variable could secretly be `null` — the "billion-dollar mistake". With NRT **enabled**, the compiler assumes `string` means "never null" and `string?` means "might be null", and it **warns** you when you might dereference a null. It's a **compile-time only** feature — no runtime change, no extra checks inserted.

| Syntax | Meaning |
|---|---|
| `string s` | non-nullable: compiler warns if you assign `null` |
| `string? s` | nullable: compiler warns if you use `s.Length` without a null check |
| `s!` | "null-forgiving" operator: *"trust me, it's not null"* — silences the warning, no runtime check |
| `#nullable enable` / `<Nullable>enable</Nullable>` | turn on per file / per project |
| `[NotNullWhen(true)]`, `[MaybeNull]`, `[MemberNotNull]` | attributes that teach the compiler about your helper methods |

```csharp
#nullable enable
string name = null;                  // warning CS8600: converting null to non-nullable type
string? maybe = GetMaybe();
Console.WriteLine(maybe.Length);     // warning CS8602: dereference of a possibly null reference
if (maybe is not null)
    Console.WriteLine(maybe.Length); // ✅ no warning: flow analysis knows it's not null here
Console.WriteLine(maybe?.Length ?? 0);
Console.WriteLine(maybe!.Length);    // no warning, but throws NullReferenceException at runtime if null!

static string? GetMaybe() => DateTime.Now.Year > 2000 ? "hi" : null;

class Customer
{
    public string Name { get; set; }   // warning CS8618: non-nullable property must contain a non-null value when exiting constructor
}
```

**Common warnings to recognize:** CS8600 (null → non-nullable), CS8602 (possible null dereference), CS8603 (possible null return), CS8604 (possible null argument), CS8618 (non-nullable member not initialized).

**How to fix CS8618** (pick one): initialize it (`= "";`), make it `required`, set it in the constructor, or make it `string?` if null is genuinely valid.

**How to adopt NRT in an existing codebase:**
1. Turn it on **file by file** with `#nullable enable` (or project-wide with `<Nullable>warnings</Nullable>` first to only see warnings).
2. Start at the "leaves" (DTOs, utilities), then work inward.
3. Annotate honestly: if something *can* be null, mark it `?` — don't sprinkle `!` to make warnings disappear.
4. Add `ArgumentNullException.ThrowIfNull(x)` at public API boundaries — NRT doesn't protect you from callers with NRT disabled.
5. Optionally treat nullable warnings as errors: `<WarningsAsErrors>nullable</WarningsAsErrors>`.

⚠️ **Interview trap:** "Does `string?` change the runtime type?" **No.** For reference types `string?` and `string` are the same `System.String`; only metadata attributes differ. For value types, `int?` *is* a different type: `Nullable<int>`.

🎯 **Say this in the interview:** "NRT is static analysis. It moves NullReferenceExceptions from production to compile-time warnings. I avoid the `!` operator except where I genuinely know more than the compiler, like right after a validation helper."

## 16.6 Records, `init`, `required` (C# 9 / 11)

Covered in depth in Section 5; here's the quick tour.

```csharp
var p1 = new Person("Ann", 30);
var p2 = p1 with { Age = 31 };                       // non-destructive mutation (copy + change)
Console.WriteLine(p1);
Console.WriteLine(p2);
Console.WriteLine(p1 == new Person("Ann", 30));      // value equality
Console.WriteLine(ReferenceEquals(p1, new Person("Ann", 30)));

var u = new User { Email = "a@b.com" };              // required must be set; Id optional
Console.WriteLine($"{u.Email} {u.Id}");

public record Person(string Name, int Age);          // positional record: init-only props, Equals, ToString, Deconstruct

public class User
{
    public required string Email { get; init; }      // must be set in the object initializer
    public int Id { get; init; }                     // can only be set during initialization
}
```

```text
Person { Name = Ann, Age = 30 }
Person { Name = Ann, Age = 31 }
True
False
a@b.com 0
```

```csharp
var u = new User { Email = "a@b.com" };
u.Email = "x@y.com";      // ❌ does not compile: CS8852 (init-only property)
var bad = new User();     // ❌ does not compile: CS9035 (required member 'User.Email' must be set)

public class User
{
    public required string Email { get; init; }
}
```

| Feature | Version | One-liner |
|---|---|---|
| `init` accessor | 9 | settable only in constructor / object initializer / `with` |
| `record` (class) | 9 | reference type with value equality, `ToString`, `with`, `Deconstruct` |
| `record struct` | 10 | value-type record (mutable by default; use `readonly record struct`) |
| `required` | 11 | caller **must** set it in the initializer — compile-time error otherwise |
| `[SetsRequiredMembers]` | 11 | put on a constructor that sets all required members so callers don't have to |

## 16.7 Less ceremony: top-level statements, usings, namespaces, `new()`

```csharp
// ----- Old style (C# 1–8) -----
using System;
namespace MyApp
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello");
        }
    }
}

// ----- Modern style (C# 9/10) — Program.cs is literally one line -----
Console.WriteLine("Hello");
```

* **Top-level statements (C# 9):** the compiler generates the `Program` class and `Main` for you. `args` is still available; `await` works (generates `async Task Main`); `return 1;` sets the exit code. Only **one file** per project can have them, and they must come **before** type declarations in that file.
* **Global usings (C# 10):** `global using System.Text.Json;` in any one file applies to the whole project.
* **Implicit usings (C# 10 / .NET 6):** `<ImplicitUsings>enable</ImplicitUsings>` auto-adds common namespaces (`System`, `System.Linq`, `System.Collections.Generic`, `System.IO`, `System.Threading.Tasks`, …). That's why snippets in this guide don't start with `using System;`.
* **File-scoped namespace (C# 10):** `namespace MyApp.Services;` — one less level of indentation for the whole file.
* **Target-typed `new` (C# 9):** when the type is already on the left, drop it on the right.

```csharp
namespace Shop.Services;                                   // file-scoped namespace

public class Inventory
{
    private readonly Dictionary<string, List<int>> _stock = new();   // target-typed new
    private readonly List<string> _log = [];                         // collection expression (C# 12)
    public Point Origin { get; } = new(0, 0);
}
public record Point(int X, int Y);
```

## 16.8 Raw string literals (C# 11)

**Simple words:** start and end a string with **three or more** quotes `"""` and *nothing inside* needs escaping — great for JSON, SQL, regex, Windows paths. Indentation to the left of the closing `"""` is removed automatically. For interpolation, the number of `$` = how many braces you need to open a hole: with `$$"""`, `{{name}}` is a hole and a single `{` is a literal brace.

```csharp
var name = "Ann";
var json = $$"""
    {
      "name": "{{name}}",
      "tags": ["a", "b"]
    }
    """;
Console.WriteLine(json);

var path = """C:\temp\new "quoted" file.txt""";
Console.WriteLine(path);
```

```text
{
  "name": "Ann",
  "tags": ["a", "b"]
}
C:\temp\new "quoted" file.txt
```

## 16.9 Primary constructors (C# 12) — and their trap

**Simple words:** put constructor parameters right after the class name. The parameters are in scope in the **whole class body**. It's perfect for dependency injection.

```csharp
// Before C# 12
public class OrderService
{
    private readonly IRepository _repo;
    private readonly ILogger _log;
    public OrderService(IRepository repo, ILogger log) { _repo = repo; _log = log; }
}

// C# 12
public class OrderService(IRepository repo, ILogger log)
{
    public void Place(Order o) { log.Info("placing"); repo.Save(o); }
}
```

**Key difference from records:** for a *class/struct* primary constructor, the compiler does **not** create public properties. The parameter is just a variable. If any member *uses* it after construction, the compiler silently creates a hidden **mutable** field to hold it ("capture").

⚠️ **Interview trap — double storage / mutability:**

```csharp
var c = new Counter(5);
c.Increment();
c.Increment();
Console.WriteLine($"Start={c.Start}, Current={c.Current}");

class Counter(int start)
{
    public int Start { get; } = start;   // copies the value ONCE into a property's backing field
    public int Current => start;         // captures 'start' into a hidden field
    public void Increment() => start++;  // primary ctor params are NOT readonly — they can be mutated!
}
```

```text
Start=5, Current=7
```

The compiler warns **CS9124** ("Parameter 'int start' is captured into the state of the enclosing type and its value is also used to initialize a field, property, or event") because now there are **two copies** that can drift apart. Rules of thumb:

* Either **capture** the parameter (use it in methods) **or** copy it to a field/property — never both.
* Parameters are **mutable**; if you need `readonly`, write `private readonly ILogger _log = log;` and use `_log` only.
* In a `record`, primary-constructor parameters become public init-only properties; in a `class`/`struct`, they do **not**.

## 16.10 Collection expressions & spread (C# 12)

**Simple words:** one syntax `[a, b, c]` to create arrays, lists, spans, sets, immutable collections… The compiler picks the best construction for the **target type**. `..x` "spreads" (flattens) another collection in.

```csharp
int[] a = [1, 2, 3];
List<int> b = [4, 5];
Span<int> s = [6, 7];
HashSet<string> tags = ["c#", "dotnet", "c#"];
int[] all = [0, .. a, .. b, .. s, 99];
List<string> empty = [];

Console.WriteLine(string.Join(",", all));
Console.WriteLine(tags.Count);
Console.WriteLine(empty.Count);
Console.WriteLine(Sum([10, 20, 30]));
Console.WriteLine(Sum(1, 2));

static int Sum(params ReadOnlySpan<int> nums)   // C# 13: params collections (no array allocation)
{
    var total = 0;
    foreach (var n in nums) total += n;
    return total;
}
```

```text
0,1,2,3,4,5,6,7,99
2
0
60
3
```

⚠️ **Interview trap:** `var x = [1, 2];` does **not** compile (**CS9176**: no target type) — the compiler needs to know *which* collection to build.

```csharp
var x = [1, 2];   // ❌ does not compile: CS9176
```

## 16.11 Lambdas: natural type & default parameters (C# 10 / 12)

```csharp
var square = (int x) => x * x;                             // C# 10: natural type -> Func<int,int>
var greet = (string name = "world") => $"Hello, {name}!";  // C# 12: default parameter
var parse = static (string s) => int.Parse(s);             // C# 9: static lambda can't capture locals

Console.WriteLine(square(7));
Console.WriteLine(greet());
Console.WriteLine(greet("Ann"));
Console.WriteLine(parse("42") + 1);
```

```text
49
Hello, world!
Hello, Ann!
43
```

## 16.12 C# 13 & 14 highlights (.NET 9 / .NET 10)

**C# 14 `field` keyword:** inside a property accessor, `field` refers to the compiler-generated backing field. You can add logic to *one* accessor without declaring a private field yourself.

**C# 14 extension members:** a new `extension(T receiver) { ... }` block inside a static class lets you write extension **properties** (and static extension members), not just methods. Old `this T x` extension methods still work and are compatible.

**C# 14 null-conditional assignment:** `obj?.Prop = value;` assigns only if `obj` isn't null.

**C# 13 `Lock` type:** `private readonly Lock _gate = new(); lock (_gate) { ... }` — a dedicated, faster lock object than `object`.

```csharp
// C# 14 — the default language version for net10.0 projects
var p = new Person { Name = "   Ann  " };
Console.WriteLine($"[{p.Name}]");

Console.WriteLine("   ".IsBlank);     // extension PROPERTY
Console.WriteLine("hi".Shout());       // extension method declared in an extension block

Person? nobody = null;
nobody?.Name = "Bob";                  // null-conditional assignment: does nothing when null
Console.WriteLine(nobody is null);

Console.WriteLine(nameof(List<>));     // unbound generic in nameof

class Person
{
    public string Name
    {
        get;
        set => field = value.Trim();    // 'field' = auto-generated backing field
    } = "";
}

static class StringExtensions
{
    extension(string s)
    {
        public bool IsBlank => string.IsNullOrWhiteSpace(s);
        public string Shout() => s.ToUpper() + "!";
    }
}
```

```text
[Ann]
True
HI!
True
List
```

⚠️ **Interview trap:** In C# 14, if your class *already* has a member named `field`, inside accessors the keyword wins — use `@field` or `this.field` to refer to your member.

## Try it yourself

1. Write a switch expression `Classify(int n)` that returns `"zero"`, `"small"` (1–9), `"big"` (≥10) or `"negative"`.
2. Using a list pattern, write `IsSandwich(int[] a)` that is true when the array has at least 2 elements and the first equals the last.
3. What does this print? `int[] a = [1, 2]; int[] b = [..a, ..a]; Console.WriteLine(b.Length);`
4. Rewrite `class Svc { private readonly ILogger _l; public Svc(ILogger l) { _l = l; } }` with a primary constructor while keeping the field `readonly`.
5. Why does `string s = null;` compile at all when NRT is enabled?
6. Make a property `Age` that throws if a negative value is set, using the C# 14 `field` keyword.

<details><summary>Answer</summary>

```csharp
// 1
static string Classify(int n) => n switch { < 0 => "negative", 0 => "zero", < 10 => "small", _ => "big" };

// 2
static bool IsSandwich(int[] a) => a is [var first, .., var last] && first == last;

// 3 -> prints 4

// 4
class Svc(ILogger l) { private readonly ILogger _l = l; }   // use _l only, never 'l' in members

// 5 -> NRT produces WARNINGS (CS8600), not errors. Nothing changes at runtime.
//      Use <WarningsAsErrors>nullable</WarningsAsErrors> to make them errors.

// 6
class Person
{
    public int Age
    {
        get;
        set => field = value >= 0 ? value : throw new ArgumentOutOfRangeException(nameof(value));
    }
}
```

</details>

## Quick revision

* C# 7 tuples: `(int A, int B)` is a `ValueTuple` struct; names are compile-time only.
* Pattern matching = test + extract in one step; switch expressions **return** a value; first match wins.
* Relational/logical patterns (C# 9): `is > 0 and < 10`, `is not null`.
* List patterns (C# 11): `[first, .., last]`, `..` matches a slice.
* NRT is compile-time only; `?` = may be null, `!` = "trust me"; warnings CS8600/8602/8618.
* Records = value equality + `with` + `ToString`; `init` = set once at creation; `required` = must set.
* Top-level statements, global/implicit usings, file-scoped namespaces remove boilerplate.
* Raw strings `"""` need no escaping; `$$` means `{{x}}` is the interpolation hole.
* Primary constructors (C# 12): params are mutable & captured; don't both capture and copy (CS9124).
* Collection expressions `[..a, ..b]` need a target type; `params ReadOnlySpan<T>` (C# 13) avoids allocations.
* C# 14: `field` keyword, extension members/properties, `a?.B = x`.

# 17. Reflection, Attributes & Source Generators (brief)

> **Why interviewers ask this:** Every framework you use — ASP.NET Core model binding, Entity Framework, xUnit, JSON serializers, DI containers — is built on reflection and attributes. Interviewers want to see that you know *what* they are, *when* they're appropriate, and that reflection is **slow** (and that source generators are the modern fix).

**Simple words:** *Reflection* is code that looks in a mirror: at runtime, your program can ask "what type is this? what properties does it have? call the method named `X`". *Attributes* are sticky notes (`[Obsolete]`, `[Required]`) you attach to code; they do nothing by themselves — some other code (a framework, the compiler, or your reflection code) reads the notes and acts on them.

## 17.1 `Type`, `GetType()` vs `typeof`

| | `typeof(T)` | `obj.GetType()` |
|---|---|---|
| When evaluated | Compile time (type name known) | Runtime (needs an instance) |
| Returns | The **declared** type you wrote | The **actual runtime** type of the object |
| Null? | Never | Throws `NullReferenceException` if `obj` is null |
| Example | `typeof(Animal)` → `Animal` | `Animal a = new Dog(); a.GetType()` → `Dog` |

```csharp
Animal a = new Dog();
Console.WriteLine(typeof(Animal).Name);
Console.WriteLine(a.GetType().Name);
Console.WriteLine(a.GetType() == typeof(Animal));       // exact type check
Console.WriteLine(a is Animal);                          // "is" respects inheritance
Console.WriteLine(typeof(Animal).IsAssignableFrom(a.GetType()));
Console.WriteLine(typeof(List<int>));
Console.WriteLine(typeof(List<>).IsGenericTypeDefinition);

class Animal { }
class Dog : Animal { }
```

```text
Animal
Dog
False
True
True
System.Collections.Generic.List`1[System.Int32]
True
```

## 17.2 Reading and invoking members dynamically

```csharp
using System.Reflection;

var type = typeof(Person);
object obj = Activator.CreateInstance(type)!;            // create without 'new Person()'

type.GetProperty("Name")!.SetValue(obj, "Ann");
type.GetProperty("Age")!.SetValue(obj, 30);

foreach (PropertyInfo p in type.GetProperties())
    Console.WriteLine($"{p.Name} ({p.PropertyType.Name}) = {p.GetValue(obj)}");

MethodInfo greet = type.GetMethod("Greet")!;
Console.WriteLine(greet.Invoke(obj, ["Bob"]));            // call a method by name

FieldInfo secret = type.GetField("_secret", BindingFlags.NonPublic | BindingFlags.Instance)!;
Console.WriteLine($"private field: {secret.GetValue(obj)}");   // reflection can read privates!

foreach (var m in type.GetMethods(BindingFlags.Public | BindingFlags.Instance | BindingFlags.DeclaredOnly))
    Console.WriteLine($"method: {m.Name}");

class Person
{
    private string _secret = "42";
    public string Name { get; set; } = "";
    public int Age { get; set; }
    public string Greet(string other) => $"Hi {other}, I'm {Name}";
}
```

```text
Name (String) = Ann
Age (Int32) = 30
Hi Bob, I'm Ann
private field: 42
method: get_Name
method: set_Name
method: get_Age
method: set_Age
method: Greet
```

Notice: properties are really pairs of methods `get_X` / `set_X` under the hood — reflection shows that.

**Real-world uses:** serializers (walk all properties), ORMs (map columns to properties), test frameworks (find methods with `[Fact]`), DI containers (find constructors and their parameter types), plugin systems (`Assembly.LoadFrom` + find types implementing `IPlugin`).

## 17.3 Custom attributes — runnable validation example

**Easy:** built-in attributes you've used: `[Obsolete]`, `[Serializable]`, `[Flags]`, `[HttpGet]`, `[Required]`, `[JsonPropertyName("x")]`, `[TestMethod]`.

**Complex:** write your own mini "DataAnnotations" validator.

```csharp
using System.Reflection;

var forms = new[]
{
    new SignUp { UserName = "ann", Age = 30, Email = "ann@x.com" },
    new SignUp { UserName = "", Age = 12, Email = null },
};
foreach (var f in forms)
{
    var errors = Validator.Validate(f);
    Console.WriteLine(errors.Count == 0 ? "valid" : string.Join("; ", errors));
}

// 1. Define attributes: class name ends with "Attribute", inherits System.Attribute
[AttributeUsage(AttributeTargets.Property)]
sealed class NotEmptyAttribute : Attribute { }

[AttributeUsage(AttributeTargets.Property)]
sealed class InRangeAttribute(int min, int max) : Attribute
{
    public int Min { get; } = min;
    public int Max { get; } = max;
}

// 2. Apply them ("Attribute" suffix is optional when using)
class SignUp
{
    [NotEmpty] public string UserName { get; set; } = "";
    [InRange(18, 99)] public int Age { get; set; }
    [NotEmpty] public string? Email { get; set; }
}

// 3. Read them with reflection
static class Validator
{
    public static List<string> Validate(object o)
    {
        var errors = new List<string>();
        foreach (var p in o.GetType().GetProperties())
        {
            var value = p.GetValue(o);
            if (p.GetCustomAttribute<NotEmptyAttribute>() is not null
                && string.IsNullOrWhiteSpace(value as string))
                errors.Add($"{p.Name} is required");

            if (p.GetCustomAttribute<InRangeAttribute>() is { } r
                && value is int n && (n < r.Min || n > r.Max))
                errors.Add($"{p.Name} must be {r.Min}-{r.Max}");
        }
        return errors;
    }
}
```

```text
valid
UserName is required; Age must be 18-99; Email is required
```

⚠️ **Interview trap:** Attributes are **passive metadata**. `[InRange(18, 99)]` does *not* stop you from setting `Age = 5`. Something must read and enforce it (ASP.NET Core model validation does this for `[Range]` automatically). Attribute arguments must be **compile-time constants** (`typeof`, enums, literals, arrays of those).

## 17.4 The cost of reflection

Reflection is **much slower** than direct calls (often 10–100× for `Invoke`/`GetValue`), allocates (boxing of value types, `object[]` for arguments), defeats compiler checks (typos in `"Name"` fail at runtime), and is hostile to trimming / Native AOT.

```csharp
using System.Diagnostics;
using System.Reflection;

var p = new Person { Name = "Ann" };
PropertyInfo prop = typeof(Person).GetProperty(nameof(Person.Name))!;   // cache lookups!
Func<Person, string> compiled = (Func<Person, string>)Delegate.CreateDelegate(
    typeof(Func<Person, string>), prop.GetGetMethod()!);                 // fast path

const int N = 5_000_000;
var sw = Stopwatch.StartNew();
for (int i = 0; i < N; i++) _ = p.Name;                  Console.WriteLine($"direct:     {sw.ElapsedMilliseconds} ms");
sw.Restart();
for (int i = 0; i < N; i++) _ = prop.GetValue(p);        Console.WriteLine($"reflection: {sw.ElapsedMilliseconds} ms");
sw.Restart();
for (int i = 0; i < N; i++) _ = compiled(p);             Console.WriteLine($"delegate:   {sw.ElapsedMilliseconds} ms");

class Person { public string Name { get; set; } = ""; }
```

Timings vary by machine (so no fixed output here), but the shape is always: *direct* ≈ *delegate* ≪ *reflection*. When I ran it, `GetValue` was roughly 25× slower than the direct call (.NET 10, Release build).

**How to reduce the cost:** cache `Type`/`PropertyInfo`/`MethodInfo` objects (lookups are expensive), convert to delegates (`CreateDelegate`) or compiled expression trees once and reuse, use generics instead of `object`, or avoid runtime reflection entirely with **source generators**.

## 17.5 `nameof` — compile-time names

`nameof(x)` turns a symbol into a string **at compile time** — zero runtime cost and refactor-safe (rename the property and the string updates).

```csharp
var c = new Customer();
Console.WriteLine(nameof(Customer));
Console.WriteLine(nameof(c.Email));
Console.WriteLine(nameof(Customer.Save));
try { c.Save(null); }
catch (ArgumentNullException ex) { Console.WriteLine(ex.ParamName); }

class Customer
{
    public string Email { get; set; } = "";
    public void Save(string? connection)
    {
        if (connection is null) throw new ArgumentNullException(nameof(connection));
    }
}
```

```text
Customer
Email
Save
connection
```

Use `nameof` in `ArgumentException`s, `INotifyPropertyChanged`, logging, and reflection lookups instead of magic strings.

## 17.6 Source generators in one paragraph

A **source generator** is a plug-in to the C# compiler (Roslyn) that runs **during build**, inspects your code (e.g., finds classes with an attribute), and **adds new C# source files** to the compilation. It's "reflection at compile time": the work that libraries used to do at runtime via reflection (serialization, DI registration, regex parsing, logging) is done once at build time, producing fast, trimming/AOT-friendly, debuggable code. Generators can only **add** code, never modify yours — which is why generated code relies on `partial` classes/methods. Examples in the box: `[GeneratedRegex]`, `System.Text.Json` `JsonSerializerContext`, `[LoggerMessage]`, `[LibraryImport]`, and the CommunityToolkit MVVM `[ObservableProperty]`.

```csharp
using System.Text.RegularExpressions;

Console.WriteLine(Patterns.Digits().IsMatch("order-123"));
Console.WriteLine(Patterns.Digits().Match("order-123").Value);

static partial class Patterns
{
    [GeneratedRegex(@"\d+")]            // regex code generated at compile time
    public static partial Regex Digits();
}
```

```text
True
123
```

## Try it yourself

1. What's printed by `object o = "hi"; Console.WriteLine(o.GetType() == typeof(object));`?
2. Write code that prints every public property name and value of any object.
3. Create a `[Table("users")]` attribute for classes and read the table name with reflection.
4. Why is `nameof(Person.Name)` better than `"Name"`?
5. Name two ways to make repeated reflection-based property reads faster.

<details><summary>Answer</summary>

```csharp
// 1 -> False. GetType() returns the runtime type System.String.

// 2
static void Dump(object o)
{
    foreach (var p in o.GetType().GetProperties())
        Console.WriteLine($"{p.Name} = {p.GetValue(o)}");
}

// 3
[AttributeUsage(AttributeTargets.Class)]
sealed class TableAttribute(string name) : Attribute { public string Name { get; } = name; }

[Table("users")] class User { }

// string? table = typeof(User).GetCustomAttribute<TableAttribute>()?.Name;   // "users"
//   (needs: using System.Reflection;)

// 4 -> Compile-time checked & renamed by refactoring tools; zero runtime cost.

// 5 -> Cache the PropertyInfo, and turn the getter into a delegate (Delegate.CreateDelegate /
//      compiled Expression) once; or use a source generator to avoid reflection entirely.
```

</details>

## Quick revision

* Reflection = inspect & use types/members at runtime (`Type`, `PropertyInfo`, `MethodInfo`).
* `typeof(T)` = compile-time declared type; `obj.GetType()` = actual runtime type.
* `GetType() == typeof(X)` is an exact match; `is X` respects inheritance.
* `Activator.CreateInstance`, `GetProperty().GetValue/SetValue`, `GetMethod().Invoke` = dynamic use.
* Reflection can read private members (`BindingFlags.NonPublic | BindingFlags.Instance`).
* Attributes are passive metadata; something must read them (`GetCustomAttribute<T>()`).
* Custom attribute: inherit `Attribute`, name ends with `Attribute`, constant arguments, `[AttributeUsage]`.
* Reflection is slow & AOT-unfriendly: cache metadata, convert to delegates.
* `nameof` is compile-time, refactor-safe, free.
* Source generators = compiler plug-ins that add code at build time (`[GeneratedRegex]`, JSON source gen).

# 18. SOLID & Design Patterns in C#

> **Why interviewers ask this:** With 2 years of experience you're expected to write code that's *easy to change and test*, not just code that works. "Explain SOLID with an example" and "Which design patterns have you used?" appear in almost every .NET interview. They're checking whether you can **name** a pattern, **show** it in C#, and — most importantly — **say when NOT to use it**.

**Simple words:** SOLID = five habits that keep classes small, replaceable and testable. Design patterns = named, proven solutions to common design problems, like recipes in a cookbook. Analogy: SOLID is "keep your kitchen organized"; patterns are "recipes".

## 18.1 SOLID — one principle, one tiny bad → good example

| Letter | Principle | One-line meaning | Smell that breaks it |
|---|---|---|---|
| **S** | Single Responsibility | A class should have **one reason to change** | `InvoiceService` that calculates, prints PDFs *and* sends emails |
| **O** | Open/Closed | Open for **extension**, closed for **modification** | Growing `switch (type)` you edit for every new case |
| **L** | Liskov Substitution | A subclass must be usable **anywhere** its base is, without surprises | Overrides that throw `NotSupportedException` or change rules |
| **I** | Interface Segregation | Many **small**, focused interfaces beat one fat one | Classes implementing methods with `throw new NotImplementedException()` |
| **D** | Dependency Inversion | Depend on **abstractions**, not concrete classes | `new SqlRepository()` inside business logic |

### S — Single Responsibility

```csharp
// ❌ Bad: three reasons to change (tax rules, PDF format, SMTP settings)
class InvoiceService
{
    public decimal CalculateTotal(Invoice i) { /* tax logic */ return 0; }
    public byte[] RenderPdf(Invoice i) { /* PDF logic */ return []; }
    public void Email(Invoice i, string to) { /* SMTP logic */ }
}

// ✅ Good: each class has one job; a coordinator wires them together
class InvoiceCalculator { public decimal Total(Invoice i) => i.Lines.Sum(l => l.Price * l.Qty) * 1.18m; }
class InvoicePdfRenderer { public byte[] Render(Invoice i) => []; }
class InvoiceMailer { public void Send(byte[] pdf, string to) { } }

record Invoice(List<Line> Lines);
record Line(decimal Price, int Qty);
```

### O — Open/Closed

```csharp
Shape[] shapes = [new Circle(1), new Rect(2, 3), new Triangle(4, 5)];
foreach (var s in shapes)
    Console.WriteLine($"{s.GetType().Name}: {s.Area():F2}");

// ❌ Bad (closed for extension): every new shape means editing this switch
static double AreaBad(object s) => s switch
{
    Circle c => Math.PI * c.R * c.R,
    Rect r => r.W * r.H,
    _ => throw new NotSupportedException()
};

// ✅ Good: add a new shape = add a new class, touch nothing existing
abstract record Shape { public abstract double Area(); }
record Circle(double R) : Shape { public override double Area() => Math.PI * R * R; }
record Rect(double W, double H) : Shape { public override double Area() => W * H; }
record Triangle(double B, double H) : Shape { public override double Area() => 0.5 * B * H; }   // new: no edits elsewhere
```

```text
Circle: 3.14
Rect: 6.00
Triangle: 10.00
```

### L — Liskov Substitution (the classic Rectangle/Square)

```csharp
Rectangle r = new Square();        // "a square IS a rectangle"… mathematically
r.Width = 5;
r.Height = 4;
Console.WriteLine($"Expected 20, got {r.Area}");

class Rectangle
{
    public virtual int Width { get; set; }
    public virtual int Height { get; set; }
    public int Area => Width * Height;
}
class Square : Rectangle               // ❌ violates LSP: changes the base class's rules
{
    public override int Width  { set { base.Width = base.Height = value; } }
    public override int Height { set { base.Width = base.Height = value; } }
}
```

```text
Expected 20, got 16
```

✅ **Fix:** don't inherit. Make both implement `IShape { int Area { get; } }`, or make them immutable records (`record Rectangle(int W, int H)`, `record Square(int Side)`). Another LSP red flag: `class ReadOnlyList : List<T>` whose `Add` throws.

### I — Interface Segregation

```csharp
// ❌ Bad: a robot is forced to "eat"
interface IWorker { void Work(); void Eat(); }
class Robot : IWorker
{
    public void Work() { }
    public void Eat() => throw new NotImplementedException();   // smell!
}

// ✅ Good: small role interfaces; classes pick what applies
interface IWorkable { void Work(); }
interface IFeedable { void Eat(); }
class Human : IWorkable, IFeedable { public void Work() { } public void Eat() { } }
class Droid : IWorkable { public void Work() { } }
```

.NET itself does this: `IEnumerable<T>` → `ICollection<T>` → `IList<T>`, and `IReadOnlyList<T>` for consumers that only read.

### D — Dependency Inversion

```csharp
var service = new OrderService(new FakeRepo());          // test or prod decides — not OrderService
service.Place("book");

// ❌ Bad: high-level policy depends on a low-level detail; untestable without a database
class OrderServiceBad
{
    private readonly SqlOrderRepo _repo = new();           // hard-wired
    public void Place(string item) => _repo.Save(item);
}
class SqlOrderRepo { public void Save(string item) { /* real SQL */ } }

// ✅ Good: both depend on an abstraction; the dependency is injected
interface IOrderRepo { void Save(string item); }
class OrderService(IOrderRepo repo)
{
    public void Place(string item) => repo.Save(item);
}
class FakeRepo : IOrderRepo { public void Save(string item) => Console.WriteLine($"saved {item} (fake)"); }
```

```text
saved book (fake)
```

🎯 **Say this in the interview:** "SOLID is about managing change. In my projects, D and S show up most: services take interfaces through constructor injection so I can unit test them with fakes, and I split classes when they start changing for unrelated reasons. I apply them pragmatically — not every class needs an interface."

⚠️ **Interview trap:** DIP (a *principle*: depend on abstractions) ≠ DI (a *technique*: pass dependencies in from outside) ≠ IoC container (a *tool* that does DI for you, e.g. `Microsoft.Extensions.DependencyInjection`).

## 18.2 Singleton — one instance for the whole app

**When:** truly global, stateless or thread-safe shared resources (configuration cache, `HttpClient` handler pool). **Prefer** registering a singleton in DI over hand-written singletons — easier to test and replace.

```csharp
// 1) Naive — NOT thread-safe: two threads can both see null and create two instances
public sealed class NaiveSingleton
{
    private static NaiveSingleton? _instance;
    private NaiveSingleton() { }
    public static NaiveSingleton Instance => _instance ??= new NaiveSingleton();
}

// 2) Double-checked locking — thread-safe, but verbose and easy to get wrong
public sealed class LockedSingleton
{
    private static volatile LockedSingleton? _instance;
    private static readonly object _gate = new();
    private LockedSingleton() { }
    public static LockedSingleton Instance
    {
        get
        {
            if (_instance is null)                 // 1st check: fast path, no lock
                lock (_gate)
                    _instance ??= new LockedSingleton();   // 2nd check inside the lock
            return _instance;
        }
    }
}

// 3) Static readonly — thread-safe by the CLR's type-initializer guarantee (simplest)
public sealed class EagerSingleton
{
    public static EagerSingleton Instance { get; } = new();
    private EagerSingleton() { }
}

// 4) Lazy<T> — thread-safe AND lazy. ✅ Recommended hand-written version
public sealed class LazySingleton
{
    private static readonly Lazy<LazySingleton> _lazy = new(() => new LazySingleton());
    public static LazySingleton Instance => _lazy.Value;
    private LazySingleton() { }
}
```

Proving `Lazy<T>` creates exactly one instance even under parallel access:

```csharp
var instances = new System.Collections.Concurrent.ConcurrentBag<AppConfig>();
Parallel.For(0, 1000, _ => instances.Add(AppConfig.Instance));
Console.WriteLine($"distinct instances: {instances.Distinct().Count()}");
Console.WriteLine($"constructor calls: {AppConfig.CtorCalls}");

public sealed class AppConfig
{
    private static readonly Lazy<AppConfig> _lazy = new(() => new AppConfig());
    public static AppConfig Instance => _lazy.Value;
    public static int CtorCalls;
    private AppConfig() { Interlocked.Increment(ref CtorCalls); Thread.Sleep(50); }
}
```

```text
distinct instances: 1
constructor calls: 1
```

**"Use DI instead":**

```csharp
// Program.cs in ASP.NET Core — the container guarantees one instance, and tests can swap it
builder.Services.AddSingleton<IClock, SystemClock>();
```

⚠️ **Interview trap:** Singletons are global state → hidden dependencies, hard to unit test, and must be thread-safe. Also: never inject a *scoped* service (like `DbContext`) into a singleton — the "captive dependency" problem.

## 18.3 Factory & Factory Method — hide `new`

**Simple words:** instead of `new EmailSender()` scattered everywhere, ask a factory: "give me a sender for *this* channel". Callers depend on the interface; the factory decides the concrete class.

* **Simple (static) factory:** one method with a `switch` that returns different implementations.
* **Factory Method pattern (GoF):** a base class defines an abstract `Create...()` method; subclasses decide what to create.

```csharp
// Simple factory
foreach (var channel in new[] { "email", "sms" })
    NotifierFactory.Create(channel).Send("Your order shipped");

// Factory Method: subclasses decide which product to create
Dialog[] dialogs = [new WindowsDialog(), new WebDialog()];
foreach (var d in dialogs) d.Render();

interface INotifier { void Send(string msg); }
class EmailNotifier : INotifier { public void Send(string msg) => Console.WriteLine($"EMAIL: {msg}"); }
class SmsNotifier : INotifier { public void Send(string msg) => Console.WriteLine($"SMS: {msg}"); }

static class NotifierFactory
{
    public static INotifier Create(string channel) => channel switch
    {
        "email" => new EmailNotifier(),
        "sms" => new SmsNotifier(),
        _ => throw new ArgumentException($"Unknown channel {channel}", nameof(channel))
    };
}

interface IButton { string Draw(); }
class WinButton : IButton { public string Draw() => "[Win Button]"; }
class HtmlButton : IButton { public string Draw() => "<button>"; }

abstract class Dialog
{
    protected abstract IButton CreateButton();          // the "factory method"
    public void Render() => Console.WriteLine($"{GetType().Name} renders {CreateButton().Draw()}");
}
class WindowsDialog : Dialog { protected override IButton CreateButton() => new WinButton(); }
class WebDialog : Dialog { protected override IButton CreateButton() => new HtmlButton(); }
```

```text
EMAIL: Your order shipped
SMS: Your order shipped
WindowsDialog renders [Win Button]
WebDialog renders <button>
```

**When to use:** object creation depends on runtime input/config, or creation is complex. In DI apps, a factory can be just a `Func<string, INotifier>` or `IServiceProvider`-backed class; .NET's `IHttpClientFactory` and `ILoggerFactory` are real-world factories.

## 18.4 Builder (fluent) — step-by-step construction

**When:** an object has many optional parts, and constructors with 8 parameters are unreadable. Examples in .NET: `StringBuilder`, `WebApplication.CreateBuilder`, `HostBuilder`, `UriBuilder`, EF Core's `ModelBuilder`.

```csharp
var request = new HttpRequestBuilder("https://api.shop.com")
    .WithPath("/orders")
    .WithHeader("Accept", "application/json")
    .WithHeader("X-Trace", "abc")
    .WithTimeout(TimeSpan.FromSeconds(5))
    .Build();
Console.WriteLine(request);

record HttpRequestSpec(string Url, IReadOnlyDictionary<string, string> Headers, TimeSpan Timeout)
{
    public override string ToString() =>
        $"GET {Url} timeout={Timeout.TotalSeconds}s headers=[{string.Join(", ", Headers.Select(h => $"{h.Key}: {h.Value}"))}]";
}

class HttpRequestBuilder(string baseUrl)
{
    private string _path = "/";
    private readonly Dictionary<string, string> _headers = [];
    private TimeSpan _timeout = TimeSpan.FromSeconds(30);

    public HttpRequestBuilder WithPath(string path) { _path = path; return this; }   // return 'this' = fluent
    public HttpRequestBuilder WithHeader(string k, string v) { _headers[k] = v; return this; }
    public HttpRequestBuilder WithTimeout(TimeSpan t) { _timeout = t; return this; }

    public HttpRequestSpec Build()
    {
        if (_timeout <= TimeSpan.Zero) throw new InvalidOperationException("Timeout must be positive");
        return new HttpRequestSpec(baseUrl + _path, new Dictionary<string, string>(_headers), _timeout);
    }
}
```

```text
GET https://api.shop.com/orders timeout=5s headers=[Accept: application/json, X-Trace: abc]
```

💡 With records + `required` + object initializers, you often don't need a builder for simple DTOs. Use a builder when there's **validation**, **ordering**, or an **immutable result**.

## 18.5 Strategy — swap an algorithm at runtime

**Simple words:** "the *what* stays the same, the *how* is pluggable". Like choosing Google Maps' route mode: car / bike / walk.

```csharp
var checkout = new Checkout(new NoDiscount());
Console.WriteLine(checkout.Total(1000m));
checkout.Discount = new PercentDiscount(10);
Console.WriteLine(checkout.Total(1000m));
checkout.Discount = new FlatDiscount(150m);
Console.WriteLine(checkout.Total(1000m));

// Lightweight strategy: just a delegate
Func<decimal, decimal> festive = amount => amount > 500 ? amount * 0.8m : amount;
Console.WriteLine(festive(1000m));

interface IDiscountStrategy { decimal Apply(decimal amount); }
class NoDiscount : IDiscountStrategy { public decimal Apply(decimal a) => a; }
class PercentDiscount(decimal pct) : IDiscountStrategy { public decimal Apply(decimal a) => a - a * pct / 100; }
class FlatDiscount(decimal off) : IDiscountStrategy { public decimal Apply(decimal a) => Math.Max(0, a - off); }

class Checkout(IDiscountStrategy discount)
{
    public IDiscountStrategy Discount { get; set; } = discount;
    public decimal Total(decimal amount) => Discount.Apply(amount);
}
```

```text
1000
900
850
800.0
```

**When to use:** several interchangeable algorithms (pricing, sorting, compression, payment providers) chosen by config/user. It's the cure for a giant `if/else` (and it's OCP in action). `IComparer<T>` passed to `List.Sort` is a built-in strategy.

## 18.6 Observer — notify subscribers when something happens

**Simple words:** a YouTube channel (subject) and subscribers (observers). In C#, the language has it built in: **events**. There's also the `IObservable<T>`/`IObserver<T>` pair (the basis of Rx.NET).

```csharp
var stock = new Stock("MSFT", 400m);
stock.PriceChanged += (_, e) => Console.WriteLine($"[Display] {e.Symbol}: {e.Old} -> {e.New}");
EventHandler<PriceChangedEventArgs> alert = (_, e) => { if (e.New > 420) Console.WriteLine("[Alert] above 420!"); };
stock.PriceChanged += alert;

stock.Price = 410m;
stock.Price = 425m;
stock.PriceChanged -= alert;        // unsubscribe (avoid memory leaks!)
stock.Price = 430m;

// IObservable<T> version
var feed = new TemperatureFeed();
using (feed.Subscribe(new ConsolePrinter()))
{
    feed.Publish(21.5);
    feed.Publish(23.0);
}
feed.Publish(99);                   // printer unsubscribed by Dispose — nothing printed

record PriceChangedEventArgs(string Symbol, decimal Old, decimal New);

class Stock(string symbol, decimal price)
{
    public event EventHandler<PriceChangedEventArgs>? PriceChanged;
    public decimal Price
    {
        get => price;
        set
        {
            if (value == price) return;
            var old = price;
            price = value;
            PriceChanged?.Invoke(this, new PriceChangedEventArgs(symbol, old, value));
        }
    }
}

class TemperatureFeed : IObservable<double>
{
    private readonly List<IObserver<double>> _observers = [];
    public IDisposable Subscribe(IObserver<double> o) { _observers.Add(o); return new Unsubscriber(() => _observers.Remove(o)); }
    public void Publish(double t) { foreach (var o in _observers.ToList()) o.OnNext(t); }
    private sealed class Unsubscriber(Action onDispose) : IDisposable { public void Dispose() => onDispose(); }
}

class ConsolePrinter : IObserver<double>
{
    public void OnNext(double v) => Console.WriteLine($"temp = {v}");
    public void OnError(Exception e) { }
    public void OnCompleted() { }
}
```

```text
[Display] MSFT: 400 -> 410
[Display] MSFT: 410 -> 425
[Alert] above 420!
[Display] MSFT: 425 -> 430
temp = 21.5
temp = 23
```

⚠️ **Interview trap:** The publisher holds a reference to every subscriber. A long-lived publisher with short-lived subscribers that never unsubscribe = **memory leak** (see Section 9).

## 18.7 Decorator — add behavior by wrapping

**Simple words:** gift-wrapping. The box (object) stays the same; you wrap layers (logging, caching, retry) around it, and each layer has the **same interface**. .NET's `Stream` family is decorators: `new GZipStream(new BufferedStream(fileStream), ...)`.

```csharp
IWeatherService service = new LoggingWeather(new CachingWeather(new SlowWeatherApi()));
Console.WriteLine(service.Get("Pune"));
Console.WriteLine(service.Get("Pune"));   // served from cache
Console.WriteLine(service.Get("Delhi"));

interface IWeatherService { string Get(string city); }

class SlowWeatherApi : IWeatherService
{
    public string Get(string city) { Console.WriteLine($"  (calling API for {city})"); return $"{city}: 31°C"; }
}

class CachingWeather(IWeatherService inner) : IWeatherService
{
    private readonly Dictionary<string, string> _cache = [];
    public string Get(string city)
    {
        if (_cache.TryGetValue(city, out var hit)) return hit;
        return _cache[city] = inner.Get(city);
    }
}

class LoggingWeather(IWeatherService inner) : IWeatherService
{
    public string Get(string city)
    {
        Console.WriteLine($"-> Get({city})");
        var result = inner.Get(city);
        Console.WriteLine($"<- Get({city})");
        return result;
    }
}
```

```text
-> Get(Pune)
  (calling API for Pune)
<- Get(Pune)
Pune: 31°C
-> Get(Pune)
<- Get(Pune)
Pune: 31°C
-> Get(Delhi)
  (calling API for Delhi)
<- Get(Delhi)
Delhi: 31°C
```

**When to use:** cross-cutting concerns (logging, caching, retries, metrics, authorization) without touching the original class and without inheritance explosions. ASP.NET Core middleware and `DelegatingHandler` in `HttpClient` follow the same idea.

## 18.8 Adapter — make an incompatible class fit your interface

**Simple words:** a travel plug adapter. Your app expects `ITemperatureSensor` in Celsius; a legacy/3rd-party library gives Fahrenheit through a different method name. Write a small class that **translates**.

```csharp
ITemperatureSensor sensor = new FahrenheitSensorAdapter(new LegacyFahrenheitSensor());
Console.WriteLine($"{sensor.ReadCelsius():F1} °C");

interface ITemperatureSensor { double ReadCelsius(); }

class LegacyFahrenheitSensor             // can't change this (3rd-party)
{
    public double GetTempF() => 98.6;
}

class FahrenheitSensorAdapter(LegacyFahrenheitSensor legacy) : ITemperatureSensor
{
    public double ReadCelsius() => (legacy.GetTempF() - 32) * 5 / 9;
}
```

```text
37.0 °C
```

**Decorator vs Adapter:** decorator keeps the **same** interface and adds behavior; adapter **changes** the interface so things fit.

## 18.9 Repository (brief)

**Simple words:** a collection-like abstraction over data storage, so business code says `repo.GetById(5)` and doesn't know whether it's SQL, Cosmos or a list in memory.

```csharp
IRepository<Product> repo = new InMemoryRepository<Product>();
repo.Add(new Product(1, "Pen", 10m));
repo.Add(new Product(2, "Book", 250m));
Console.WriteLine(repo.GetById(2));
Console.WriteLine(repo.Find(p => p.Price < 100).Count());

interface IEntity { int Id { get; } }
record Product(int Id, string Name, decimal Price) : IEntity;

interface IRepository<T> where T : IEntity
{
    T? GetById(int id);
    IEnumerable<T> Find(Func<T, bool> predicate);
    void Add(T entity);
}

class InMemoryRepository<T> : IRepository<T> where T : IEntity
{
    private readonly Dictionary<int, T> _items = [];
    public T? GetById(int id) => _items.GetValueOrDefault(id);
    public IEnumerable<T> Find(Func<T, bool> predicate) => _items.Values.Where(predicate);
    public void Add(T entity) => _items[entity.Id] = entity;
}
```

```text
Product { Id = 2, Name = Book, Price = 250 }
1
```

⚠️ **Interview trap:** EF Core's `DbContext` is *already* a Unit of Work and `DbSet<T>` is *already* a repository. Wrapping them in a generic repository often adds little; a *specific* repository (`IOrderRepository.GetPendingOrders()`) is more valuable. Also: a repository that returns `IQueryable<T>` leaks the data layer.

## 18.10 Dependency Injection by hand (no container)

**Simple words:** a class **asks** for what it needs in its constructor instead of creating it. One place — the **composition root** (`Program.cs`) — builds the object graph. A DI container just automates that wiring.

```csharp
// Composition root: the ONLY place that knows concrete types
IClock clock = new FixedClock(new DateTime(2025, 1, 1, 9, 0, 0));
ILogger logger = new ConsoleLogger(clock);
IEmailSender email = new FakeEmailSender(logger);
var signup = new SignupService(email, logger);

signup.Register("ann@example.com");

interface IClock { DateTime Now { get; } }
interface ILogger { void Log(string msg); }
interface IEmailSender { void Send(string to, string body); }

class FixedClock(DateTime now) : IClock { public DateTime Now => now; }
class ConsoleLogger(IClock clock) : ILogger { public void Log(string msg) => Console.WriteLine($"{clock.Now:HH:mm} {msg}"); }
class FakeEmailSender(ILogger log) : IEmailSender { public void Send(string to, string body) => log.Log($"email to {to}: {body}"); }

class SignupService(IEmailSender email, ILogger log)     // constructor injection
{
    public void Register(string address)
    {
        ArgumentException.ThrowIfNullOrWhiteSpace(address);
        log.Log($"registering {address}");
        email.Send(address, "Welcome!");
    }
}
```

```text
09:00 registering ann@example.com
09:00 email to ann@example.com: Welcome!
```

The same graph with the built-in container would be: `services.AddSingleton<IClock, SystemClock>(); services.AddSingleton<ILogger, ConsoleLogger>(); services.AddTransient<IEmailSender, SmtpSender>(); services.AddScoped<SignupService>();`

| Lifetime | One instance per… | Typical use |
|---|---|---|
| `Singleton` | application | caches, config, `IHttpClientFactory` |
| `Scoped` | request / scope | `DbContext`, unit of work |
| `Transient` | every resolve | lightweight stateless services |

## 18.11 Which pattern when? (cheat table)

| Problem | Pattern |
|---|---|
| Exactly one shared instance | Singleton (prefer DI `AddSingleton`) |
| Choose a concrete class at runtime | Factory / Factory Method |
| Object with many optional parts + validation | Builder |
| Interchangeable algorithms | Strategy (or `Func<>`) |
| Notify many listeners of changes | Observer (`event`, `IObservable<T>`) |
| Add logging/caching/retry without editing a class | Decorator |
| Make a 3rd-party API fit your interface | Adapter |
| Hide data-access details | Repository |
| Decouple classes, enable unit tests | Dependency Injection |

## Try it yourself

1. Which SOLID principle does a class with methods `SaveToDb()`, `ToJson()`, and `CalculateTax()` violate? How would you fix it?
2. Is `public static Config Instance { get; } = new();` thread-safe? Is it lazy?
3. Implement a `RetryDecorator` for `IWeatherService` that retries `Get` up to 3 times on exception.
4. Replace the `IDiscountStrategy` interface with a delegate-based strategy. When is the interface still better?
5. Why is `Square : Rectangle` an LSP violation even though "a square is a rectangle"?
6. What's the difference between DIP, DI and an IoC container?

<details><summary>Answer</summary>

1. **SRP** — three reasons to change (DB schema, JSON format, tax rules). Split into `TaxCalculator`, a serializer, and a repository.
2. **Thread-safe: yes** (the CLR runs the static initializer once, under a lock). **Lazy: mostly** — it's created when the type is first accessed, not exactly when `Instance` is first read. Use `Lazy<T>` for precise laziness.
3.
```csharp
class RetryWeather(IWeatherService inner, int maxAttempts = 3) : IWeatherService
{
    public string Get(string city)
    {
        for (int attempt = 1; ; attempt++)
        {
            try { return inner.Get(city); }
            catch (Exception) when (attempt < maxAttempts) { /* log & retry */ }
        }
    }
}
```
4. `class Checkout(Func<decimal, decimal> discount) { public decimal Total(decimal a) => discount(a); }`. The interface is better when the strategy has **multiple methods**, **state/dependencies**, needs a **name** for DI registration, or must be discoverable.
5. Code written against `Rectangle` assumes setting `Width` doesn't change `Height`. `Square` breaks that *behavioral contract*, so substituting it causes surprises — LSP is about behavior, not "is-a" in English.
6. DIP = principle (depend on abstractions). DI = technique (pass dependencies in). IoC container = tool that automates DI (creates and wires objects, manages lifetimes).

</details>

## Quick revision

* **S**RP: one reason to change. **O**CP: extend by adding classes, not editing switches.
* **L**SP: subtypes must honor the base's behavior (Rectangle/Square). **I**SP: small interfaces.
* **D**IP: depend on abstractions; DI is how; containers automate it.
* Singleton: prefer `Lazy<T>` or `static readonly`; better still, `AddSingleton` in DI.
* Factory hides `new`; Factory Method lets subclasses choose the product.
* Builder: fluent `return this;`, validates in `Build()`.
* Strategy: swappable algorithm (interface or `Func<>`); kills `if/else` chains.
* Observer: C# `event` / `IObservable<T>`; always unsubscribe.
* Decorator: same interface, adds behavior (Streams, middleware). Adapter: converts interface.
* Repository: hide persistence; `DbContext` already is UoW + repositories.
* Constructor injection + composition root = DI without a container.

# 19. Coding Problems Interviewers Ask (with solutions)

> **How to use this section:** Don't memorize code — memorize the **approach line** for each problem (it's what you say out loud before typing). Then cover the solution, type it yourself in a scratch console app, and compare. In the interview: (1) repeat the problem & ask about edge cases (null? empty? duplicates? case-sensitive?), (2) say the brute force and its Big-O, (3) code the better approach, (4) walk through a sample input, (5) state time/space complexity. Every solution below is a complete top-level program and the outputs shown were produced by running it.

| # | Problem | Key idea | Time |
|---|---|---|---|
| 1 | Reverse a string | two pointers / `Array.Reverse` | O(n) |
| 2 | Palindrome | two pointers, skip non-letters | O(n) |
| 3 | Anagram | count array of 26 | O(n) |
| 4 | Count vowels / char frequency | `Dictionary<char,int>` | O(n) |
| 5 | First non-repeating char | count, then 2nd pass | O(n) |
| 6 | Reverse words | split + reverse | O(n) |
| 7 | FizzBuzz | modulo order | O(n) |
| 8 | Fibonacci | iterative / memo | O(n) |
| 9 | Factorial | loop, `checked`, `BigInteger` | O(n) |
| 10 | Prime & sieve | test up to √n; sieve | O(√n) / O(n log log n) |
| 11 | Swap without temp | tuple / XOR | O(1) |
| 12 | Duplicates in array | `HashSet.Add` returns false | O(n) |
| 13 | Second largest | one pass, two variables | O(n) |
| 14 | Missing number | sum formula / XOR | O(n) |
| 15 | Two-sum | value → index map | O(n) |
| 16 | Max subarray | Kadane | O(n) |
| 17 | Rotate array | triple reverse | O(n) |
| 18 | Merge sorted arrays | two pointers | O(n+m) |
| 19 | Binary search | halve the range | O(log n) |
| 20 | Remove duplicates keep order | `HashSet` + list | O(n) |
| 21 | Balanced parentheses | `Stack<char>` | O(n) |
| 22 | String compression | run-length + `StringBuilder` | O(n) |
| 23 | Top-K frequent words | count + sort / heap | O(n log n) |
| 24 | LRU cache | `Dictionary` + `LinkedList` | O(1) |
| 25 | Generic `Stack<T>` | array + resize | O(1) amortized |
| 26 | Thread-safe singleton | `Lazy<T>` | O(1) |
| 27 | Matrix transpose / rotate | swap `[i,j]`↔`[j,i]` | O(r·c) |

---

## 19.1 Reverse a string (several ways)

**Problem:** Reverse `"hello world"` → `"dlrow olleh"`.
**Approach:** Strings are immutable, so copy to a `char[]`, swap from both ends towards the middle, and build a new string. Know the one-liners too, but be ready to write the manual loop.

```csharp
string s = "hello world";

// 1) Two pointers on a char array (what interviewers want to see)
static string ReverseTwoPointers(string s)
{
    char[] c = s.ToCharArray();
    for (int i = 0, j = c.Length - 1; i < j; i++, j--)
        (c[i], c[j]) = (c[j], c[i]);
    return new string(c);
}

// 2) Array.Reverse
static string ReverseArray(string s) { var c = s.ToCharArray(); Array.Reverse(c); return new string(c); }

// 3) StringBuilder, walking backwards
static string ReverseBuilder(string s)
{
    var sb = new System.Text.StringBuilder(s.Length);
    for (int i = s.Length - 1; i >= 0; i--) sb.Append(s[i]);
    return sb.ToString();
}

// 4) Recursion (cute, but O(n^2) because of Substring + concatenation)
static string ReverseRecursive(string s) => s.Length <= 1 ? s : ReverseRecursive(s[1..]) + s[0];

// 5) Zero-extra-copy with string.Create + Span
static string ReverseSpan(string s) =>
    string.Create(s.Length, s, (span, src) => { src.AsSpan().CopyTo(span); span.Reverse(); });

Console.WriteLine(ReverseTwoPointers(s));
Console.WriteLine(ReverseArray(s));
Console.WriteLine(ReverseBuilder(s));
Console.WriteLine(ReverseRecursive(s));
Console.WriteLine(ReverseSpan(s));
Console.WriteLine(new string(s.Reverse().ToArray()));   // 6) LINQ one-liner
```

```text
dlrow olleh
dlrow olleh
dlrow olleh
dlrow olleh
dlrow olleh
dlrow olleh
```

**Complexity:** O(n) time, O(n) space (a new string is unavoidable). Recursive version: O(n²).
⚠️ Strings with surrogate pairs/emoji (`"👍"` is 2 `char`s) get broken by char-reversal — mention `StringInfo` text elements if asked.

## 19.2 Palindrome check

**Problem:** Is a string the same forwards and backwards, ignoring case and non-alphanumeric chars? `"A man, a plan, a canal: Panama"` → true.
**Approach:** Two pointers from both ends; skip characters that aren't letters/digits; compare lower-cased chars. No extra string needed.

```csharp
static bool IsPalindrome(string s)
{
    int i = 0, j = s.Length - 1;
    while (i < j)
    {
        if (!char.IsLetterOrDigit(s[i])) { i++; continue; }
        if (!char.IsLetterOrDigit(s[j])) { j--; continue; }
        if (char.ToLowerInvariant(s[i]) != char.ToLowerInvariant(s[j])) return false;
        i++; j--;
    }
    return true;
}

foreach (var t in new[] { "A man, a plan, a canal: Panama", "race a car", "madam", "", "ab" })
    Console.WriteLine($"\"{t}\" -> {IsPalindrome(t)}");

// Number palindrome without converting to string
static bool IsNumberPalindrome(int n)
{
    if (n < 0) return false;
    int original = n, reversed = 0;
    while (n > 0) { reversed = reversed * 10 + n % 10; n /= 10; }
    return original == reversed;
}
Console.WriteLine($"121 -> {IsNumberPalindrome(121)}, 123 -> {IsNumberPalindrome(123)}");
```

```text
"A man, a plan, a canal: Panama" -> True
"race a car" -> False
"madam" -> True
"" -> True
"ab" -> False
121 -> True, 123 -> False
```

**Complexity:** O(n) time, O(1) space.

## 19.3 Anagram check

**Problem:** Are two strings anagrams (same letters, same counts)? Ignore case and spaces. `"Dormitory"` / `"dirty room"` → true.
**Approach:** Count letters of the first string (+1) and the second (−1) in an `int[26]`; all counts must end at zero. (Sorting both strings also works: O(n log n).)

```csharp
static bool AreAnagrams(string a, string b)
{
    var counts = new int[26];
    foreach (char ch in a.ToLowerInvariant()) if (ch is >= 'a' and <= 'z') counts[ch - 'a']++;
    foreach (char ch in b.ToLowerInvariant()) if (ch is >= 'a' and <= 'z') counts[ch - 'a']--;
    return counts.All(c => c == 0);
}

// Sorting approach (simple to explain)
static bool AreAnagramsSort(string a, string b) =>
    a.Replace(" ", "").ToLower().Order().SequenceEqual(b.Replace(" ", "").ToLower().Order());

Console.WriteLine(AreAnagrams("listen", "silent"));
Console.WriteLine(AreAnagrams("Dormitory", "dirty room"));
Console.WriteLine(AreAnagrams("abc", "abd"));
Console.WriteLine(AreAnagramsSort("Listen", "Silent"));
```

```text
True
True
False
True
```

**Complexity:** counting: O(n) time, O(1) space (fixed 26). Sorting: O(n log n). For full Unicode, use `Dictionary<char,int>`.

## 19.4 Count vowels, consonants & character frequency

**Problem:** For `"Hello World"`, count vowels and consonants, and print each letter's frequency in order of first appearance.
**Approach:** One pass; classify each letter. For frequency, a `Dictionary<char,int>` (insertion order is preserved when you never remove).

```csharp
string text = "Hello World";
int vowels = 0, consonants = 0;
var freq = new Dictionary<char, int>();

foreach (char raw in text)
{
    if (!char.IsLetter(raw)) continue;
    char c = char.ToLowerInvariant(raw);
    if ("aeiou".Contains(c)) vowels++; else consonants++;
    freq[c] = freq.GetValueOrDefault(c) + 1;
}

Console.WriteLine($"Vowels: {vowels}, Consonants: {consonants}");
Console.WriteLine(string.Join(" ", freq.Select(kv => $"{kv.Key}:{kv.Value}")));

// LINQ version
var linq = text.ToLower().Where(char.IsLetter).GroupBy(c => c).Select(g => $"{g.Key}:{g.Count()}");
Console.WriteLine(string.Join(" ", linq));

// Count occurrences of a specific char
Console.WriteLine($"'l' appears {text.Count(c => c == 'l')} times");
```

```text
Vowels: 3, Consonants: 7
h:1 e:1 l:3 o:2 w:1 r:1 d:1
h:1 e:1 l:3 o:2 w:1 r:1 d:1
'l' appears 3 times
```

**Complexity:** O(n) time; O(k) space where k = distinct characters.

## 19.5 First non-repeating character

**Problem:** Return the first character that occurs exactly once. `"swiss"` → `'w'`.
**Approach:** Pass 1: count every char. Pass 2: walk the **string** again (not the dictionary) and return the first char with count 1.

```csharp
static char? FirstUnique(string s)
{
    var counts = new Dictionary<char, int>();
    foreach (char c in s) counts[c] = counts.GetValueOrDefault(c) + 1;
    foreach (char c in s) if (counts[c] == 1) return c;
    return null;
}

foreach (var s in new[] { "swiss", "leetcode", "aabb", "x" })
    Console.WriteLine($"{s} -> {FirstUnique(s)?.ToString() ?? "none"}");
```

```text
swiss -> w
leetcode -> l
aabb -> none
x -> x
```

**Complexity:** O(n) time, O(k) space.

## 19.6 Reverse the words in a sentence

**Problem:** `"  the sky   is blue "` → `"blue is sky the"` (single spaces, trimmed).
**Approach:** Split on spaces removing empty entries, reverse the array, join with one space. Manual version: walk from the end collecting words.

```csharp
static string ReverseWords(string s)
{
    var words = s.Split(' ', StringSplitOptions.RemoveEmptyEntries);
    Array.Reverse(words);
    return string.Join(' ', words);
}

static string ReverseWordsManual(string s)
{
    var sb = new System.Text.StringBuilder();
    int end = s.Length;
    for (int i = s.Length - 1; i >= -1; i--)
    {
        if (i == -1 || s[i] == ' ')
        {
            if (end - i - 1 > 0)
            {
                if (sb.Length > 0) sb.Append(' ');
                sb.Append(s, i + 1, end - i - 1);
            }
            end = i;
        }
    }
    return sb.ToString();
}

// Variant: reverse each word in place, keep word order
static string ReverseEachWord(string s) =>
    string.Join(' ', s.Split(' ').Select(w => new string(w.Reverse().ToArray())));

Console.WriteLine($"[{ReverseWords("  the sky   is blue ")}]");
Console.WriteLine($"[{ReverseWordsManual("  the sky   is blue ")}]");
Console.WriteLine(ReverseEachWord("hello world"));
```

```text
[blue is sky the]
[blue is sky the]
olleh dlrow
```

**Complexity:** O(n) time, O(n) space.

## 19.7 FizzBuzz

**Problem:** For 1..15 print "Fizz" for multiples of 3, "Buzz" for 5, "FizzBuzz" for both, else the number.
**Approach:** Check 15 (both) **first** — the classic bug is checking 3 first. A switch expression on a tuple is a neat modern version.

```csharp
for (int i = 1; i <= 15; i++)
{
    string output = (i % 3, i % 5) switch
    {
        (0, 0) => "FizzBuzz",
        (0, _) => "Fizz",
        (_, 0) => "Buzz",
        _ => i.ToString()
    };
    Console.Write(output + (i < 15 ? " " : "\n"));
}

// Classic if/else version
static string Classic(int i) =>
    i % 15 == 0 ? "FizzBuzz" : i % 3 == 0 ? "Fizz" : i % 5 == 0 ? "Buzz" : i.ToString();
Console.WriteLine(string.Join(",", Enumerable.Range(9, 7).Select(Classic)));
```

```text
1 2 Fizz 4 Buzz Fizz 7 8 Fizz Buzz 11 Fizz 13 14 FizzBuzz
Fizz,Buzz,11,Fizz,13,14,FizzBuzz
```

**Complexity:** O(n) time, O(1) space.

## 19.8 Fibonacci — recursive, memoized, iterative

**Problem:** Return the n-th Fibonacci number (F0 = 0, F1 = 1).
**Approach:** Naive recursion recomputes the same values → O(2ⁿ). Memoization caches results → O(n). Iterative with two variables → O(n) time, O(1) space. Use `long` (F92 is the largest that fits).

```csharp
static long FibRecursive(int n) => n < 2 ? n : FibRecursive(n - 1) + FibRecursive(n - 2);

static long FibMemo(int n, Dictionary<int, long>? memo = null)
{
    memo ??= new();
    if (n < 2) return n;
    if (memo.TryGetValue(n, out var cached)) return cached;
    return memo[n] = FibMemo(n - 1, memo) + FibMemo(n - 2, memo);
}

static long FibIterative(int n)
{
    long a = 0, b = 1;
    for (int i = 0; i < n; i++) (a, b) = (b, a + b);
    return a;
}

static IEnumerable<long> FibSequence()       // lazy infinite sequence with yield
{
    long a = 0, b = 1;
    while (true) { yield return a; (a, b) = (b, a + b); }
}

Console.WriteLine(string.Join(" ", FibSequence().Take(10)));
Console.WriteLine(FibRecursive(30));
Console.WriteLine(FibMemo(50));
Console.WriteLine(FibIterative(50));
Console.WriteLine(FibIterative(92));
```

```text
0 1 1 2 3 5 8 13 21 34
832040
12586269025
12586269025
7540113804746346429
```

**Complexity:** recursive O(2ⁿ) time, O(n) stack; memo O(n)/O(n); iterative O(n)/O(1).

## 19.9 Factorial (and overflow)

**Problem:** Compute n!.
**Approach:** Multiply 1..n. `int` overflows at 13!, `long` at 21! — silently, unless you use `checked`. For big values use `System.Numerics.BigInteger`.

```csharp
using System.Numerics;

static long Factorial(int n)
{
    if (n < 0) throw new ArgumentOutOfRangeException(nameof(n));
    long result = 1;
    for (int i = 2; i <= n; i++) result = checked(result * i);   // throw instead of silently wrapping
    return result;
}

static long FactorialRecursive(int n) => n <= 1 ? 1 : n * FactorialRecursive(n - 1);

static BigInteger BigFactorial(int n)
{
    BigInteger r = BigInteger.One;
    for (int i = 2; i <= n; i++) r *= i;
    return r;
}

Console.WriteLine(Factorial(5));
Console.WriteLine(FactorialRecursive(10));
Console.WriteLine(Factorial(20));
Console.WriteLine(BigFactorial(25));
try { Factorial(21); }
catch (OverflowException) { Console.WriteLine("21! overflows long"); }
```

```text
120
3628800
2432902008176640000
15511210043330985984000000
21! overflows long
```

**Complexity:** O(n) time; O(1) space iterative, O(n) stack recursive.

## 19.10 Prime check & Sieve of Eratosthenes

**Problem:** (a) Is n prime? (b) List all primes ≤ N.
**Approach:** (a) Check divisors only up to √n, skipping even numbers. (b) Sieve: start with all true; for each prime p, cross out p², p²+p, …

```csharp
static bool IsPrime(int n)
{
    if (n < 2) return false;
    if (n % 2 == 0) return n == 2;
    for (int d = 3; (long)d * d <= n; d += 2)
        if (n % d == 0) return false;
    return true;
}

static List<int> Sieve(int max)
{
    var composite = new bool[max + 1];
    var primes = new List<int>();
    for (int p = 2; p <= max; p++)
    {
        if (composite[p]) continue;
        primes.Add(p);
        for (long m = (long)p * p; m <= max; m += p) composite[m] = true;
    }
    return primes;
}

Console.WriteLine(string.Join(" ", new[] { 1, 2, 17, 21, 97, 100 }.Select(n => $"{n}:{IsPrime(n)}")));
Console.WriteLine(string.Join(" ", Sieve(50)));
Console.WriteLine($"primes below 1,000,000: {Sieve(1_000_000).Count}");
```

```text
1:False 2:True 17:True 21:False 97:True 100:False
2 3 5 7 11 13 17 19 23 29 31 37 41 43 47
primes below 1,000,000: 78498
```

**Complexity:** IsPrime O(√n) time, O(1) space. Sieve O(N log log N) time, O(N) space.

## 19.11 Swap two numbers without a temp variable

**Problem:** Swap `a` and `b` without a third variable.
**Approach:** In modern C#, a tuple swap. Interviewers also want the arithmetic and XOR tricks (and their caveats).

```csharp
int a = 5, b = 10;
(a, b) = (b, a);                         // 1) tuple (cleanest; compiler uses a hidden temp)
Console.WriteLine($"tuple:      a={a}, b={b}");

a = 5; b = 10;
a = a + b; b = a - b; a = a - b;         // 2) arithmetic (can overflow for big values)
Console.WriteLine($"arithmetic: a={a}, b={b}");

a = 5; b = 10;
a ^= b; b ^= a; a ^= b;                  // 3) XOR (breaks if a and b are the same variable/ref)
Console.WriteLine($"xor:        a={a}, b={b}");

static void Swap<T>(ref T x, ref T y) => (x, y) = (y, x);   // 4) generic swap via ref
string s1 = "left", s2 = "right";
Swap(ref s1, ref s2);
Console.WriteLine($"generic:    {s1}, {s2}");
```

```text
tuple:      a=10, b=5
arithmetic: a=10, b=5
xor:        a=10, b=5
generic:    right, left
```

**Complexity:** O(1) time & space.

## 19.12 Find duplicates in an array

**Problem:** Return the values that appear more than once in `[4, 3, 2, 7, 8, 2, 3, 1]`.
**Approach:** Walk the array with a `HashSet` of seen values. `HashSet.Add` returns **false** if the item is already there — that's a duplicate. Use a second set so each duplicate is reported once.

```csharp
int[] nums = [4, 3, 2, 7, 8, 2, 3, 1, 2];

static List<int> FindDuplicates(int[] nums)
{
    var seen = new HashSet<int>();
    var reported = new HashSet<int>();
    var result = new List<int>();
    foreach (var n in nums)
        if (!seen.Add(n) && reported.Add(n))   // already seen, not yet reported
            result.Add(n);
    return result;                             // order = when the 2nd occurrence was found
}

Console.WriteLine(string.Join(", ", FindDuplicates(nums)));

// LINQ: order = first appearance; also gives counts
var dupes = nums.GroupBy(n => n).Where(g => g.Count() > 1).Select(g => $"{g.Key} (x{g.Count()})");
Console.WriteLine(string.Join(", ", dupes));

// Brute force for comparison: O(n^2)
bool hasDup = false;
for (int i = 0; i < nums.Length && !hasDup; i++)
    for (int j = i + 1; j < nums.Length; j++)
        if (nums[i] == nums[j]) { hasDup = true; break; }
Console.WriteLine($"has duplicate: {hasDup}");
```

```text
2, 3
3 (x2), 2 (x3)
has duplicate: True
```

**Complexity:** HashSet: O(n) time, O(n) space. Brute force: O(n²) time, O(1) space. (Sorting first: O(n log n), O(1) extra.)

## 19.13 Second largest element

**Problem:** Find the second largest **distinct** value. `[12, 35, 1, 10, 34, 1]` → 34. `[5, 5, 5]` → none.
**Approach:** One pass with two variables `first` and `second`. When you see a bigger value, the old `first` drops to `second`. Ignore values equal to `first`.

```csharp
static int? SecondLargest(int[] a)
{
    int? first = null, second = null;
    foreach (var x in a)
    {
        if (first is null || x > first) { second = first; first = x; }
        else if (x != first && (second is null || x > second)) second = x;
    }
    return second;
}

Console.WriteLine(SecondLargest([12, 35, 1, 10, 34, 1]));
Console.WriteLine(SecondLargest([10, 10, 9]));
Console.WriteLine(SecondLargest([5, 5, 5])?.ToString() ?? "none");
Console.WriteLine(SecondLargest([-3, -1, -2]));

// LINQ one-liner (O(n log n))
int[] data = [12, 35, 1, 10, 34, 1];
Console.WriteLine(data.Distinct().OrderByDescending(x => x).Skip(1).First());
```

```text
34
9
none
-2
34
```

**Complexity:** O(n) time, O(1) space. Generalize to k-th largest with a min-heap (`PriorityQueue<int,int>`) of size k: O(n log k).

## 19.14 Missing number in 1..n

**Problem:** An array contains n−1 distinct numbers from 1..n. Find the missing one. `[1, 2, 4, 5, 6]` → 3.
**Approach:** Expected sum is n(n+1)/2; subtract the actual sum. Use `long` to avoid overflow, or XOR all indices and values (no overflow at all).

```csharp
static int MissingBySum(int[] a)
{
    long n = a.Length + 1;
    long expected = n * (n + 1) / 2;
    long actual = 0;
    foreach (var x in a) actual += x;
    return (int)(expected - actual);
}

static int MissingByXor(int[] a)
{
    int xor = 0;
    for (int i = 1; i <= a.Length + 1; i++) xor ^= i;
    foreach (var x in a) xor ^= x;            // pairs cancel out; the missing number remains
    return xor;
}

Console.WriteLine(MissingBySum([1, 2, 4, 5, 6]));
Console.WriteLine(MissingByXor([1, 2, 4, 5, 6]));
Console.WriteLine(MissingBySum([2, 3, 1, 5]));
Console.WriteLine(MissingByXor([1]));
```

```text
3
3
4
2
```

**Complexity:** O(n) time, O(1) space.

## 19.15 Two-sum (HashMap)

**Problem:** Given `nums` and `target`, return indices of the two numbers that add up to `target`. `[2, 7, 11, 15]`, 9 → `(0, 1)`.
**Approach:** Brute force checks every pair: O(n²). Better: one pass with a `Dictionary<value, index>`; for each number, check whether its **complement** `target − x` was already seen.

```csharp
static (int, int)? TwoSum(int[] nums, int target)
{
    var seen = new Dictionary<int, int>();          // value -> index
    for (int i = 0; i < nums.Length; i++)
    {
        if (seen.TryGetValue(target - nums[i], out int j)) return (j, i);
        seen[nums[i]] = i;                           // add AFTER checking (so we don't reuse the same element)
    }
    return null;
}

Console.WriteLine(TwoSum([2, 7, 11, 15], 9));
Console.WriteLine(TwoSum([3, 2, 4], 6));
Console.WriteLine(TwoSum([3, 3], 6));
Console.WriteLine(TwoSum([1, 2], 10)?.ToString() ?? "no pair");
```

```text
(0, 1)
(1, 2)
(0, 1)
no pair
```

**Complexity:** O(n) time, O(n) space. (If the array is sorted: two pointers from both ends, O(1) space.)

## 19.16 Maximum subarray sum (Kadane's algorithm)

**Problem:** Find the contiguous subarray with the largest sum. `[-2, 1, -3, 4, -1, 2, 1, -5, 4]` → 6 (`[4, -1, 2, 1]`).
**Approach:** Walk once. At each element decide: *extend* the current run or *start fresh* here (if the running sum went negative, it only hurts). Track the best seen. Initialize with the first element so all-negative arrays work.

```csharp
static (int Sum, int Start, int End) MaxSubarray(int[] a)
{
    int best = a[0], current = a[0];
    int start = 0, bestStart = 0, bestEnd = 0;
    for (int i = 1; i < a.Length; i++)
    {
        if (current < 0) { current = a[i]; start = i; }   // start fresh
        else current += a[i];                              // extend
        if (current > best) { best = current; bestStart = start; bestEnd = i; }
    }
    return (best, bestStart, bestEnd);
}

int[] data = [-2, 1, -3, 4, -1, 2, 1, -5, 4];
var (sum, s, e) = MaxSubarray(data);
Console.WriteLine($"sum={sum}, subarray=[{string.Join(", ", data[s..(e + 1)])}]");
Console.WriteLine(MaxSubarray([-3, -1, -2]).Sum);
Console.WriteLine(MaxSubarray([5, 4, -1, 7, 8]).Sum);
```

```text
sum=6, subarray=[4, -1, 2, 1]
-1
23
```

**Complexity:** O(n) time, O(1) space (brute force is O(n²)).

## 19.17 Rotate an array by k

**Problem:** Rotate `[1..7]` right by 3 → `[5, 6, 7, 1, 2, 3, 4]`, in place.
**Approach:** Triple-reverse trick: reverse the whole array, then reverse the first k, then the rest. Use `k %= n` for k > n.

```csharp
static void RotateRight(int[] a, int k)
{
    int n = a.Length;
    if (n == 0) return;
    k %= n;
    Array.Reverse(a);            // 7 6 5 4 3 2 1
    Array.Reverse(a, 0, k);      // 5 6 7 | 4 3 2 1
    Array.Reverse(a, k, n - k);  // 5 6 7 | 1 2 3 4
}

static int[] RotateLeftCopy(int[] a, int k)
{
    k %= a.Length;
    return [.. a[k..], .. a[..k]];   // ranges + collection expression (extra O(n) space)
}

int[] arr = [1, 2, 3, 4, 5, 6, 7];
RotateRight(arr, 3);
Console.WriteLine(string.Join(" ", arr));

int[] arr2 = [1, 2, 3];
RotateRight(arr2, 10);           // 10 % 3 = 1
Console.WriteLine(string.Join(" ", arr2));

Console.WriteLine(string.Join(" ", RotateLeftCopy([1, 2, 3, 4, 5], 2)));
```

```text
5 6 7 1 2 3 4
3 1 2
3 4 5 1 2
```

**Complexity:** O(n) time, O(1) extra space (in-place version).

## 19.18 Merge two sorted arrays

**Problem:** Merge `[1, 3, 5, 7]` and `[2, 4, 6]` into one sorted array.
**Approach:** Two pointers: compare the heads, take the smaller, advance that pointer. When one runs out, copy the rest of the other. (This is the "merge" step of merge sort.)

```csharp
static int[] Merge(int[] a, int[] b)
{
    var result = new int[a.Length + b.Length];
    int i = 0, j = 0, k = 0;
    while (i < a.Length && j < b.Length)
        result[k++] = a[i] <= b[j] ? a[i++] : b[j++];   // <= keeps it stable
    while (i < a.Length) result[k++] = a[i++];
    while (j < b.Length) result[k++] = b[j++];
    return result;
}

Console.WriteLine(string.Join(" ", Merge([1, 3, 5, 7], [2, 4, 6])));
Console.WriteLine(string.Join(" ", Merge([], [1, 2])));
Console.WriteLine(string.Join(" ", Merge([1, 1, 2], [1, 3])));
```

```text
1 2 3 4 5 6 7
1 2
1 1 1 2 3
```

**Complexity:** O(n + m) time, O(n + m) space. (Concat + sort would be O((n+m) log(n+m)).)

## 19.19 Binary search

**Problem:** Find the index of a target in a **sorted** array, or −1.
**Approach:** Keep `[lo, hi]`; look at the middle; discard the half that can't contain the target. Use `lo + (hi - lo) / 2` to avoid overflow.

```csharp
static int BinarySearch(int[] a, int target)
{
    int lo = 0, hi = a.Length - 1;
    while (lo <= hi)
    {
        int mid = lo + (hi - lo) / 2;
        if (a[mid] == target) return mid;
        if (a[mid] < target) lo = mid + 1; else hi = mid - 1;
    }
    return -1;
}

static int BinarySearchRecursive(int[] a, int target, int lo, int hi)
{
    if (lo > hi) return -1;
    int mid = lo + (hi - lo) / 2;
    return a[mid] == target ? mid
         : a[mid] < target ? BinarySearchRecursive(a, target, mid + 1, hi)
         : BinarySearchRecursive(a, target, lo, mid - 1);
}

int[] sorted = [1, 3, 5, 7, 9, 11];
Console.WriteLine(BinarySearch(sorted, 7));
Console.WriteLine(BinarySearch(sorted, 4));
Console.WriteLine(BinarySearchRecursive(sorted, 11, 0, sorted.Length - 1));
int builtIn = Array.BinarySearch(sorted, 4);       // negative = bitwise complement of insert position
Console.WriteLine($"{builtIn} -> insert at {~builtIn}");
```

```text
3
-1
5
-3 -> insert at 2
```

**Complexity:** O(log n) time; O(1) space iterative, O(log n) recursive.

## 19.20 Remove duplicates while preserving order

**Problem:** `[3, 1, 3, 2, 1, 4]` → `[3, 1, 2, 4]`; `"programming"` → `"progamin"`.
**Approach:** Keep a `HashSet` of seen items; append an item to the output only when `seen.Add(item)` returns true (first time).

```csharp
static List<T> RemoveDuplicates<T>(IEnumerable<T> items)
{
    var seen = new HashSet<T>();
    var result = new List<T>();
    foreach (var item in items)
        if (seen.Add(item)) result.Add(item);
    return result;
}

Console.WriteLine(string.Join(" ", RemoveDuplicates([3, 1, 3, 2, 1, 4])));
Console.WriteLine(new string(RemoveDuplicates("programming").ToArray()));
Console.WriteLine(string.Join(" ", new[] { 3, 1, 3, 2, 1, 4 }.Distinct()));   // LINQ: same result in practice

// In-place on a SORTED array (classic follow-up): return new length
static int DedupSorted(int[] a)
{
    if (a.Length == 0) return 0;
    int w = 1;
    for (int r = 1; r < a.Length; r++)
        if (a[r] != a[w - 1]) a[w++] = a[r];
    return w;
}
int[] sortedArr = [1, 1, 2, 2, 2, 3, 5, 5];
int len = DedupSorted(sortedArr);
Console.WriteLine(string.Join(" ", sortedArr[..len]));
```

```text
3 1 2 4
progamin
3 1 2 4
1 2 3 5
```

**Complexity:** HashSet: O(n) time, O(n) space. Sorted in-place: O(n) time, O(1) space.

## 19.21 Balanced parentheses (Stack)

**Problem:** Is a string of `()[]{}` (possibly with other chars) properly balanced? `"{[()]}"` → true, `"([)]"` → false.
**Approach:** Push opening brackets on a `Stack<char>`. On a closing bracket, the stack top must be its matching opener — pop it. At the end the stack must be empty.

```csharp
static bool IsBalanced(string s)
{
    var stack = new Stack<char>();
    foreach (char c in s)
    {
        switch (c)
        {
            case '(' or '[' or '{':
                stack.Push(c);
                break;
            case ')' or ']' or '}':
                char expected = c switch { ')' => '(', ']' => '[', _ => '{' };
                if (stack.Count == 0 || stack.Pop() != expected) return false;
                break;
        }
    }
    return stack.Count == 0;
}

foreach (var t in new[] { "{[()]}", "([)]", "((", "", "a(b)c[d]", "())" })
    Console.WriteLine($"\"{t}\" -> {IsBalanced(t)}");
```

```text
"{[()]}" -> True
"([)]" -> False
"((" -> False
"" -> True
"a(b)c[d]" -> True
"())" -> False
```

**Complexity:** O(n) time, O(n) space.

## 19.22 String compression (run-length encoding)

**Problem:** `"aabccc"` → `"a2b1c3"`. (Variant: return the original if the compressed one isn't shorter.)
**Approach:** Walk the string counting runs of the same char; when the char changes, append char + count. Use `StringBuilder` — string concatenation in a loop is O(n²).

```csharp
using System.Text;

static string Compress(string s)
{
    if (string.IsNullOrEmpty(s)) return s;
    var sb = new StringBuilder();
    int count = 1;
    for (int i = 1; i <= s.Length; i++)
    {
        if (i < s.Length && s[i] == s[i - 1]) { count++; continue; }
        sb.Append(s[i - 1]).Append(count);
        count = 1;
    }
    return sb.ToString();
}

static string CompressIfShorter(string s) { var c = Compress(s); return c.Length < s.Length ? c : s; }

static string Decompress(string s)
{
    var sb = new StringBuilder();
    for (int i = 0; i < s.Length;)
    {
        char c = s[i++];
        int start = i;
        while (i < s.Length && char.IsDigit(s[i])) i++;
        sb.Append(c, int.Parse(s.AsSpan(start, i - start)));
    }
    return sb.ToString();
}

Console.WriteLine(Compress("aabccc"));
Console.WriteLine(Compress("abc"));
Console.WriteLine(Compress("aaaaaaaaaaaab"));
Console.WriteLine(CompressIfShorter("abc"));
Console.WriteLine(Decompress("a2b1c3"));
```

```text
a2b1c3
a1b1c1
a12b1
abc
aabccc
```

**Complexity:** O(n) time, O(n) space.

## 19.23 Word frequency & top-K words

**Problem:** Count word frequencies (case-insensitive, ignore punctuation) and return the top K, ties broken alphabetically.
**Approach:** Split into words, count in a `Dictionary<string,int>` with `StringComparer.OrdinalIgnoreCase`, sort by count desc then word asc, take K. For huge inputs with small K, a size-K min-heap (`PriorityQueue`) gives O(n log k).

```csharp
string text = "The quick brown fox jumps over the lazy dog. The dog sleeps; the FOX runs!";

var counts = new Dictionary<string, int>(StringComparer.OrdinalIgnoreCase);
foreach (var word in text.Split([' ', '.', ',', ';', '!', '?'], StringSplitOptions.RemoveEmptyEntries))
    counts[word] = counts.GetValueOrDefault(word) + 1;

int k = 3;
var top = counts.OrderByDescending(kv => kv.Value)
                .ThenBy(kv => kv.Key, StringComparer.OrdinalIgnoreCase)
                .Take(k);
foreach (var (word, count) in top)
    Console.WriteLine($"{word.ToLower()}: {count}");

// LINQ-only version (CountBy is .NET 9+)
var top2 = text.ToLower()
    .Split([' ', '.', ',', ';', '!', '?'], StringSplitOptions.RemoveEmptyEntries)
    .CountBy(w => w)
    .OrderByDescending(p => p.Value).ThenBy(p => p.Key)
    .Take(2)
    .Select(p => $"{p.Key}={p.Value}");
Console.WriteLine(string.Join(", ", top2));
```

```text
the: 4
dog: 2
fox: 2
the=4, dog=2
```

**Complexity:** counting O(n); sorting distinct words O(d log d); space O(d).

## 19.24 Implement an LRU cache (Dictionary + LinkedList)

**Problem:** A fixed-capacity cache where `Get` and `Put` are O(1); when full, evict the **least recently used** item.
**Approach:** A `LinkedList` keeps usage order (most recent at the front). A `Dictionary<key, LinkedListNode>` finds the node in O(1). On every access, move the node to the front; evict from the back.

```csharp
var cache = new LruCache<int, string>(2);
cache.Put(1, "one");
cache.Put(2, "two");
Console.WriteLine(cache.Get(1));                 // 1 becomes most recent
cache.Put(3, "three");                           // evicts 2 (least recently used)
Console.WriteLine(cache.Get(2) ?? "miss");
cache.Put(4, "four");                            // evicts 1
Console.WriteLine(cache.Get(1) ?? "miss");
Console.WriteLine(cache.Get(3));
Console.WriteLine(cache.Get(4));
Console.WriteLine(string.Join(",", cache.Keys));

class LruCache<TKey, TValue>(int capacity) where TKey : notnull
{
    private readonly Dictionary<TKey, LinkedListNode<(TKey Key, TValue Value)>> _map = new();
    private readonly LinkedList<(TKey Key, TValue Value)> _order = new();   // front = most recent

    public TValue? Get(TKey key)
    {
        if (!_map.TryGetValue(key, out var node)) return default;
        _order.Remove(node);                     // O(1): we have the node
        _order.AddFirst(node);
        return node.Value.Value;
    }

    public void Put(TKey key, TValue value)
    {
        if (_map.TryGetValue(key, out var existing))
        {
            _order.Remove(existing);
        }
        else if (_map.Count == capacity)
        {
            var lru = _order.Last!;
            _order.RemoveLast();
            _map.Remove(lru.Value.Key);
        }
        _map[key] = _order.AddFirst((key, value));
    }

    public IEnumerable<TKey> Keys => _order.Select(n => n.Key);   // most -> least recent
}
```

```text
one
miss
miss
three
four
4,3
```

**Complexity:** `Get`/`Put` O(1) time; O(capacity) space. Not thread-safe — wrap with a `lock` for concurrent use (or use `MemoryCache` in real apps).

## 19.25 Implement a generic `Stack<T>`

**Problem:** Build your own stack with `Push`, `Pop`, `Peek`, `Count`, and `foreach` support.
**Approach:** Back it with an array; double the array when full (amortized O(1) push). Clear popped slots so the GC can collect references. Implement `IEnumerable<T>` with `yield` (top to bottom).

```csharp
using System.Collections;

var stack = new MyStack<int>();
foreach (var n in new[] { 1, 2, 3, 4, 5 }) stack.Push(n);
Console.WriteLine($"Pop: {stack.Pop()}");
Console.WriteLine($"Peek: {stack.Peek()}");
Console.WriteLine($"Count: {stack.Count}");
Console.WriteLine($"Items: {string.Join(" ", stack)}");

var empty = new MyStack<string>();
try { empty.Pop(); }
catch (InvalidOperationException ex) { Console.WriteLine(ex.Message); }

class MyStack<T> : IEnumerable<T>
{
    private T[] _items = new T[2];
    public int Count { get; private set; }

    public void Push(T item)
    {
        if (Count == _items.Length) Array.Resize(ref _items, _items.Length * 2);
        _items[Count++] = item;
    }

    public T Pop()
    {
        if (Count == 0) throw new InvalidOperationException("Stack is empty");
        T item = _items[--Count];
        _items[Count] = default!;     // don't keep a reference alive (helps GC)
        return item;
    }

    public T Peek() => Count == 0 ? throw new InvalidOperationException("Stack is empty") : _items[Count - 1];

    public IEnumerator<T> GetEnumerator()
    {
        for (int i = Count - 1; i >= 0; i--) yield return _items[i];
    }
    IEnumerator IEnumerable.GetEnumerator() => GetEnumerator();
}
```

```text
Pop: 5
Peek: 4
Count: 4
Items: 4 3 2 1
Stack is empty
```

**Complexity:** Push amortized O(1) (O(n) on resize), Pop/Peek O(1); O(n) space.
Follow-up: "Implement a queue using two stacks" — push to `inbox`; on dequeue, if `outbox` is empty move everything from `inbox` to `outbox`, then pop `outbox` (amortized O(1)).

## 19.26 Thread-safe singleton

**Problem:** Write a singleton that's safe when many threads access it at once, and prove it.
**Approach:** `Lazy<T>` (default mode `ExecutionAndPublication`) guarantees the factory runs once. Full discussion of alternatives is in 18.2.

```csharp
var tasks = Enumerable.Range(0, 50).Select(_ => Task.Run(() => Logger.Instance)).ToArray();
Logger[] results = await Task.WhenAll(tasks);

Console.WriteLine($"All same instance: {results.All(r => ReferenceEquals(r, results[0]))}");
Console.WriteLine($"Instances created: {Logger.Created}");
Logger.Instance.Log("hello");

public sealed class Logger
{
    private static readonly Lazy<Logger> _instance = new(() => new Logger());
    private static int _created;
    private readonly Lock _gate = new();     // C# 13 Lock type (use 'object' on older versions)

    private Logger() { Interlocked.Increment(ref _created); Thread.Sleep(20); }

    public static Logger Instance => _instance.Value;
    public static int Created => _created;

    public void Log(string msg)
    {
        lock (_gate) Console.WriteLine($"[LOG] {msg}");   // instance methods must be thread-safe too!
    }
}
```

```text
All same instance: True
Instances created: 1
[LOG] hello
```

**Complexity:** O(1). 🎯 Point out that *creating* the singleton safely is only half the job — its **methods** must also be thread-safe.

## 19.27 Matrix transpose (and rotate 90°)

**Problem:** Transpose a 2×3 matrix to 3×2; rotate a square matrix 90° clockwise in place.
**Approach:** Transpose: `result[j, i] = m[i, j]`. In-place rotate of an n×n matrix = transpose (swap across the diagonal, `j > i` only) + reverse each row.

```csharp
int[,] m = { { 1, 2, 3 }, { 4, 5, 6 } };
Print(Transpose(m));
Console.WriteLine("--");

int[][] sq = [[1, 2, 3], [4, 5, 6], [7, 8, 9]];
RotateClockwise(sq);
foreach (var row in sq) Console.WriteLine(string.Join(" ", row));

static int[,] Transpose(int[,] m)
{
    int rows = m.GetLength(0), cols = m.GetLength(1);
    var t = new int[cols, rows];
    for (int i = 0; i < rows; i++)
        for (int j = 0; j < cols; j++)
            t[j, i] = m[i, j];
    return t;
}

static void RotateClockwise(int[][] a)
{
    int n = a.Length;
    for (int i = 0; i < n; i++)
        for (int j = i + 1; j < n; j++)            // only above the diagonal, or you swap twice!
            (a[i][j], a[j][i]) = (a[j][i], a[i][j]);
    foreach (var row in a) Array.Reverse(row);
}

static void Print(int[,] m)
{
    for (int i = 0; i < m.GetLength(0); i++)
        Console.WriteLine(string.Join(" ", Enumerable.Range(0, m.GetLength(1)).Select(j => m[i, j])));
}
```

```text
1 4
2 5
3 6
--
7 4 1
8 5 2
9 6 3
```

**Complexity:** transpose O(r·c) time and space; in-place rotate O(n²) time, O(1) space.
⚠️ Know the difference: `int[,]` is a **rectangular** (multi-dimensional) array — use `GetLength(dim)`; `int[][]` is a **jagged** array (array of arrays — rows can differ in length).

# 20. Output-Prediction Puzzles (25)

> **How to use this section:** Read each snippet and **write down your predicted output before opening the answer**. These are exactly the "what does this print?" questions interviewers paste into a chat window. Every snippet is a complete program; every answer was produced by actually running it on .NET 10. If you get one wrong, re-read the linked section — the explanation tells you which rule you missed.

### Puzzle 1 — String immutability

```csharp
string a = "hello";
string b = a;
a += " world";
Console.WriteLine(b);
Console.WriteLine(a);

string s = "abc";
s.ToUpper();
Console.WriteLine(s);
```

<details><summary>Answer</summary>

```text
hello
hello world
abc
```

Strings are **immutable**. `a += " world"` creates a *new* string and points `a` at it; `b` still points at the old `"hello"`. `ToUpper()` returns a new string that's thrown away — you must write `s = s.ToUpper();`. (Section 2)

</details>

### Puzzle 2 — Passing a reference type by value vs by `ref`

```csharp
var list = new List<int> { 1 };
Modify(list);
Console.WriteLine(list.Count);
Replace(list);
Console.WriteLine(list.Count);
ReplaceRef(ref list);
Console.WriteLine(list.Count);

static void Modify(List<int> l) => l.Add(2);
static void Replace(List<int> l) { l = new List<int> { 7, 8, 9, 10 }; }
static void ReplaceRef(ref List<int> l) => l = new List<int> { 7 };
```

<details><summary>Answer</summary>

```text
2
2
1
```

By default C# passes a **copy of the reference**. `Modify` uses the copy to change the *same object* → visible. `Replace` points its *local copy* at a new list → the caller never sees it. With `ref`, the method gets the caller's variable itself, so reassignment is visible. (Section 3)

</details>

### Puzzle 3 — Static constructor and constructor order

```csharp
Console.WriteLine("Main start");
var d = new Derived();
Console.WriteLine("Main end");

class Base
{
    static Base() => Console.WriteLine("Base static ctor");
    public Base() => Console.WriteLine("Base ctor");
}
class Derived : Base
{
    static Derived() => Console.WriteLine("Derived static ctor");
    public Derived() => Console.WriteLine("Derived ctor");
}
```

<details><summary>Answer</summary>

```text
Main start
Derived static ctor
Base static ctor
Base ctor
Derived ctor
Main end
```

`new Derived()` triggers **Derived's** static constructor first. The `Derived` instance constructor then chains to `Base()`, which is the first use of `Base`, so **Base's** static ctor runs, then instance constructors run **base → derived**. Static ctors run at most once, lazily, on first use. (Section 4)

</details>

### Puzzle 4 — `virtual`/`override` vs `new` (method hiding)

```csharp
Animal a = new Dog();
a.Speak();
a.Eat();
Dog d = (Dog)a;
d.Speak();
d.Eat();

class Animal
{
    public virtual void Speak() => Console.WriteLine("Animal speaks");
    public void Eat() => Console.WriteLine("Animal eats");
}
class Dog : Animal
{
    public override void Speak() => Console.WriteLine("Dog barks");
    public new void Eat() => Console.WriteLine("Dog eats");
}
```

<details><summary>Answer</summary>

```text
Dog barks
Animal eats
Dog barks
Dog eats
```

`override` = **runtime** dispatch on the actual object (always "Dog barks"). `new` **hides** the base method; which one runs depends on the **compile-time type of the variable** (`Animal` → "Animal eats", `Dog` → "Dog eats"). (Section 4)

</details>

### Puzzle 5 — Virtual call from a base constructor + field initializer order

```csharp
new Derived();

class Base
{
    public Base() => Show();
    public virtual void Show() => Console.WriteLine("Base.Show");
}
class Derived : Base
{
    private string _initialized = "field-init";
    private string? _setInCtor;
    public Derived() { _setInCtor = "ctor"; Show(); }
    public override void Show() => Console.WriteLine($"Derived.Show: {_initialized}, {_setInCtor ?? "null"}");
}
```

<details><summary>Answer</summary>

```text
Derived.Show: field-init, null
Derived.Show: field-init, ctor
```

Order of construction: **derived field initializers → base constructor body → derived constructor body**. The base ctor calls a *virtual* method, which dispatches to the not-yet-constructed `Derived`: field initializers have already run, but the `Derived` ctor body hasn't. That's why "don't call virtual members from constructors" is a rule (CA2214).

</details>

### Puzzle 6 — Closures in loops

```csharp
var actions = new List<Action>();
for (int i = 0; i < 3; i++)
    actions.Add(() => Console.Write(i + " "));
foreach (var a in actions) a();
Console.WriteLine();

actions.Clear();
foreach (var x in new[] { 0, 1, 2 })
    actions.Add(() => Console.Write(x + " "));
foreach (var a in actions) a();
Console.WriteLine();
```

<details><summary>Answer</summary>

```text
3 3 3
0 1 2
```

Lambdas capture **variables, not values**. A `for` loop has **one** `i` shared by all iterations; by the time the lambdas run, `i` is 3. Since C# 5, `foreach` creates a **fresh** `x` per iteration, so each lambda has its own. Fix for `for`: `int copy = i;` inside the loop and capture `copy`. (Section 9)

</details>

### Puzzle 7 — Boxing and `==`

```csharp
int n = 5;
object o1 = n;
object o2 = n;
Console.WriteLine(o1 == o2);
Console.WriteLine(o1.Equals(o2));
Console.WriteLine((int)o1 == (int)o2);

object s1 = "hi";
object s2 = "hi";
Console.WriteLine(s1 == s2);
```

<details><summary>Answer</summary>

```text
False
True
True
True
```

Each assignment to `object` **boxes** a separate copy on the heap. `==` on `object` compares **references** → False. `Equals` is virtual → `Int32.Equals` compares values → True. String literals are **interned** (one shared instance), so even reference comparison is True — don't rely on it. (Sections 2 & 6)

</details>

### Puzzle 8 — Struct copies

```csharp
var p = new Point { X = 1 };
var q = p;
q.X = 99;
Console.WriteLine(p.X);

var arr = new[] { new Point { X = 1 } };
arr[0].X = 5;
Console.WriteLine(arr[0].X);

var list = new List<Point> { new Point { X = 1 } };
var tmp = list[0];
tmp.X = 5;
Console.WriteLine(list[0].X);

struct Point { public int X; }
```

<details><summary>Answer</summary>

```text
1
5
1
```

Assigning a struct **copies** it. Array elements are *variables* — `arr[0].X = 5` modifies the element in place. A `List<T>` indexer *returns a copy*, so `tmp` is a copy; writing `list[0].X = 5` directly doesn't even compile (**CS1612**). Lesson: keep structs immutable. (Section 5)

</details>

### Puzzle 9 — Integer overflow

```csharp
int max = int.MaxValue;
int x = max + 1;
Console.WriteLine(x);
try
{
    int y = checked(max + 1);
}
catch (OverflowException) { Console.WriteLine("Overflow!"); }

byte b = 255;
b++;
Console.WriteLine(b);

int min = int.MinValue;
Console.WriteLine(-min == min);
Console.WriteLine(Math.Abs(-5));
```

<details><summary>Answer</summary>

```text
-2147483648
Overflow!
0
True
5
```

Integer arithmetic is **unchecked by default** — it silently wraps around. `checked(...)` (or `<CheckForOverflowUnderflow>`) throws `OverflowException`. `-int.MinValue` has no positive counterpart in 32 bits, so it wraps back to itself (and `Math.Abs(int.MinValue)` throws). Constant expressions are checked at compile time: `int z = int.MaxValue + 1;` is error **CS0220**.

</details>

### Puzzle 10 — Floating-point precision

```csharp
Console.WriteLine(0.1 + 0.2 == 0.3);
Console.WriteLine((0.1 + 0.2).ToString(System.Globalization.CultureInfo.InvariantCulture));
Console.WriteLine(0.1m + 0.2m == 0.3m);
Console.WriteLine(Math.Abs(0.1 + 0.2 - 0.3) < 1e-9);

double zero = 0;
Console.WriteLine(double.IsPositiveInfinity(1 / zero));
double nan = zero / zero;
Console.WriteLine(nan == nan);
Console.WriteLine(nan.Equals(nan));
```

<details><summary>Answer</summary>

```text
False
0.30000000000000004
True
True
True
False
True
```

`double` is **binary** floating point: 0.1 and 0.2 can't be represented exactly. Use `decimal` (base-10) for money, or compare doubles with a tolerance. Dividing a *double* by zero gives `Infinity` (no exception — only *integer* division by zero throws). `NaN == NaN` is False by IEEE rules, but `Equals` returns True so `NaN` works as a dictionary key.

</details>

### Puzzle 11 — `return` inside `try` with `finally`

```csharp
Console.WriteLine(GetNumber());
Console.WriteLine(GetList().Count);

static int GetNumber()
{
    int x = 1;
    try { return x; }
    finally { x = 99; Console.WriteLine("finally ran"); }
}

static List<int> GetList()
{
    var list = new List<int>();
    try { return list; }
    finally { list.Add(42); }
}
```

<details><summary>Answer</summary>

```text
finally ran
1
1
```

The return **value** is captured when `return x;` executes; `finally` runs afterwards, so changing `x` doesn't change the result (1). For a reference type the captured value is the *reference*, so mutating the object in `finally` **is** visible (Count = 1). You can't `return` from a `finally` block at all (**CS0157**). (Section 12)

</details>

### Puzzle 12 — LINQ deferred execution

```csharp
var nums = new List<int> { 1, 2, 3 };
var query = nums.Where(n => n > 1);
nums.Add(4);
Console.WriteLine(string.Join(",", query));

var snapshot = nums.Where(n => n > 1).ToList();
nums.Add(5);
Console.WriteLine(string.Join(",", snapshot));
Console.WriteLine(query.Count());
```

<details><summary>Answer</summary>

```text
2,3,4
2,3,4
4
```

`Where` doesn't run when declared — it's a **recipe** executed each time you enumerate. So the first print sees 4, and `query.Count()` later sees 5 too (2,3,4,5 → 4 items). `ToList()` executes **immediately** and stores a snapshot. (Section 10)

</details>

### Puzzle 13 — Multiple enumeration side effects

```csharp
int calls = 0;
var q = Enumerable.Range(1, 3).Select(x => { calls++; return x * 10; });

var sum = q.Sum();
var max = q.Max();
Console.WriteLine($"{sum} {max} calls={calls}");

var first = q.First();
Console.WriteLine($"first={first} calls={calls}");
```

<details><summary>Answer</summary>

```text
60 30 calls=6
first=10 calls=7
```

Each terminal operator re-runs the whole pipeline: `Sum` (3 calls) + `Max` (3 calls) = 6. `First` stops after the first element (1 more call). If the source is a DB query or expensive, materialize once with `ToList()`. Tools warn: "Possible multiple enumeration".

</details>

### Puzzle 14 — `async` ordering

```csharp
Console.WriteLine("1");
var task = DoWorkAsync();
Console.WriteLine("3");
await task;
Console.WriteLine("5");

static async Task DoWorkAsync()
{
    Console.WriteLine("2");
    await Task.Delay(100);
    Console.WriteLine("4");
}
```

<details><summary>Answer</summary>

```text
1
2
3
4
5
```

An `async` method runs **synchronously until its first `await` of an incomplete task**. So "2" prints before control returns to the caller. `Task.Delay` isn't complete, so the method returns a `Task` and the caller prints "3". After the delay, the rest ("4") runs, then the `await task` in the caller resumes ("5"). (Section 14)

</details>

### Puzzle 15 — When does an `async` exception surface?

```csharp
var t = FailAsync();
Console.WriteLine("after call");
try { await t; }
catch (InvalidOperationException e) { Console.WriteLine($"caught: {e.Message}"); }

static async Task FailAsync()
{
    Console.WriteLine("in FailAsync");
    await Task.Yield();
    throw new InvalidOperationException("boom");
}
```

<details><summary>Answer</summary>

```text
in FailAsync
after call
caught: boom
```

Exceptions inside an `async Task` method are **stored in the returned Task**, not thrown at the call site. They're rethrown when you `await` it. That's why `async void` is dangerous: there's no Task to hold the exception, so it crashes the process. (Section 14)

</details>

### Puzzle 16 — Null-coalescing and lifted operators

```csharp
string? a = null;
string? b = "";
int? n = null;

Console.WriteLine($"[{a ?? "default"}]");
Console.WriteLine($"[{b ?? "default"}]");
Console.WriteLine(n + 5 ?? -1);
Console.WriteLine(a?.Length ?? 0);
a ??= "assigned";
Console.WriteLine(a);
Console.WriteLine($"{n > 0} {n <= 0} {n == null}");
```

<details><summary>Answer</summary>

```text
[default]
[]
-1
0
assigned
False False True
```

`??` only replaces **null** — an empty string is not null. Arithmetic with a null `int?` is "lifted": `null + 5` is `null`, so `?? -1` kicks in (`+` binds tighter than `??`). Comparisons with null `int?` are **always False** (both `>` and `<=`!), but `== null` is True. `??=` assigns only if null.

</details>

### Puzzle 17 — `==` vs `Equals` vs `ReferenceEquals` on strings

```csharp
string s1 = "hello";
string s2 = new string("hello".ToCharArray());
object o1 = s1, o2 = s2;

Console.WriteLine(s1 == s2);
Console.WriteLine(o1 == o2);
Console.WriteLine(o1.Equals(o2));
Console.WriteLine(ReferenceEquals(s1, s2));
Console.WriteLine(string.Equals("Hello", "hello", StringComparison.OrdinalIgnoreCase));
Console.WriteLine(ReferenceEquals(s1, string.Intern(s2)));
```

<details><summary>Answer</summary>

```text
True
False
True
False
True
True
```

`string` **overloads** `==` to compare content — but operators are chosen at **compile time** by the variable type. With `object` variables, `object ==` (reference comparison) is used → False, even though the content is equal. `Equals` is **virtual**, so it still compares content. `s2` is a separate object; `string.Intern` returns the pooled literal instance. (Section 6)

</details>

### Puzzle 18 — Default interface methods (C# 8)

```csharp
IGreeter g = new English();
Console.WriteLine(g.Greet("Ann"));
Console.WriteLine(((IGreeter)new Polite()).Greet("Ann"));

var e = new English();
// e.Greet("Ann");   // would not compile: CS1061 — English doesn't "inherit" the default method
Console.WriteLine(e is IGreeter);

interface IGreeter
{
    string Name => "anonymous";
    string Greet(string who) => $"Hello {who}, from {Name}";
}
class English : IGreeter { }
class Polite : IGreeter { public string Greet(string who) => $"Good day, {who}"; }
```

<details><summary>Answer</summary>

```text
Hello Ann, from anonymous
Good day, Ann
True
```

A default interface member is only reachable **through the interface type**. The class doesn't get it as its own member, which is why `e.Greet(...)` fails to compile. A class can provide its own implementation (`Polite`), which wins. DIMs exist so library authors can add members to interfaces without breaking implementers. (Section 4)

</details>

### Puzzle 19 — Record equality is shallow

```csharp
var a = new Person("Ann", ["c#"]);
var b = new Person("Ann", ["c#"]);
Console.WriteLine(a == b);

var c = a with { };
Console.WriteLine(a == c);
Console.WriteLine(ReferenceEquals(a, c));
c.Tags.Add("sql");
Console.WriteLine(a.Tags.Count);

record Person(string Name, List<string> Tags);
```

<details><summary>Answer</summary>

```text
False
True
False
2
```

Record equality compares each property with `EqualityComparer<T>.Default`. `List<string>` uses **reference** equality, so two lists with the same content are different → `a != b`. `with` makes a **shallow copy**: `c` is a new object (not reference-equal) but shares the same `Tags` list, so adding to `c.Tags` changes `a.Tags` too. Use `ImmutableArray<T>` or a custom `Equals` for deep semantics. (Sections 5 & 6)

</details>

### Puzzle 20 — Mutable struct in a `readonly` field (defensive copy)

```csharp
var h1 = new ReadonlyHolder();
h1.Counter.Increment();
h1.Counter.Increment();
Console.WriteLine(h1.Counter.Value);

var h2 = new MutableHolder();
h2.Counter.Increment();
h2.Counter.Increment();
Console.WriteLine(h2.Counter.Value);

struct MutableCounter
{
    public int Value;
    public void Increment() => Value++;
}
class ReadonlyHolder { public readonly MutableCounter Counter; }
class MutableHolder { public MutableCounter Counter; }
```

<details><summary>Answer</summary>

```text
0
2
```

Calling a method on a `readonly` struct field makes the compiler create a **defensive copy** (it can't prove the method won't mutate), so `Increment` changes a temporary copy. Declaring the struct as `readonly struct` (or the method as a `readonly` member) makes intent explicit and avoids hidden copies. Another reason: **make structs immutable**. (Section 5)

</details>

### Puzzle 21 — Exception filters run *before* inner `finally`

```csharp
try
{
    try { throw new InvalidOperationException("inner"); }
    finally { Console.WriteLine("inner finally"); }
}
catch (Exception e) when (Log(e)) { Console.WriteLine("never"); }
catch (InvalidOperationException e) { Console.WriteLine($"caught {e.Message}"); }

static bool Log(Exception e)
{
    Console.WriteLine($"filter sees: {e.Message}");
    return false;    // don't handle — just observe
}
```

<details><summary>Answer</summary>

```text
filter sees: inner
inner finally
caught inner
```

.NET exception handling has **two passes**: first it searches for a handler, evaluating `when` filters (stack not yet unwound); then it unwinds, running `finally` blocks, and enters the chosen `catch`. A filter returning `false` is the classic "log without catching" trick and doesn't lose the original stack. (Section 12)

</details>

### Puzzle 22 — `char` and `string` arithmetic

```csharp
Console.WriteLine('a' + 'b');
Console.WriteLine("a" + 'b');
Console.WriteLine('a' + "b" + 1 + 2);
Console.WriteLine(1 + 2 + "3" + 4 + 5);
char c = 'a';
c++;
Console.WriteLine(c);
Console.WriteLine((char)('a' + 2));
```

<details><summary>Answer</summary>

```text
195
ab
ab12
3345
b
c
```

`char + char` is **integer addition** (97 + 98 = 195). As soon as a `string` is involved, `+` becomes concatenation — evaluated **left to right**: `1 + 2` = 3 first, then `"3"` turns everything after into string concatenation. `c++` works on chars (it's a numeric type underneath), but `'a' + 2` is an `int` and needs a cast back to `char`.

</details>

### Puzzle 23 — Static fields in generic types

```csharp
Counter<int>.Count++;
Counter<int>.Count++;
Counter<string>.Count++;
Console.WriteLine($"{Counter<int>.Count} {Counter<string>.Count} {Counter<double>.Count}");

class Counter<T> { public static int Count; }
```

<details><summary>Answer</summary>

```text
2 1 0
```

Each **closed** generic type (`Counter<int>`, `Counter<string>`, …) is a separate type with its **own** static fields and static constructor. Handy for per-type caches (`EqualityComparer<T>.Default` uses this), surprising if you expected one global counter. (Section 7)

</details>

### Puzzle 24 — Iterators are lazy (even their validation)

```csharp
var seq = GetNumbers(-1);
Console.WriteLine("sequence created");
try
{
    foreach (var n in seq) Console.WriteLine(n);
}
catch (ArgumentException) { Console.WriteLine("thrown during enumeration"); }

static IEnumerable<int> GetNumbers(int count)
{
    if (count < 0) throw new ArgumentException("negative", nameof(count));
    for (int i = 0; i < count; i++) yield return i;
}
```

<details><summary>Answer</summary>

```text
sequence created
thrown during enumeration
```

A method containing `yield return` doesn't execute **any** of its body when called — not even the argument check. It runs on the first `MoveNext()`. Fix: split into a non-iterator public method that validates eagerly and a private iterator (or a local iterator function). (Section 11)

</details>

### Puzzle 25 — `using` disposal order

```csharp
using var a = new Resource("A");
using (var b = new Resource("B"))
{
    Console.WriteLine("inside B block");
}
using var c = new Resource("C");
Console.WriteLine("end of program");

class Resource : IDisposable
{
    private readonly string _name;
    public Resource(string name) { _name = name; Console.WriteLine($"open {name}"); }
    public void Dispose() => Console.WriteLine($"dispose {_name}");
}
```

<details><summary>Answer</summary>

```text
open A
open B
inside B block
dispose B
open C
end of program
dispose C
dispose A
```

A `using` **block** disposes at its closing brace. A `using` **declaration** (C# 8, `using var`) disposes at the end of the enclosing scope — here, the end of the top-level program — in **reverse order of declaration** (C, then A), like a stack. Dispose runs even if an exception is thrown (it's a `try/finally`). (Section 13)

</details>

# 21. Theory Q&A (80 questions)

> **How to use this section:** Read the question, answer **out loud** in your own words, then compare. The model answers are deliberately short (2–6 lines) — that's the right length for a spoken interview answer. If they want more, they'll ask a follow-up; that's where the deeper sections (1–18) help. ★ marks the "difference between X and Y" classics that come up most.

## .NET & CLR (Section 1)

**Q1. What is the CLR, and what happens when you run a C# program?**
The Common Language Runtime is .NET's execution engine. The C# compiler (Roslyn) turns source into **IL** (Intermediate Language) + metadata in an assembly (.dll). At runtime the CLR loads it, the **JIT** compiles IL to native machine code method-by-method on first call, and the CLR provides GC, type safety, exception handling and threading.

**Q2. ★ .NET Framework vs .NET Core vs .NET (5+)?**
.NET Framework (up to 4.8.x) is Windows-only, legacy, maintenance mode. .NET Core was the cross-platform, open-source rewrite; from .NET 5 it's just ".NET". New work goes on .NET 8/10 (LTS releases, even numbers; odd numbers are STS).

**Q3. What are IL, JIT, and AOT?**
IL is CPU-independent bytecode. JIT compiles IL to native code at runtime (can optimize for the actual CPU; tiered compilation recompiles hot methods). AOT (ReadyToRun / Native AOT) compiles ahead of time for faster startup and smaller memory, at the cost of restrictions (limited reflection, trimming).

**Q4. What are CTS and CLS?**
The Common Type System defines how types are declared and used across .NET languages (so C# `int` = VB `Integer` = `System.Int32`). The Common Language Specification is the subset of rules all languages agree on — CLS-compliant code is usable from any .NET language (e.g. no public unsigned types).

**Q5. What's an assembly? Managed vs unmanaged code?**
An assembly is the deployment unit (.dll/.exe) containing IL, metadata, and a manifest (name, version, references). Managed code runs under the CLR (GC, type safety); unmanaged code (C/C++, Win32 APIs, file handles) runs outside it and must be cleaned up explicitly.

## Types & memory (Section 2)

**Q6. ★ Value types vs reference types?**
Value types (`int`, `bool`, `struct`, `enum`) hold the data directly; assignment copies the data. Reference types (`class`, `string`, arrays, delegates) hold a reference to an object on the managed heap; assignment copies the reference, so two variables can point to the same object. Value types can't be null unless `Nullable<T>`.

**Q7. "Value types live on the stack" — true?**
Only partly. Locals and parameters of value type usually live on the stack, but a value type that's a **field of a class**, an **array element**, **boxed**, or **captured by a lambda/async method** lives on the heap. The real difference is copy semantics, not location.

**Q8. What are boxing and unboxing? Why care?**
Boxing wraps a value type in a heap object (`object o = 5;`); unboxing extracts it with a cast (`(int)o`). Each box allocates and adds GC pressure, and unboxing to the wrong type throws `InvalidCastException`. Generics (`List<int>` instead of `ArrayList`) avoid boxing.

**Q9. ★ `const` vs `readonly` vs `static readonly`?**
`const` is a compile-time constant (primitives/strings only), implicitly static, and **baked into callers' IL** — changing it requires recompiling consumers. `readonly` fields are set at declaration or in the constructor, can be any type, and are per-instance. `static readonly` is set once per type (e.g. `static readonly HttpClient`).

**Q10. ★ `var` vs `dynamic` vs `object`?**
`var` is **static typing** — the compiler infers the exact type; zero runtime cost. `object` is the base type; you need casts to use members. `dynamic` skips compile-time checks; members are resolved at **runtime** (DLR), so typos become runtime `RuntimeBinderException`s. Use `dynamic` only for COM/interop/dynamic JSON.

**Q11. ★ `String` vs `StringBuilder`?**
`string` is immutable — every `+` creates a new string, so concatenating in a loop is O(n²). `StringBuilder` uses a growable buffer; appending is amortized O(1). Use `StringBuilder` for many appends in loops; for a few pieces, `+`, interpolation or `string.Join`/`Concat` is fine and clearer.

## Methods (Section 3)

**Q12. ★ `ref` vs `out` vs `in`?**
All pass by reference. `ref`: must be initialized by the caller; method may read and write. `out`: caller needn't initialize; method **must** assign before returning (e.g. `TryParse`). `in`: read-only reference — used to pass large structs without copying.

**Q13. ★ Method overloading vs overriding?**
Overloading: same name, **different parameter lists**, in the same type — resolved at **compile time**. Overriding: a derived class replaces a `virtual`/`abstract` base method with the **same signature** using `override` — resolved at **runtime** based on the object's actual type (polymorphism).

**Q14. What are optional, named, and `params` parameters?**
Optional parameters have defaults (`void Log(string m, int level = 1)`) — the default is compiled into the caller. Named arguments (`Log(level: 2, m: "x")`) improve readability. `params int[] xs` (or `params ReadOnlySpan<int>` in C# 13) accepts a variable number of arguments.

**Q15. What are local functions, and how do they differ from lambdas?**
Methods declared inside another method. Unlike lambdas they can be generic, use `ref`/`out`/`params`, be recursive easily, and don't allocate a delegate unless converted to one. `static` local functions can't capture variables — useful to prevent accidental closures.

## OOP (Section 4)

**Q16. Name the four pillars of OOP with a C# example each.**
**Encapsulation**: private fields + public properties/methods. **Abstraction**: interfaces/abstract classes exposing *what*, hiding *how*. **Inheritance**: `class Dog : Animal`. **Polymorphism**: `Animal a = new Dog(); a.Speak();` calls the `Dog` override at runtime.

**Q17. ★ Abstract class vs interface?**
Abstract class: can have state (fields), constructors, implemented + abstract members, any access modifiers; a class can inherit **only one**. Interface: a contract with no instance state; a class can implement **many**; since C# 8 it may have default implementations and static members. Use an abstract class for shared code in an "is-a" family; interfaces for capabilities ("can-do").

**Q18. ★ `virtual`/`override` vs `new` (hiding)?**
`override` participates in runtime polymorphism — the object's actual type decides. `new` hides the base member; which method runs depends on the **compile-time type of the variable**. Hiding is usually a smell; the compiler warns (CS0108) if you hide without `new`.

**Q19. ★ Early (static) vs late (dynamic) binding?**
Early binding: the method is resolved at compile time (non-virtual calls, overloads) — fast and type-checked. Late binding: resolved at runtime — virtual dispatch via the v-table, `dynamic`, or reflection. Virtual calls are still type-checked; `dynamic`/reflection are not.

**Q20. What does `sealed` do?**
On a class: it can't be inherited (`string` is sealed). On an override: further derived classes can't override it again. Sealing helps the JIT devirtualize calls and documents that the class wasn't designed for extension.

**Q21. Explain C# access modifiers.**
`public` (anyone), `private` (same type; default for members), `protected` (type + derived), `internal` (same assembly; default for top-level types), `protected internal` (same assembly **or** derived), `private protected` (derived types **in the same assembly**), `file` (C# 11, same source file).

**Q22. Can you call a constructor from another constructor? Static constructors?**
Yes: `: this(...)` chains to another constructor in the same class; `: base(...)` calls the base class constructor. A static constructor has no parameters/access modifier, runs **once** automatically before first use of the type, and is thread-safe by the CLR.

## class, struct, record (Section 5)

**Q23. ★ Class vs struct?**
Class: reference type, heap-allocated, supports inheritance, reference equality by default, can be null. Struct: value type, copied on assignment, no inheritance (can implement interfaces), value equality via reflection-based `ValueType.Equals` by default (override it!). Use structs for small (≤16 bytes guideline), immutable, short-lived values like `Point`, `DateTime`.

**Q24. What is a record? `record` vs `record struct`?**
A record (C# 9) is a type designed for data: compiler-generated value equality, `ToString`, `Deconstruct`, and `with` cloning. `record` (= `record class`) is a reference type; `record struct` (C# 10) is a value type. Positional records create init-only properties; positional record structs create mutable ones unless declared `readonly record struct`.

**Q25. What are `init` and `required`?**
`init` (C# 9) is a setter usable only during object construction (constructor, object initializer, `with`) — immutability with initializer syntax. `required` (C# 11) forces callers to set the member in the object initializer — missing it is a compile error (CS9035).

**Q26. ★ Shallow copy vs deep copy?**
A shallow copy duplicates the top-level object but **shares** referenced objects (`MemberwiseClone`, record `with`). A deep copy recursively duplicates everything so the copies are fully independent (manual copy constructors, or serialization round-trip). Mutating a shared nested list after a shallow copy affects both.

## Equality (Section 6)

**Q27. ★ `==` vs `Equals()` vs `ReferenceEquals()`?**
`==` is an operator resolved at **compile time**; for classes it's reference equality unless overloaded (`string`, records overload it). `Equals` is **virtual**, so the runtime type's implementation is used (value comparison for `string`, value types, records). `ReferenceEquals` always checks identity.

**Q28. Why override `GetHashCode` when you override `Equals`?**
Hash-based collections (`Dictionary`, `HashSet`) first compare hash codes, then `Equals`. Equal objects **must** return equal hash codes, otherwise lookups fail. Use `HashCode.Combine(a, b)` and base the hash on immutable fields — a key whose hash changes after insertion is "lost" in the dictionary.

**Q29. How do you compare strings correctly?**
Use `string.Equals(a, b, StringComparison.OrdinalIgnoreCase)` (or `Ordinal`) for identifiers, keys, paths; `CurrentCulture` only for user-facing sorting. Avoid `a.ToLower() == b.ToLower()` — allocates and is culture-sensitive (the Turkish "I" problem).

## Generics (Section 7)

**Q30. Why use generics?**
Type safety (compile-time checks instead of casts), performance (no boxing for value types — `List<int>` vs `ArrayList`), and code reuse (one `Repository<T>` for all entities). The CLR creates specialized native code per value-type argument and shares code for reference types.

**Q31. What are generic constraints? Give examples.**
`where T : class` (reference type), `struct` (non-nullable value type), `new()` (has parameterless ctor), `notnull`, `unmanaged`, `BaseClass`, `IInterface`, `where T : U`. They let you call members on `T` and give compile-time safety: `T Max<T>(T a, T b) where T : IComparable<T>`.

**Q32. ★ Covariance vs contravariance (`out` / `in`)?**
Covariance (`out T`): you can use a *more derived* type where a less derived one is expected for **outputs** — `IEnumerable<string>` → `IEnumerable<object>`. Contravariance (`in T`): use a *less derived* type for **inputs** — `Action<object>` → `Action<string>`. Only interfaces and delegates support variance; `List<T>` is invariant.

## Collections (Section 8)

**Q33. ★ Array vs `List<T>`?**
Arrays have a fixed size set at creation; `List<T>` wraps an array and grows (doubling capacity) automatically, with `Add`/`Remove`/`Insert`. Both have O(1) indexing. Use arrays for fixed-size/performance-critical data; `List<T>` for most everyday code.

**Q34. How does `Dictionary<TKey,TValue>` work? Complexity?**
It's a hash table: the key's `GetHashCode` picks a bucket, `Equals` resolves collisions. Lookup, add and remove are O(1) average (O(n) worst case with bad hashes). Keys must be unique and non-null; use `TryGetValue` to avoid double lookups and `KeyNotFoundException`.

**Q35. ★ `IEnumerable<T>` vs `ICollection<T>` vs `IList<T>`?**
`IEnumerable<T>`: can only iterate (foreach), possibly lazily. `ICollection<T>`: adds `Count`, `Add`, `Remove`, `Contains`. `IList<T>`: adds index access and `Insert`/`RemoveAt`. Accept the *least* capable interface you need as parameters; return read-only interfaces (`IReadOnlyList<T>`) where possible.

**Q36. ★ `IEnumerable<T>` vs `IQueryable<T>`?**
`IEnumerable<T>` LINQ runs **in memory** using delegates (LINQ to Objects). `IQueryable<T>` builds an **expression tree** that a provider (EF Core) translates to SQL and runs **in the database**. Calling `.AsEnumerable()`/`.ToList()` too early pulls all rows into memory and filters there — a common performance bug.

**Q37. Which collections are thread-safe?**
Standard collections are not safe for concurrent writes. Use `ConcurrentDictionary`, `ConcurrentQueue`, `ConcurrentBag`, `BlockingCollection`, `Channel<T>`, or immutable collections (`ImmutableList<T>`) — or guard a normal collection with a `lock`. Note: `ConcurrentDictionary.GetOrAdd`'s factory can run more than once.

## Delegates, events, lambdas (Section 9)

**Q38. What is a delegate? `Func` vs `Action` vs `Predicate`?**
A delegate is a type-safe reference to a method (a "function pointer" object), which can be multicast. `Func<T..., TResult>` returns a value, `Action<T...>` returns void, `Predicate<T>` returns bool (older; `Func<T,bool>` is used by LINQ).

**Q39. ★ Delegate vs event?**
An event is a delegate field wrapped with restricted access: outside code can only `+=`/`-=`, while only the declaring class can invoke or reset it. With a public delegate field, any caller could do `handler = null` or invoke it. Events implement the observer pattern safely.

**Q40. What is a closure, and what's the loop trap?**
A lambda that captures outer variables; the compiler hoists them into a hidden class, so the lambda sees the **variable**, not a snapshot. In a `for` loop all lambdas share one counter variable (all print the final value); `foreach` gets a fresh variable per iteration since C# 5. Copy to a local inside the loop to fix.

**Q41. How can events cause memory leaks?**
The publisher's delegate list holds strong references to subscribers. If a long-lived publisher (static event, singleton) has short-lived subscribers that never `-=`, the subscribers can't be garbage-collected. Fix: unsubscribe (e.g., in `Dispose`), or use weak event patterns.

## LINQ (Section 10)

**Q42. What is deferred execution?**
Most LINQ operators (`Where`, `Select`, `OrderBy`) build a query that runs only when enumerated — and re-runs **each** time. Immediate operators (`ToList`, `ToArray`, `Count`, `First`, `Sum`, `Any`) execute right away. Use `ToList()` to snapshot results and avoid multiple enumeration.

**Q43. ★ `First` vs `FirstOrDefault` vs `Single` vs `SingleOrDefault`?**
`First`: first match, throws if none. `FirstOrDefault`: default (null/0) if none. `Single`: exactly one, throws if zero **or more than one**. `SingleOrDefault`: default if none, throws if more than one. `Single*` must scan further to prove uniqueness.

**Q44. ★ `Select` vs `SelectMany`?**
`Select` maps each element to one result (a list of orders → a list of order-line-lists). `SelectMany` maps each element to a sequence and **flattens** them (→ one list of all order lines). It's how you write nested `from ... from ...` queries.

**Q45. `Any()` vs `Count() > 0`?**
`Any()` stops at the first element (O(1) for most sources) and translates to `EXISTS` in SQL. `Count()` may enumerate everything. Prefer `Any()` for "is there at least one?"; use the `Count` **property** on collections when you need the number.

## Extension methods & yield (Section 11)

**Q46. What are extension methods and their limitations?**
Static methods in a static class whose first parameter has `this`, callable like instance methods (`"x".IsBlank()`); LINQ is built on them. They can't access private members, are resolved at compile time (no polymorphism), and lose to instance methods with the same signature. C# 14 adds extension properties via `extension` blocks.

**Q47. How does `yield return` work?**
The compiler turns the method into a **state machine** class implementing `IEnumerable<T>`/`IEnumerator<T>`. Each `MoveNext()` runs until the next `yield return`, pausing there. Execution is lazy (nothing runs until enumeration — even argument validation), enabling infinite sequences and streaming.

## Exceptions (Section 12)

**Q48. ★ `throw` vs `throw ex`?**
`throw;` rethrows the current exception **preserving the original stack trace**. `throw ex;` resets the stack trace to the current line, losing where the error happened. To add context, wrap it: `throw new OrderException("...", ex);` (inner exception preserved).

**Q49. What are exception filters (`when`)?**
`catch (HttpRequestException e) when (e.StatusCode == 404)` handles only if the condition is true. Filters run **before** the stack unwinds, so if false, the exception continues untouched with its full stack — also useful for logging (`when (Log(e))` returning false).

**Q50. Does `finally` always run?**
It runs on normal exit, `return`, `break`, and exceptions (caught or not, in practice). It does not run on `Environment.FailFast`, process kill, power loss, or `StackOverflowException` (which can't be caught). Use it (or `using`) for cleanup.

**Q51. Best practices for exceptions?**
Catch specific exceptions, only where you can handle or add context; don't swallow (`catch {}`). Don't use exceptions for control flow — use `TryParse`/`TryGetValue`. Validate arguments with `ArgumentNullException.ThrowIfNull`. Create custom exceptions ending in `Exception` for domain errors; log once at the boundary.

## GC & IDisposable (Section 13)

**Q52. How does the .NET garbage collector work?**
It's a tracing, generational, compacting GC. It finds objects reachable from **roots** (stack variables, statics, CPU registers, GC handles); unreachable ones are reclaimed and memory is compacted. Generations: Gen0 (new, collected often), Gen1 (buffer), Gen2 (long-lived). Objects ≥ 85,000 bytes go to the Large Object Heap (collected with Gen2, not compacted by default).

**Q53. ★ `Dispose` vs `Finalize` (destructor)?**
`Dispose()` (from `IDisposable`) is called **deterministically** by your code or `using` to release resources now. A finalizer (`~MyClass()`) is called **non-deterministically** by the GC's finalizer thread — a safety net for unmanaged resources, slow (object survives an extra GC). Proper pattern: `Dispose` cleans up and calls `GC.SuppressFinalize(this)`.

**Q54. What does `using` do?**
`using (var x = ...) { }` or `using var x = ...;` compiles to `try { ... } finally { x?.Dispose(); }`, guaranteeing disposal even on exceptions. `await using` does the same with `IAsyncDisposable.DisposeAsync()`.

**Q55. Can you have memory leaks in .NET?**
Yes — "logical" leaks where objects remain reachable: event handlers not unsubscribed, static collections/caches that only grow, timers holding references, captured closures, undisposed unmanaged resources. Diagnose with dotnet-counters, dotnet-gcdump, or a memory profiler.

**Q56. Should you call `GC.Collect()`?**
Almost never. The GC tunes itself; forcing a collection promotes objects to older generations and hurts performance. Valid exceptions are rare (benchmarks, after unloading a huge one-off structure in a client app).

## Async/await (Section 14)

**Q57. What does `async`/`await` actually do?**
The compiler turns an `async` method into a state machine. `await` on an incomplete task registers the rest of the method as a continuation and **returns the thread** to the caller/pool; when the task completes, execution resumes (possibly on another thread). It's about not **blocking** threads during I/O — not about creating threads.

**Q58. ★ `Task` vs `Thread`?**
A `Thread` is an OS-level thread — expensive (≈1 MB stack), you manage it. A `Task` is a higher-level promise of a result, usually scheduled on the **thread pool**, composable (`await`, `WhenAll`, continuations, cancellation, exception propagation). I/O tasks may not use any thread while waiting. Prefer `Task` in modern code.

**Q59. Why is `async void` bad?**
The caller can't await it, can't know when it finishes, and exceptions can't be caught by the caller — they're raised on the synchronization context and typically **crash the process**. Use `async Task`. The only valid use is event handlers.

**Q60. Why can `.Result` / `.Wait()` deadlock?**
In environments with a single-threaded `SynchronizationContext` (WinForms/WPF UI, classic ASP.NET), blocking the context thread with `.Result` while the awaited method needs that same thread to resume → both wait forever. It also wastes threads (thread-pool starvation) in ASP.NET Core. Rule: **async all the way**.

**Q61. What does `ConfigureAwait(false)` do?**
It tells `await` not to resume on the captured synchronization context, but on any thread-pool thread. Recommended in **library** code (avoids deadlocks and context overhead). Unnecessary in ASP.NET Core (no sync context); avoid in UI code that touches controls after the await.

**Q62. `Task.WhenAll` vs awaiting tasks one by one? `Task.Run` vs async I/O?**
Awaiting sequentially runs operations one after another; starting them all then `await Task.WhenAll(...)` runs them **concurrently** (total ≈ slowest). `Task.Run` offloads **CPU-bound** work to the thread pool; for I/O-bound work use naturally async APIs (`HttpClient.GetAsync`) — wrapping sync I/O in `Task.Run` just burns a thread.

## Multithreading (Section 15)

**Q63. What is a race condition? How do you prevent it?**
When the result depends on unsynchronized timing of threads accessing shared data (e.g. `count++` is read-modify-write, not atomic). Prevent with `lock`, `Interlocked.Increment`, concurrent collections, immutability, or by not sharing state (message passing/`Channel<T>`).

**Q64. What is a deadlock? How do you avoid it?**
Two+ threads each hold a resource and wait for the other's — forever. Avoid by acquiring locks in a **consistent order**, keeping lock scopes small, never calling unknown code (events/callbacks) inside a lock, using timeouts (`Monitor.TryEnter`), and not blocking on async code.

**Q65. ★ `lock` vs `Monitor` vs `Mutex` vs `SemaphoreSlim`?**
`lock` is syntax sugar for `Monitor.Enter/Exit` in try/finally — in-process, one thread at a time (C# 13 adds `System.Threading.Lock`). `Mutex` works **across processes** (slower, OS kernel object). `SemaphoreSlim` allows **N** concurrent holders and supports `await WaitAsync()` — the go-to for async code, since you can't `await` inside a `lock`.

**Q66. What does `volatile` do? What is `Interlocked`?**
`volatile` prevents the compiler/CPU from caching or reordering reads/writes of a field, guaranteeing visibility — but **not** atomicity of compound operations. `Interlocked` provides atomic operations (`Increment`, `Add`, `Exchange`, `CompareExchange`) without locks.

**Q67. What is the thread pool, and what's thread-pool starvation?**
A pool of reusable worker threads used by `Task.Run`, timers, and async continuations, avoiding the cost of creating threads. Starvation happens when pool threads are blocked (sync-over-async `.Result`, long `Thread.Sleep`), so queued work waits while the pool slowly injects threads — requests slow to a crawl.

## Modern C# (Section 16)

**Q68. What are nullable reference types?**
A C# 8 compile-time feature: with it enabled, `string` means "should never be null" and `string?` means "may be null"; the compiler warns on possible null dereferences (CS8602) or null assignments (CS8600). No runtime effect. `!` (null-forgiving) suppresses a warning.

**Q69. What is pattern matching? Give a few patterns.**
Testing a value's shape and extracting data in one step: type (`o is string s`), property (`{ Age: > 18 }`), relational/logical (`> 0 and < 10`, `not null`), positional (`Point(0, 0)`), list (`[1, .., var last]`). Mostly used in `is` expressions and switch expressions.

**Q70. ★ `is` vs `as` vs a cast?**
A cast `(T)o` throws `InvalidCastException` if it fails. `o as T` returns null on failure (reference/nullable types only). `o is T t` tests and assigns in one step — the modern preferred form. `is` also supports all other patterns (`is null`, `is > 5`).

**Q71. What are primary constructors (C# 12) and the gotcha?**
Constructor parameters declared on the type (`class Svc(ILogger log)`) that are in scope throughout the class. For classes they're not properties; if members use them, the compiler captures them in hidden **mutable** fields. Gotcha: using a parameter both to initialize a property *and* in methods creates two copies (warning CS9124).

**Q72. Name new features in C# 12, 13 and 14.**
C# 12: primary constructors, collection expressions `[..]`, default lambda parameters, alias any type. C# 13: `params` collections, the `Lock` type, partial properties. C# 14: the `field` keyword, extension members (extension properties), null-conditional assignment `a?.B = x`, `nameof` of unbound generics.

## Reflection & attributes (Section 17)

**Q73. ★ `typeof` vs `GetType()`?**
`typeof(T)` gets the `Type` for a type name at **compile time**. `obj.GetType()` returns the **runtime** type of an instance (`Animal a = new Dog(); a.GetType()` → `Dog`), and throws if `obj` is null.

**Q74. What are attributes, and why is reflection considered slow?**
Attributes are declarative metadata (`[Obsolete]`, `[Required]`) read via reflection by frameworks or tools; they do nothing on their own. Reflection is slow because it does string-based lookups, security/visibility checks, argument boxing into `object[]`, and blocks inlining — cache `MemberInfo`s, compile to delegates, or use source generators.

## SOLID & patterns (Section 18)

**Q75. Explain SOLID in one line each.**
S: one reason to change per class. O: extend behavior by adding code, not modifying existing code. L: subclasses must be substitutable for their base without surprises. I: prefer small, focused interfaces. D: depend on abstractions; inject dependencies.

**Q76. What is Dependency Injection, and what are the service lifetimes?**
Supplying an object's dependencies from outside (usually via constructor) instead of creating them inside — enables loose coupling and unit testing with fakes. In ASP.NET Core: **Transient** (new every resolve), **Scoped** (one per request), **Singleton** (one per app). Don't inject scoped into singleton (captive dependency).

**Q77. How do you implement a thread-safe singleton? Downsides?**
`private static readonly Lazy<T> _inst = new(() => new T()); public static T Instance => _inst.Value;` with a private constructor (or a `static readonly` field). Downsides: global state, hidden dependencies, hard to mock — prefer registering as a singleton in DI.

**Q78. ★ Strategy vs Factory? Decorator vs Adapter?**
Factory decides **which object to create**; Strategy decides **which algorithm to run** (often a factory picks the strategy). Decorator wraps an object with the **same interface** to add behavior (logging, caching); Adapter wraps an object to **convert** its interface into the one the client expects.

**Q79. What is the Repository pattern? Is it needed with EF Core?**
An abstraction that makes data access look like an in-memory collection (`GetById`, `Add`, `Find`), hiding persistence details and simplifying testing. EF Core's `DbSet<T>`/`DbContext` already implement repository/unit of work, so a thin generic wrapper often adds little; domain-specific repositories with meaningful queries are still useful.

**Q80. ★ Composition vs inheritance — which do you prefer?**
"Favor composition over inheritance." Inheritance creates tight coupling and fragile hierarchies (Rectangle/Square LSP problem); composition (has-a, delegating to injected interfaces) is flexible and testable — patterns like Strategy and Decorator are composition. Use inheritance for genuine "is-a" relationships with shared behavior.

# 22. Mock Interview & Cheat Sheet

> **How to use this section:** Do this the evening before the interview, with a timer. Say your "Tell me about yourself" out loud twice. Run the 45-minute mock: answer the rapid-fire questions aloud *before* opening each answer, solve the three coding tasks in an empty console app without looking, then hunt the bugs. Finish with the cheat sheet and checklist — and then stop studying and sleep.

## 22.1 "Tell me about yourself" — template for a 2-YOE C#/.NET developer

Keep it to **60–90 seconds**: *present → past → impact → why this role*. Fill the brackets with your real details.

> "Hi, I'm **[Name]**. I'm a .NET developer with about **two years** of experience, currently at **[Company]**, where I work on **[product/domain — e.g. a B2B order-management platform]**.
>
> Day-to-day I build **ASP.NET Core Web APIs in C#**, using **Entity Framework Core with SQL Server**, and I write **unit tests with xUnit and Moq**. I'm comfortable with **async/await, LINQ, dependency injection**, and I've worked with **[Azure App Service / Service Bus / Docker / CI pipelines]**.
>
> One thing I'm proud of: **[concrete achievement with a number]** — for example, I found that an endpoint was loading whole tables with `ToList()` before filtering; moving the filter into the `IQueryable` and adding an index cut response time from **2 seconds to 200 ms**.
>
> I've also **[growth signal — reviewed PRs, mentored an intern, owned a feature end-to-end, handled on-call incidents]**.
>
> I'm looking for a role where I can **[go deeper on distributed systems / own larger features / work on cloud-native services]**, and **[Company]**'s work on **[something specific about them]** is exactly that, which is why I'm excited to be here."

**Tips:** one concrete story with a metric beats a list of 15 technologies. Prepare 2–3 STAR stories (Situation, Task, Action, Result): a bug you hunted, a performance win, a disagreement you resolved.

## 22.2 The 45-minute mock

| Time | Part |
|---|---|
| 0–5 min | Introduction ("tell me about yourself") |
| 5–15 min | 15 rapid-fire questions (~40 s each) |
| 15–40 min | 3 live-coding tasks (~8 min each) |
| 40–45 min | Find the bugs + your questions for them |

### Rapid-fire (answer aloud in ≤ 40 seconds each)

**1. What's the difference between `IEnumerable<T>` and `IQueryable<T>`?**
<details><summary>Answer</summary>

`IEnumerable<T>` executes LINQ in memory with delegates; `IQueryable<T>` builds an expression tree that a provider (EF Core) translates to SQL and executes in the database. Filtering on `IQueryable` before materializing avoids loading whole tables.
</details>

**2. Is `string` a value type or reference type? Why does it behave like a value?**
<details><summary>Answer</summary>

Reference type. It *feels* like a value because it's **immutable** and overloads `==`/`Equals` to compare contents; every "modification" returns a new string.
</details>

**3. What happens if you don't `await` a Task?**
<details><summary>Answer</summary>

It still runs ("fire and forget"), but you lose its exceptions (they're unobserved), can't know when it finishes, and code after it may run before it completes. The compiler warns (CS4014) inside async methods.
</details>

**4. `struct` vs `class` — when would you choose a struct?**
<details><summary>Answer</summary>

For small (≈16 bytes or less), immutable, value-like data with no identity — coordinates, money amounts, ranges — especially when creating many of them (avoids heap allocations). Otherwise a class (or record).
</details>

**5. What does the `virtual` keyword do?**
<details><summary>Answer</summary>

It allows a derived class to `override` the method; calls are dispatched at runtime based on the object's actual type (polymorphism). Non-virtual methods are bound at compile time.
</details>

**6. `throw` vs `throw ex`?**
<details><summary>Answer</summary>

`throw;` preserves the original stack trace; `throw ex;` resets it to the current line, hiding where the error originated. Wrap with `new XException("context", ex)` to add information.
</details>

**7. What's a `Dictionary` lookup complexity, and what makes it degrade?**
<details><summary>Answer</summary>

O(1) on average. It degrades towards O(n) with poor `GetHashCode` implementations (many collisions), and breaks entirely if a key's hash changes after it's inserted (mutable keys).
</details>

**8. What is dependency injection, and the three lifetimes?**
<details><summary>Answer</summary>

Passing dependencies into a class (usually via constructor) instead of creating them inside — enables loose coupling and mocking. Lifetimes: Transient (new each time), Scoped (per request), Singleton (per app).
</details>

**9. What does `using` do?**
<details><summary>Answer</summary>

It guarantees `Dispose()` is called (it compiles to try/finally) — even if an exception is thrown. `using var x = ...;` disposes at the end of the enclosing scope. (The `using Namespace;` directive is unrelated.)
</details>

**10. Why is `async void` dangerous?**
<details><summary>Answer</summary>

It can't be awaited and its exceptions can't be caught by the caller — they crash the process. Use `async Task`; `async void` only for event handlers.
</details>

**11. `const` vs `readonly`?**
<details><summary>Answer</summary>

`const`: compile-time value, implicitly static, only primitives/strings, copied into callers' IL. `readonly`: set at declaration or in the constructor, runtime value, any type, per instance (unless `static readonly`).
</details>

**12. What is LINQ deferred execution?**
<details><summary>Answer</summary>

Query operators like `Where`/`Select` don't run until the result is enumerated, and re-run every enumeration. `ToList()`/`Count()`/`First()` execute immediately.
</details>

**13. How do you make `count++` thread-safe?**
<details><summary>Answer</summary>

`Interlocked.Increment(ref count);` (atomic, lock-free) — or wrap it in a `lock`. `volatile` alone does **not** make it atomic.
</details>

**14. Abstract class or interface for a plug-in contract?**
<details><summary>Answer</summary>

An interface: implementers may already have a base class, can implement several contracts, and it's easy to mock. Use an abstract base class *additionally* if you want to share default code.
</details>

**15. What does the GC do with an object that has a finalizer?**
<details><summary>Answer</summary>

When unreachable it isn't freed immediately; it's placed on the finalization queue, its finalizer runs on the finalizer thread, and memory is reclaimed in a **later** GC — so it lives longer. `Dispose` + `GC.SuppressFinalize(this)` avoids this cost.
</details>

### Live-coding task 1 — LINQ reporting (≈8 min)

*"Given a list of employees, print each department's headcount, average salary, and top earner, ordered by average salary descending."*

<details><summary>Answer</summary>

```csharp
List<Employee> employees =
[
    new("Asha", "Engineering", 120_000),
    new("Ravi", "Engineering", 95_000),
    new("Meera", "Sales", 70_000),
    new("John", "Sales", 82_000),
    new("Li", "HR", 60_000),
];

var report = employees
    .GroupBy(e => e.Department)
    .Select(g => new
    {
        Department = g.Key,
        Count = g.Count(),
        Average = g.Average(e => e.Salary),
        Top = g.MaxBy(e => e.Salary)!.Name          // MaxBy: .NET 6+
    })
    .OrderByDescending(r => r.Average);

foreach (var r in report)
    Console.WriteLine($"{r.Department,-12} count={r.Count} avg={r.Average:N0} top={r.Top}");

record Employee(string Name, string Department, decimal Salary);
```

```text
Engineering  count=2 avg=107,500 top=Asha
Sales        count=2 avg=76,000 top=John
HR           count=1 avg=60,000 top=Li
```

**Talking points:** `GroupBy` + projection into an anonymous type; `MaxBy` instead of `OrderByDescending().First()` (O(n) vs O(n log n)); `decimal` for money; with EF Core, this would translate to SQL `GROUP BY` (though `MaxBy` may not translate — be ready to say so).
</details>

### Live-coding task 2 — Concurrent async calls with a limit and timeout (≈8 min)

*"Fetch data for several IDs concurrently, at most 2 at a time, with an overall timeout. Return results in the original order."* (Simulate the HTTP call with `Task.Delay`.)

<details><summary>Answer</summary>

```csharp
int[] ids = [1, 2, 3, 4, 5];
using var cts = new CancellationTokenSource(TimeSpan.FromSeconds(5));
using var throttle = new SemaphoreSlim(2);          // max 2 in flight
int inFlight = 0, maxInFlight = 0;

async Task<string> FetchAsync(int id, CancellationToken ct)
{
    await throttle.WaitAsync(ct);
    try
    {
        int now = Interlocked.Increment(ref inFlight);
        InterlockedMax(ref maxInFlight, now);
        await Task.Delay(100 * (6 - id), ct);       // pretend HTTP call; later IDs finish sooner
        return $"item-{id}";
    }
    finally
    {
        Interlocked.Decrement(ref inFlight);
        throttle.Release();
    }
}

try
{
    string[] results = await Task.WhenAll(ids.Select(id => FetchAsync(id, cts.Token)));
    Console.WriteLine(string.Join(", ", results));  // WhenAll keeps input order
    Console.WriteLine($"max concurrent calls: {maxInFlight}");
}
catch (OperationCanceledException)
{
    Console.WriteLine("timed out");
}

static void InterlockedMax(ref int target, int value)
{
    int current;
    while ((current = Volatile.Read(ref target)) < value &&
           Interlocked.CompareExchange(ref target, value, current) != current) { }
}
```

```text
item-1, item-2, item-3, item-4, item-5
max concurrent calls: 2
```

**Talking points:** `SemaphoreSlim.WaitAsync` (not `lock`, which can't contain `await`); release in `finally`; one `CancellationToken` flows everywhere; `Task.WhenAll` preserves order regardless of completion order; on .NET 6+ `Parallel.ForEachAsync(ids, new ParallelOptions { MaxDegreeOfParallelism = 2 }, ...)` is an alternative.
</details>

### Live-coding task 3 — A thread-safe bank account (≈8 min)

*"Write an `Account` with `Deposit` and `Withdraw`. Validate inputs, throw a custom exception on insufficient funds, and make it safe for concurrent use. Show it works."*

<details><summary>Answer</summary>

```csharp
var account = new Account("ACC-1", 100m);

Parallel.For(0, 1000, _ => account.Deposit(1m));
Console.WriteLine($"After 1000 parallel deposits: {account.Balance}");

account.Withdraw(600m);
Console.WriteLine($"After withdrawing 600: {account.Balance}");

try { account.Withdraw(10_000m); }
catch (InsufficientFundsException ex) { Console.WriteLine($"{ex.Message} (short by {ex.Shortfall})"); }

try { account.Deposit(-5m); }
catch (ArgumentOutOfRangeException ex) { Console.WriteLine($"Rejected: {ex.ParamName}"); }

public sealed class InsufficientFundsException(string accountId, decimal shortfall)
    : Exception($"Account {accountId} has insufficient funds")
{
    public decimal Shortfall { get; } = shortfall;
}

public sealed class Account(string id, decimal openingBalance)
{
    private readonly Lock _gate = new();             // C# 13; use 'private readonly object' before .NET 9
    private decimal _balance = openingBalance;

    public string Id { get; } = id;
    public decimal Balance { get { lock (_gate) return _balance; } }

    public void Deposit(decimal amount)
    {
        ArgumentOutOfRangeException.ThrowIfNegativeOrZero(amount);
        lock (_gate) _balance += amount;
    }

    public void Withdraw(decimal amount)
    {
        ArgumentOutOfRangeException.ThrowIfNegativeOrZero(amount);
        lock (_gate)                                   // check-then-act must be ONE atomic step
        {
            if (amount > _balance) throw new InsufficientFundsException(Id, amount - _balance);
            _balance -= amount;
        }
    }
}
```

```text
After 1000 parallel deposits: 1100
After withdrawing 600: 500
Account ACC-1 has insufficient funds (short by 9500)
Rejected: amount
```

**Talking points:** `decimal` for money; validation with `ThrowIfNegativeOrZero` (.NET 8); the *check-then-act* in `Withdraw` must be inside the lock (a race otherwise lets two threads overdraw); `Interlocked` doesn't support `decimal`, hence `lock`; custom exception carries useful data; encapsulation — no public setter on `Balance`.
</details>

### Find the bugs (≈5 min)

*"This code compiles. Find at least six problems."*

```csharp
var processor = new OrderProcessor();
processor.Run();
Console.ReadLine();

struct Counter { public int Value; }

class OrderProcessor
{
    private readonly List<int> _orders = [1, 2, 3, 4, 5];

    public void Run()
    {
        var counters = new List<Counter> { new Counter() };
        var c = counters[0];
        c.Value++;

        var actions = new List<Action>();
        for (int i = 0; i < _orders.Count; i++)
            actions.Add(() => Console.WriteLine(_orders[i]));

        foreach (var order in _orders)
            if (order % 2 == 0) _orders.Remove(order);

        var reader = new StreamReader("orders.txt");
        string firstLine = reader.ReadLine() ?? "";

        string page = DownloadAsync().Result;
        SaveAsync(page);
    }

    private async Task<string> DownloadAsync()
    {
        using var http = new HttpClient();
        return await http.GetStringAsync("https://example.com");
    }

    private async void SaveAsync(string data)
    {
        await File.WriteAllTextAsync("page.html", data);
    }
}
```

<details><summary>Answer</summary>

| # | Bug | Why it's wrong | Fix |
|---|---|---|---|
| 1 | `var c = counters[0]; c.Value++;` | **Mutable struct in a list**: the indexer returns a *copy*; the list element stays 0 | Make `Counter` a class, or write back `counters[0] = c;`, or better: make the struct immutable |
| 2 | `actions.Add(() => ... _orders[i])` in a `for` loop | **Closure captures the loop variable**: all lambdas see the final `i` (= `Count`) → `ArgumentOutOfRangeException` when invoked | `int idx = i;` inside the loop and capture `idx` (or use `foreach`) |
| 3 | `_orders.Remove(order)` inside `foreach` | **Modifying a collection while enumerating** → `InvalidOperationException: Collection was modified` | `_orders.RemoveAll(o => o % 2 == 0);` or iterate backwards with a `for` loop |
| 4 | `new StreamReader(...)` never disposed | **Missing Dispose**: file handle leaks until the GC finalizes it; file stays locked | `using var reader = new StreamReader("orders.txt");` |
| 5 | `DownloadAsync().Result` | **Sync-over-async**: blocks a thread; can **deadlock** under a UI/legacy ASP.NET sync context; exceptions wrapped in `AggregateException` | Make `Run` `async Task` and `await DownloadAsync()` |
| 6 | `private async void SaveAsync` called without await | **`async void`**: caller can't await it or catch its exceptions; a failure crashes the process; the program may exit before it finishes | `private async Task SaveAsync(...)` and `await SaveAsync(page);` |
| 7 | `new HttpClient()` per call in a `using` | Creating/disposing `HttpClient` repeatedly can exhaust sockets (TIME_WAIT) | Reuse a `static readonly HttpClient` or inject one via `IHttpClientFactory` |
| 8 | `Console.ReadLine()` at the end of `Main` hiding async issues; `Run` isn't awaitable | Design smell: the program relies on the user pressing Enter for background work to finish | `await processor.RunAsync();` |

**Fixed version (core parts):**

```csharp
var processor = new OrderProcessor();
await processor.RunAsync();

class OrderProcessor
{
    private static readonly HttpClient Http = new();
    private readonly List<int> _orders = [1, 2, 3, 4, 5];

    public async Task RunAsync()
    {
        var actions = new List<Action>();
        for (int i = 0; i < _orders.Count; i++)
        {
            int idx = i;
            actions.Add(() => Console.WriteLine(_orders[idx]));
        }

        _orders.RemoveAll(o => o % 2 == 0);

        using var reader = new StreamReader("orders.txt");
        string firstLine = await reader.ReadLineAsync() ?? "";

        string page = await Http.GetStringAsync("https://example.com");
        await File.WriteAllTextAsync("page.html", page);
    }
}
```
</details>

## 22.3 CHEAT SHEET

### Value types vs reference types

| | Value type | Reference type |
|---|---|---|
| Examples | `int`, `double`, `decimal`, `bool`, `char`, `DateTime`, `struct`, `enum`, `record struct`, tuples | `class`, `record`, `string`, arrays, `object`, delegates, interfaces (as variables) |
| Variable holds | the data itself | a reference to a heap object |
| Assignment | copies the data | copies the reference (same object) |
| Default | `0` / `false` / all-zero struct | `null` |
| Null | only via `Nullable<T>` (`int?`) | yes (warned by NRT if not `?`) |
| Inheritance | no (can implement interfaces) | yes (single class inheritance) |
| Equality default | field-by-field (`ValueType.Equals`) | reference |
| Stored | inline (stack for locals, inside the containing object otherwise) | managed heap, GC-collected |

### Access modifiers

| Modifier | Visible to |
|---|---|
| `public` | everyone |
| `private` | containing type only (default for members) |
| `protected` | containing type + derived types |
| `internal` | same assembly (default for top-level types) |
| `protected internal` | same assembly **OR** derived types |
| `private protected` | derived types **AND** same assembly |
| `file` | same source file (C# 11, types only) |

### Collections — Big-O

| Collection | Access by index | Search | Add | Remove | Notes |
|---|---|---|---|---|---|
| `T[]` | O(1) | O(n) | — | — | fixed size |
| `List<T>` | O(1) | O(n) | O(1) amortized (end) | O(n) | `Insert`/`RemoveAt(0)` O(n) |
| `LinkedList<T>` | O(n) | O(n) | O(1) at node | O(1) at node | no index |
| `Dictionary<K,V>` | O(1) by key | O(1) key, O(n) value | O(1) avg | O(1) avg | unique keys |
| `HashSet<T>` | — | O(1) `Contains` | O(1) | O(1) | unique items, `Add` returns bool |
| `SortedDictionary<K,V>` | O(log n) | O(log n) | O(log n) | O(log n) | red-black tree, sorted keys |
| `SortedList<K,V>` | O(1) by index, O(log n) by key | O(log n) | O(n) | O(n) | compact memory |
| `Stack<T>` / `Queue<T>` | — | O(n) | O(1) Push/Enqueue | O(1) Pop/Dequeue | LIFO / FIFO |
| `PriorityQueue<T,P>` | — | — | O(log n) | O(log n) Dequeue | min-heap (.NET 6) |
| `ConcurrentDictionary<K,V>` | O(1) | O(1) | O(1) | O(1) | thread-safe |

### LINQ operators at a glance

| Category | Operators | Executes |
|---|---|---|
| Filter | `Where`, `OfType`, `Distinct`, `DistinctBy` | deferred |
| Project | `Select`, `SelectMany`, `Zip`, `Cast` | deferred |
| Sort | `OrderBy`, `OrderByDescending`, `ThenBy`, `Reverse`, `Order` | deferred (buffers on enumeration) |
| Group / join | `GroupBy`, `Join`, `GroupJoin`, `ToLookup` (immediate) | deferred (except `ToLookup`) |
| Partition | `Take`, `Skip`, `TakeWhile`, `SkipWhile`, `Chunk` | deferred |
| Set | `Union`, `Intersect`, `Except`, `Concat` | deferred |
| Element | `First(OrDefault)`, `Single(OrDefault)`, `Last`, `ElementAt` | **immediate** |
| Aggregate | `Count`, `Sum`, `Min`, `Max`, `MinBy`, `MaxBy`, `Average`, `Aggregate`, `CountBy` | **immediate** |
| Quantifier | `Any`, `All`, `Contains` | **immediate** |
| Convert | `ToList`, `ToArray`, `ToDictionary`, `ToHashSet` | **immediate** |

### Async rules

| ✅ Do | ❌ Don't |
|---|---|
| `async Task` / `async Task<T>` | `async void` (except event handlers) |
| `await` all the way up | `.Result`, `.Wait()`, `.GetAwaiter().GetResult()` |
| `Task.WhenAll` for independent work | `await` in a loop when calls are independent |
| Pass and honor `CancellationToken` | ignore cancellation |
| `Task.Run` only for CPU-bound work | wrap I/O in `Task.Run` |
| `ConfigureAwait(false)` in libraries | touch UI after `ConfigureAwait(false)` |
| `SemaphoreSlim.WaitAsync` to limit concurrency | `lock` around `await` (won't compile) |
| `ValueTask` only for hot paths that often complete synchronously | await a `ValueTask` twice |
| Name async methods `...Async` | return `null` from a `Task`-returning method |

### Keyword one-liners

| Keyword | One-liner |
|---|---|
| `static` | belongs to the type, not an instance |
| `readonly` | assign only at declaration/constructor |
| `const` | compile-time constant, baked into callers |
| `sealed` | can't inherit / can't override further |
| `abstract` | no implementation; derived must override; class can't be instantiated |
| `virtual` / `override` | allow / provide runtime polymorphic replacement |
| `new` (modifier) | hide a base member (compile-time binding) |
| `base` / `this` | parent / current instance; constructor chaining |
| `ref` / `out` / `in` | by reference: read-write / must-assign / read-only |
| `params` | variable number of arguments |
| `is` / `as` | type test (+pattern) / safe cast returning null |
| `typeof` / `nameof` / `sizeof` | `Type` object / symbol name string / size of unmanaged type |
| `checked` / `unchecked` | overflow throws / wraps |
| `using` | dispose scope; also namespace import / alias |
| `lock` | mutual exclusion via `Monitor` |
| `volatile` | no caching/reordering of a field (not atomic) |
| `yield` | build an iterator lazily |
| `async` / `await` | state machine / asynchronously wait without blocking |
| `delegate` / `event` | method reference type / restricted delegate for pub-sub |
| `partial` | split a type/method across files (source generators) |
| `record` / `init` / `required` / `with` | data type / set-once / must-set / copy-and-modify |
| `default` | default value of a type (`default(T)`) |
| `dynamic` | runtime-resolved member access |
| `field` | (C# 14) property's auto backing field |

## 22.4 Last-day checklist

- [ ] Can explain **value vs reference types**, boxing, and `string` immutability with an example.
- [ ] Can write a **switch expression** with patterns and a **record** from memory.
- [ ] Can explain **abstract class vs interface**, **virtual/override vs new**, and the four OOP pillars.
- [ ] Can explain **`==` vs `Equals`** and why `GetHashCode` must match.
- [ ] Can write `GroupBy`, `SelectMany`, `ToDictionary` queries and explain **deferred execution** and **`IEnumerable` vs `IQueryable`**.
- [ ] Can explain **async/await** (state machine, no thread while waiting), why **`async void`** and **`.Result`** are bad, and `Task.WhenAll`.
- [ ] Can explain `lock`, `Interlocked`, `SemaphoreSlim`, race conditions and deadlocks.
- [ ] Can explain **GC generations**, **`IDisposable` / `using`**, and Dispose vs finalizer.
- [ ] Can explain **SOLID** with one example each and write **DI by constructor**, **Singleton with `Lazy<T>`**, **Strategy**, **Decorator**.
- [ ] Can code in < 5 min: reverse string, palindrome, anagram, first unique char, two-sum, FizzBuzz, balanced brackets, LRU cache outline.
- [ ] Know what's new in **C# 12–14** (primary ctors, collection expressions, `field`, extension members).
- [ ] "Tell me about yourself" rehearsed (≤ 90 s) + 2–3 STAR stories with numbers.
- [ ] Laptop charged, IDE/online editor tested, quiet room, water. Sleep > cramming.

**During coding rounds:** clarify → examples → brute force → optimize → code → test with edge cases (empty, one element, null, duplicates, negatives, very large) → state Big-O. Think aloud. If stuck, say what you're considering — silence is the only wrong answer.

## 22.5 Questions to ask the interviewer

Pick 2–3. Good questions show you think like a teammate.

1. "What does a typical week look like for a developer on this team?"
2. "Which .NET version and architecture are you on — monolith, microservices, cloud services? Any migrations in progress?"
3. "How do code reviews, testing and deployments work here? How often do you ship to production?"
4. "What would success look like for this role in the first 3–6 months?"
5. "What's the biggest technical challenge the team is facing right now?"
6. "How is on-call / production support handled?"
7. "How do engineers grow here — mentoring, learning time, moving to larger ownership?"
8. "What do you personally enjoy most about working on this team?"

Avoid asking about salary/leave in technical rounds (save it for HR), and never say "No, I don't have any questions."
