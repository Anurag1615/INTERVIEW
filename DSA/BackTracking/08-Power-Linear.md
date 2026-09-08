# Power (Linear Recursion)

## Introduction

In this chapter, we will calculate:

```text
xⁿ
```

using recursion.

Examples:

```text
2⁵ = 32

3⁴ = 81

5³ = 125
```

This problem teaches:

- Return Type Recursion
- Faith and Expectation
- Mathematical Recurrence Relations
- Building Blocks for Fast Exponentiation

---

# Problem Statement

Given:

```text
x = base
n = exponent
```

Find:

```text
xⁿ
```

Example:

```text
Input:

x = 2
n = 5

Output:

32
```

---

# Mathematical Observation

Let's expand:

```text
2⁵
```

```text
=
2 × 2 × 2 × 2 × 2
```

We can rewrite it as:

```text
2⁵
=
2 × 2⁴
```

Similarly:

```text
2⁴
=
2 × 2³
```

```text
2³
=
2 × 2²
```

This naturally creates recursion.

---

# Recursive Relation

```text
power(x,n)

=
x × power(x,n-1)
```

---

# Faith and Expectation

Suppose:

```java
power(2,5)
```

My Work:

```text
Multiply by 2
```

Expectation:

```java
power(2,4)
```

should return:

```text
2⁴
```

I trust the recursive call.

This trust is called:

```text
Faith
```

---

# Recursive Solution

```java
public static int power(int x, int n){

    if(n == 0)
        return 1;

    return x * power(x, n - 1);
}
```

---

# Base Case

Any number raised to power 0:

```text
x⁰ = 1
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
power(2,5)
```

---

## Recursive Expansion

```text
2 × power(2,4)
```

```text
2 × (2 × power(2,3))
```

```text
2 × (2 × (2 × power(2,2)))
```

```text
2 × (2 × (2 × (2 × power(2,1))))
```

```text
2 × (2 × (2 × (2 × (2 × power(2,0)))))
```

---

# Base Case

```java
power(2,0)
```

returns:

```text
1
```

---

# Stack Unwinding

```text
power(2,1)

=
2 × 1

=
2
```

---

```text
power(2,2)

=
2 × 2

=
4
```

---

```text
power(2,3)

=
2 × 4

=
8
```

---

```text
power(2,4)

=
2 × 8

=
16
```

---

```text
power(2,5)

=
2 × 16

=
32
```

---

# Final Answer

```text
32
```

---

# Visualization

## Way Down

```text
power(2,5)

power(2,4)

power(2,3)

power(2,2)

power(2,1)

power(2,0)
```

---

## Way Up

```text
1

2

4

8

16

32
```

Computation happens during:

```text
Way Up
```

---

# Recursion Tree

```text
power(2,5)
      |
power(2,4)
      |
power(2,3)
      |
power(2,2)
      |
power(2,1)
      |
power(2,0)
```

This is:

```text
Linear Recursion
```

because only one recursive call is made.

---

# Stack Visualization

Before Base Case:

```text
----------------
power(2,0)
----------------
power(2,1)
----------------
power(2,2)
----------------
power(2,3)
----------------
power(2,4)
----------------
power(2,5)
----------------
```

---

After Base Case:

```text
power(2,0) removed

power(2,1) removed

power(2,2) removed

power(2,3) removed

power(2,4) removed

power(2,5) removed
```

---

# Complexity Analysis

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

# Limitation

For:

```text
2¹⁰⁰⁰⁰⁰
```

the recursive calls become:

```text
100000
```

which is inefficient.

We need a faster approach.

---

# Iterative Solution

```java
public static int power(int x, int n){

    int ans = 1;

    for(int i = 1; i <= n; i++){
        ans *= x;
    }

    return ans;
}
```

---

# Recursion vs Iteration

| Feature | Recursion | Iteration |
|----------|----------|----------|
| Readability | High | Medium |
| Stack Usage | O(n) | O(1) |
| Speed | Similar | Slightly Faster |
| Learning Value | Excellent | Low |

---

# Common Mistakes

## Mistake 1

Wrong Base Case

```java
if(n == 1)
    return 1;
```

Incorrect.

---

Correct:

```java
if(n == 0)
    return 1;
```

---

## Mistake 2

Forgetting Return

Wrong:

```java
power(x,n-1);
```

Correct:

```java
return x * power(x,n-1);
```

---

## Mistake 3

Not Understanding Faith

Wrong Thinking:

```text
How does power(2,4) work?
```

Correct Thinking:

```text
Assume power(2,4)
returns 2⁴.
```

---

# Interview Insight

Whenever you see:

```text
Current Answer

=
Current Work

×

Smaller Problem
```

you can often use recursion.

Examples:

- Factorial
- Power
- Product of Array
- Tree Multiplication Problems

---

# Key Takeaways

- Power follows:

```text
xⁿ = x × xⁿ⁻¹
```

- Faith and Expectation are essential.
- Computation happens during Way Up.
- Linear recursion requires O(n) calls.
- This solution can be optimized further.

---

# Practice Problems

## Easy

- Power Function
- Factorial
- Sum of N Numbers

## Medium

- Product of Array Elements
- Recursive Multiplication

## Advanced

- Fast Exponentiation
- Matrix Exponentiation

---

# Next Topic

09-Power-Logarithmic.md

In the next chapter, we will optimize:

```text
O(n)
```

to:

```text
O(log n)
```

using **Exponentiation by Squaring**, one of the most important interview algorithms.
