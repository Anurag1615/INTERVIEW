# Recursion on the Way Down and Way Up

## Introduction

One of the most important concepts in recursion is understanding:

- Way Down
- Way Up

Almost every recursion and backtracking problem is built upon these two phases.

Understanding them properly will make:

- Subsequences
- Permutations
- N Queens
- Sudoku
- Combination Sum
- Word Search

much easier.

---

# Understanding Function Calls

Consider:

```java
fun(3);
```

Function:

```java
void fun(int n){

    if(n == 0)
        return;

    System.out.println("Down : " + n);

    fun(n - 1);

    System.out.println("Up : " + n);
}
```

---

## Dry Run

```java
fun(3)
```

Execution:

```text
Down : 3

    Down : 2

        Down : 1

            return

        Up : 1

    Up : 2

Up : 3
```

Output:

```text
Down : 3
Down : 2
Down : 1
Up : 1
Up : 2
Up : 3
```

---

# Visualization

```text
fun(3)
 |
 +-- fun(2)
      |
      +-- fun(1)
            |
            +-- fun(0)
```

Going deeper:

```text
3 → 2 → 1 → 0
```

This is called:

# Way Down

Returning back:

```text
0 ← 1 ← 2 ← 3
```

This is called:

# Way Up

---

# Pre Area and Post Area

Every recursive function has two areas.

```java
void fun(int n){

    if(n == 0)
        return;

    // PRE AREA

    fun(n - 1);

    // POST AREA
}
```

---

## Pre Area

Code executed before recursion call.

```java
System.out.println("Down");
```

---

## Post Area

Code executed after recursion call returns.

```java
System.out.println("Up");
```

---

# Example 1

Print Numbers N to 1

```java
void printDecreasing(int n){

    if(n == 0)
        return;

    System.out.println(n);

    printDecreasing(n - 1);
}
```

Output:

```text
5
4
3
2
1
```

---

## Analysis

Print statement is before recursion.

```java
System.out.println(n);

printDecreasing(n - 1);
```

Therefore output comes during:

```text
Way Down
```

---

# Example 2

Print Numbers 1 to N

```java
void printIncreasing(int n){

    if(n == 0)
        return;

    printIncreasing(n - 1);

    System.out.println(n);
}
```

Output:

```text
1
2
3
4
5
```

---

## Analysis

Print statement is after recursion.

```java
printIncreasing(n - 1);

System.out.println(n);
```

Therefore output comes during:

```text
Way Up
```

---

# Visual Comparison

## Print Decreasing

```java
print(n);
fun(n-1);
```

Output:

```text
5
4
3
2
1
```

Generated During:

```text
Way Down
```

---

## Print Increasing

```java
fun(n-1);
print(n);
```

Output:

```text
1
2
3
4
5
```

Generated During:

```text
Way Up
```

---

# Example 3

Print Both

```java
void pdi(int n){

    if(n == 0)
        return;

    System.out.println("Pre : " + n);

    pdi(n - 1);

    System.out.println("Post : " + n);
}
```

Call:

```java
pdi(3);
```

Output:

```text
Pre : 3
Pre : 2
Pre : 1
Post : 1
Post : 2
Post : 3
```

---

# Recursion Tree

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

# Why Way Up Matters?

Most Backtracking problems use Way Up.

Examples:

- Generate Subsequences
- Generate Permutations
- Generate Parentheses
- N Queens
- Sudoku
- Maze Paths

Because answers are usually built while returning.

---

# Example: Factorial

```java
int factorial(int n){

    if(n == 0)
        return 1;

    return n * factorial(n - 1);
}
```

Call:

```java
factorial(5)
```

---

## Way Down

```text
factorial(5)
factorial(4)
factorial(3)
factorial(2)
factorial(1)
factorial(0)
```

---

## Way Up

```text
1
1 × 1
2 × 1
3 × 2
4 × 6
5 × 24
```

Answer:

```text
120
```

Computation happens during:

```text
Way Up
```

---

# Pattern Recognition

Whenever you see:

```java
work

recursiveCall()
```

Think:

```text
Way Down
```

---

Whenever you see:

```java
recursiveCall()

work
```

Think:

```text
Way Up
```

---

Whenever you see:

```java
work

recursiveCall()

work
```

Think:

```text
Both Way Down and Way Up
```

---

# Common Mistakes

## Mistake 1

Confusing execution order.

Wrong assumption:

```text
Function returns immediately.
```

Reality:

```text
Function waits for child call to finish.
```

---

## Mistake 2

Ignoring Post Area.

Many beginners only focus on:

```java
before recursion
```

and forget:

```java
after recursion
```

which is where most backtracking logic exists.

---

## Mistake 3

Not visualizing stack.

Remember:

```text
Way Down
=
Stack Building

Way Up
=
Stack Unwinding
```

---

# Interview Insight

Many recursion questions can be solved by asking:

```text
Should work happen:

1. Before recursion?
2. After recursion?
3. Both?
```

This single question often reveals the solution.

---

# Key Takeaways

- Way Down = Before recursive call.
- Way Up = After recursive call.
- Pre Area executes while stack grows.
- Post Area executes while stack unwinds.
- Print before recursion → Decreasing Order.
- Print after recursion → Increasing Order.
- Most Backtracking solutions use Way Up.

---

# Practice Problems

## Easy

- Print N to 1
- Print 1 to N
- Print Increasing Decreasing

## Medium

- Factorial
- Power Function
- Fibonacci

## Advanced

- Subsequences
- Permutations
- Maze Paths
- N Queens
