# TreeMap in Java

`TreeMap` is a class in Java that stores **key-value pairs in sorted order of keys**.

Unlike `HashMap` and `LinkedHashMap`, a `TreeMap` keeps elements **automatically sorted**.

It is implemented using a **Red-Black Tree**, which is a type of **self-balancing binary search tree**.

---

# Key Features of TreeMap

```
1️⃣ Stores data in key-value pairs
2️⃣ Keys are automatically sorted
3️⃣ Uses Red-Black Tree internally
4️⃣ No duplicate keys allowed
5️⃣ Does NOT allow null keys
```

Example:

```java
import java.util.TreeMap;

public class Example {

    public static void main(String[] args) {

        TreeMap<Integer,String> map = new TreeMap<>();

        map.put(3,"C");
        map.put(1,"A");
        map.put(2,"B");

        System.out.println(map);
    }
}
```

Output

```
{1=A, 2=B, 3=C}
```

Even though we inserted:

```
3 → 1 → 2
```

TreeMap automatically sorted the keys.

---

# Internal Data Structure

TreeMap internally uses a **Red-Black Tree**.

```
Red-Black Tree = Self-balancing Binary Search Tree
```

Structure:

```
          2
        /   \
       1     3
```

Properties of Red-Black Tree:

```
1️⃣ Each node is either RED or BLACK
2️⃣ Root node is always BLACK
3️⃣ No two consecutive RED nodes
4️⃣ Every path from root to leaf has same number of BLACK nodes
```

These rules keep the tree **balanced**, ensuring fast operations.

---

# Node Structure in TreeMap

Simplified structure:

```java
class Entry<K,V> {
    K key;
    V value;
    Entry<K,V> left;
    Entry<K,V> right;
    Entry<K,V> parent;
    boolean color;
}
```

Fields:

| Field | Description |
|------|-------------|
| key | key object |
| value | stored value |
| left | left child |
| right | right child |
| parent | parent node |
| color | RED or BLACK |

---

# Insertion Process (Easy Explanation)

Consider this code:

```java
TreeMap<Integer,String> map = new TreeMap<>();

map.put(3,"C");
map.put(1,"A");
map.put(2,"B");
```

---

# Step 1: Insert First Element

```
map.put(3,"C")
```

Tree is empty, so this becomes the **root node**.

```
    3
```

Root is always **BLACK**.

---

# Step 2: Insert Second Element

```
map.put(1,"A")
```

Comparison:

```
1 < 3
```

So it goes to the **left side**.

Tree becomes:

```
      3
     /
    1
```

---

# Step 3: Insert Third Element

```
map.put(2,"B")
```

Comparisons:

```
2 < 3 → go left
2 > 1 → go right
```

Tree becomes:

```
      3
     /
    1
     \
      2
```

But this structure **violates Red-Black Tree rules**, so Java performs **rotation and recoloring** to balance the tree.

Balanced tree becomes:

```
      2
     / \
    1   3
```

Now the tree is balanced again.

---

# Final Tree Structure

```
      2
     / \
    1   3
```

When iterating through TreeMap, it follows **in-order traversal**.

```
1 → 2 → 3
```

Which keeps keys **sorted**.

---

# Working of get()

Example:

```java
map.get(2)
```

Steps:

1️⃣ Start at root

```
2 == 2 → match found
```

Return value:

```
"B"
```

If searching for another value:

```
map.get(1)
```

Traversal:

```
1 < 2 → go left
1 == 1 → found
```

---

# TreeMap Time Complexity

Because of the Red-Black Tree structure:

| Operation | Complexity |
|----------|------------|
put() | O(log n) |
get() | O(log n) |
remove() | O(log n) |

---

# TreeMap vs HashMap vs LinkedHashMap

| Feature | HashMap | LinkedHashMap | TreeMap |
|------|------|------|------|
Order | No order | Insertion order | Sorted order |
Data Structure | Hash Table | Hash Table + Linked List | Red-Black Tree |
Null Key | Allowed | Allowed | Not allowed |
Time Complexity | O(1) | O(1) | O(log n) |

---

# Example with Custom Sorting

TreeMap can also sort using a **Comparator**.

Example:

```java
TreeMap<Integer,String> map =
    new TreeMap<>((a,b) -> b - a);

map.put(1,"A");
map.put(3,"C");
map.put(2,"B");

System.out.println(map);
```

Output:

```
{3=C, 2=B, 1=A}
```

Sorted in **descending order**.

---

# Real World Use Cases

TreeMap is useful when:

```
Data must remain sorted
Range queries are needed
Ordered navigation is required
```

Example applications:

```
Leaderboard ranking
Scheduling systems
Sorted logs
Priority processing
```

---

# Quick Summary

```
HashMap       → Fast but unordered
LinkedHashMap → Ordered by insertion
TreeMap       → Automatically sorted
```

---

# Interview One-Line Definition

> TreeMap is a Map implementation that stores key-value pairs in **sorted order using a Red-Black Tree**, providing O(log n) performance for insert, delete, and search operations.
