# ConcurrentHashMap in Java

`ConcurrentHashMap` is a **thread-safe implementation of Map** designed for **high concurrency**.

It allows **multiple threads to read and write simultaneously** without locking the entire map.

Unlike `Hashtable`, it uses **fine-grained locking**, which improves performance in multi-threaded environments.

---

# Why ConcurrentHashMap?

Problem with `HashMap`:

```
Not thread-safe
Multiple threads may corrupt data
```

Problem with `Hashtable`:

```
Thread-safe but locks the whole map
Poor performance in multi-threaded applications
```

Solution:

```
ConcurrentHashMap
```

It allows:

```
Multiple reads simultaneously
Multiple writes with minimal locking
```

---

# Example Code

```java
import java.util.concurrent.ConcurrentHashMap;

public class Example {

    public static void main(String[] args) {

        ConcurrentHashMap<Integer,String> map = new ConcurrentHashMap<>();

        map.put(1,"Java");
        map.put(2,"Python");
        map.put(3,"Go");

        System.out.println(map.get(2));
    }
}
```

Output

```
Python
```

---

# Internal Data Structure

Before Java 8:

```
Segmented Locking
```

Structure:

```
ConcurrentHashMap
      |
   Segments
      |
   HashTables
```

Each segment had its own lock.

Example:

```
Segment 1 → bucket array
Segment 2 → bucket array
Segment 3 → bucket array
Segment 4 → bucket array
```

Only the **segment being modified is locked**.

---

# After Java 8 (Modern Implementation)

Java 8 removed segments and uses:

```
Array + Linked List + Red Black Tree
```

Very similar to HashMap.

But it uses:

```
CAS (Compare And Swap)
+
Fine-grained synchronization
```

Structure:

```
Bucket Array

Index  Node
---------------------
0      null
1      Node → Node
2      TreeNode
3      Node
...
```

---

# Node Structure

Simplified node class:

```java
class Node<K,V> {
    int hash;
    K key;
    V value;
    Node<K,V> next;
}
```

Same concept as HashMap but with **thread-safe updates**.

---

# How put() Works

Example:

```java
map.put("apple",10);
```

### Step 1: Calculate Hash

```
hash = key.hashCode()
```

---

### Step 2: Calculate Bucket Index

```
index = hash & (n - 1)
```

Example:

```
hash = 118
capacity = 16

index = 118 & 15
index = 6
```

---

### Step 3: Check Bucket

Cases:

```
1️⃣ Bucket empty
2️⃣ Bucket contains nodes
3️⃣ Bucket contains tree
```

---

### Case 1: Bucket Empty

CAS operation inserts node directly.

```
Bucket[6] → [apple,10]
```

CAS means:

```
Compare And Swap
```

It ensures **atomic insertion without locking**.

---

### Case 2: Bucket Already Has Node

Collision occurs.

Nodes are stored as linked list.

```
Bucket[6]

[apple,10] → [orange,20]
```

---

### Case 3: Too Many Nodes

If nodes exceed **8**, the linked list becomes a **Red-Black Tree**.

```
Linked List → TreeNode
```

This improves lookup time.

---

# How get() Works

Example:

```java
map.get("apple")
```

Steps:

```
1️⃣ Calculate hash
2️⃣ Find bucket index
3️⃣ Traverse linked list or tree
4️⃣ Compare keys using equals()
5️⃣ Return value
```

Reads **do not require locking**, so performance is high.

---

# Important Features

```
Thread-safe
High concurrency
No full-table locking
Better performance than Hashtable
```

---

# Null Handling

Unlike HashMap:

```
ConcurrentHashMap does NOT allow null keys or null values
```

Example:

```java
ConcurrentHashMap<String,String> map = new ConcurrentHashMap<>();

map.put(null,"A");  // ❌ Exception
```

Reason:

```
Avoid ambiguity in concurrent operations
```

---

# Time Complexity

| Operation | Complexity |
|------|------|
put() | O(1) |
get() | O(1) |
collision worst case | O(log n) |

---

# HashMap vs Hashtable vs ConcurrentHashMap

| Feature | HashMap | Hashtable | ConcurrentHashMap |
|------|------|------|------|
Thread Safe | ❌ No | ✅ Yes | ✅ Yes |
Locking | None | Whole map | Fine-grained |
Performance | Fast | Slow | Very fast |
Null Keys | Allowed | Not allowed | Not allowed |
Null Values | Allowed | Not allowed | Not allowed |

---

# Performance Behavior

```
HashMap → fastest but unsafe for threads
Hashtable → thread-safe but slow
ConcurrentHashMap → thread-safe + high performance
```

---

# Real World Use Cases

ConcurrentHashMap is used in:

```
Multi-threaded caching systems
High performance servers
Concurrent data processing
Microservices
Thread-safe counters
```

Example:

```
Web servers
Distributed systems
Parallel processing
```

---

# Quick Summary

```
HashMap → Fast but not thread-safe
Hashtable → Thread-safe but slow
ConcurrentHashMap → Thread-safe and scalable
```

---

# Interview One-Line Definition

> ConcurrentHashMap is a thread-safe Map implementation that allows concurrent read and write operations using fine-grained locking and CAS operations.
