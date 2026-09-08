# HashMap Internal Working in Java

`HashMap` is one of the most commonly used data structures in Java.  
It stores data in **key–value pairs** and provides **fast operations** like:

```
Insertion → O(1)
Deletion  → O(1)
Lookup    → O(1)
```

This performance is possible because HashMap uses **Hashing** internally.

---

# Basic Structure of HashMap

Internally, a `HashMap` is implemented as an **array of nodes**.

Each index of the array is called a **bucket**.

```java
class Node<K, V> {
    int hash;
    K key;
    V value;
    Node<K, V> next;
}
```

Each `Node` contains:

| Field | Description |
|------|-------------|
| hash | Hash value of the key |
| key | The key object |
| value | Associated value |
| next | Reference to next node (used during collision) |

---

# Internal Structure Diagram

```
HashMap Table (Array)

Index → Bucket

0  →  null
1  →  Node(key,value)
2  →  null
3  →  Node(key,value)
4  →  null
5  →  null
6  →  Node(key,value) → Node(key,value)
7  →  null
8  →  null
...
15 →  null
```

Default capacity:

```
16 buckets
```

---

# What is Hashing?

Hashing is a technique that converts a **key into an integer index** so the data can be stored quickly.

Instead of searching the whole array, we directly jump to the **correct bucket**.

Example:

```
key → hashCode() → bucket index
```

---

# Hashing Process

When inserting a key-value pair:

```
Step 1 → key.hashCode()
Step 2 → apply hash function
Step 3 → calculate bucket index
Step 4 → store in that bucket
```

---

# Index Calculation Formula

```
index = hash & (n - 1)
```

Where:

```
n = number of buckets (capacity)
```

Example:

```
hash = 118
n = 16

index = 118 & (16-1)
index = 118 & 15
index = 6
```

So the data goes into **bucket 6**.

---

# Why `(n-1)`?

Because HashMap capacity is always **power of 2**.

```
16 = 2^4
32 = 2^5
64 = 2^6
```

Using bitwise AND is **faster than modulo (%)**.

Instead of:

```
hash % n
```

Java uses:

```
hash & (n-1)
```

which is faster.

---

# Example Code

```java
import java.util.HashMap;

public class Example {

    public static void main(String[] args) {

        HashMap<String, Integer> map = new HashMap<>();

        map.put("apple", 3);
        map.put("banana", 5);
        map.put("orange", 2);

        System.out.println(map.get("banana"));
    }
}
```

Output

```
5
```

---

# Default HashMap Initialization

```java
HashMap map = new HashMap();
```

Default values:

```
Initial Capacity = 16
Load Factor = 0.75
```

Meaning:

```
Resize happens when 75% buckets are filled
```

---

# Internal Working of put()

Example:

```java
map.put("vishal", 20);
```

---

## Step 1: Calculate Hash

```
hashCode("vishal") = 118
```

---

## Step 2: Calculate Index

```
index = hash & (n - 1)

118 & 15 = 6
```

So the element goes to:

```
bucket 6
```

---

## Step 3: Create Node

```
Node {
    hash = 118
    key = "vishal"
    value = 20
    next = null
}
```

Since bucket 6 is empty, the node is stored there.

---

# Insert Second Element

```
map.put("sachin", 30)
```

Hash calculation:

```
hashCode("sachin") = 115
index = 115 & 15 = 3
```

Stored at bucket **3**.

---

# Collision Example

```
map.put("vaibhav", 40)
```

Suppose:

```
hashCode("vaibhav") = 118
```

Index calculation:

```
118 & 15 = 6
```

Bucket **6 already contains "vishal"**

This situation is called **Collision**.

---

# What is Collision?

Collision occurs when **two keys map to the same bucket index**.

Example:

```
Key1 → index 6
Key2 → index 6
```

Both keys must be stored in the same bucket.

---

# Collision Handling

### Before Java 8

Collision handled using **Linked List**

```
Bucket 6

[vishal,20] → [vaibhav,40]
```

---

### Since Java 8

If bucket size exceeds **8 nodes**, it converts to **Balanced Tree (Red Black Tree)**.

```
Linked List → Red Black Tree
```

Time complexity improves:

| Structure | Time Complexity |
|----------|----------------|
Linked List | O(n) |
Tree | O(log n) |

---

# Internal Working of get()

Example:

```java
map.get("banana")
```

---

## Step 1: Calculate Hash

```
hashCode("banana") = 115
```

---

## Step 2: Calculate Index

```
index = 115 & 15
index = 3
```

---

## Step 3: Go to Bucket

```
bucket[3]
```

---

## Step 4: Compare Keys

HashMap checks:

```
key.equals(existingKey)
```

If match found:

```
return value
```

---

# Visual Representation

```
HashMap Table

Index   Data
-------------------------------
0       null
1       null
2       null
3       [sachin,30]
4       null
5       null
6       [vishal,20] → [vaibhav,40]
7       null
...
15      null
```

---

# Important Interview Points

### 1️⃣ HashMap Default Capacity

```
16
```

---

### 2️⃣ Load Factor

```
0.75
```

Resize occurs when:

```
size > capacity × loadFactor
```

Example:

```
16 × 0.75 = 12
```

When 13th element inserted → **resize to 32**

---

### 3️⃣ Time Complexity

| Operation | Complexity |
|----------|------------|
put() | O(1) |
get() | O(1) |
collision worst case | O(n) |
tree case | O(log n) |

---

# HashMap Summary

```
HashMap = Array + LinkedList + RedBlackTree
```

Uses:

```
hashCode()
equals()
bitwise hashing
collision handling
dynamic resizing
```

---

# Quick Interview Recap

```
HashMap Internal Working

Key → hashCode()
      ↓
hash function
      ↓
index calculation
      ↓
bucket selection
      ↓
store node
```

If collision occurs:

```
LinkedList (Java 7)
Tree (Java 8+ if nodes > 8)
```

---

# Final One-Line Definition

> HashMap stores key-value pairs using hashing to provide **constant time complexity for insertion and lookup**.
