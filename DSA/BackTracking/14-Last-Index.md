# Last Index Using Recursion

## Introduction

In the previous chapter, we solved:

```text
First Index
```

using:

```text
Check Current First
```

and then recurse.

For Last Index, the strategy changes completely.

We need:

```text
Last Occurrence
```

which means we cannot decide immediately.

Instead:

```text
Search Remaining Array First
```

and make the decision during:

```text
Way Up
```

This chapter introduces one of the most important recursion patterns:

```text
Recursive Call First

↓

Decision Later
```

This pattern appears heavily in:

- Backtracking
- DFS
- Tree Problems
- Dynamic Programming

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
3
```

because the last occurrence of:

```text
20
```

is at index:

```text
3
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
3
```

---

# Example 3

Input:

```java
arr = {1,2,3}
target = 10
```

Output:

```text
-1
```

Target does not exist.

---

# Why First Index Logic Fails?

For:

```java
arr = {20,20,20}
```

If we check current first:

```java
if(arr[idx] == target)
    return idx;
```

Answer becomes:

```text
0
```

which is incorrect.

Need:

```text
2
```

---

# Key Observation

To find last occurrence:

```text
Search the remaining array first.
```

If target exists later:

```text
Use that answer.
```

Otherwise:

```text
Use current index.
```

---

# Faith and Expectation

Suppose:

```java
lastIndex(arr,0,target)
```

Faith:

```java
lastIndex(arr,1,target)
```

returns:

```text
Last occurrence
in remaining array.
```

I trust recursion.

---

# Recursive Relation

```text
Ask recursion for answer.

If recursion found target:
    return recursion answer

Else:
    check current index
```

---

# Recursive Solution

```java
public static int lastIndex(
        int[] arr,
        int idx,
        int target){

    if(idx == arr.length)
        return -1;

    int answerFromRest =
            lastIndex(
                    arr,
                    idx + 1,
                    target
            );

    if(answerFromRest != -1){
        return answerFromRest;
    }

    if(arr[idx] == target){
        return idx;
    }

    return -1;
}
```

---

# Base Case

When:

```java
idx == arr.length
```

Target not found.

Return:

```java
-1;
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
lastIndex(arr,0,20)
```

---

# Recursive Calls

```text
last(0)

last(1)

last(2)

last(3)

last(4)
```

---

# Base Case

```java
last(4)
```

returns:

```text
-1
```

---

# Stack Unwinding

## idx = 3

```text
answerFromRest = -1

arr[3] = 20
```

Return:

```text
3
```

---

## idx = 2

```text
answerFromRest = 3
```

Already found later.

Return:

```text
3
```

---

## idx = 1

```text
answerFromRest = 3
```

Return:

```text
3
```

---

## idx = 0

```text
answerFromRest = 3
```

Return:

```text
3
```

---

# Final Answer

```text
3
```

---

# Visualization

## Way Down

```text
last(0)

last(1)

last(2)

last(3)

last(4)
```

No decisions yet.

---

## Way Up

```text
idx = 3 → Found

idx = 2 → Keep 3

idx = 1 → Keep 3

idx = 0 → Keep 3
```

Decision making happens during:

```text
Way Up
```

---

# Another Example

Input:

```java
arr = {20,20,20}
target = 20
```

---

# Calls

```text
last(0)

last(1)

last(2)

last(3)
```

---

# Unwinding

```text
idx=2 → return 2

idx=1 → return 2

idx=0 → return 2
```

Answer:

```text
2
```

Correct.

---

# Why This Works?

Recursion explores:

```text
Right Side First
```

Therefore:

```text
Last occurrence
is discovered first.
```

Then answer propagates upward.

---

# Complexity Analysis

All elements may be visited.

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

# Common Mistakes

## Mistake 1

Checking Current Before Recursion

Wrong:

```java
if(arr[idx] == target)
    return idx;
```

This becomes:

```text
First Index
```

logic.

---

## Mistake 2

Ignoring Recursive Answer

Wrong:

```java
return idx;
```

Need to respect later occurrences.

---

## Mistake 3

Wrong Base Case

Wrong:

```java
idx == arr.length - 1
```

Can miss answers.

---

Correct:

```java
idx == arr.length
```

---

# Pattern Recognition

Whenever question asks:

```text
Last Occurrence
Rightmost Answer
Latest Match
```

Think:

```text
Recurse First

↓

Decide Later
```

---

# Interview Insight

This is a classic:

```text
Way Up Decision
```

problem.

Very important because:

- Backtracking uses it.
- DFS uses it.
- Tree algorithms use it.

Understanding this pattern makes advanced recursion much easier.

---

# Generic Template

```java
answerFromRest =
        recursion(smallerProblem);

if(answerFromRest exists)
    return answerFromRest;

return currentAnswer;
```

---

# Comparison

| Problem | Strategy |
|----------|----------|
| First Index | Check First |
| Last Index | Recurse First |
| First Match | Way Down |
| Last Match | Way Up |

---

# Key Takeaways

- Last Index is solved during Way Up.
- Recursive answer gets priority.
- Current index is considered only if recursion fails.
- This pattern appears in many advanced problems.
- Foundation for backtracking decisions.

---

# Practice Problems

## Easy

- Last Index
- Last Character Occurrence

## Medium

- Last Node Matching Condition
- Rightmost DFS Search

## Advanced

- Backtracking State Restoration
- Tree Path Problems

---

# Next Topic

15-All-Indices.md

Question:

```text
Return all indices
where target occurs.
```

Example:

```java
arr = {10,20,30,20,40,20}
target = 20
```

Output:

```text
[1,3,5]
```

This is one of the most important Pepcoding recursion problems because it teaches:

- Dynamic Result Construction
- Array Creation During Way Up
- Faith + Counting Pattern
