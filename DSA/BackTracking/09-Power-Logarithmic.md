# Power (Logarithmic Recursion)

## Introduction

In the previous chapter, we solved:

```text
xⁿ
```

using linear recursion.

```java
power(x, n) = x * power(x, n - 1)
```

Time Complexity:

```text
O(n)
```

This is acceptable for small values of `n`.

However:

```text
2¹⁰⁰⁰⁰⁰
```

would require:

```text
100000 recursive calls
```

which is inefficient.

In this chapter, we will reduce complexity from:

```text
O(n)
```

to:

```text
O(log n)
```

using:

```text
Exponentiation by Squaring
```

---

# Key Observation

Suppose:

```text
2¹⁰
```

Instead of calculating:

```text
2 × 2 × 2 × 2 × ...
```

observe:

```text
2¹⁰
=
(2⁵)²
```

Similarly:

```text
2⁸
=
(2⁴)²
```

```text
2⁴
=
(2²)²
```

```text
2²
=
(2¹)²
```

We are reducing the exponent by half every time.

---

# Mathematical Formula

## Even Power

If:

```text
n is even
```

then:

```text
xⁿ = (xⁿ⁄²)²
```

Example:

```text
2⁸

=
(2⁴)²

=
16²

=
256
```

---

## Odd Power

If:

```text
n is odd
```

then:

```text
xⁿ = x × (xⁿ⁄²)²
```

Example:

```text
2⁹

=
2 × (2⁴)²

=
2 × 16²

=
512
```

---

# Faith and Expectation

Suppose:

```java
power(2, 10)
```

My expectation:

```java
power(2, 5)
```

returns:

```text
2⁵
```

I trust the recursive call.

Then:

```text
2¹⁰
=
(2⁵)²
```

Done.

---

# Recursive Solution

```java
public static int powerLog(int x, int n){

    if(n == 0)
        return 1;

    int halfPower = powerLog(x, n / 2);

    int result = halfPower * halfPower;

    if(n % 2 == 1){
        result = result * x;
    }

    return result;
}
```

---

# Dry Run

Input:

```java
powerLog(2, 10)
```

---

## Recursive Calls

```text
powerLog(2,10)

→ powerLog(2,5)

→ powerLog(2,2)

→ powerLog(2,1)

→ powerLog(2,0)
```

---

# Base Case

```java
powerLog(2,0)
```

returns:

```text
1
```

---

# Stack Unwinding

### powerLog(2,1)

```text
halfPower = 1

result = 1 × 1

odd power

result = result × 2

= 2
```

---

### powerLog(2,2)

```text
halfPower = 2

result = 2 × 2

= 4
```

---

### powerLog(2,5)

```text
halfPower = 4

result = 4 × 4

= 16

odd power

16 × 2

= 32
```

---

### powerLog(2,10)

```text
halfPower = 32

result = 32 × 32

= 1024
```

---

# Final Answer

```text
1024
```

---

# Recursion Tree

```text
power(2,10)
       |
power(2,5)
       |
power(2,2)
       |
power(2,1)
       |
power(2,0)
```

Notice:

```text
10 → 5 → 2 → 1 → 0
```

Exponent is halved each time.

---

# Why Logarithmic?

Observe:

```text
16 → 8 → 4 → 2 → 1
```

Number of divisions:

```text
log₂(16)
=
4
```

Similarly:

```text
1024
```

requires:

```text
log₂(1024)
=
10
```

recursive calls.

---

# Complexity Analysis

## Time Complexity

Recurrence:

```text
T(n) = T(n/2) + O(1)
```

Result:

```text
O(log n)
```

---

## Space Complexity

Recursion depth:

```text
log n
```

Therefore:

```text
O(log n)
```

---

# Comparison

| Approach | Time | Space |
|----------|----------|----------|
| Linear Power | O(n) | O(n) |
| Logarithmic Power | O(log n) | O(log n) |

---

# Example Comparison

### Calculate

```text
2¹⁰⁰⁰⁰⁰
```

---

## Linear Recursion

```text
100000 calls
```

---

## Logarithmic Recursion

```text
≈ 17 calls
```

because:

```text
log₂(100000)
≈ 17
```

Huge improvement.

---

# Optimized Version

```java
public static long powerLog(long x, long n){

    if(n == 0)
        return 1;

    long half = powerLog(x, n / 2);

    long result = half * half;

    if(n % 2 == 1){
        result *= x;
    }

    return result;
}
```

---

# Common Mistakes

## Mistake 1

Calling recursion twice.

Wrong:

```java
return powerLog(x,n/2)
     * powerLog(x,n/2);
```

Complexity becomes:

```text
O(n)
```

instead of:

```text
O(log n)
```

---

## Mistake 2

Forgetting odd case.

Wrong:

```java
return half * half;
```

Fails for:

```text
2⁵
```

---

## Mistake 3

Using int for large powers.

Use:

```java
long
```

when necessary.

---

# Interview Insight

This technique is known as:

```text
Binary Exponentiation
```

or

```text
Exponentiation by Squaring
```

Frequently used in:

- Competitive Programming
- Number Theory
- Matrix Exponentiation
- Modular Arithmetic
- FAANG Interviews

---

# Pattern Recognition

Whenever you see:

```text
Problem Size

n

↓

n/2

↓

n/4

↓

n/8
```

Think:

```text
O(log n)
```

---

# Key Takeaways

- Reduce exponent by half.
- Use Faith and Expectation.
- Solve once and reuse result.
- Time Complexity becomes:

```text
O(log n)
```

- One of the most important recursion optimizations.

---

# Practice Problems

## Easy

- Power Function

## Medium

- Binary Exponentiation
- Modular Power

## Advanced

- Matrix Exponentiation
- Fibonacci using Matrix Power

---

# Next Topic

10-Display-Array.md

We now move from mathematical recursion to array recursion.

Topics:

- Traversing arrays recursively
- Way Down traversal
- Foundation for array-based recursion problems
