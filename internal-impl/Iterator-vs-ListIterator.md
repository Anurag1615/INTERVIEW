# Iterator vs ListIterator

---

## 🔹 Iterator

`Iterator` is a universal interface used to traverse **any** `Collection` (`List`, `Set`, `Queue`).

```java
public interface Iterator<E> {
    boolean hasNext();
    E next();
    void remove(); // optional operation
}
```

### Example

```java
List<Integer> list = new ArrayList<>(List.of(10, 20, 30));

Iterator<Integer> it = list.iterator();
while (it.hasNext()) {
    int val = it.next();
    if (val == 20) {
        it.remove(); // ✔ safe removal during iteration
    }
}
System.out.println(list); // [10, 30]
```

### Capabilities

- Traverse **forward only**.
- Can `remove()` elements safely during iteration.
- Cannot add elements.
- Cannot access elements by index.

---

## 🔹 ListIterator

`ListIterator` extends `Iterator` and is available **only for `List`** implementations (not `Set` or `Queue`).

```java
public interface ListIterator<E> extends Iterator<E> {
    boolean hasNext();
    E next();
    boolean hasPrevious();
    E previous();
    int nextIndex();
    int previousIndex();
    void remove();
    void set(E e);
    void add(E e);
}
```

### Example

```java
List<Integer> list = new ArrayList<>(List.of(10, 20, 30));

ListIterator<Integer> lit = list.listIterator();
while (lit.hasNext()) {
    int val = lit.next();
    if (val == 20) {
        lit.set(200);   // ✔ modify element during iteration
        lit.add(25);    // ✔ insert new element during iteration
    }
}
System.out.println(list); // [10, 200, 25, 30]

// Traverse backward
while (lit.hasPrevious()) {
    System.out.println(lit.previous());
}
```

### Capabilities

- Traverse **both forward and backward**.
- Can `add()`, `remove()`, and `set()` (modify) elements during iteration.
- Provides current index via `nextIndex()`/`previousIndex()`.

---

## 🔹 Comparison Table

| Feature | Iterator | ListIterator |
|---------|----------|--------------|
| Applicable to | `List`, `Set`, `Queue` (any `Collection`) | `List` only |
| Direction | Forward only | Forward & backward |
| Can remove elements | ✔ Yes | ✔ Yes |
| Can add elements | ❌ No | ✔ Yes |
| Can modify (set) elements | ❌ No | ✔ Yes |
| Index access | ❌ No | ✔ Yes (`nextIndex()`, `previousIndex()`) |

---

## 🔹 Why Not Use a Regular `for` Loop with `remove()`?

```java
List<Integer> list = new ArrayList<>(List.of(10, 20, 30));

for (Integer val : list) {
    if (val == 20) {
        list.remove(val); // ❌ ConcurrentModificationException
    }
}
```

Removing directly from the list while iterating with a for-each loop (which uses `Iterator` internally without going through `it.remove()`) throws `ConcurrentModificationException`. Always use `Iterator.remove()` or `ListIterator` for safe modification during iteration.

---

## 🔥 Interview Questions

1. What is the difference between `Iterator` and `ListIterator`?
2. Why does `ListIterator` only work with `List`, not `Set`?
3. Why does modifying a list directly during a for-each loop throw `ConcurrentModificationException`?
4. How can you safely add/remove elements while iterating over a `List`?
5. Can `Iterator` traverse backward? (No — only `ListIterator` can)

---

## 🎯 Summary

✔ `Iterator` → universal, forward-only, supports `remove()`  
✔ `ListIterator` → `List`-only, bidirectional, supports `add()`/`remove()`/`set()`  
✔ Use them instead of direct collection mutation to avoid `ConcurrentModificationException`
