# Introduction to Recursion

## What is Recursion?

Recursion is a programming technique where a function calls itself to solve a smaller version of the same problem.

Instead of solving the entire problem at once, recursion breaks it into smaller subproblems until a stopping condition (Base Case) is reached.

---

## Why Learn Recursion?

Recursion is the foundation of:

- Backtracking
- Dynamic Programming
- Trees
- Graph DFS
- Divide and Conquer
- Many FAANG Interview Problems

Examples:

- N Queens
- Sudoku Solver
- Subsets
- Permutations
- Combination Sum
- Word Search

---

## Components of Recursion

Every recursive solution contains three parts:

### 1. Base Case

The condition where recursion stops.

```java
if(n == 0)
    return;
```

Without a base case:

```java
fun(n){
    fun(n - 1);
}
```

Result:

```text
StackOverflowError
```

---

### 2. Self Work

Work performed by the current function.

```java
System.out.println(n);
```

---

### 3. Recursive Call

Delegating the smaller problem.

```java
fun(n - 1);
```

---

## Basic Example

### Print Numbers from N to 1

```java
public class Main {

    static void printNumbers(int n){

        if(n == 0)
            return;

        System.out.println(n);

        printNumbers(n - 1);
    }

    public static void main(String[] args) {
        printNumbers(5);
    }
}
```

### Output

```text
5
4
3
2
1
```

---

## Visualization

### Function Calls

```text
printNumbers(5)
    |
    v
printNumbers(4)
    |
    v
printNumbers(3)
    |
    v
printNumbers(2)
    |
    v
printNumbers(1)
    |
    v
printNumbers(0)
```

Base Case reached.

Now calls start returning.

---

## Recursion Stack

During execution:

```text
----------------
print(0)
----------------
print(1)
----------------
print(2)
----------------
print(3)
----------------
print(4)
----------------
print(5)
----------------
```

After reaching Base Case:

```text
print(0) returns
print(1) returns
print(2) returns
print(3) returns
print(4) returns
print(5) returns
```

This process is called:

## Stack Unwinding

---

## Faith and Expectation

The most important concept in recursion.

Suppose:

```java
printNumbers(5);
```

Your responsibility:

```text
Print 5
```

Faith:

```text
printNumbers(4)
will correctly print:

4
3
2
1
```

Never think about how the recursive call works internally.

Trust it.

This trust is called Faith.

---

## Dry Run

### Input

```java
printNumbers(3);
```

### Execution

```text
printNumbers(3)
Print 3

    printNumbers(2)
    Print 2

        printNumbers(1)
        Print 1

            printNumbers(0)
            return
```

### Output

```text
3
2
1
```

---

## Recursive Thinking

Wrong Thinking:

```text
How will recursion do everything?
```

Correct Thinking:

```text
What is my work?

What smaller problem can I delegate?
```

Example:

```java
printNumbers(5);
```

Current Function Work:

```text
Print 5
```

Delegated Work:

```text
printNumbers(4)
```

---

## Time Complexity

Number of recursive calls:

```text
n
```

Therefore:

| Complexity Type | Value |
|----------------|--------|
| Time Complexity | O(n) |
| Space Complexity | O(n) |

Space is O(n) because recursion stack stores n function calls.

---

## Common Mistakes

### Mistake 1: Missing Base Case

```java
void fun(int n){
    fun(n - 1);
}
```

Result:

```text
StackOverflowError
```

---

### Mistake 2: Wrong Base Condition

```java
if(n == 1)
    return;
```

May fail for edge cases.

---

### Mistake 3: Infinite Recursion

```java
fun(n){
    fun(n + 1);
}
```

Problem size is increasing instead of decreasing.

---

## Interview Tip

Whenever you see:

- Subsets
- Permutations
- Combinations
- Maze Paths
- N Queens
- Sudoku

Think:

```text
Recursion + Backtracking
```

---

## Key Takeaways

- Every recursion requires a Base Case.
- Solve one small part yourself.
- Delegate the remaining work.
- Trust the recursive call (Faith).
- Recursion uses the Call Stack internally.
- Smaller Problem + Faith = Recursion.

---

## Related Problems

### Easy

- Print N to 1
- Print 1 to N
- Factorial
- Sum of First N Numbers

### Medium

- Fibonacci
- Power Function

### Advanced

- Subsequences
- Permutations
- N Queens
- Sudoku Solver
