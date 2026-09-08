# Hashtable in Java

`Hashtable` is one of the **oldest Map implementations in Java** (introduced in Java 1.0).  
It stores data in **key-value pairs** just like `HashMap`.

However, the major difference is:

```
Hashtable is thread-safe (synchronized)
```

Because of synchronization, multiple threads can safely access it.

---

# Basic Example

```java
import java.util.Hashtable;

public class Example {

    public static void main(String[] args) {

        Hashtable<Integer,String> table = new Hashtable<>();

        table.put(1,"Java");
        table.put(2,"Python");
        table.put(3,"Go");

        System.out.println(table.get(2));
    }
}
```

Output

```
Python
```

---

# Internal Working of Hashtable

Internally `Hashtable` works **very similar to HashMap**.

It uses:

```
Array of Buckets
+
Linked List for collision handling
```

Node structure (simplified):

```java
class Entry<K,V> {
    int hash;
    K key;
    V value;
    Entry<K,V> next;
}
```

Fields:

| Field | Description |
|------|-------------|
| hash | Hash value of key |
| key | Key object |
| value | Stored value |
| next | Next node in case of collision |

---

# How Insertion Works

Example:

```java
table.put("apple",10);
```

### Step 1: Calculate Hash

```
hash = key.hashCode()
```

---

### Step 2: Find Bucket Index

```
index = hash % capacity
```

Example:

```
hash = 118
capacity = 16

index = 118 % 16
index = 6
```

---

### Step 3: Insert Node

Node is created:

```
Entry {
 hash = 118
 key = "apple"
 value = 10
 next = null
}
```

---

### Step 4: Handle Collision

If another key maps to the same bucket:

```
Bucket[6]

[apple,10] → [orange,20]
```

Nodes are stored using a **linked list**.

---

# Important Characteristics of Hashtable

```
Thread-safe (synchronized)
No null keys allowed
No null values allowed
Slower than HashMap
Legacy class
```

Example:

```java
Hashtable<String,String> table = new Hashtable<>();

table.put(null,"A"); // ❌ Exception
```

Error:

```
NullPointerException
```

---

# Why Hashtable Is Rarely Used Today

Modern Java applications prefer:

```
HashMap
ConcurrentHashMap
```

Because:

```
Hashtable synchronization is inefficient
Entire table is locked
```

Better alternatives exist.

---

# HashMap vs Hashtable

| Feature | HashMap | Hashtable |
|------|------|------|
Thread Safety | ❌ Not synchronized | ✅ Synchronized |
Performance | Faster | Slower |
Null Key | Allowed (one) | Not allowed |
Null Value | Allowed | Not allowed |
Introduced | Java 1.2 | Java 1.0 |
Locking | No locking | Whole table locked |
Usage | Modern applications | Legacy code |

---

# Example Comparison

### HashMap

```java
HashMap<String,Integer> map = new HashMap<>();

map.put(null,10);   // allowed
map.put("A",null);  // allowed
```

---

### Hashtable

```java
Hashtable<String,Integer> table = new Hashtable<>();

table.put(null,10);   // ❌ Exception
table.put("A",null);  // ❌ Exception
```

---

# Performance Comparison

| Operation | HashMap | Hashtable |
|------|------|------|
put() | O(1) | O(1) but slower |
get() | O(1) | O(1) but slower |

Reason:

```
Hashtable uses synchronization
```

---

# When to Use Hashtable

Rare cases:

```
Legacy systems
Old Java libraries
Simple thread-safe map requirement
```

Better alternative today:

```
ConcurrentHashMap
```

---

# Quick Summary

```
HashMap → Fast, not thread-safe
LinkedHashMap → Maintains insertion order
TreeMap → Maintains sorted order
Hashtable → Thread-safe but legacy
```

---

# Interview One-Line Definition

> Hashtable is a legacy synchronized Map implementation that stores key-value pairs using hashing but does not allow null keys or values.
