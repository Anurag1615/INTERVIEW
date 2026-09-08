# LinkedList Internal Working in Java

`LinkedList` implements both the `List` and `Deque` interfaces, and is internally implemented as a **doubly linked list**.

---

## 🔹 Internal Structure

Each element is wrapped inside a `Node`:

```java
private static class Node<E> {
    E item;
    Node<E> next;
    Node<E> prev;
}
```

`LinkedList` maintains references to:
- `first` (head node)
- `last` (tail node)

```
null ← [prev|item|next] ⇄ [prev|item|next] ⇄ [prev|item|next] → null
         (first)                                  (last)
```

There is **no backing array** — elements are connected purely through node references.

---

## 🔹 Time Complexity

| Operation | Time Complexity | Reason |
|-----------|------------------|--------|
| `addFirst()` / `addLast()` | O(1) | Just update head/tail pointers |
| `add(index, element)` | O(n) | Must traverse to the index first |
| `get(index)` | O(n) | No random access — must traverse from head or tail |
| `remove(Node)` (if node reference known) | O(1) | Just update `prev`/`next` pointers |
| `removeFirst()` / `removeLast()` | O(1) | Direct access to head/tail |

---

## 🔹 Example

```java
LinkedList<Integer> list = new LinkedList<>();
list.addFirst(10);
list.addLast(20);
list.add(1, 15);
System.out.println(list); // [10, 15, 20]

list.removeFirst();
System.out.println(list); // [15, 20]
```

---

## 🔹 LinkedList as a Deque / Queue / Stack

Because it implements `Deque`, `LinkedList` can be used as:

```java
Deque<Integer> stack = new LinkedList<>();
stack.push(1);
stack.push(2);
System.out.println(stack.pop()); // 2 → LIFO (stack behavior)

Queue<Integer> queue = new LinkedList<>();
queue.offer(1);
queue.offer(2);
System.out.println(queue.poll()); // 1 → FIFO (queue behavior)
```

---

## 🔹 ArrayList vs LinkedList

| Aspect | ArrayList | LinkedList |
|--------|-----------|------------|
| Backing structure | Dynamic array | Doubly linked list |
| Random access (`get(index)`) | O(1) | O(n) |
| Insert/Delete at beginning | O(n) (shifting) | O(1) |
| Insert/Delete in middle | O(n) | O(n) (traversal) + O(1) (unlink) |
| Memory overhead | Lower (just array slots) | Higher (extra `prev`/`next` pointers per node) |
| Implements | `List`, `RandomAccess` | `List`, `Deque` |
| Best for | Frequent random access/reads | Frequent insertions/deletions at ends |

---

## 🔥 Interview Questions

1. Why is `get(index)` O(n) in `LinkedList` but O(1) in `ArrayList`?
2. When would you prefer `LinkedList` over `ArrayList`?
3. How does `LinkedList` implement `Deque` behavior (stack + queue)?
4. Why does `LinkedList` NOT implement the `RandomAccess` marker interface?
5. What is the memory overhead of `LinkedList` compared to `ArrayList`?

---

## 🎯 Summary

✔ Doubly linked list — no backing array  
✔ O(1) insertion/deletion at ends, O(n) random access  
✔ Implements `List` + `Deque` → usable as List, Stack, and Queue  
✔ Higher memory overhead due to node pointers vs `ArrayList`
