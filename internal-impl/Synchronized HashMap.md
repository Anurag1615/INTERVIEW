# Synchronized HashMap in Java

A **Synchronized HashMap** is a `HashMap` wrapped with synchronization so that **only one thread can access it at a time**.

Normally:

```
HashMap → Not Thread Safe
```

To make it thread-safe, Java provides a wrapper method:

```
Collections.synchronizedMap()
```

This method converts a normal `HashMap` into a **synchronized (thread-safe) map**.

---

# Example Code

```java
import java.util.*;

public class Example {

    public static void main(String[] args) {

        Map<Integer,String> map =
                Collections.synchronizedMap(new HashMap<>());

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

# How Synchronization Works

When we create a synchronized map:

```java
Map map = Collections.synchronizedMap(new HashMap<>());
```

Java wraps the HashMap inside a **synchronized wrapper class**.

Internally:

```
Every method is synchronized
```

Example:

```java
public synchronized V put(K key, V value)
```

This means:

```
Only one thread can execute the method at a time
```

---

# Internal Structure

```
SynchronizedMap
      |
      ↓
   HashMap
      |
Bucket Array
      |
Linked List / Tree
```

So the structure remains:

```
HashMap internals
```

But **all operations are synchronized**.

---

# Example with Multiple Threads

```java
Map<Integer,String> map =
        Collections.synchronizedMap(new HashMap<>());

Thread t1 = new Thread(() -> {
    map.put(1,"Java");
});

Thread t2 = new Thread(() -> {
    map.put(2,"Python");
});

t1.start();
t2.start();
```

Only **one thread can modify the map at a time**, preventing data corruption.

---

# Important Note About Iteration

When iterating over a synchronized map, **manual synchronization is required**.

Example:

```java
Map<Integer,String> map =
        Collections.synchronizedMap(new HashMap<>());

synchronized (map) {

    for(Map.Entry<Integer,String> entry : map.entrySet()){
        System.out.println(entry.getKey());
    }

}
```

Reason:

```
Iteration is not automatically synchronized
```

---

# Time Complexity

Same as HashMap:

| Operation | Complexity |
|------|------|
put() | O(1) |
get() | O(1) |
remove() | O(1) |

But slower due to **locking overhead**.

---

# HashMap vs Synchronized HashMap

| Feature | HashMap | Synchronized HashMap |
|------|------|------|
Thread Safe | ❌ No | ✅ Yes |
Locking | None | Whole map locked |
Performance | Fast | Slower |
Null Key | Allowed | Allowed |
Null Value | Allowed | Allowed |

---

# Synchronized HashMap vs ConcurrentHashMap

| Feature | Synchronized HashMap | ConcurrentHashMap |
|------|------|------|
Thread Safety | Yes | Yes |
Locking | Entire map locked | Segment / bucket locking |
Performance | Slower | Faster |
Concurrency | Low | High |

---

# When to Use Synchronized HashMap

Use when:

```
Simple thread safety is required
Small multi-threaded applications
Legacy code
```

But in modern applications prefer:

```
ConcurrentHashMap
```

because it provides **better performance and scalability**.

---

# Quick Summary

```
HashMap → Fast but not thread-safe
Synchronized HashMap → Thread-safe but slow
ConcurrentHashMap → Thread-safe and scalable
```

---

# Interview One-Line Definition

> A Synchronized HashMap is a HashMap wrapped using `Collections.synchronizedMap()` where every method is synchronized to make it thread-safe.
