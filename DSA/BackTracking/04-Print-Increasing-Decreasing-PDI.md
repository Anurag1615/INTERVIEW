# Print Increasing Decreasing (PDI)

## Introduction

The **Print Increasing Decreasing (PDI)** problem is one of the most important recursion problems.

Although the problem looks simple, it teaches:

- Pre Area
- Post Area
- Way Down
- Way Up
- Stack Unwinding
- Recursion Tree Traversal

This problem is often considered the first real recursion pattern.

---

# Problem Statement

Print:

```text
3
2
1
1
2
3
```

Using recursion.

---

# Recursive Solution

```java
public static void pdi(int n) {

    if (n == 0)
        return;

    System.out.println(n);

    pdi(n - 1);

    System.out.println(n);
}
```

---

# Dry Run

Input:

```java
pdi(3);
```

---

## Step 1

```text
pdi(3)

Print 3
```

Calls:

```java
pdi(2)
```

---

## Step 2

```text
pdi(2)

Print 2
```

Calls:

```java
pdi(1)
```

---

## Step 3

```text
pdi(1)

Print 1
```

Calls:

```java
pdi(0)
```

---

## Base Case

```java
if(n == 0)
    return;
```

Returns immediately.

---

# Stack Unwinding

Now recursion starts returning.

---

## Return to pdi(1)

```text
Print 1
```

---

## Return to pdi(2)

```text
Print 2
```

---

## Return to pdi(3)

```text
Print 3
```

---

# Final Output

```text
3
2
1
1
2
3
```

---

# Visualization

## Way Down

```text
pdi(3)
|
+-- pdi(2)
    |
    +-- pdi(1)
        |
        +-- pdi(0)
```

Generated Output:

```text
3
2
1
```

---

## Way Up

Returning:

```text
pdi(0)
← pdi(1)
← pdi(2)
← pdi(3)
```

Generated Output:

```text
1
2
3
```

---

# Complete Recursion Tree

```text
pdi(3)

Pre 3
|
+---- pdi(2)

      Pre 2
      |
      +---- pdi(1)

            Pre 1
            |
            +---- pdi(0)

            Post 1

      Post 2

Post 3
```

---

# Understanding Pre and Post Area

Function:

```java
void pdi(int n){

    if(n == 0)
        return;

    System.out.println(n);

    pdi(n - 1);

    System.out.println(n);
}
```

---

## Pre Area

```java
System.out.println(n);
```

Executed before recursion.

Output:

```text
3
2
1
```

---

## Post Area

```java
System.out.println(n);
```

Executed after recursion returns.

Output:

```text
1
2
3
```

---

# Important Observation

Anything written before:

```java
pdi(n - 1);
```

executes on:

```text
Way Down
```

Anything written after:

```java
pdi(n - 1);
```

executes on:

```text
Way Up
```

---

# Euler Tour Concept

A recursive function is visited multiple times.

For every node:

```text
Entry
Middle
Exit
```

PDI demonstrates:

```text
Entry  -> Print Before
Exit   -> Print After
```

This idea later becomes:

- Tree Traversal
- DFS
- Backtracking

---

# Stack Visualization

When calling:

```java
pdi(3);
```

Stack becomes:

```text
-----------------
pdi(0)
-----------------
pdi(1)
-----------------
pdi(2)
-----------------
pdi(3)
-----------------
```

After reaching base case:

```text
pdi(0) removed
pdi(1) removed
pdi(2) removed
pdi(3) removed
```

This process is:

```text
Stack Unwinding
```

---

# Time Complexity

Each function executes once.

Number of calls:

```text
n + 1
```

Therefore:

| Metric | Complexity |
|----------|----------|
| Time | O(n) |
| Space | O(n) |

---

# Common Mistakes

## Mistake 1

Using wrong base case.

Wrong:

```java
if(n == 1)
    return;
```

Output becomes incorrect.

---

## Mistake 2

Forgetting Post Area.

```java
System.out.println(n);

pdi(n - 1);
```

Only decreasing sequence gets printed.

---

## Mistake 3

Thinking recursion returns immediately.

Reality:

```text
Parent waits
for child call
to finish.
```

---

# Interview Pattern

Whenever you see:

```java
work

recursiveCall()

work
```

Think:

```text
PDI Pattern
```

This pattern appears in:

- Subsequences
- Permutations
- Maze Paths
- N Queens
- Sudoku Solver
- DFS Backtracking

---

# Key Takeaways

- PDI teaches both Way Down and Way Up.
- Code before recursion executes first.
- Code after recursion executes during stack unwinding.
- Parent function waits for child recursion.
- Most backtracking problems are built on this concept.
- Understanding PDI makes advanced recursion easier.

---

# Practice Problems

## Easy

- Print Increasing
- Print Decreasing
- PDI

## Medium

- Factorial
- Power
- Fibonacci

## Advanced

- Subsequences
- Maze Paths
- N Queens
- Sudoku

---

# Next Topic

**Recursion Tree and Euler Tour**

This chapter explains:

- Why a recursive function appears multiple times
- Entry, In, and Exit states
- Euler Traversal of Recursion Trees
- Foundation of Backtracking
