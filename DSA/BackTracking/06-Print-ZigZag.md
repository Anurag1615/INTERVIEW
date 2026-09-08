# Print ZigZag

## Introduction

Print ZigZag is one of the most famous recursion problems from Pepcoding.

The goal of this problem is not the output itself.

The real objective is to understand:

- Pre Area
- In Area
- Post Area
- Euler Tour
- Multiple Recursive Calls

This problem builds the foundation for:

- Recursion Trees
- Backtracking
- DFS Traversal
- N Queens
- Sudoku Solver

---

## Problem Statement

For a given number `n`, print:

```java
System.out.println("Pre " + n);

recursiveCall(n - 1);

System.out.println("In " + n);

recursiveCall(n - 1);

System.out.println("Post " + n);
```

---

## Code

```java
public static void printZigZag(int n) {

    if (n == 0)
        return;

    System.out.println("Pre " + n);

    printZigZag(n - 1);

    System.out.println("In " + n);

    printZigZag(n - 1);

    System.out.println("Post " + n);
}
```

---

## Dry Run

### Input

```java
printZigZag(2);
```

---

## Recursion Tree

```text
                    2
                 /     \
                1       1
               / \     / \
              0   0   0   0
```

---

## Execution

### Root Node

```text
Pre 2
```

---

### Left Child

```text
Pre 1
In 1
Post 1
```

---

### Back to Root

```text
In 2
```

---

### Right Child

```text
Pre 1
In 1
Post 1
```

---

### Root Exit

```text
Post 2
```

---

## Output

```text
Pre 2
Pre 1
In 1
Post 1
In 2
Pre 1
In 1
Post 1
Post 2
```

---

## Euler Tour Visualization

Each node is visited three times:

```text
Entry  -> Pre
Middle -> In
Exit   -> Post
```

For node 2:

```text
Pre 2

Left Subtree

In 2

Right Subtree

Post 2
```

---

## Why Called ZigZag?

Execution moves:

```text
Down
Up
Down
Up
```

continuously.

Pattern:

```text
↙
↗
↘
↖
```

which resembles a ZigZag traversal.

---

## Understanding Pre, In, Post

### Pre Area

Executed before first recursive call.

```java
System.out.println("Pre " + n);
```

---

### In Area

Executed between recursive calls.

```java
System.out.println("In " + n);
```

---

### Post Area

Executed after second recursive call.

```java
System.out.println("Post " + n);
```

---

## Function Structure

```java
Pre

Left Recursion

In

Right Recursion

Post
```

This is exactly how tree traversals work.

---

## Relation to Tree Traversals

### Preorder

```text
Node
Left
Right
```

Equivalent:

```text
Pre
```

---

### Inorder

```text
Left
Node
Right
```

Equivalent:

```text
In
```

---

### Postorder

```text
Left
Right
Node
```

Equivalent:

```text
Post
```

---

## Time Complexity

Recurrence:

```text
T(n) = 2T(n-1) + O(1)
```

---

### Expansion

```text
T(n) ≈ 2^n
```

---

### Complexity

| Metric | Value |
|----------|----------|
| Time Complexity | O(2^n) |
| Space Complexity | O(n) |

---

## Common Mistakes

### Mistake 1

Forgetting second recursive call.

```java
printZigZag(n - 1);
```

must appear twice.

---

### Mistake 2

Confusing In Area with Post Area.

Remember:

```java
Pre

Left Call

In

Right Call

Post
```

---

### Mistake 3

Trying to memorize output.

Instead:

Draw recursion tree.

---

## Interview Insight

Whenever a recursion problem contains:

```java
recursiveCall();

work;

recursiveCall();
```

think:

```text
Binary Recursion Tree
```

and use Euler Tour visualization.

---

## Key Takeaways

- Print ZigZag demonstrates Euler Tour perfectly.
- Every node has:
  - Pre Area
  - In Area
  - Post Area
- Two recursive calls create a binary recursion tree.
- Time Complexity becomes exponential.
- Foundation for Trees and Backtracking.

---

## Practice Problems

- Print ZigZag
- Fibonacci
- Tower of Hanoi
- Generate Parentheses
- N Queens

---

## Next Topic

07-Factorial.md
