# Garbage Collection (GC) in Java

---

## 🔹 Definition

Garbage Collection is the process by which the JVM **automatically reclaims memory** occupied by objects that are no longer reachable/referenced by the running program.

👉 Unlike C/C++, Java developers don't manually free memory (`free()`/`delete`).

---

## 🔹 When Is an Object Eligible for GC?

An object becomes eligible when it has **no live references** pointing to it.

```java
Employee e = new Employee();
e = null; // object is now unreachable → eligible for GC
```

Common ways objects become eligible:

1. Reference set to `null`.
2. Reference reassigned to another object.
3. Object created inside a method, method returns (local reference goes out of scope).
4. Island of Isolation — two objects reference each other but neither is reachable from outside.

```java
class Node {
    Node ref;
}

Node a = new Node();
Node b = new Node();
a.ref = b;
b.ref = a;

a = null;
b = null; 
// a and b reference each other, but nothing external references either
// → both become eligible for GC (JVM's GC can detect these "islands")
```

---

## 🔹 Generational Garbage Collection

The Heap is divided into generations based on the observation that **"most objects die young"**:

```
┌───────────────────────────────────────────┐
│                   Heap                      │
│  ┌───────────────┐   ┌───────────────────┐ │
│  │  Young Gen     │   │    Old Gen        │ │
│  │ Eden | S0 | S1 │   │  (Tenured)         │ │
│  └───────────────┘   └───────────────────┘ │
└───────────────────────────────────────────┘
         (Metaspace is separate, off-heap since Java 8)
```

### Young Generation

- **Eden Space**: new objects are created here first.
- **Survivor Spaces (S0, S1)**: objects that survive a Minor GC are moved here.
- Objects surviving multiple Minor GCs get **promoted** to Old Generation.

### Old Generation (Tenured)

- Holds long-lived objects.
- Collected via **Major GC / Full GC** (more expensive, causes longer pauses).

### Minor GC vs Major GC

| Aspect | Minor GC | Major GC (Full GC) |
|--------|----------|----------------------|
| Collects | Young Generation | Old Generation (and often the whole heap) |
| Frequency | Frequent | Less frequent |
| Pause time | Short | Longer (can cause "stop-the-world" pauses) |

---

## 🔹 GC Algorithms (Modern JVMs)

| Collector | Best For |
|-----------|----------|
| **Serial GC** | Small applications, single-threaded environments |
| **Parallel GC** | Throughput-focused, multi-core batch processing |
| **CMS (Concurrent Mark Sweep)** | Low pause time (deprecated since Java 9) |
| **G1 GC (Garbage First)** | Default since Java 9 — balances throughput and pause time, works on region-based heap |
| **ZGC / Shenandoah** | Ultra-low pause times (sub-millisecond), for very large heaps |

---

## 🔹 `finalize()` and GC

```java
class Test {
    @Override
    protected void finalize() {
        System.out.println("Cleaning up before destruction");
    }
}
```

- Called by GC just before destroying an object.
- **Not guaranteed** to run at a specific time (or at all).
- **Deprecated since Java 9** — prefer `try-with-resources` and `AutoCloseable`/`Cleaner` API instead.

---

## 🔹 Can We Force Garbage Collection?

```java
System.gc(); // only a REQUEST/hint to JVM, not a guarantee
```

The JVM decides when GC actually runs — `System.gc()` does not force immediate collection.

---

## 🔹 Strong, Weak, Soft, Phantom References

| Reference Type | GC Behavior |
|------------------|-------------|
| **Strong** (default, `Object o = new Object()`) | Never collected while reachable |
| **Soft** (`SoftReference`) | Collected only when JVM needs memory (good for memory-sensitive caches) |
| **Weak** (`WeakReference`) | Collected on next GC cycle regardless of memory pressure (used in `WeakHashMap`) |
| **Phantom** (`PhantomReference`) | Used for cleanup actions after object is finalized, before memory is reclaimed |

---

## 🔥 Interview Questions

1. When does an object become eligible for garbage collection?
2. What is the difference between Minor GC and Major/Full GC?
3. Why is the heap divided into generations?
4. Does calling `System.gc()` guarantee garbage collection?
5. What is G1 GC and why did it become the default collector?
6. What's the difference between Soft, Weak, and Phantom references?

---

## 🎯 Summary

✔ GC automatically reclaims memory of unreachable objects  
✔ Generational hypothesis: Young Gen (Eden + Survivor) → Old Gen (Tenured)  
✔ Minor GC (young gen, frequent) vs Major/Full GC (old gen, expensive)  
✔ G1 GC is the modern default; ZGC/Shenandoah for ultra-low pause needs
