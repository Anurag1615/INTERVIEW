# Maximum Element in Array Using Recursion

## Introduction

In previous chapters, recursion was used to:

- Print values
- Traverse arrays
- Understand Way Down and Way Up

Now we move to a different category:

```text
Return Type Recursion
```

Instead of printing values, we return answers.

This chapter introduces one of the most important recursion patterns:

```text
My Answer

=

Current Element

vs

Answer From Smaller Problem
```

This pattern appears in:

- Maximum Element
- Minimum Element
- First Index
- Last Index
- All Indices
- Tree Problems
- Dynamic Programming

---

# Problem Statement

Given:

```java
int[] arr = {10, 40, 20, 80, 30};
```

Find:

```text
80
```

using recursion.

---

# Recursive Thinking

Suppose:

```java
maxOfArray(arr, 0)
```

Current index:

```text
0
```

Current element:

```text
10
```

My expectation:

```java
maxOfArray(arr, 1)
```

returns:

```text
Maximum element
from remaining array.
```

Then:

```text
Final Answer

=
max(
    currentElement,
    answerFromSmallerProblem
)
```

---

# Faith and Expectation

For:

```java
maxOfArray(arr, 0)
```

Faith:

```java
maxOfArray(arr, 1)
```

correctly returns:

```text
Maximum element
from index 1 onward.
```

I don't care how.

I trust recursion.

---

# Recursive Relation

```text
maxOfArray(idx)

=

max(
    arr[idx],
    maxOfArray(idx + 1)
)
```

---

# Recursive Solution

```java
public static int maxOfArray(int[] arr, int idx){

    if(idx == arr.length - 1){
        return arr[idx];
    }

    int maxFromRest = maxOfArray(arr, idx + 1);

    return Math.max(arr[idx], maxFromRest);
}
```

---

# Base Case

At last element:

```java
idx == arr.length - 1
```

Only one element remains.

Maximum is:

```java
arr[idx]
```

Return it.

---

# Dry Run

Input:

```java
arr = {10,40,20,80,30}

maxOfArray(arr,0)
```

---

## Recursive Calls

```text
max(10, maxOfArray(1))

max(40, maxOfArray(2))

max(20, maxOfArray(3))

max(80, maxOfArray(4))

return 30
```

---

# Stack Unwinding

### idx = 4

```text
return 30
```

---

### idx = 3

```text
max(80,30)

=
80
```

Return:

```text
80
```

---

### idx = 2

```text
max(20,80)

=
80
```

Return:

```text
80
```

---

### idx = 1

```text
max(40,80)

=
80
```

Return:

```text
80
```

---

### idx = 0

```text
max(10,80)

=
80
```

Return:

```text
80
```

---

# Final Answer

```text
80
```

---

# Visualization

## Way Down

```text
max(0)

max(1)

max(2)

max(3)

max(4)
```

No comparison yet.

---

## Way Up

```text
30

max(80,30)

80

max(20,80)

80

max(40,80)

80

max(10,80)

80
```

Comparisons happen during:

```text
Way Up
```

---

# Recursion Tree

```text
max(0)
   |
max(1)
   |
max(2)
   |
max(3)
   |
max(4)
```

Linear recursion.

---

# Why Comparison Happens on Way Up?

Because:

```java
int maxFromRest =
        maxOfArray(arr, idx + 1);
```

must return first.

Only then:

```java
Math.max(...)
```

can be evaluated.

---

# Alternative Code

```java
public static int maxOfArray(int[] arr, int idx){

    if(idx == arr.length - 1)
        return arr[idx];

    return Math.max(
            arr[idx],
            maxOfArray(arr, idx + 1)
    );
}
```

Same logic.

---

# Complexity Analysis

Every element is visited once.

### Time Complexity

```text
O(n)
```

### Space Complexity

```text
O(n)
```

due to recursion stack.

---

# Iterative Solution

```java
int max = arr[0];

for(int i = 1; i < arr.length; i++){
    max = Math.max(max, arr[i]);
}
```

---

# Recursive vs Iterative

| Feature | Recursion | Iteration |
|----------|----------|----------|
| Readability | High | High |
| Stack Usage | O(n) | O(1) |
| Learning Value | Excellent | Medium |

---

# Common Mistakes

## Mistake 1

Wrong Base Case

Wrong:

```java
if(idx == arr.length)
```

Then:

```java
arr[idx]
```

causes:

```text
ArrayIndexOutOfBoundsException
```

---

Correct:

```java
if(idx == arr.length - 1)
```

---

## Mistake 2

Comparing Before Recursive Call

Wrong thinking:

```text
Compare now.
```

Need answer from smaller problem first.

---

## Mistake 3

Ignoring Faith

Wrong:

```text
How does recursion find max?
```

Correct:

```text
Assume recursion returns
max from remaining array.
```

---

# Pattern Recognition

Whenever you see:

```text
Current Element

vs

Answer From Remaining Array
```

Think:

```java
recursiveAnswer

=

function(idx + 1)
```

then combine answers.

---

# Interview Insight

This pattern appears everywhere:

### Maximum

```java
Math.max(current, rest)
```

### Minimum

```java
Math.min(current, rest)
```

### Sum

```java
current + rest
```

### Product

```java
current * rest
```

---

# Generic Template

```java
int answerFromRest =
        function(idx + 1);

return combine(
        current,
        answerFromRest
);
```

---

# Key Takeaways

- Maximum of Array is a return-type recursion problem.
- Faith: remaining array returns its maximum.
- Comparison happens during Way Up.
- Recursion combines current answer with smaller answer.
- Foundation for many advanced recursion problems.

---

# Practice Problems

## Easy

- Maximum Element
- Minimum Element

## Medium

- Sum of Array
- Product of Array

## Advanced

- First Index
- Last Index
- All Indices

---

# Next Topic

13-First-Index.md

Question:

```text
Find the first occurrence
of a target element
using recursion.
```

This introduces early return recursion.
