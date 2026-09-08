# All Indices Using Recursion

## Introduction

In previous chapters, we solved:

- First Index
- Last Index

Both problems returned:

```text
A Single Answer
```

Now we solve a more interesting problem.

We need:

```text
All occurrences of a target element.
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

This problem is extremely important because it introduces:

- Building Arrays During Recursion
- Count Parameter Pattern
- Result Construction on Way Up
- Faith + Array Creation

---

# Problem Statement

Given:

```java
int[] arr = {10,20,30,20,40,20};
```

Target:

```java
20
```

Return:

```text
[1,3,5]
```

using recursion.

---

# Why Is This Problem Special?

Previous problems returned:

```java
int
```

Example:

```java
return index;
```

Now we need:

```java
int[]
```

and we do not know the size beforehand.

---

# Key Idea

While moving forward:

```text
Count how many matches
have been found.
```

When reaching the base case:

```text
Create array of exact size.
```

Then:

```text
Fill answers while returning.
```

---

# Function Signature

```java
public static int[] allIndices(
        int[] arr,
        int idx,
        int target,
        int count)
```

---

# Parameters

| Parameter | Meaning |
|------------|------------|
| arr | Input Array |
| idx | Current Index |
| target | Element To Find |
| count | Matches Found So Far |

---

# Recursive Solution

```java
public static int[] allIndices(
        int[] arr,
        int idx,
        int target,
        int count){

    if(idx == arr.length){
        return new int[count];
    }

    if(arr[idx] == target){

        int[] answer =
                allIndices(
                        arr,
                        idx + 1,
                        target,
                        count + 1
                );

        answer[count] = idx;

        return answer;
    }

    return allIndices(
            arr,
            idx + 1,
            target,
            count
    );
}
```

---

# Understanding The Logic

Suppose:

```java
arr = {10,20,30,20}
target = 20
```

Matches:

```text
Index 1
Index 3
```

Total:

```text
2
```

At Base Case:

```java
return new int[2];
```

Array created:

```text
[_, _]
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
allIndices(arr,0,20,0)
```

---

## idx = 0

```text
10 != 20
```

Count remains:

```text
0
```

---

## idx = 1

```text
20 == 20
```

Count becomes:

```text
1
```

---

## idx = 2

```text
30 != 20
```

Count remains:

```text
1
```

---

## idx = 3

```text
20 == 20
```

Count becomes:

```text
2
```

---

## Base Case

```java
idx == arr.length
```

Return:

```java
new int[2]
```

Array:

```text
[_, _]
```

---

# Stack Unwinding

## idx = 3

Current count:

```text
1
```

Store:

```java
answer[1] = 3;
```

Array:

```text
[_, 3]
```

---

## idx = 1

Current count:

```text
0
```

Store:

```java
answer[0] = 1;
```

Array:

```text
[1, 3]
```

---

# Final Answer

```text
[1,3]
```

---

# Visualization

## Way Down

```text
idx=0 count=0

idx=1 count=0

idx=2 count=1

idx=3 count=1

idx=4 count=2
```

---

## Base Case

```java
new int[2]
```

created.

---

## Way Up

```text
Store 3

Store 1
```

Answer built while returning.

---

# Why Does answer[count] Work?

At:

```java
idx = 1
```

Count was:

```text
0
```

Meaning:

```text
This is the 1st occurrence.
```

Store at:

```java
answer[0]
```

---

At:

```java
idx = 3
```

Count was:

```text
1
```

Meaning:

```text
This is the 2nd occurrence.
```

Store at:

```java
answer[1]
```

---

# Recursion Tree

```text
all(0,0)
     |
all(1,0)
     |
all(2,1)
     |
all(3,1)
     |
all(4,2)
```

Linear recursion.

---

# Complexity Analysis

Each element visited once.

---

## Time Complexity

```text
O(n)
```

---

## Space Complexity

Recursion stack:

```text
O(n)
```

Result array:

```text
O(k)
```

where:

```text
k = number of matches
```

Total:

```text
O(n + k)
```

---

# Common Mistakes

## Mistake 1

Creating Array Too Early

Wrong:

```java
new int[arr.length]
```

Wastes memory.

---

Correct:

```java
new int[count]
```

at Base Case.

---

## Mistake 2

Using idx Instead of count

Wrong:

```java
answer[idx] = idx;
```

Can cause:

```text
ArrayIndexOutOfBoundsException
```

---

Correct:

```java
answer[count] = idx;
```

---

## Mistake 3

Incrementing Count Incorrectly

Wrong:

```java
count++;
```

inside recursive call arguments.

---

Correct:

```java
count + 1
```

---

# Pattern Recognition

Whenever question asks:

```text
Return all answers
```

Think:

```text
Count First

↓

Create Result

↓

Fill During Way Up
```

---

# Interview Insight

This problem teaches:

```text
Result Construction Recursion
```

which appears in:

- Subsequences
- Keypad Combinations
- Maze Paths
- N Queens
- Backtracking

Understanding this problem makes advanced recursion significantly easier.

---

# Generic Template

```java
Base Case:
Create Result

Way Up:
Fill Result

Return Result
```

---

# Key Takeaways

- Count tracks total matches.
- Array created at Base Case.
- Answer built during Way Up.
- Exact-sized array avoids wasted memory.
- One of the most important recursion patterns.

---

# Practice Problems

## Easy

- All Indices
- All Character Positions

## Medium

- Collect All Even Numbers
- Collect All Matching Elements

## Advanced

- Subsequences
- Keypad Combinations
- Maze Paths
- N Queens

---

# Next Topic

16-Get-Subsequence-GSS.md

Question:

```text
Generate all subsequences
of a string.
```

Example:

```text
abc
```

Output:

```text
""
"a"
"b"
"c"
"ab"
"ac"
"bc"
"abc"
```

This is the chapter where real recursion and backtracking begin.
