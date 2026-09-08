# Recursion Tree and Euler Tour

## Introduction

A recursion tree is a visual representation of all recursive calls made during execution.

Understanding recursion trees helps in:

- Visualizing recursive calls
- Understanding stack behavior
- Learning backtracking
- Solving subsets and permutations
- Mastering DFS traversal

This chapter introduces the concept of:

- Recursion Tree
- Euler Tour
- Pre Area
- In Area
- Post Area

These concepts are heavily used in advanced recursion and backtracking problems.

---

# What is a Recursion Tree?

Consider:

```java
void fun(int n){

    if(n == 0)
        return;

    fun(n - 1);
}
```

Call:

```java
fun(3);
```

Recursion Tree:

```text
fun(3)
   |
   v
fun(2)
   |
   v
fun(1)
   |
   v
fun(0)
```

Every function call becomes a node in the recursion tree.

---

# More Interesting Example

Consider:

```java
void fun(int n){

    if(n == 0)
        return;

    System.out.println("Pre " + n);

    fun(n - 1);

    System.out.println("Post " + n);
}
```

Call:

```java
fun(3);
```

---

# Tree Representation

```text
fun(3)
   |
   +---- fun(2)
              |
              +---- fun(1)
                         |
                         +---- fun(0)
```

---

# Euler Tour Concept

Imagine an ant walking around the recursion tree.

Every node is visited multiple times.

A node can be visited during:

1. Entry
2. Middle (between child calls)
3. Exit

This complete traversal is called:

```text
Euler Tour
```

---

# Understanding Entry and Exit

Function:

```java
void fun(int n){

    if(n == 0)
        return;

    System.out.println("Pre " + n);

    fun(n - 1);

    System.out.println("Post " + n);
}
```

For:

```java
fun(3)
```

Execution:

```text
Enter 3
Enter 2
Enter 1
Enter 0

Exit 1
Exit 2
Exit 3
```

Output:

```text
Pre 3
Pre 2
Pre 1

Post 1
Post 2
Post 3
```

---

# Visualization

```text
        fun(3)
          |
          |
        fun(2)
          |
          |
        fun(1)
          |
          |
        fun(0)
```

Moving Down:

```text
3 → 2 → 1 → 0
```

Way Down

Moving Up:

```text
0 ← 1 ← 2 ← 3
```

Way Up

---

# Entry State (Pre Area)

Code before recursive call.

Example:

```java
System.out.println("Pre " + n);
```

Runs while moving downward.

---

# Exit State (Post Area)

Code after recursive call.

Example:

```java
System.out.println("Post " + n);
```

Runs while returning upward.

---

# Example with Multiple Recursive Calls

```java
void fun(int n){

    if(n == 0)
        return;

    System.out.println("Pre " + n);

    fun(n - 1);

    System.out.println("In " + n);

    fun(n - 1);

    System.out.println("Post " + n);
}
```

---

# Why This Example Matters?

Now each node has:

```text
Pre Area
In Area
Post Area
```

This is where Euler Tour becomes important.

---

# Recursion Tree for n = 2

```text
                     2
                  /     \
                 1       1
                / \     / \
               0   0   0   0
```

---

# Euler Traversal

For root node 2:

### First Visit

```text
Pre 2
```

---

### Between Child Calls

```text
In 2
```

---

### Final Visit

```text
Post 2
```

---

# Dry Run

Call:

```java
fun(2)
```

Output:

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

# Node Visits

Each node is visited three times.

```text
Pre
In
Post
```

This forms the Euler Tour.

---

# Why Euler Tour Matters?

Many recursion problems are solved by placing logic in:

```text
Pre Area
```

or

```text
In Area
```

or

```text
Post Area
```

depending on the requirement.

---

# Connection to Tree Traversals

Euler Tour is the foundation of:

## Preorder

```text
Node
Left
Right
```

Equivalent to:

```text
Pre Area
```

---

## Inorder

```text
Left
Node
Right
```

Equivalent to:

```text
In Area
```

---

## Postorder

```text
Left
Right
Node
```

Equivalent to:

```text
Post Area
```

---

# Relation to Backtracking

Backtracking mostly uses:

```text
Post Area
```

because choices are reverted while returning.

Example:

```java
path.remove(path.size() - 1);
```

This statement executes during:

```text
Way Up
```

which is essentially Post Area.

---

# Stack Behavior

Call:

```java
fun(3)
```

Stack:

```text
-------------
fun(0)
-------------
fun(1)
-------------
fun(2)
-------------
fun(3)
-------------
```

During return:

```text
fun(0) removed

fun(1) removed

fun(2) removed

fun(3) removed
```

This is Stack Unwinding.

---

# Pattern Recognition

When you see:

```java
work

recursiveCall()
```

Think:

```text
Pre Area
```

---

When you see:

```java
recursiveCall()

work
```

Think:

```text
Post Area
```

---

When you see:

```java
recursiveCall()

work

recursiveCall()
```

Think:

```text
In Area
```

---

# Time Complexity

Single Recursive Call:

```java
fun(n - 1)
```

Complexity:

```text
O(n)
```

---

Two Recursive Calls:

```java
fun(n - 1);
fun(n - 1);
```

Complexity:

```text
O(2^n)
```

Recursion tree helps visualize this growth.

---

# Common Mistakes

## Mistake 1

Ignoring Post Area.

Most backtracking logic exists here.

---

## Mistake 2

Not drawing recursion tree.

Drawing the tree often reveals the solution.

---

## Mistake 3

Confusing function calls with iterations.

Recursive calls create new stack frames.

---

# Interview Insight

Whenever stuck in recursion:

Ask:

```text
Where should my logic go?

Pre Area?
In Area?
Post Area?
```

This question often reveals the correct solution.

---

# Key Takeaways

- Every recursive call becomes a node in a recursion tree.
- Euler Tour means traversing all node visits.
- A node can have:
  - Pre Area
  - In Area
  - Post Area
- Pre Area executes during Way Down.
- Post Area executes during Way Up.
- Backtracking is mostly implemented in Post Area.
- Recursion Trees help understand time complexity.

---

# Practice Problems

## Easy

- Print Increasing
- Print Decreasing
- PDI

## Medium

- Factorial
- Fibonacci
- Power

## Advanced

- Subsequences
- Maze Paths
- N Queens
- Sudoku Solver
- Combination Sum

---

# Next Topic

**06-Factorial-And-Recursive-Faith.md**

In the next chapter, we will solve our first real recursion-return problem:

```java
factorial(n)
```

and learn how recursion returns values using Faith and Expectation.
