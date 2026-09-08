# 🚀 JAVA MULTITHREADING – PART 7  
# (Future, Callable & CompletableFuture)

---

# 1️⃣ Future  
📄 (Reference: Page 8)

## 🔹 Definition

Future is an interface that represents the result of an asynchronous computation.

It allows us to:

✔ Check if task is completed  
✔ Get the result  
✔ Cancel the task  
✔ Handle exception  

---

## 🔹 Example

```java
ExecutorService pool = Executors.newFixedThreadPool(2);

Future<String> future = pool.submit(() -> {
    Thread.sleep(2000);
    return "Task Completed";
});

System.out.println(future.isDone());  // false

String result = future.get();  // blocks

System.out.println(result);
```

---

## 🔹 Important Methods

### get()

- Waits if necessary for computation to complete
- Returns result
- Throws ExecutionException if exception occurred

---

### get(timeout, unit)

Waits for given time.

Throws:
- TimeoutException
- ExecutionException
- InterruptedException

---

### cancel(boolean mayInterruptIfRunning)

- Attempts to cancel execution
- Returns false if task cannot be cancelled

---

### isDone()

Returns true if task completed.

---

### isCancelled()

Returns true if task was cancelled.

---

# 2️⃣ Problem with Future

❌ Blocking (get() blocks)  
❌ No chaining  
❌ No non-blocking callback  
❌ Cannot combine multiple futures easily  

Solution → CompletableFuture

---

# 3️⃣ Callable  
📄 (Reference: Page 10)

Callable represents task that returns result.

Difference from Runnable:

| Runnable | Callable |
|-----------|----------|
| No return value | Returns value |
| Cannot throw checked exception | Can throw exception |

---

## Example

```java
Callable<Integer> task = () -> {
    return 10 + 20;
};

Future<Integer> future = pool.submit(task);

System.out.println(future.get());
```

---

# 4️⃣ CompletableFuture  
📄 (Reference: Page 10+)

Introduced in Java 8.

Advanced version of Future.

Supports:

✔ Non-blocking  
✔ Chaining  
✔ Combining tasks  
✔ Exception handling  
✔ Async programming  

---

# 5️⃣ supplyAsync()  
📄 (Reference: Page 11)

Used to initiate async task.

```java
CompletableFuture<String> cf =
    CompletableFuture.supplyAsync(() -> {
        return "Hello";
    });
```

✔ Executes Supplier asynchronously  
✔ By default uses ForkJoinPool  
✔ Custom executor can be passed  

---

# 6️⃣ thenApply()  
📄 (Reference: Page 11,13)

Applies function to result of previous stage.

```java
cf.thenApply(result -> result + " World");
```

✔ Synchronous execution  
✔ Uses same thread  

---

# 7️⃣ thenApplyAsync()

```java
cf.thenApplyAsync(result -> result + " Async");
```

✔ Asynchronous execution  
✔ Uses different thread  

If multiple thenApplyAsync → order not guaranteed.

---

# 8️⃣ thenAccept()

Used when you don’t want to return anything.

```java
cf.thenAccept(result -> {
    System.out.println(result);
});
```

✔ Terminal stage  
✔ Returns void  

---

# 9️⃣ thenCombine()  
📄 (Reference: Page 12)

Used to combine two independent futures.

```java
CompletableFuture<Integer> f1 =
    CompletableFuture.supplyAsync(() -> 10);

CompletableFuture<Integer> f2 =
    CompletableFuture.supplyAsync(() -> 20);

CompletableFuture<Integer> combined =
    f1.thenCombine(f2, (a, b) -> a + b);
```

---

# 🔟 thenCompose()  
📄 (Reference: Page 13)

Used to chain dependent async operations.

When next async depends on previous result.

```java
cf.thenCompose(result ->
    CompletableFuture.supplyAsync(() -> result + " Chained")
);
```

---

# 1️⃣1️⃣ Exception Handling

### exceptionally()

```java
cf.exceptionally(ex -> {
    return "Fallback value";
});
```

---

### handle()

```java
cf.handle((result, ex) -> {
    if (ex != null) return "Error";
    return result;
});
```

---

# 1️⃣2️⃣ Complete vs CompleteExceptionally

```java
cf.complete("Manually completed");
cf.completeExceptionally(new RuntimeException());
```

---

# 1️⃣3️⃣ Execution Flow (Conceptual)

Without CompletableFuture:

Task1 → wait → Task2 → wait → Task3

With CompletableFuture:

Task1 → thenApply → thenCompose → thenCombine

Non-blocking chain

---

# 🎯 Interview Questions (Part 7)

1. Difference between Runnable and Callable?
2. Why CompletableFuture better than Future?
3. thenApply vs thenApplyAsync?
4. thenCompose vs thenCombine?
5. How to handle exceptions in CompletableFuture?
6. Does supplyAsync use new thread?
7. Default thread pool used by CompletableFuture?
8. What happens if exception thrown inside async task?

---

# 🔥 Final Summary

✔ Future represents async result  
✔ Callable returns value  
✔ Future.get() blocks  
✔ CompletableFuture supports chaining  
✔ thenApply = sync  
✔ thenApplyAsync = async  
✔ thenCompose = dependent tasks  
✔ thenCombine = independent tasks  

---

🔥 END OF PART 7
