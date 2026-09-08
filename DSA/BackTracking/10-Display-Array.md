# Display Array Using Recursion

## Introduction

Until now, we have solved recursion problems involving:

- Numbers
- Factorial
- Power
- ZigZag

Now we move to:

```text
Array Recursion
```

This chapter teaches how to traverse an array recursively.

Although the problem looks simple, it introduces a very important pattern:

```text
Index Based Recursion
```

This pattern is used in:

- Display Array
- Maximum Element
- First Index
- Last Index
- All Indices
- Subsequences

---

# Problem Statement

Given an array:

```java
int[] arr = {10, 20, 30, 40, 50};
```

Print:

```text
10
20
30
40
50
```

using recursion.

---

# Recursive Thinking

Suppose:

```java
display(arr, 0);
```

Current index:

```text
0
```

My Work:

```text
Print arr[0]
```

Faith:

```java
display(arr, 1)
```

will print the remaining array.

---

# Recursive Relation

```text
display(arr, idx)

=

Print Current Element

+

Display Remaining Elements
```

---

# Recursive Solution

```java
public static void display(int[] arr, int idx){

    if(idx == arr.length)
        return;

    System.out.println(arr[idx]);

    display(arr, idx + 1);
}
```

---

# Base Case

When:

```java
idx == arr.length
```

all elements have been processed.

Therefore:

```java
return;
```

---

# Dry Run

Input:

```java
arr = {10,20,30}

display(arr,0);
```

---

## Step 1

```text
idx = 0

Print 10
```

Call:

```java
display(arr,1)
```

---

## Step 2

```text
idx = 1

Print 20
```

Call:

```java
display(arr,2)
```

---

## Step 3

```text
idx = 2

Print 30
```

Call:

```java
display(arr,3)
```

---

## Base Case

```java
idx == arr.length
```

returns.

---

# Output

```text
10
20
30
```

---

# Visualization

## Recursive Calls

```text
display(0)

display(1)

display(2)

display(3)
```

---

## Recursion Tree

```text
display(0)
      |
display(1)
      |
display(2)
      |
display(3)
```

This is:

```text
Linear Recursion
```

---

# Way Down Analysis

Function:

```java
System.out.println(arr[idx]);

display(arr, idx + 1);
```

Printing happens before recursion.

Therefore:

```text
Output is generated on Way Down.
```

---

# Stack Visualization

Before Base Case:

```text
------------------
display(3)
------------------
display(2)
------------------
display(1)
------------------
display(0)
------------------
```

---

After Base Case:

```text
display(3) removed

display(2) removed

display(1) removed

display(0) removed
```

---

# Faith and Expectation

For:

```java
display(arr,0)
```

My Work:

```text
Print arr[0]
```

Faith:

```java
display(arr,1)
```

prints:

```text
Remaining Array
```

---

Example:

```java
display(arr,2)
```

My Work:

```text
Print arr[2]
```

Faith:

```java
display(arr,3)
```

prints everything after index 2.

---

# Time Complexity

Every element is visited once.

Therefore:

| Metric | Complexity |
|----------|----------|
| Time Complexity | O(n) |
| Space Complexity | O(n) |

where:

```text
n = array length
```

---

# Iterative Solution

```java
for(int i = 0; i < arr.length; i++){
    System.out.println(arr[i]);
}
```

---

# Recursive vs Iterative

| Feature | Recursion | Iteration |
|----------|----------|----------|
| Readability | Good | Excellent |
| Stack Usage | O(n) | O(1) |
| Learning Value | High | Low |
| Interview Importance | High | Medium |

---

# Common Mistakes

## Mistake 1

Wrong Base Case

Wrong:

```java
if(idx == arr.length - 1)
    return;
```

Last element never gets printed.

---

Correct:

```java
if(idx == arr.length)
    return;
```

---

## Mistake 2

Forgetting Recursive Call

Wrong:

```java
System.out.println(arr[idx]);
```

Only first element gets printed.

---

## Mistake 3

Increasing Wrong Index

Wrong:

```java
display(arr, idx);
```

Creates infinite recursion.

---

Correct:

```java
display(arr, idx + 1);
```

---

# Pattern Recognition

Whenever you see:

```text
Array + Index
```

Think:

```java
process(arr[idx]);

recursiveCall(idx + 1);
```

This pattern appears repeatedly.

---

# Interview Insight

Most array recursion problems follow:

```java
function(arr, idx)
```

where:

```text
idx represents current position
```

and recursion handles:

```text
remaining array
```

---

# Key Takeaways

- Array recursion is usually index-based.
- Process current element.
- Delegate remaining array.
- Base Case:

```java
idx == arr.length
```

- This is a Way Down problem.
- Foundation for all array recursion questions.

---

# Practice Problems

## Easy

- Display Array
- Display Reverse Array

## Medium

- Maximum Element
- Minimum Element

## Advanced

- First Index
- Last Index
- All Indices

---

# Next Topic

11-Display-Array-Reverse.md

We will learn how changing the position of work relative to the recursive call changes the output order.

This introduces:

```text
Way Up Traversal
```
