# TreeSet Internal Working in Java

`TreeSet` is a `Set` implementation that stores **unique elements in sorted (ascending) order**.

---

## 🔹 Internal Structure

`TreeSet` is internally backed by a `TreeMap`:

```java
public class TreeSet<E> {
    private transient NavigableMap<E, Object> m;

    private static final Object PRESENT = new Object();

    public TreeSet() {
        m = new TreeMap<>();
    }

    public boolean add(E e) {
        return m.put(e, PRESENT) == null;
    }
}
```

Since `TreeMap` is implemented using a **Red-Black Tree** (self-balancing binary search tree),  
`TreeSet` automatically keeps its elements sorted at all times.

---

## 🔹 Sorting Behavior

By default, elements are sorted using their **natural ordering** (`Comparable`):

```java
TreeSet<Integer> set = new TreeSet<>();
set.add(30);
set.add(10);
set.add(20);
System.out.println(set); // [10, 20, 30] → always sorted
```

For custom objects, either:

1. The class implements `Comparable`, OR
2. A `Comparator` is passed to the `TreeSet` constructor.

```java
TreeSet<String> set = new TreeSet<>(Comparator.reverseOrder());
set.add("banana");
set.add("apple");
set.add("cherry");
System.out.println(set); // [cherry, banana, apple]
```

⚠ If elements are not `Comparable` and no `Comparator` is provided → `ClassCastException` at runtime on `add()`.

---

## 🔹 No `null` Elements

```java
TreeSet<String> set = new TreeSet<>();
set.add(null); // ❌ NullPointerException
```

Because `TreeSet` must compare elements to place them correctly, and comparing with `null` fails.

---

## 🔹 Time Complexity

| Operation | Time Complexity | Reason |
|-----------|------------------|--------|
| `add()` | O(log n) | Tree insertion with rebalancing |
| `remove()` | O(log n) | Tree deletion with rebalancing |
| `contains()` | O(log n) | Binary search through the tree |
| `first()` / `last()` | O(log n) | Leftmost/rightmost node lookup |

---

## 🔹 Useful Navigation Methods

```java
TreeSet<Integer> set = new TreeSet<>(List.of(10, 20, 30, 40));

set.first();          // 10
set.last();            // 40
set.higher(20);        // 30 (strictly greater)
set.lower(20);         // 10 (strictly less)
set.ceiling(25);       // 30 (>= 25)
set.floor(25);         // 20 (<= 25)
set.headSet(30);       // [10, 20] (elements < 30)
set.tailSet(20);       // [20, 30, 40] (elements >= 20)
```

---

## 🔹 HashSet vs TreeSet

| Aspect | HashSet | TreeSet |
|--------|---------|---------|
| Ordering | None | Sorted |
| Backing structure | `HashMap` | `TreeMap` (Red-Black Tree) |
| add/remove/contains | O(1) avg | O(log n) |
| Null allowed | Yes (one) | No |
| Requires `Comparable`/`Comparator` | No | Yes |

---

## 🔥 Interview Questions

1. How does `TreeSet` maintain sorted order internally?
2. What happens if you try to add a `null` to a `TreeSet`?
3. What happens if the elements don't implement `Comparable` and no `Comparator` is supplied?
4. What's the time complexity of `add()` in `TreeSet` vs `HashSet`, and why?
5. What are `ceiling()`, `floor()`, `higher()`, `lower()` used for?

---

## 🎯 Summary

✔ `TreeSet` = `TreeMap` internally (Red-Black Tree)  
✔ Always sorted (natural order or custom `Comparator`)  
✔ O(log n) for add/remove/contains  
✔ Does not allow `null` elements
