# Parallel Streams (Java 8)

---

## 🔹 What Is a Parallel Stream?

A parallel stream splits the source data into multiple chunks and processes them **concurrently across multiple threads**, using the common `ForkJoinPool`.

```java
List<Integer> list = List.of(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

int sum = list.parallelStream()
              .mapToInt(Integer::intValue)
              .sum();

System.out.println(sum); // 55
```

Any sequential stream can be converted to parallel:

```java
list.stream().parallel();     // convert sequential → parallel
list.parallelStream();        // create directly as parallel
```

---

## 🔹 How It Works Internally

- Uses the **common `ForkJoinPool`** (shared across the whole JVM by default).
- The source is split using the **Spliterator** (splittable iterator) into sub-tasks.
- Each sub-task is processed independently, then results are combined (fork-join / divide-and-conquer model).

```
        [1,2,3,4,5,6,7,8]
             /      \
       [1,2,3,4]   [5,6,7,8]
        /    \        /    \
    [1,2]  [3,4]  [5,6]  [7,8]
       ↓ process in parallel on multiple threads ↓
             combine results
```

By default, the pool size = `Runtime.getRuntime().availableProcessors() - 1`.

---

## 🔹 When to Use Parallel Streams

✔ Good fit when:
- Dataset is **large** (small datasets have overhead that outweighs benefits).
- Operations are **CPU-intensive** and **stateless**.
- Order of processing doesn't matter.

❌ Avoid when:
- Dataset is small (thread coordination overhead > actual work).
- Operations have **side effects** or share mutable state (race conditions).
- Order matters (e.g., using `forEach` expecting sequential order).
- I/O-bound operations (parallel streams are meant for CPU-bound work, not blocking I/O — blocking calls can starve the shared `ForkJoinPool`).

---

## 🔹 Common Pitfall — Shared Mutable State

```java
List<Integer> results = new ArrayList<>(); // NOT thread-safe

list.parallelStream().forEach(results::add); // ❌ race condition, may lose elements or throw exceptions
```

### Correct Approach

```java
List<Integer> results = list.parallelStream()
                             .collect(Collectors.toList()); // thread-safe combining via Collector
```

---

## 🔹 Order Matters? Use `forEachOrdered`

```java
list.parallelStream().forEach(System.out::println);        // order NOT guaranteed
list.parallelStream().forEachOrdered(System.out::println); // preserves encounter order (but loses some parallel benefit)
```

---

## 🔹 Sequential vs Parallel Comparison

| Aspect | Sequential Stream | Parallel Stream |
|--------|---------------------|-------------------|
| Execution | Single thread | Multiple threads (ForkJoinPool) |
| Best for | Small datasets, I/O-bound, ordered operations | Large datasets, CPU-bound, stateless operations |
| Overhead | Minimal | Thread coordination overhead |
| Thread-safety concerns | None | Must avoid shared mutable state |

---

## 🔥 Interview Questions

1. How does a parallel stream split and process work internally?
2. Which thread pool do parallel streams use by default?
3. Why can using shared mutable state inside `parallelStream().forEach()` be dangerous?
4. When should you avoid using parallel streams?
5. What's the difference between `forEach()` and `forEachOrdered()` in a parallel stream?

---

## 🎯 Summary

✔ Parallel streams use the common `ForkJoinPool` to process elements concurrently  
✔ Best for large, CPU-bound, stateless operations  
✔ Avoid shared mutable state — use `collect()` instead of manually adding to a shared list  
✔ Not always faster — measure before using in performance-critical code
