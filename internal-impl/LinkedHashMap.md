# Insertion Process in LinkedHashMap (Easy Explanation)

`LinkedHashMap` stores elements using **two data structures together**:

```
1️⃣ Hash Table (same as HashMap)
2️⃣ Doubly Linked List (to maintain order)
```

So whenever we insert an element, **two things happen**:

```
• It is placed in the correct bucket using hashing
• It is added to the end of the linked list to maintain order
```

---

# Step-by-Step Insertion Example

Consider this code:

```java
LinkedHashMap<Integer, String> map = new LinkedHashMap<>();

map.put(3, "C");
map.put(1, "A");
map.put(2, "B");
```

We will understand what happens internally.

---

# Step 1: Calculate Hash

When we insert:

```java
map.put(3, "C");
```

Java first calculates the hash value of the key.

```
hash = key.hashCode()
```

Example:

```
hash(3) = 3
```

---

# Step 2: Find Bucket Index

Next, HashMap finds the **bucket index** where the data should be stored.

Formula:

```
index = hash & (capacity - 1)
```

Default capacity:

```
16
```

Example:

```
index = 3 & (16 - 1)
index = 3 & 15
index = 3
```

So the element goes into **bucket 3**.

---

# Step 3: Create Entry Node

A new node (entry) is created.

```
Entry {
 hash = 3
 key = 3
 value = "C"
 next = null
 before = null
 after = null
}
```

---

# Step 4: Insert Into Hash Table

The node is stored in the calculated bucket.

```
Bucket[3] → [3,C]
```

---

# Step 5: Add Node to Linked List

LinkedHashMap also maintains a **doubly linked list** to preserve insertion order.

Since this is the first element:

```
Head → [3,C] ← Tail
```

---

# Insert Second Element

```
map.put(1,"A")
```

### Step 1

```
hash(1) = 1
```

### Step 2

```
index = 1 & 15
index = 1
```

### Step 3

Insert in bucket:

```
Bucket[1] → [1,A]
```

### Step 4

Add to end of linked list:

```
[3,C] ⇄ [1,A]
```

---

# Insert Third Element

```
map.put(2,"B")
```

### Hash

```
hash(2) = 2
```

### Index

```
index = 2 & 15 = 2
```

### Insert in bucket

```
Bucket[2] → [2,B]
```

### Update Linked List

```
[3,C] ⇄ [1,A] ⇄ [2,B]
```

---

# Final Internal Structure

### Hash Table

```
Index    Node
----------------
1        [1,A]
2        [2,B]
3        [3,C]
```

### Linked List (Maintains Order)

```
Head
 ↓
[3,C] ⇄ [1,A] ⇄ [2,B]
                     ↑
                   Tail
```

---

# Important Point

Even if hashing places elements in different buckets,  
**LinkedHashMap still maintains insertion order using the linked list.**

So iteration will always be:

```
3 → 1 → 2
```

---

# Simple Rule to Remember

Insertion in LinkedHashMap performs **two operations**:

```
1️⃣ Insert element into hash table (using hashing)
2️⃣ Add element to the end of the doubly linked list
```

---

# Why LinkedHashMap Is Used

Because it provides:

```
Fast lookup (like HashMap)
+
Ordered iteration
```

---

# Interview One-Line Explanation

> When inserting into a LinkedHashMap, Java stores the entry in a hash table bucket and also links it into a doubly linked list to maintain insertion order.
