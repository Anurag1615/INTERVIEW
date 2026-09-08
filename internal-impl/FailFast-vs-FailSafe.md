# Fail-Fast vs Fail-Safe Iterators

---

## 🔹 Fail-Fast Iterators

## 🔹 Definition

Fail-fast iterators throw `ConcurrentModificationException` **immediately** if the underlying collection is **structurally modified** after the iterator is created (except through the iterator's own `remove()`/`add()` methods).

Examples: `ArrayList`, `HashMap`, `HashSet`, `Vector` iterators.

### How It Works — `modCount`

- Collections maintain an internal counter: `modCount`.
- Every structural modification (`add`, `remove`) increments `modCount`.
- The iterator captures `expectedModCount` when created.
- On every `next()` call, it checks: `if (modCount != expectedModCount) throw new ConcurrentModificationException();`

### Example

```java
List<Integer> list = new ArrayList<>(List.of(10, 20, 30));

for (Integer val : list) {
    if (val == 20) {
        list.remove(val); // modifies list directly, NOT via iterator
    }
}
// ❌ Throws ConcurrentModificationException
```

---

## 🔹 Fail-Safe Iterators

## 🔹 Definition

Fail-safe iterators do **NOT** throw `ConcurrentModificationException` because they operate on a **separate copy (or snapshot)** of the collection, instead of the original.

Examples: `CopyOnWriteArrayList`, `ConcurrentHashMap` iterators.

### Example

```java
CopyOnWriteArrayList<Integer> list = new CopyOnWriteArrayList<>(List.of(10, 20, 30));

for (Integer val : list) {
    if (val == 20) {
        list.remove(val); // ✔ safe — no exception
    }
}
System.out.println(list); // [10, 30]
```

### How It Works

- `CopyOnWriteArrayList` creates a **new copy of the underlying array** on every write (`add`/`remove`).
- The iterator was created over the *old* array snapshot, so it's unaffected by concurrent modifications.
- `ConcurrentHashMap` uses segmented/bucket-level locking and weakly-consistent iterators that reflect some (not necessarily all) concurrent changes without throwing exceptions.

---

## 🔹 Comparison Table

| Aspect | Fail-Fast | Fail-Safe |
|--------|-----------|-----------|
| Throws `ConcurrentModificationException` | ✔ Yes | ❌ No |
| Works on | Original collection | Copy/snapshot of collection |
| Memory overhead | Low | Higher (extra copies created) |
| Data freshness | Always reflects latest state (until exception) | May not reflect the most recent modifications |
| Examples | `ArrayList`, `HashMap`, `HashSet` | `CopyOnWriteArrayList`, `ConcurrentHashMap` |

---

## 🔹 Why Does Java Design It This Way?

- Fail-fast: prioritizes **correctness feedback** — better to fail loudly than silently produce wrong results from an inconsistent view.
- Fail-safe: prioritizes **availability in concurrent environments** — worth the memory/staleness tradeoff to avoid crashing under concurrent access.

---

## 🔥 Interview Questions

1. What causes `ConcurrentModificationException`?
2. How does `modCount` help detect concurrent structural modification?
3. Why doesn't `CopyOnWriteArrayList` throw `ConcurrentModificationException`?
4. Is `ConcurrentHashMap`'s iterator fail-safe? What tradeoff does that bring?
5. How would you safely remove elements from an `ArrayList` while iterating?

---

## 🎯 Summary

✔ Fail-fast → throws `ConcurrentModificationException`, operates directly on collection (`ArrayList`, `HashMap`)  
✔ Fail-safe → operates on a copy/snapshot, no exception (`CopyOnWriteArrayList`, `ConcurrentHashMap`)  
✔ Trade-off: correctness/fail-loud vs availability/fail-safe
