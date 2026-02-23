# 🔐 Distributed Locking Techniques Using Redis

**Senior Software Engineer – System Design & Concurrency Guide**

---

# 📌 Why Distributed Locking is Required?

In **distributed systems & microservices**, multiple services run on **different machines / containers**.

If multiple instances try to **modify the same shared resource**, it can cause:

* Data inconsistency
* Race conditions
* Duplicate processing
* Financial losses (in banking systems)

👉 **Distributed Lock ensures only ONE service instance can access a critical section at a time.**

---

# 📌 Real Enterprise Scenarios

| Scenario                    | Why Lock Needed                 |
| --------------------------- | ------------------------------- |
| Bank transaction processing | Prevent double debit            |
| Payment gateways            | Avoid double payment            |
| Order processing            | Prevent duplicate orders        |
| Inventory management        | Prevent overselling             |
| Job schedulers              | Prevent duplicate job execution |

---

# 📌 What is Redis Distributed Lock?

Redis distributed locking uses **atomic operations** to create **mutual exclusion across multiple services**.

Redis provides:

* Extremely fast operations
* Atomic commands
* TTL-based expiration
* High availability

---

# 📌 Basic Redis Lock Pattern

### Lock Key Structure:

```
lock:<resource-id>
```

Example:

```
lock:account:12345
```

---

# 📌 Redis Lock Algorithm (Basic)

### Step 1: Acquire Lock

```
SET lock:key unique-value NX PX 30000
```

| Option | Meaning                        |
| ------ | ------------------------------ |
| NX     | Only set if key does NOT exist |
| PX     | Set expiration in milliseconds |

---

### Step 2: Perform Business Operation

Only if lock acquired.

---

### Step 3: Release Lock

```
DEL lock:key
```

---

# 📌 Redis Lock in .NET Core (Production-Grade Implementation)

### Install Package

```bash
dotnet add package StackExchange.Redis
```

---

## 🔹 Redis Distributed Lock Service

```csharp
public class RedisDistributedLock
{
    private readonly IDatabase _redisDb;

    public RedisDistributedLock(IConnectionMultiplexer redis)
    {
        _redisDb = redis.GetDatabase();
    }

    public async Task<bool> AcquireLockAsync(string key, string value, TimeSpan expiry)
    {
        return await _redisDb.StringSetAsync(
            key,
            value,
            expiry,
            when: When.NotExists
        );
    }

    public async Task<bool> ReleaseLockAsync(string key, string value)
    {
        var script = @"
            if redis.call('get', KEYS[1]) == ARGV[1]
            then
                return redis.call('del', KEYS[1])
            else
                return 0
            end";

        return (int)await _redisDb.ScriptEvaluateAsync(
            script,
            new RedisKey[] { key },
            new RedisValue[] { value }
        ) == 1;
    }
}
```

---

## 🔹 Usage Example – Banking Transaction API

```csharp
public async Task<bool> ProcessTransaction(string accountId, decimal amount)
{
    string lockKey = $"lock:account:{accountId}";
    string lockValue = Guid.NewGuid().ToString();

    if (!await _lock.AcquireLockAsync(lockKey, lockValue, TimeSpan.FromSeconds(30)))
        return false;

    try
    {
        await PerformDebit(accountId, amount);
        return true;
    }
    finally
    {
        await _lock.ReleaseLockAsync(lockKey, lockValue);
    }
}
```

---

# 📌 Why Unique Lock Value Is Important?

Without unique value:

❌ One service could **accidentally release another service’s lock**.

Using **GUID value ensures only owner can release lock.**

---

# 📌 Handling Lock Expiry & Failures

### Why expiry needed?

If service crashes, lock **must auto-expire**, otherwise **deadlock forever**.

---

# 📌 RedLock Algorithm (Redis Cluster – Enterprise Grade)

**RedLock** is Redis’s official distributed locking algorithm.

### Why RedLock?

* Multiple Redis nodes
* Fault tolerance
* No single point of failure

---

## 🔹 RedLock Algorithm Steps

1. Acquire lock in **majority of Redis nodes**
2. If majority success → lock granted
3. If fail → release all acquired locks

---

## 🔹 RedLock Implementation Using RedLock.net

### Install:

```bash
dotnet add package RedLock.net
dotnet add package RedLock.net.SERedis
```

---

### Configuration:

```csharp
var redisEndpoints = new List<RedLockMultiplexer>
{
    new(ConnectionMultiplexer.Connect("localhost:6379")),
};

var redlockFactory = RedLockFactory.Create(redisEndpoints);
```

---

### Acquire Lock:

```csharp
using (var redLock = await redlockFactory.CreateLockAsync(
    "lock:payment:123",
    TimeSpan.FromSeconds(30)))
{
    if (redLock.IsAcquired)
    {
        await ProcessPayment();
    }
}
```

---

# 📌 Redis Lock vs DB Lock vs In-Memory Lock

| Type       | Scope          | Performance | Use Case       |
| ---------- | -------------- | ----------- | -------------- |
| lock(obj)  | Single process | Very fast   | Local apps     |
| DB Lock    | Multi server   | Slow        | Legacy systems |
| Redis Lock | Distributed    | Ultra-fast  | Microservices  |

---

# 📌 Common Pitfalls (Interview Favorite Questions)

---

## ❌ What happens if service crashes after acquiring lock?

**Answer:** TTL automatically releases lock.

---

## ❌ What if lock expires before work completes?

**Solution:**

* Use **lock renewal / heartbeat mechanism**
* Extend TTL periodically

---

## ❌ Can Redis lock guarantee 100% safety?

**Answer:**
No system guarantees 100%. RedLock gives **best tradeoff between safety and performance**.

---

# 📌 Advanced Pattern – Lock Renewal (Heartbeat)

```csharp
public async Task RenewLockAsync(string key, TimeSpan expiry)
{
    await _redisDb.KeyExpireAsync(key, expiry);
}
```

Used for **long-running tasks**.

---

# 📌 Real Enterprise Use Case – Payment Gateway Flow

```
Client → API Gateway → Payment Service
                       |
                       ↓
               Redis Distributed Lock
                       |
                       ↓
                    Database
```

Ensures **no double payment**.

---

# 📌 Interview Questions Based on Redis Distributed Locking

---

### Q1. Why do we need distributed locking?

To ensure **data consistency & prevent race conditions** in distributed environments.

---

### Q2. How does Redis locking work?

Using **atomic SET NX PX** operations.

---

### Q3. What is RedLock?

Redis’s **fault-tolerant distributed locking algorithm**.

---

### Q4. Redis lock vs database lock?

Redis is **faster, scalable, cloud-native**.

---

### Q5. What happens if Redis crashes?

Use **Redis Cluster + RedLock algorithm**.

---

# 📌 Senior Level Design Insight (Very Important)

Distributed locking is a **critical system design skill** for:

* Banking systems
* Payment gateways
* Trading platforms
* Booking engines
* Inventory systems

💡 **This topic alone can decide system design interview result.**

---



