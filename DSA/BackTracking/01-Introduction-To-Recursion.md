# Introduction to Recursion

## What is Recursion?

Recursion is a programming technique in which a function calls itself to solve a smaller version of the same problem.

---

## Basic Structure

Every recursive solution contains:

1. Base Case
2. Self Work
3. Recursive Call

```java
void solve(int n) {

    // Base Case
    if(n == 0)
        return;

    // Self Work
    System.out.println(n);

    // Recursive Call
    solve(n - 1);
}
```

---

## Example

```java
public class Main {

    static void printNumbers(int n){

        if(n == 0)
            return;

        System.out.println(n);

        printNumbers(n - 1);
    }

    public static void main(String[] args){
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

## Recursion Tree

```text
print(5)
 |
 +-- print(4)
      |
      +-- print(3)
            |
            +-- print(2)
                  |
                  +-- print(1)
                        |
                        +-- print(0)
```

---

## Time Complexity

| Metric | Value |
|----------|----------|
| Time Complexity | O(n) |
| Space Complexity | O(n) |

---

## Common Mistakes

### Missing Base Case

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

## Key Takeaways

- Every recursion needs a base case.
- Trust the recursive call (Faith).
- Think in terms of smaller subproblems.
- Recursion uses the call stack internally.
