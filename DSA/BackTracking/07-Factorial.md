# Factorial Using Recursion

## Introduction

Factorial is usually the first problem used to learn recursion with return values.

In previous chapters, we focused on:

- Way Down
- Way Up
- Pre Area
- Post Area

Now we will learn:

- Returning values from recursion
- Faith and Expectation
- Recursive Mathematical Relations

Factorial is the perfect starting point.

---

# Problem Statement

Find:

```text
n!
```

Where:

```text
n! = n × (n-1) × (n-2) × ... × 1
```

Examples:

```text
5! = 120

4! = 24

3! = 6

2! = 2

1! = 1

0! = 1
```

---

# Mathematical Observation

Let's write:

```text
5!
```

as:

```text
5 × 4!
```

Similarly:

```text
4!
=
4 × 3!
```

```text
3!
=
3 × 2!
```

```text
2!
=
2 × 1!
```

This naturally creates recursion.

---

# Recursive Relation

```text
factorial(n)

=
n × factorial(n-1)
```

---

# Faith and Expectation

Suppose:

```java
factorial(5)
```

My work:

```text
Multiply by 5
```

Expectation:

```java
factorial(4)
```

should return:

```text
4!
```

I don't care how.

I simply trust it.

This trust is called:

```text
Faith
```

---

# Recursive Code

```java
public static int factorial(int n) {

    if (n == 0)
        return 1;

    return n * factorial(n - 1);
}
```

---

# Why Base Case Returns 1?

Mathematically:

```text
0! = 1
```

Therefore:

```java
if(n == 0)
    return 1;
```

---

# Dry Run

Input:

```java
factorial(5)
```

---

## Recursive Calls

```text
factorial(5)

=
5 × factorial(4)
```

```text
=
5 × (4 × factorial(3))
```

```text
=
5 × (4 × (3 × factorial(2)))
```

```text
=
5 × (4 × (3 × (2 × factorial(1))))
```

```text
=
5 × (4 × (3 × (2 × (1 × factorial(0)))))
```

---

# Base Case

```java
factorial(0)
```

returns:

```text
1
```

---

# Stack Unwinding

```text
factorial(1)

=
1 × 1

=
1
```

---

```text
factorial(2)

=
2 × 1

=
2
```

---

```text
factorial(3)

=
3 × 2

=
6
```

---

```text
factorial(4)

=
4 × 6

=
24
```

---

```text
factorial(5)

=
5 × 24

=
120
```

---

# Final Answer

```text
120
```

---

# Visualization

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

1 × 1 = 1

2 × 1 = 2

3 × 2 = 6

4 × 6 = 24

5 × 24 = 120
```

Actual computation happens during:

```text
Way Up
```

---

# Recursion Tree

```text
factorial(5)
    |
factorial(4)
    |
factorial(3)
    |
factorial(2)
    |
factorial(1)
    |
factorial(0)
```

Unlike ZigZag:

```text
Only One Recursive Call
```

Therefore:

```text
Linear Recursion
```

---

# Stack Visualization

Before Base Case:

```text
----------------
factorial(0)
----------------
factorial(1)
----------------
factorial(2)
----------------
factorial(3)
----------------
factorial(4)
----------------
factorial(5)
----------------
```

---

After Base Case:

```text
factorial(0) removed

factorial(1) removed

factorial(2) removed

factorial(3) removed

factorial(4) removed

factorial(5) removed
```

---

# Time Complexity

Number of recursive calls:

```text
n + 1
```

Therefore:

| Metric | Complexity |
|----------|----------|
| Time Complexity | O(n) |
| Space Complexity | O(n) |

---

# Iterative Solution

```java
public static int factorial(int n) {

    int ans = 1;

    for(int i = 1; i <= n; i++) {
        ans *= i;
    }

    return ans;
}
```

---

# Recursion vs Iteration

| Feature | Recursion | Iteration |
|----------|----------|----------|
| Readability | High | Medium |
| Space Usage | O(n) | O(1) |
| Call Stack | Required | Not Required |
| Performance | Slightly Slower | Faster |

---

# Common Mistakes

## Mistake 1

Wrong Base Case

```java
if(n == 1)
    return 1;
```

May fail for:

```text
0!
```

---

## Mistake 2

Forgetting Return

Wrong:

```java
factorial(n - 1);
```

Correct:

```java
return n * factorial(n - 1);
```

---

## Mistake 3

Trying to Calculate Entire Answer Yourself

Wrong Thinking:

```text
How does factorial(4) work?
```

Correct Thinking:

```text
Assume factorial(4)
returns 4!
```

---

# Interview Pattern

Whenever you see:

```text
Current Answer

=
My Work

+

Smaller Problem
```

think:

```text
Faith + Expectation
```

This pattern appears in:

- Factorial
- Power
- Fibonacci
- Climbing Stairs
- Tree Problems
- Dynamic Programming

---

# Key Takeaways

- Factorial is the simplest return-type recursion problem.
- Use Faith and Expectation.
- Base Case prevents infinite recursion.
- Computation happens during Way Up.
- Linear recursion creates O(n) stack space.
- Factorial follows:

```text
n!
=
n × (n-1)!
```

---

# Practice Problems

## Easy

- Factorial
- Sum of N Numbers

## Medium

- Power (Linear)

## Advanced

- Fibonacci
- Climbing Stairs

---

# Next Topic

08-Power-Linear.md

Learn how to calculate:

```text
x^n
```

using recursion and build the foundation for Fast Exponentiation.
