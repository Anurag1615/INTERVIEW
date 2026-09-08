# Display Array in Reverse Using Recursion

## Introduction

In the previous chapter, we displayed an array in normal order:

```text
10
20
30
40
50
```

using recursion.

Now we will print the same array in reverse order:

```text
50
40
30
20
10
```

without using loops.

This chapter is extremely important because it teaches:

- Way Up Execution
- Stack Unwinding
- Post Area Logic
- Foundation of Backtracking

---

# Problem Statement

Given:

```java
int[] arr = {10,20,30,40,50};
```

Print:

```text
50
40
30
20
10
```

using recursion.

---

# Key Observation

Previous chapter:

```java
System.out.println(arr[idx]);

display(arr, idx + 1);
```

Output:

```text
10
20
30
40
50
```

Why?

Because printing happens before recursion.

---

Now let's move print statement after recursion:

```java
displayReverse(arr, idx + 1);

System.out.println(arr[idx]);
```

Something interesting happens.

---

# Recursive Solution

```java
public static void displayReverse(int[] arr, int idx){

    if(idx == arr.length)
        return;

    displayReverse(arr, idx + 1);

    System.out.println(arr[idx]);
}
```

---

# Base Case

When:

```java
idx == arr.length
```

all elements are processed.

Return:

```java
return;
```

---

# Dry Run

Input:

```java
arr = {10,20,30}

displayReverse(arr,0);
```

---

## Recursive Calls

```text
displayReverse(0)

displayReverse(1)

displayReverse(2)

displayReverse(3)
```

---

## Base Case

```java
displayReverse(3)
```

returns.

---

# Stack Unwinding

Now execution starts returning.

---

## Return to idx = 2

```text
Print 30
```

---

## Return to idx = 1

```text
Print 20
```

---

## Return to idx = 0

```text
Print 10
```

---

# Output

```text
30
20
10
```

Reverse order achieved.

---

# Visualization

## Way Down

```text
idx=0

idx=1

idx=2

idx=3
```

No output generated.

---

## Way Up

```text
idx=2 → Print 30

idx=1 → Print 20

idx=0 → Print 10
```

Output is generated here.

---

# Recursion Tree

```text
display(0)
      |
display(1)
      |
display(2)
      |
display(3)
```

---

# Important Insight

Function:

```java
displayReverse(arr, idx + 1);

System.out.println(arr[idx]);
```

The recursive call finishes first.

Only then current element gets printed.

Therefore:

```text
Last Element Prints First
```

which naturally produces reverse order.

---

# Faith and Expectation

Suppose:

```java
displayReverse(arr,0)
```

My Faith:

```java
displayReverse(arr,1)
```

will print:

```text
20
30
40
50
```

in reverse order.

After that:

```java
System.out.println(arr[0]);
```

prints:

```text
10
```

---

# Way Down vs Way Up

## Display Array

```java
System.out.println(arr[idx]);

display(arr, idx + 1);
```

Output:

```text
10
20
30
40
50
```

Generated During:

```text
Way Down
```

---

## Display Reverse

```java
displayReverse(arr, idx + 1);

System.out.println(arr[idx]);
```

Output:

```text
50
40
30
20
10
```

Generated During:

```text
Way Up
```

---

# Stack Visualization

Before Base Case:

```text
-----------------------
displayReverse(5)
-----------------------
displayReverse(4)
-----------------------
displayReverse(3)
-----------------------
displayReverse(2)
-----------------------
displayReverse(1)
-----------------------
displayReverse(0)
-----------------------
```

---

After Base Case:

```text
Print 50

Print 40

Print 30

Print 20

Print 10
```

while stack frames are removed.

---

# Time Complexity

Each element is visited once.

Therefore:

| Metric | Complexity |
|----------|----------|
| Time Complexity | O(n) |
| Space Complexity | O(n) |

---

# Iterative Solution

```java
for(int i = arr.length - 1; i >= 0; i--){
    System.out.println(arr[i]);
}
```

---

# Why Learn Recursive Version?

Because this pattern appears in:

- Backtracking
- DFS
- Tree Traversals
- N Queens
- Maze Paths
- Sudoku Solver

---

# Common Mistakes

## Mistake 1

Printing before recursion.

Wrong:

```java
System.out.println(arr[idx]);

displayReverse(arr, idx + 1);
```

Produces normal order.

---

## Mistake 2

Wrong Base Case.

Wrong:

```java
if(idx == arr.length - 1)
    return;
```

Last element skipped.

---

Correct:

```java
if(idx == arr.length)
    return;
```

---

## Mistake 3

Confusing Way Up with Way Down.

Remember:

```java
Before Recursion
```

means:

```text
Way Down
```

---

```java
After Recursion
```

means:

```text
Way Up
```

---

# Pattern Recognition

Whenever you see:

```java
recursiveCall();

work;
```

Think:

```text
Way Up Logic
```

---

Whenever you see:

```java
work;

recursiveCall();
```

Think:

```text
Way Down Logic
```

---

# Interview Insight

Many backtracking solutions work exactly like:

```java
recursiveCall();

undo work;
```

The undo operation executes during:

```text
Way Up
```

This is the foundation of:

- Backtracking
- DFS Reversal
- Path Construction

---

# Key Takeaways

- Display Reverse uses Way Up execution.
- Recursive call finishes first.
- Current work happens during stack unwinding.
- Output becomes naturally reversed.
- Foundation of backtracking.
- Extremely important recursion pattern.

---

# Practice Problems

## Easy

- Display Array
- Display Reverse

## Medium

- Reverse String using Recursion
- Reverse Linked List

## Advanced

- Backtracking Problems
- DFS Path Reconstruction

---

# Next Topic

12-Max-Of-Array.md

We will now move from printing values to returning values.

Question:

```text
What is the maximum element in an array?
```

This is where Faith and Expectation become extremely important.
