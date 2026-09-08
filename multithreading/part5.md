# 🚀 JAVA MULTITHREADING – PART 5 (Lock-Free Concurrency & Atomic Classes)

---

# 1️⃣ What is Lock-Free Concurrency?

Traditional approach:
→ synchronized / Lock  
→ Thread blocks  
→ Context switching

Problem:
Blocking reduces performance.

Solution:
Non-blocking concurrency  
Using CAS (Compare-And-Swap)

---

# 2️⃣ What is CAS?

CAS = Compare And Swap

Atomic CPU instruction.

Steps:

1. Compare current value with expected value
2. If equal → update
3. If not equal → retry

No locking required.

---

# 3️⃣ Why CAS is Faster?

✔ No blocking  
✔ No thread suspension  
✔ No context switching  
✔ Works at CPU level  

Used internally by:
- AtomicInteger
- AtomicLong
- AtomicReference
- ConcurrentHashMap

---

# 4️⃣ AtomicInteger Example

Without Atomic:

```java
int count = 0;
count++;
```

Not thread-safe ❌

---

Using AtomicInteger:

```java
import java.util.concurrent.atomic.AtomicInteger;

class Counter {

    AtomicInteger count = new AtomicInteger(0);

    public void increment() {
        count.incrementAndGet();
    }
}
```

✔ Thread-safe  
✔ Lock-free  

---

# 5️⃣ Common Atomic Methods

```java
incrementAndGet()
getAndIncrement()
compareAndSet(expected, newValue)
get()
set()
```

---

## 🔹 compareAndSet Example

```java
AtomicInteger ai = new AtomicInteger(10);

boolean updated = ai.compareAndSet(10, 20);

System.out.println(updated);  // true
System.out.println(ai.get()); // 20
```

If expected value mismatch → update fails.

---

# 6️⃣ CAS Retry Concept

Pseudo logic:

```
do {
   oldValue = value;
   newValue = oldValue + 1;
} while(!CAS(oldValue, newValue));
```

If another thread changes value → retry.

---

# 7️⃣ AtomicReference

Used for objects.

```java
AtomicReference<String> ref = new AtomicReference<>("Hello");

ref.compareAndSet("Hello", "World");
```

---

# 8️⃣ AtomicStampedReference (Solves ABA Problem)

---

## 🔹 What is ABA Problem?

Thread 1 reads value A  
Thread 2 changes A → B → A  
Thread 1 sees A again  
Thinks no change happened ❌

This is ABA problem.

Solution:
Use versioning (stamp).

---

# 9️⃣ AtomicStampedReference Example

```java
AtomicStampedReference<Integer> ref =
        new AtomicStampedReference<>(10, 1);
```

Stamp changes with value.

---

# 🔟 LongAdder (Better than AtomicLong)

AtomicLong uses single variable → contention high.

LongAdder:
Uses multiple cells → high performance under heavy load.

```java
LongAdder adder = new LongAdder();
adder.increment();
```

Best for:
High concurrency counters.

---

# 1️⃣1️⃣ Volatile Keyword

Ensures:

✔ Visibility  
✔ Latest value read  

Example:

```java
volatile boolean flag = true;
```

Guarantees:
Thread always reads latest value.

---

## 🔹 Volatile Limitations

❌ Not atomic  
❌ Does not prevent race condition  
✔ Only ensures visibility  

---

# 1️⃣2️⃣ Thread-Safe Collections

Old:
Vector, Hashtable (synchronized)

Modern:
- ConcurrentHashMap
- CopyOnWriteArrayList
- ConcurrentLinkedQueue

---

## 🔹 ConcurrentHashMap Example

```java
ConcurrentHashMap<String, Integer> map =
        new ConcurrentHashMap<>();

map.put("A", 1);
```

✔ Segment locking  
✔ Better performance than Hashtable  

---

# 1️⃣3️⃣ CopyOnWriteArrayList

Creates new copy on modification.

Best for:
Read-heavy systems.

---

# 1️⃣4️⃣ Lock-Free vs Lock-Based

| Lock-Based | Lock-Free |
|------------|-----------|
| Uses synchronized | Uses CAS |
| Blocking | Non-blocking |
| Context switching | No suspension |
| Slower | Faster |
| Deadlock possible | No deadlock |

---

# 1️⃣5️⃣ When to Use Atomic?

✔ Simple counters  
✔ Flags  
✔ Compare-update operations  
✔ High performance systems  

---

# 🎯 Interview Questions (Part 5)

1. What is CAS?
2. What is ABA problem?
3. Difference between volatile and atomic?
4. AtomicInteger vs synchronized?
5. What is LongAdder?
6. Why ConcurrentHashMap better than Hashtable?
7. Is volatile thread-safe?
8. Can volatile prevent race condition?

---

# 🔥 Key Takeaways

✔ CAS = Compare-And-Swap  
✔ Atomic classes are lock-free  
✔ Volatile ensures visibility  
✔ ABA problem solved by stamped reference  
✔ LongAdder better for high concurrency  
✔ Concurrent collections improve scalability  

---

🔥 END OF PART 5
