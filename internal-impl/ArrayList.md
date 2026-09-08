# ArrayList Internal Working in Java

`ArrayList` is one of the most commonly used `List` implementations in Java.  
It stores elements in a **dynamically resizable array**.

---

## 🔹 Internal Structure

Internally, `ArrayList` maintains:

```java
transient Object[] elementData;
private int size;
```

- `elementData` → backing array that actually stores the elements.
- `size` → number of elements currently added (not the array's capacity).

---

## 🔹 Default Capacity

```java
ArrayList<Integer> list = new ArrayList<>();
```

- Default initial capacity = **10** (lazily allocated on the first `add()` in modern JDKs, not at construction time).

---

## 🔹 How Resizing Works (Growth)

When the internal array becomes full and a new element is added:

1. A **new array** is created with **1.5x** the old capacity:
   ```java
   int newCapacity = oldCapacity + (oldCapacity >> 1);
   ```
2. All existing elements are copied (`Arrays.copyOf`) into the new array.
3. The new element is added.

```
Capacity: 10 → 15 → 22 → 33 → ...
```

👉 This resize operation is **O(n)**, but since it happens infrequently (amortized), the **average time complexity of `add()` is O(1) amortized**.

---

## 🔹 Time Complexity

| Operation | Time Complexity | Reason |
|-----------|------------------|--------|
| `get(index)` | O(1) | Direct array index access |
| `add(element)` (at end) | O(1) amortized | Occasional resize costs O(n) |
| `add(index, element)` | O(n) | Requires shifting elements right |
| `remove(index)` | O(n) | Requires shifting elements left |
| `contains()` / `indexOf()` | O(n) | Linear search |

---

## 🔹 Example

```java
List<Integer> list = new ArrayList<>();
list.add(10);
list.add(20);
list.add(0, 5);   // shifts 10, 20 to the right → O(n)
System.out.println(list); // [5, 10, 20]
```

---

## 🔹 ArrayList vs Array

| Array | ArrayList |
|-------|-----------|
| Fixed size | Dynamically resizable |
| Can hold primitives | Can only hold objects (autoboxes primitives) |
| Faster (no overhead) | Slight overhead due to resizing logic |
| `length` field | `size()` method |

---

## 🔹 ArrayList is NOT Thread-Safe

Multiple threads modifying an `ArrayList` concurrently can cause `ConcurrentModificationException` or data corruption.

Thread-safe alternatives:
- `Collections.synchronizedList(new ArrayList<>())`
- `CopyOnWriteArrayList` (from `java.util.concurrent`, ideal for read-heavy scenarios)

---

## 🔥 Interview Questions

1. How does `ArrayList` grow internally when it's full?
2. What is the time complexity of `add()`, `get()`, and `remove()`?
3. Why is `add()` considered O(1) even though resizing is O(n)?
4. How is `ArrayList` different from `LinkedList`?
5. Is `ArrayList` thread-safe? How do you make it thread-safe?

---

## 🎯 Summary

✔ Backed by a dynamically growing `Object[]` array  
✔ Grows by ~1.5x when full, copying all elements  
✔ O(1) random access, O(n) insertion/deletion in the middle  
✔ Not thread-safe by default
