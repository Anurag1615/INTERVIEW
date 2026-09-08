# 🚀 THREAD POOL (As Per Notes)

---

# 1️⃣ Thread Pool – Definition  
📄 (Reference: Page 1)

Thread Pool is a collection of threads which are available to perform submitted tasks.

Once a task is completed:
- Worker thread goes back to the thread pool
- Waits for a new task to be assigned

👉 Meaning: Threads can be reused.

---

## ✅ Advantages

- Thread creation time can be saved
- Overhead of managing thread lifecycle can be removed
- Increases performance

---

# 2️⃣ Executor Framework Structure  
📄 (Reference: Page 2 Diagram)

Executor (Interface)  
⬇  
ExecutorService (Interface)  
⬇  
Implementations:

- ThreadPoolExecutor
- ForkJoinPool
- ScheduledThreadPoolExecutor

---

# 3️⃣ ThreadPoolExecutor Constructor

📄 (Reference: Page 2)

```
ThreadPoolExecutor(
   int corePoolSize,
   int maxPoolSize,
   long keepAliveTime,
   TimeUnit unit,
   BlockingQueue<Runnable> workQueue,
   ThreadFactory threadFactory,
   RejectedExecutionHandler handler
)
```

It helps to create customized ThreadPool.

---

# 4️⃣ Core Pool Size  
📄 (Reference: Page 3)

- Number of threads initially created
- Kept in pool even if idle

---

# 5️⃣ allowCoreThreadTimeOut

📄 (Reference: Page 3)

If property set to TRUE (Default FALSE):

- Idle core threads will terminate
- Based on keepAliveTime

---

# 6️⃣ Keep Alive Time  
📄 (Reference: Page 3)

Time after which idle thread gets terminated.

---

# 7️⃣ Maximum Pool Size  
📄 (Reference: Page 3)

Maximum number of threads allowed in pool.

If:
- No. of threads == corePoolSize
- Queue is full
- Then new threads created (till maxPoolSize)

---

# 8️⃣ TimeUnit

📄 (Reference: Page 3)

Defines unit for keepAliveTime:
- Milliseconds
- Seconds
- Hours
etc.

---

# 9️⃣ BlockingQueue  
📄 (Reference: Page 4)

Used to hold tasks before worker thread picks them.

## Types:

### 1️⃣ Bounded Queue
- Fixed size capacity
- Example: ArrayBlockingQueue

### 2️⃣ Unbounded Queue
- No fixed capacity
- Example: LinkedBlockingQueue

---

# 🔟 ThreadFactory  
📄 (Reference: Page 4)

Factory for creating new threads.

ThreadPoolExecutor uses it to create new threads.

Used to:
- Give custom thread name
- Set priority
- Set daemon flag

---

# 1️⃣1️⃣ RejectedExecutionHandler  
📄 (Reference: Page 4)

Handles tasks that cannot be accepted by thread pool.

Generally logging logic can be added here.

---

# 1️⃣2️⃣ Rejection Policies  
📄 (Reference: Page 5)

### 1️⃣ AbortPolicy (Default)
- Throws RejectedExecutionException

### 2️⃣ CallerRunsPolicy
- Executes rejected task in caller thread

### 3️⃣ DiscardPolicy
- Silently discards rejected task

### 4️⃣ DiscardOldestPolicy
- Discards oldest task in queue
- Adds new task

---

# 1️⃣3️⃣ Lifecycle of ThreadPoolExecutor  
📄 (Reference: Page 6 Diagram)

States:

Running  
⬇  
Shutdown  
⬇  
Stop  
⬇  
Terminated  

---

## 🔹 Running
- Accepts new tasks
- Executes submitted tasks

## 🔹 Shutdown
- Does not accept new tasks
- Completes existing tasks

## 🔹 Stop (shutdownNow)
- Does not accept new tasks
- Attempts to stop executing tasks

## 🔹 Terminated
- All tasks completed
- Executor fully terminated

---

# 1️⃣4️⃣ Future  
📄 (Reference: Page 8)

Future represents result of async task.

It allows:

- Check if computation completed
- Get result
- Handle exception

---

## Methods:

- get()
- isDone()
- cancel()
- isCancelled()

---

# 1️⃣5️⃣ Callable  
📄 (Reference: Page 10)

Represents task like Runnable.

Difference:

Runnable → No return value  
Callable → Can return value  

---

# 1️⃣6️⃣ CompletableFuture  
📄 (Reference: Page 10+)

Introduced in Java 8.

Helps in async programming.

Advanced version of Future.

Provides chaining capability.

---

## supplyAsync()

📄 (Reference: Page 11)

- Executes Supplier asynchronously
- By default uses ForkJoinPool
- Custom executor can be passed

---

## thenApply vs thenApplyAsync  
📄 (Reference: Page 13)

thenApply:
- Synchronous
- Uses same thread

thenApplyAsync:
- Asynchronous
- Uses different thread

If multiple thenApplyAsync:
- Order not guaranteed
- Run concurrently

---

## thenCompose

📄 (Reference: Page 13)

Used to chain dependent async operations.

---

## thenCombine  
📄 (Reference: Page 12)

Used to combine two CompletableFuture results.

---

# 1️⃣7️⃣ ScheduledThreadPoolExecutor  
📄 (Reference: Page 14)

Used to schedule tasks.

---

## Methods:

### 1️⃣ schedule(Runnable, delay, TimeUnit)
- Runs task once after delay

### 2️⃣ schedule(Callable, delay, TimeUnit)

### 3️⃣ scheduleAtFixedRate(Runnable, initialDelay, period, unit)
- Runs task repeatedly
- Can cancel using cancel()

---

# 🔥 Final Interview Summary

✔ ThreadPool reuses threads  
✔ corePoolSize vs maxPoolSize difference  
✔ BlockingQueue stores tasks  
✔ Rejection policy important  
✔ Future vs Callable difference  
✔ CompletableFuture supports chaining  
✔ ScheduledThreadPoolExecutor used for scheduling  
