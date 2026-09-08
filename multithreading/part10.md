# 🚀 PART 10 – Volatile Keyword (Easy + Interview Ready)

---

# 1️⃣ What is volatile?

## 🔹 Definition

The volatile keyword is used to ensure visibility of changes to a variable across multiple threads.

It tells JVM:

👉 "Always read this variable from main memory, not from thread cache."

---

# 2️⃣ Why Do We Need volatile?

## 🔹 The Problem (Without volatile)

Each thread has:

- Its own CPU cache
- A working copy of variables

If one thread updates a variable,
other threads may NOT see the updated value.

This causes visibility issues.

---

# 3️⃣ Example Without volatile

```java
class Test {
    static boolean flag = true;

    public static void main(String[] args) {

        Thread t = new Thread(() -> {
            while (flag) {
                // loop
            }
            System.out.println("Stopped");
        });

        t.start();

        try { Thread.sleep(1000); } catch (Exception e) {}

        flag = false;
        System.out.println("Flag changed to false");
    }
}
```

❗ Problem:

Thread may never stop.

Because:

Thread may read flag from its local cache,
not from main memory.

---

# 4️⃣ Fix Using volatile

```java
static volatile boolean flag = true;
```

Now:

When one thread changes flag,
other threads immediately see the updated value.

---

# 5️⃣ What volatile Guarantees

✔ Visibility  
✔ Prevents instruction reordering  

It does NOT guarantee:

❌ Atomicity  

---

# 6️⃣ Important: volatile is NOT Atomic

Example:

```java
volatile int count = 0;

count++;
```

Still unsafe!

Because:

count++ =

1. Read
2. Increment
3. Write

Multiple threads can still corrupt value.

---

# 7️⃣ When to Use volatile?

✔ For status flags  
✔ For shutdown signals  
✔ For simple state indicators  
✔ When only one thread writes  

---

# 8️⃣ When NOT to Use volatile?

❌ For counters  
❌ For multiple dependent operations  
❌ For complex critical sections  

Use instead:

✔ synchronized  
✔ Lock  
✔ AtomicInteger  

---

# 9️⃣ volatile vs synchronized

| volatile | synchronized |
|----------|--------------|
| Only visibility | Visibility + Atomicity |
| No locking | Uses lock |
| Faster | Slower than volatile |
| No mutual exclusion | Mutual exclusion |

---

# 🔟 Real-World Use Case

Server shutdown flag:

```java
volatile boolean running = true;

while (running) {
    processRequests();
}
```

Another thread:

```java
running = false;
```

Thread stops immediately.

---

# 🎯 Interview Questions

1. What problem does volatile solve?
2. Is volatile thread-safe?
3. Does volatile guarantee atomicity?
4. Difference between volatile and synchronized?
5. How does volatile work internally?
6. What is visibility problem?

---

# 🧠 Internal Concept (Advanced)

JVM ensures:

- Write to volatile → goes to main memory
- Read of volatile → always from main memory

It also adds memory barriers to prevent reordering.

---

# 🔥 Final Summary

✔ volatile ensures visibility  
✔ Prevents reordering  
❌ Does NOT ensure atomicity  
✔ Good for flags  
✔ Not good for counters  

---

🔥 END OF PART 10
