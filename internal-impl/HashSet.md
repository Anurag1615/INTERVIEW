# HashSet Internal Working in Java

`HashSet` is a `Set` implementation that stores **unique elements** with **no guaranteed order**.

---

## 🔹 Internal Structure

`HashSet` is internally backed by a `HashMap`:

```java
public class HashSet<E> {
    private transient HashMap<E, Object> map;

    private static final Object PRESENT = new Object();

    public HashSet() {
        map = new HashMap<>();
    }

    public boolean add(E e) {
        return map.put(e, PRESENT) == null;
    }
}
```

👉 Every element added to a `HashSet` becomes a **key** in the internal `HashMap`,  
with a dummy constant object (`PRESENT`) as the value.

So `HashSet` uniqueness relies entirely on **`HashMap`'s key uniqueness**, which in turn relies on `hashCode()` and `equals()`.

---

## 🔹 How Uniqueness Is Enforced

```java
HashSet<String> set = new HashSet<>();
set.add("apple");
set.add("banana");
set.add("apple"); // duplicate → ignored

System.out.println(set); // [apple, banana] (order not guaranteed)
```

Internally: `map.put("apple", PRESENT)` returns the old value if the key already exists.  
`add()` returns `false` if `map.put()` returned a non-null value (i.e., key existed already).

---

## 🔹 Why `equals()` and `hashCode()` Matter

For custom objects, `HashSet` (via `HashMap`) uses:
1. `hashCode()` → to find the bucket.
2. `equals()` → to check for actual duplicates within that bucket.

```java
class Employee {
    int id;
    Employee(int id) { this.id = id; }
    // No equals()/hashCode() override
}

Set<Employee> set = new HashSet<>();
set.add(new Employee(1));
set.add(new Employee(1)); // ❌ treated as a DIFFERENT object (added, not rejected)

System.out.println(set.size()); // 2 (bug — should ideally be 1 if id defines equality)
```

If `equals()`/`hashCode()` aren't overridden, `HashSet` falls back to reference-based comparison — leading to unexpected duplicates.

---

## 🔹 Time Complexity

| Operation | Average Case | Worst Case |
|-----------|--------------|------------|
| `add()` | O(1) | O(n) (all elements hash to same bucket) |
| `contains()` | O(1) | O(n) |
| `remove()` | O(1) | O(n) |

(Same as `HashMap`, since `HashSet` delegates to it internally.)

---

## 🔹 HashSet vs LinkedHashSet vs TreeSet

| Aspect | HashSet | LinkedHashSet | TreeSet |
|--------|---------|---------------|---------|
| Ordering | No guaranteed order | Insertion order preserved | Sorted order |
| Backing structure | `HashMap` | `LinkedHashMap` | `TreeMap` (Red-Black tree) |
| Null elements | One `null` allowed | One `null` allowed | Not allowed (throws `NullPointerException`) |
| Performance | O(1) avg | O(1) avg (slightly slower due to linked list) | O(log n) |

---

## 🔥 Interview Questions

1. How does `HashSet` internally guarantee uniqueness?
2. What role does `PRESENT` play internally in `HashSet`?
3. What happens if you don't override `equals()`/`hashCode()` for objects stored in a `HashSet`?
4. What is the time complexity of `add()`/`contains()` in `HashSet`?
5. What's the difference between `HashSet` and `LinkedHashSet`?

---

## 🎯 Summary

✔ `HashSet` = `HashMap` internally, elements stored as keys  
✔ Uniqueness relies on `hashCode()` + `equals()`  
✔ O(1) average time complexity for add/remove/contains  
✔ No guaranteed iteration order (use `LinkedHashSet`/`TreeSet` if order matters)
