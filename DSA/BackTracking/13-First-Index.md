# First Index Using Recursion

## Introduction

In the previous chapter, we learned how to return a value from recursion by combining:

```text
Current Element

+

Answer From Smaller Problem
```

Now we will solve a searching problem.

Given an array and a target value, find the:

```text
First Occurrence Index
```

using recursion.

This chapter introduces:

- Early Return Pattern
- Search Recursion
- Short-Circuiting
- Recursive Delegation

---

# Problem Statement

Given:

```java
int[] arr = {10,20,30,20,40};
```

Target:

```java
20
```

Output:

```text
1
```

because first occurrence of:

```text
20
```

is at index:

```text
1
```

---

# Example 2

Input:

```java
arr = {5,8,7,8,10}
target = 8
```

Output:

```text
1
```

---

# Example 3

Input:

```java
arr = {1,2,3,4}
target = 10
```

Output:

```text
-1
```

Target does not exist.

---

# Recursive Thinking

Suppose:

```java
firstIndex(arr, 0, target)
```

Current index:

```text
0
```

Question:

```text
Can I answer right now?
```

If:

```java
arr[idx] == target
```

then yes.

Return immediately.

Otherwise:

```text
Delegate remaining array
to recursion.
```

---

# Faith and Expectation

For:

```java
firstIndex(arr, idx, target)
```

Faith:

```java
firstIndex(arr, idx + 1, target)
```

will correctly find the first occurrence in the remaining array.

---

# Important Observation

Since we need:

```text
First Occurrence
```

we must check:

```java
current element first
```

before recursion.

---

# Recursive Relation

```text
If current element matches

Return current index

Else

Search remaining array
```

---

# Recursive Solution

```java
public static int firstIndex(
        int[] arr,
        int idx,
        int target){

    if(idx == arr.length)
        return -1;

    if(arr[idx] == target)
        return idx;

    return firstIndex(
            arr,
            idx + 1,
            target
    );
}
```

---

# Base Case

When:

```java
idx == arr.length
```

entire array has been searched.

Target not found.

Return:

```java
-1
```

---

# Dry Run

Input:

```java
arr = {10,20,30,20}
target = 20
```

Call:

```java
firstIndex(arr,0,20)
```

---

## idx = 0

```text
10 != 20
```

Search remaining array.

---

## idx = 1

```text
20 == 20
```

Return:

```text
1
```

---

# Stack Unwinding

```text
return 1
```

propagates back through all calls.

Final answer:

```text
1
```

---

# Visualization

## Recursive Calls

```text
firstIndex(0)

firstIndex(1)

FOUND
```

Execution stops immediately.

---

# Another Dry Run

Input:

```java
arr = {5,8,7,8}
target = 8
```

---

## idx = 0

```text
5 != 8
```

Go deeper.

---

## idx = 1

```text
8 == 8
```

Return:

```text
1
```

---

# Target Not Present

Input:

```java
arr = {1,2,3}
target = 5
```

---

## Calls

```text
idx = 0

idx = 1

idx = 2

idx = 3
```

Base Case:

```java
return -1;
```

---

# Recursion Tree

```text
first(0)
    |
first(1)
    |
first(2)
    |
first(3)
```

Linear recursion.

---

# Why Check Before Recursion?

Suppose:

```java
arr = {20,20,20}
```

Need:

```text
First Occurrence
```

Checking current index first guarantees:

```text
Leftmost match
```

is returned.

---

# Wrong Approach

```java
int ans =
    firstIndex(arr, idx + 1, target);

if(arr[idx] == target)
    return idx;
```

This behaves more like:

```text
Last Occurrence Logic
```

and is inefficient.

---

# Complexity Analysis

Worst case:

```text
Target not present
```

All elements visited.

---

## Time Complexity

```text
O(n)
```

---

## Space Complexity

```text
O(n)
```

due to recursion stack.

---

# Iterative Solution

```java
for(int i = 0; i < arr.length; i++){

    if(arr[i] == target)
        return i;
}

return -1;
```

---

# Common Mistakes

## Mistake 1

Wrong Base Case

Wrong:

```java
if(idx == arr.length - 1)
```

Last element may never be checked.

---

Correct:

```java
if(idx == arr.length)
```

---

## Mistake 2

Checking After Recursion

Wrong for First Index.

Need:

```java
Current Check

↓

Recursive Call
```

---

## Mistake 3

Returning Wrong Value

Wrong:

```java
return idx + 1;
```

Return exact index.

---

# Pattern Recognition

Whenever question asks:

```text
First Occurrence
First Match
First Valid Position
```

Think:

```text
Check Current First
```

Then recurse.

---

# Interview Insight

This pattern is called:

```text
Early Return Recursion
```

because recursion can stop as soon as answer is found.

Very common in:

- Searching
- DFS
- Trees
- Graphs
- Backtracking

---

# Generic Template

```java
if(current satisfies condition)
    return answer;

return recursion(smallerProblem);
```

---

# Key Takeaways

- First Index checks current element first.
- Found answer returns immediately.
- No need to search remaining array once answer is found.
- Uses Early Return Pattern.
- Foundation for recursive searching problems.

---

# Practice Problems

## Easy

- First Index
- First Character Occurrence

## Medium

- Search in Linked List
- First Node Matching Condition

## Advanced

- DFS Search
- Path Finding Problems

---

# Next Topic

14-Last-Index.md

Question:

```text
Find the last occurrence
of a target element.
```

Unlike First Index:

```text
Recursion must search first
and decide later.
```

This introduces:

```text
Way Up Decision Making
```
