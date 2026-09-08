# Faith and Expectation in Recursion

## Introduction

Most beginners struggle with recursion because they try to understand every recursive call in detail.

This approach quickly becomes confusing.

The secret to mastering recursion is understanding:

- Faith
- Expectation

These two concepts are the foundation of recursive thinking.

---

## What is Faith?

Faith means trusting that the recursive call will correctly solve the smaller problem.

You do not need to know how it solves it.

You simply assume that it works.

---

## Example

Suppose we want to print:

```text
5 4 3 2 1
```

### Function

```java
void printNumbers(int n){

    if(n == 0)
        return;

    System.out.println(n);

    printNumbers(n - 1);
}
```

Call:

```java
printNumbers(5);
```

---

## What is My Work?

For:

```java
printNumbers(5)
```

Your work is only:

```text
Print 5
```

Remaining work:

```text
Print 4 3 2 1
```

Delegated to:

```java
printNumbers(4);
```

---

## Faith

You assume:

```java
printNumbers(4);
```

will correctly print:

```text
4
3
2
1
```

without worrying about its internal implementation.

This trust is called Faith.

---

## Visual Representation

```text
printNumbers(5)

My Work:
Print 5

Faith:
printNumbers(4)
will print

4
3
2
1
```

---

## Expectation

Expectation means:

"What should the recursive call return or accomplish?"

Before writing recursion, clearly define:

```text
What am I expecting from the smaller problem?
```

---

## Example: Factorial

### Problem

Find:

```text
5!
```

Mathematically:

```text
5! = 5 × 4!
```

---

### Recursive Function

```java
int factorial(int n){

    if(n == 0)
        return 1;

    return n * factorial(n - 1);
}
```

---

## Expectation Analysis

For:

```java
factorial(5)
```

Expectation:

```java
factorial(4)
```

should return:

```text
4!
```

You don't care how.

You only trust that it will.

---

## Dry Run

### factorial(5)

```text
5 * factorial(4)
```

Faith:

```text
factorial(4)
returns 24
```

Now:

```text
5 × 24
=
120
```

Answer:

```text
120
```

---

## Recursive Thinking Pattern

Always ask:

### Question 1

```text
What is my work?
```

### Question 2

```text
What smaller problem can I delegate?
```

### Question 3

```text
What should I expect from that smaller problem?
```

---

## Example: Sum of First N Numbers

Problem:

```text
1 + 2 + 3 + 4 + 5
```

---

### Recursive Relation

```text
sum(5)

=
5 + sum(4)
```

---

### Code

```java
int sum(int n){

    if(n == 0)
        return 0;

    return n + sum(n - 1);
}
```

---

## Faith Analysis

For:

```java
sum(5)
```

My Work:

```text
Add 5
```

Faith:

```text
sum(4)
returns

1 + 2 + 3 + 4
```

Result:

```text
5 + 10

=
15
```

---

## Example: Power Function

Problem:

```text
2^5
```

---

### Recursive Relation

```text
2^5

=
2 × 2^4
```

---

### Code

```java
int power(int x, int n){

    if(n == 0)
        return 1;

    return x * power(x, n - 1);
}
```

---

## Faith

For:

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

returns:

```text
2^4
```

---

## How Experts Think

Beginners:

```text
Let me trace all recursive calls.
```

Experts:

```text
What is my work?

What smaller problem can solve the rest?
```

This is the biggest difference.

---

## Faith Template

Whenever solving recursion:

```text
Step 1:
Assume recursion works.

Step 2:
Define smaller problem.

Step 3:
Use result of smaller problem.

Step 4:
Return final answer.
```

---

## Generic Template

```java
return myWork + recursiveCall(smallerProblem);
```

Examples:

Factorial:

```java
return n * factorial(n - 1);
```

Sum:

```java
return n + sum(n - 1);
```

Power:

```java
return x * power(x, n - 1);
```

---

## Common Mistakes

### Mistake 1

Trying to understand entire recursion tree first.

Wrong:

```text
How does factorial(4)
call factorial(3)?
```

Correct:

```text
Assume factorial(4)
returns correct answer.
```

---

### Mistake 2

Not defining expectation.

Wrong:

```text
Call recursion somehow.
```

Correct:

```text
Clearly define what recursion should return.
```

---

### Mistake 3

No smaller problem.

Wrong:

```java
fun(n){
    return fun(n);
}
```

Problem size never decreases.

---

## Interview Insight

Almost every recursion problem follows:

```text
My Work
+
Faith on Smaller Problem
=
Final Answer
```

Examples:

- Factorial
- Fibonacci
- Power
- Subsequences
- Maze Paths
- N Queens
- Sudoku

---

## Key Takeaways

- Faith is trust in the recursive call.
- Expectation defines what the recursive call should return.
- Never try to solve the entire problem yourself.
- Solve one small part.
- Delegate the rest.
- Smaller Problem + Faith = Recursion.

---

## Practice Problems

### Easy

- Factorial
- Sum of N Numbers
- Power Function

### Medium

- Fibonacci
- Climbing Stairs

### Advanced

- Subsequences
- Maze Paths
- N Queens
