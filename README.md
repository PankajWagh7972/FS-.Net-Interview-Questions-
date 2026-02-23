# FS-.Net-Interview-Questions-

---

# 📘 Senior .NET Full Stack Developer – Interview Preparation Guide

**Tech Stack:** .NET Core, C#, ASP.NET Core Web API, Angular, SQL Server, Azure, CI/CD, Microservices
**Experience Level:** 5–10 Years

---

# Table of Contents

1. .NET Core & C#
2. ASP.NET Core Web API
3. Entity Framework Core & SQL Server
4. Angular
5. System Design (Banking Focus)
6. CI/CD, Git & Cloud
7. Unit Testing & Code Quality
8. Agile & Team Leadership
9. Scenario-Based Troubleshooting

---

# 1️⃣ .NET Core & C#

---

## 1. Explain ASP.NET Core Request Pipeline in detail

ASP.NET Core processes HTTP requests using a **middleware pipeline**. Middleware components are executed in the **order they are registered** in the `Program.cs` file.

### Request Flow:

Client → Middleware → Routing → Controller → Response

Each middleware:

* Can process the request
* Modify the request
* Pass control to the next middleware
* Short-circuit the pipeline and return response

### Common Middlewares:

* Exception Handling
* HTTPS Redirection
* Authentication
* Authorization
* Logging
* Routing

### Example:

```csharp
app.UseExceptionHandler("/error");
app.UseHttpsRedirection();
app.UseAuthentication();
app.UseAuthorization();
app.MapControllers();
```

### Why order matters?

Authentication must run before Authorization, otherwise the user identity won't exist.

---

## 2. Explain Dependency Injection (DI) in ASP.NET Core

Dependency Injection is a **design pattern used to reduce tight coupling** between components.

ASP.NET Core provides **built-in IoC container**.

### Benefits:

* Loose coupling
* Easy testing
* Better maintainability
* Cleaner architecture

### Service Lifetimes:

| Lifetime  | Description                    | Use Case             |
| --------- | ------------------------------ | -------------------- |
| Transient | New instance per request       | Lightweight services |
| Scoped    | One instance per HTTP request  | DbContext            |
| Singleton | Single instance for entire app | Caching, logging     |

### Example:

```csharp
services.AddScoped<IOrderService, OrderService>();
```

### Internal Working:

The container builds an **object dependency tree** at runtime and resolves dependencies automatically.

---

## 3. Difference between .NET Framework and .NET Core

| Feature       | .NET Framework | .NET Core       |
| ------------- | -------------- | --------------- |
| Platform      | Windows only   | Cross-platform  |
| Performance   | Moderate       | High            |
| Microservices | Limited        | Excellent       |
| Cloud Ready   | Partial        | Fully optimized |
| Deployment    | Machine-based  | Self-contained  |

### Why enterprises prefer .NET Core?

* Faster APIs
* Microservices friendly
* Docker & Kubernetes support
* Cloud-native architecture
* High scalability

---

## 4. Explain async/await in detail

`async/await` enables **asynchronous programming**, preventing thread blocking.

### Without async:

Threads are blocked → Poor scalability

### With async:

Thread released → Better performance

### Example:

```csharp
public async Task<List<User>> GetUsersAsync()
{
   return await db.Users.ToListAsync();
}
```

### How it works internally?

* Uses Task-based asynchronous pattern (TAP)
* State machine handles execution flow

### Deadlock scenario:

Using `.Result` or `.Wait()` in UI or main thread.

**Avoid:**

```csharp
var result = task.Result;
```

**Use:**

```csharp
await task;
```

---

## 5. Explain Garbage Collection (GC)

GC manages memory automatically by freeing unused objects.

### Generations:

* **Gen0:** Short-lived objects
* **Gen1:** Medium lifespan
* **Gen2:** Long-lived objects

### Working:

1. Object created in Gen0
2. If survives → moved to Gen1
3. Long-living → Gen2

### Why GC is efficient?

Most objects die young → Gen0 cleaned frequently → Fast cleanup

---

# 2️⃣ ASP.NET Core Web API

---

## 6. How to design scalable REST APIs?

### Best Practices:

* Stateless APIs
* Proper HTTP verbs (GET, POST, PUT, DELETE)
* DTO pattern
* Pagination
* Caching
* Versioning
* Async calls

### Architecture:

Client → API Gateway → Microservices → Database

---

## 7. Explain JWT Authentication Flow in detail

### Flow:

1. Client sends login request
2. Server validates credentials
3. Server generates JWT token
4. Client stores token
5. Client sends token in Authorization header

### Token Structure:

Header + Payload + Signature

### Benefits:

* Stateless
* Secure
* Scalable

---

## 8. How to handle Global Exception Handling?

Using **Exception Middleware**:

```csharp
app.UseExceptionHandler(errorApp =>
{
   errorApp.Run(async context =>
   {
       context.Response.StatusCode = 500;
       await context.Response.WriteAsync("Unexpected error occurred");
   });
});
```

### Benefits:

* Centralized error handling
* Consistent API responses
* Better logging

---

# 3️⃣ Entity Framework Core & SQL Server

---

## 9. Explain Lazy, Eager and Explicit Loading

| Type     | Description         | Performance |
| -------- | ------------------- | ----------- |
| Lazy     | Loads when accessed | Poor        |
| Eager    | Loads upfront       | Best        |
| Explicit | Manually triggered  | Moderate    |

### Example:

```csharp
_context.Users.Include(u => u.Orders);
```

**Best practice:** Use Eager loading for APIs.

---

## 10. How do you optimize SQL Server performance?

### Techniques:

* Proper indexing
* Query optimization
* Execution plan analysis
* Avoid SELECT *
* Use stored procedures
* Caching

---

## 11. Clustered vs Non-clustered Index

| Clustered        | Non-clustered    |
| ---------------- | ---------------- |
| Physical order   | Logical order    |
| One per table    | Multiple allowed |
| Faster retrieval | Moderate         |

---

# 4️⃣ Angular

---

## 12. Explain Angular Architecture

Angular uses **component-based architecture**.

### Core Blocks:

* Modules
* Components
* Services
* Dependency Injection
* RxJS Observables

### Data Flow:

Component → Service → API → Response → UI

---

## 13. Observable vs Promise

| Observable      | Promise         |
| --------------- | --------------- |
| Multiple values | Single value    |
| Cancelable      | Not cancelable  |
| Lazy execution  | Eager execution |

**Angular heavily uses Observables via RxJS.**

---

## 14. What are HTTP Interceptors?

Used to **intercept all HTTP requests & responses**.

### Use Cases:

* Add JWT token
* Logging
* Error handling

---

# 5️⃣ System Design (Banking Focus)

---

## 15. Design a Banking Transaction Processing System

### Architecture:

Client → API Gateway → Auth Service → Transaction Service → Database
          → Kafka → Audit Logs

### Key Requirements:

* High availability
* ACID compliance
* Security
* Scalability
* Audit logging

---

## 16. Design Loan Processing System

### Flow:

User → Loan Service → Credit Check → Approval Engine → Disbursement → Notification

### Architecture:

Microservices + Event-driven workflow

---

# 6️⃣ CI/CD, Git & Cloud

---

## 17. Explain CI/CD Pipeline Flow

### Typical Flow:

1. Git Push
2. Build
3. Unit Tests
4. Code Quality Scan
5. Docker Build
6. Deployment

---

## 18. Git Branching Strategy

* main → production
* develop → integration
* feature → development
* hotfix → production fix

---

# 7️⃣ Unit Testing & Code Quality

---

## 19. How to write Unit Tests?

Frameworks:

* xUnit
* NUnit
* MSTest

Mocking:

* Moq

```csharp
var mock = new Mock<IUserService>();
```

---

## 20. Unit vs Integration vs E2E Testing

| Type        | Scope        |
| ----------- | ------------ |
| Unit        | Method       |
| Integration | API + DB     |
| E2E         | UI → Backend |

---

# 8️⃣ Agile & Team Leadership

---

## 21. How do you mentor junior developers?

* Code reviews
* Pair programming
* Architecture walkthroughs
* Knowledge sessions

---

## 22. How do you estimate sprint stories?

Using **Planning Poker + Story Points** based on:

* Complexity
* Risk
* Effort

---

# 9️⃣ Scenario-Based Troubleshooting

---

## 23. API slow in production — troubleshooting steps

* Check logs
* APM tools
* SQL profiling
* Caching
* Horizontal scaling

---

## 24. Angular UI slow — optimization

* Lazy loading
* OnPush change detection
* TrackBy
* Bundle optimization

---

## 25. SQL query slow — resolution steps

* Execution plan
* Index tuning
* Query rewriting
* Partitioning

---

# ⭐ How To Use This Repository

* Daily revision
* Interview refresh
* Architecture practice
* Teaching juniors

---

# 🏆 Recommended Next Additions

* System design diagrams
* Real coding problems
* Microservices architecture samples
* Angular enterprise architecture

---
---

# 💻 Real Coding Interview Problems with Solutions

**For Senior .NET Full Stack Developers**

---

# Table of Contents

1. C# Logical Problems
2. Data Structures & Algorithms (C#)
3. .NET Core API Coding Problems
4. SQL Query Problems
5. Angular / TypeScript Coding Problems
6. Real-world Enterprise Coding Scenarios

---

# 1️⃣ C# Logical Problems

---

## 1. Reverse a String Without Using Built-in Functions

### Problem:

Reverse a string **without using built-in reverse methods**.

### Solution:

```csharp
public static string ReverseString(string input)
{
    char[] arr = input.ToCharArray();
    int left = 0, right = arr.Length - 1;

    while (left < right)
    {
        (arr[left], arr[right]) = (arr[right], arr[left]);
        left++;
        right--;
    }

    return new string(arr);
}
```

### Time Complexity:

**O(n)**

---

## 2. Find Second Largest Number in an Array

### Solution:

```csharp
public static int SecondLargest(int[] arr)
{
    int first = int.MinValue, second = int.MinValue;

    foreach (int num in arr)
    {
        if (num > first)
        {
            second = first;
            first = num;
        }
        else if (num > second && num != first)
        {
            second = num;
        }
    }

    return second;
}
```

---

## 3. Check if String is Palindrome

```csharp
public static bool IsPalindrome(string input)
{
    int left = 0, right = input.Length - 1;

    while (left < right)
    {
        if (input[left] != input[right])
            return false;

        left++;
        right--;
    }

    return true;
}
```

---

# 2️⃣ Data Structures & Algorithms (C#)

---

## 4. Find First Non-Repeating Character

```csharp
public static char FirstUniqueChar(string input)
{
    var map = new Dictionary<char, int>();

    foreach (char c in input)
        map[c] = map.ContainsKey(c) ? map[c] + 1 : 1;

    foreach (char c in input)
        if (map[c] == 1) return c;

    return '_';
}
```

---

## 5. Two Sum Problem (Most Asked)

### Problem:

Find indices of two numbers that add up to target.

```csharp
public static int[] TwoSum(int[] nums, int target)
{
    var map = new Dictionary<int, int>();

    for (int i = 0; i < nums.Length; i++)
    {
        int diff = target - nums[i];

        if (map.ContainsKey(diff))
            return new[] { map[diff], i };

        map[nums[i]] = i;
    }

    return Array.Empty<int>();
}
```

**Time Complexity:** O(n)

---

## 6. Binary Search Implementation

```csharp
public static int BinarySearch(int[] arr, int target)
{
    int low = 0, high = arr.Length - 1;

    while (low <= high)
    {
        int mid = low + (high - low) / 2;

        if (arr[mid] == target) return mid;
        if (arr[mid] < target) low = mid + 1;
        else high = mid - 1;
    }

    return -1;
}
```

---

# 3️⃣ .NET Core API Coding Problems

---

## 7. Create REST API for CRUD Operations (Entity + Controller)

### Model:

```csharp
public class Employee
{
    public int Id { get; set; }
    public string Name { get; set; }
    public decimal Salary { get; set; }
}
```

### Controller:

```csharp
[ApiController]
[Route("api/[controller]")]
public class EmployeesController : ControllerBase
{
    private readonly AppDbContext _context;

    public EmployeesController(AppDbContext context)
    {
        _context = context;
    }

    [HttpGet]
    public async Task<IActionResult> Get()
    {
        return Ok(await _context.Employees.ToListAsync());
    }

    [HttpPost]
    public async Task<IActionResult> Post(Employee emp)
    {
        _context.Employees.Add(emp);
        await _context.SaveChangesAsync();
        return Ok(emp);
    }
}
```

---

## 8. Global Exception Middleware

```csharp
public class ExceptionMiddleware
{
    private readonly RequestDelegate _next;

    public ExceptionMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task Invoke(HttpContext context)
    {
        try
        {
            await _next(context);
        }
        catch (Exception ex)
        {
            context.Response.StatusCode = 500;
            await context.Response.WriteAsync(ex.Message);
        }
    }
}
```

---

# 4️⃣ SQL Query Problems (Very Important)

---

## 9. Find Second Highest Salary

```sql
SELECT MAX(Salary)
FROM Employees
WHERE Salary < (SELECT MAX(Salary) FROM Employees);
```

---

## 10. Remove Duplicate Records

```sql
WITH CTE AS (
   SELECT *, ROW_NUMBER() OVER(PARTITION BY Email ORDER BY Id) rn
   FROM Users
)
DELETE FROM CTE WHERE rn > 1;
```

---

## 11. Find Employees Without Managers

```sql
SELECT *
FROM Employees e
WHERE NOT EXISTS (
   SELECT 1 FROM Employees m WHERE e.ManagerId = m.Id
);
```

---

# 5️⃣ Angular / TypeScript Coding Problems

---

## 12. Debounce Search Input (VERY COMMON)

```typescript
this.searchControl.valueChanges
  .pipe(debounceTime(300), distinctUntilChanged())
  .subscribe(value => {
    this.search(value);
  });
```

---

## 13. Route Guard Example

```typescript
@Injectable({ providedIn: 'root' })
export class AuthGuard implements CanActivate {
  
  canActivate(): boolean {
    return !!localStorage.getItem('token');
  }
}
```

---

## 14. HTTP Interceptor (JWT Token)

```typescript
intercept(req: HttpRequest<any>, next: HttpHandler) {
  const token = localStorage.getItem('token');

  if (token) {
    req = req.clone({
      setHeaders: { Authorization: `Bearer ${token}` }
    });
  }

  return next.handle(req);
}
```

---

# 6️⃣ Real Enterprise-Level Coding Scenarios

---

## 15. Rate Limiting Middleware (.NET Core)

```csharp
public class RateLimitMiddleware
{
    private static Dictionary<string, DateTime> cache = new();

    public async Task Invoke(HttpContext context, RequestDelegate next)
    {
        var ip = context.Connection.RemoteIpAddress.ToString();

        if (cache.ContainsKey(ip) && DateTime.Now < cache[ip])
        {
            context.Response.StatusCode = 429;
            await context.Response.WriteAsync("Too many requests");
            return;
        }

        cache[ip] = DateTime.Now.AddSeconds(5);
        await next(context);
    }
}
```

---

## 16. Caching in .NET Core

```csharp
public async Task<List<Product>> GetProducts()
{
    return await _cache.GetOrCreateAsync("products", async entry =>
    {
        entry.AbsoluteExpirationRelativeToNow = TimeSpan.FromMinutes(5);
        return await _context.Products.ToListAsync();
    });
}
```

---

# ⭐ Must Practice Before Interview

| Category      | Priority |
| ------------- | -------- |
| C# Logic      | ⭐⭐⭐⭐     |
| REST API      | ⭐⭐⭐⭐⭐    |
| SQL Queries   | ⭐⭐⭐⭐⭐    |
| Angular       | ⭐⭐⭐⭐     |
| System Design | ⭐⭐⭐⭐⭐    |

---

# 🏆 How To Use This Repo

* Daily coding practice
* Interview revision
* Teaching juniors
* System design prep

---

# 🔥 Next Level Additions (Optional)

If you want, I can also prepare:

✅ **Advanced System Design Coding Problems**
✅ **Multithreading + Async Coding Problems**
✅ **Microservices Design + Implementation**
✅ **Azure Cloud Coding Scenarios**

---
---

# 🧵 Multithreading + Async Programming – Coding Interview Problems

**For Senior .NET Developers**

---

# Table of Contents

1. Thread vs Task vs Async
2. Parallel Programming
3. Synchronization & Thread Safety
4. Deadlocks & Race Conditions
5. Producer–Consumer Problems
6. High-Performance Async APIs
7. Real Enterprise Scenarios

---

# 1️⃣ Thread vs Task vs Async (Concept + Coding)

---

## 1. Difference between Thread, Task, and async/await

| Feature     | Thread | Task        | async/await |
| ----------- | ------ | ----------- | ----------- |
| Abstraction | Low    | High        | Highest     |
| Resource    | Heavy  | Lightweight | Efficient   |
| Pooling     | No     | Yes         | Yes         |
| Scalability | Poor   | Good        | Excellent   |

### Best Practice:

Use **Task + async/await**, not raw threads.

---

## 2. Create Multi-threaded Program Using Task

### Problem:

Run 3 background jobs in parallel.

```csharp
public static async Task RunTasksAsync()
{
    Task t1 = Task.Run(() => Print("Task1"));
    Task t2 = Task.Run(() => Print("Task2"));
    Task t3 = Task.Run(() => Print("Task3"));

    await Task.WhenAll(t1, t2, t3);
}

static void Print(string name)
{
    Console.WriteLine($"{name} running on Thread {Thread.CurrentThread.ManagedThreadId}");
}
```

---

# 2️⃣ Parallel Programming

---

## 3. Parallel.For vs foreach

### Problem:

Process 1 million records efficiently.

```csharp
Parallel.For(0, 1_000_000, i =>
{
    Process(i);
});
```

### Why Parallel.For?

* Uses thread pool
* Efficient CPU utilization
* Automatic load balancing

---

## 4. Parallel LINQ (PLINQ)

```csharp
var result = numbers
    .AsParallel()
    .Where(n => n % 2 == 0)
    .ToList();
```

---

# 3️⃣ Synchronization & Thread Safety

---

## 5. Race Condition Example

```csharp
int counter = 0;

Parallel.For(0, 10000, i =>
{
    counter++;   // ❌ Not thread safe
});
```

### Fix using lock:

```csharp
int counter = 0;
object locker = new();

Parallel.For(0, 10000, i =>
{
    lock (locker)
    {
        counter++;
    }
});
```

---

## 6. Thread-Safe Counter using Interlocked

```csharp
int counter = 0;

Parallel.For(0, 10000, i =>
{
    Interlocked.Increment(ref counter);
});
```

---

## 7. SemaphoreSlim Example (Concurrency Control)

### Problem:

Allow only **3 concurrent requests**

```csharp
SemaphoreSlim semaphore = new(3);

async Task ProcessAsync()
{
    await semaphore.WaitAsync();
    try
    {
        await Task.Delay(2000);
    }
    finally
    {
        semaphore.Release();
    }
}
```

---

# 4️⃣ Deadlocks & Race Conditions

---

## 8. Deadlock Scenario

```csharp
lock(obj1)
{
    lock(obj2) { }
}

lock(obj2)
{
    lock(obj1) { }
}
```

### Solution:

**Always lock in same order.**

---

## 9. Deadlock in async/await

### Problem:

```csharp
var data = GetDataAsync().Result; // ❌
```

### Correct:

```csharp
var data = await GetDataAsync();  // ✅
```

---

# 5️⃣ Producer–Consumer Problem (Very Important)

---

## 10. Producer Consumer using BlockingCollection

```csharp
BlockingCollection<int> queue = new(5);

Task producer = Task.Run(() =>
{
    for (int i = 1; i <= 10; i++)
    {
        queue.Add(i);
        Console.WriteLine($"Produced {i}");
    }
    queue.CompleteAdding();
});

Task consumer = Task.Run(() =>
{
    foreach (var item in queue.GetConsumingEnumerable())
    {
        Console.WriteLine($"Consumed {item}");
    }
});

await Task.WhenAll(producer, consumer);
```

---

# 6️⃣ High Performance Async APIs

---

## 11. Async API Call Pattern (Real World)

```csharp
public async Task<List<User>> GetUsersAsync()
{
    var users = await _context.Users
                              .AsNoTracking()
                              .ToListAsync();
    return users;
}
```

### Best Practices:

* Always use async DB calls
* Avoid blocking threads
* Use `AsNoTracking()` for read-only

---

## 12. Parallel API Aggregator (VERY COMMON)

### Problem:

Call **multiple microservices in parallel**

```csharp
public async Task<ApiResult> GetDashboardAsync()
{
    var ordersTask = GetOrdersAsync();
    var usersTask = GetUsersAsync();
    var paymentTask = GetPaymentsAsync();

    await Task.WhenAll(ordersTask, usersTask, paymentTask);

    return new ApiResult
    {
        Orders = ordersTask.Result,
        Users = usersTask.Result,
        Payments = paymentTask.Result
    };
}
```

---

# 7️⃣ Real Enterprise Multithreading Scenarios

---

## 13. Background Worker using Hosted Service

```csharp
public class Worker : BackgroundService
{
    protected override async Task ExecuteAsync(CancellationToken token)
    {
        while (!token.IsCancellationRequested)
        {
            await ProcessQueue();
            await Task.Delay(5000, token);
        }
    }
}
```

---

## 14. Thread-Safe In-Memory Cache

```csharp
private static readonly ConcurrentDictionary<int, string> cache = new();

public string GetData(int id)
{
    return cache.GetOrAdd(id, key => LoadFromDb(key));
}
```

---

# 8️⃣ Common Interview Tricky Questions

---

## 15. Task.WhenAll vs Task.WhenAny

| WhenAll             | WhenAny          |
| ------------------- | ---------------- |
| Waits for all tasks | Waits for first  |
| Batch processing    | Fastest response |

---

## 16. async void vs async Task

| async void          | async Task        |
| ------------------- | ----------------- |
| Fire & forget       | Awaitable         |
| Error not catchable | Exception handled |

**Never use async void except event handlers.**

---

# 9️⃣ Advanced Coding Challenges (Senior Level)

---

## 17. Rate Limited API Executor

```csharp
SemaphoreSlim limiter = new(5);

async Task CallApi()
{
    await limiter.WaitAsync();
    try
    {
        await Task.Delay(1000);
    }
    finally
    {
        limiter.Release();
    }
}
```

---

## 18. Parallel File Processing

```csharp
Parallel.ForEach(files, file =>
{
    ProcessFile(file);
});
```

---

# 🔥 Must Practice Before Interview

| Topic          | Priority |
| -------------- | -------- |
| async / await  | ⭐⭐⭐⭐⭐    |
| Task.WhenAll   | ⭐⭐⭐⭐⭐    |
| Parallel loops | ⭐⭐⭐⭐     |
| Deadlocks      | ⭐⭐⭐⭐⭐    |
| Thread safety  | ⭐⭐⭐⭐⭐    |

---

# 🏆 Interview Tips (VERY IMPORTANT)

* Prefer **Task + async/await**
* Avoid raw threads
* Avoid blocking calls
* Use **parallelism carefully**
* Focus on **scalability + performance**

---

# 🚀 Next Level Prep Additions (Optional)

If you want, I can also provide:

✅ **Advanced System Design Coding Problems**
✅ **Distributed Locking Techniques (Redis)**
✅ **Event-driven async architecture patterns**
✅ **High-performance microservices design**

---



