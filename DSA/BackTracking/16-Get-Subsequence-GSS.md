# Get Subsequence (GSS)

## Introduction

This is one of the most important recursion problems.

Almost every backtracking problem is built on the same idea.

Topics that directly use this pattern:

- Subsets (LeetCode 78)
- Subsets II
- Combination Sum
- Target Sum
- N Queens
- Partition Problems
- Backtracking

If you understand GSS properly, recursion becomes much easier.

---

# What is a Subsequence?

A subsequence is formed by:

```text
Keeping some characters
Removing some characters
```

while maintaining relative order.

Example:

```text
abc
```

Possible subsequences:

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

Total:

```text
2^3 = 8
```

---

# Problem Statement

Given:

```text
abc
```

Return:

```text
[
"",
"c",
"b",
"bc",
"a",
"ac",
"ab",
"abc"
]
```

---

# The Most Important Observation

For every character:

```text
Take it

OR

Don't take it
```

Only two choices exist.

Example:

```text
abc
 ^
```

For character:

```text
a
```

Choice 1:

```text
Include a
```

Choice 2:

```text
Exclude a
```

This is called:

```text
Include / Exclude Pattern
```

---

# Recursive Thinking

Suppose:

```text
abc
```

Current character:

```text
a
```

Remaining string:

```text
bc
```

Faith:

```java
gss("bc")
```

returns all subsequences of:

```text
bc
```

Then:

```text
I will create two groups

1. Without a
2. With a
```

---

# Faith and Expectation

For:

```java
gss("abc")
```

Expectation:

```java
gss("bc")
```

returns:

```text
[
"",
"c",
"b",
"bc"
]
```

Now my job is simple.

Create:

```text
Without a

+
With a
```

---

# Recursive Relation

```text
Subsequence(s)

=

Subsequence(rest)

+

Current Character
added to every answer
```

---

# Recursive Code

```java
public static ArrayList<String> gss(String str){

    if(str.length() == 0){

        ArrayList<String> base =
                new ArrayList<>();

        base.add("");

        return base;
    }

    char ch = str.charAt(0);

    String ros = str.substring(1);

    ArrayList<String> rres =
            gss(ros);

    ArrayList<String> myres =
            new ArrayList<>();

    for(String s : rres){
        myres.add(s);
    }

    for(String s : rres){
        myres.add(ch + s);
    }

    return myres;
}
```

---

# Base Case

When:

```java
str.length() == 0
```

Return:

```java
[""]
```

Why?

Because:

```text
Empty string
has one subsequence

""
```

---

# Dry Run

Input:

```java
gss("abc")
```

---

# Step 1

Current Character:

```text
a
```

Remaining String:

```text
bc
```

Need:

```java
gss("bc")
```

---

# Step 2

Current Character:

```text
b
```

Remaining:

```text
c
```

Need:

```java
gss("c")
```

---

# Step 3

Current Character:

```text
c
```

Remaining:

```text
""
```

Need:

```java
gss("")
```

---

# Base Case

Returns:

```text
[""]
```

---

# Stack Unwinding

## Processing c

Recursive Result:

```text
[""]
```

Without c:

```text
[""]
```

With c:

```text
["c"]
```

Combined:

```text
["", "c"]
```

Return.

---

## Processing b

Recursive Result:

```text
["", "c"]
```

Without b:

```text
["", "c"]
```

With b:

```text
["b", "bc"]
```

Combined:

```text
["", "c", "b", "bc"]
```

Return.

---

## Processing a

Recursive Result:

```text
["", "c", "b", "bc"]
```

Without a:

```text
["", "c", "b", "bc"]
```

With a:

```text
["a", "ac", "ab", "abc"]
```

Combined:

```text
[
"",
"c",
"b",
"bc",
"a",
"ac",
"ab",
"abc"
]
```

---

# Final Answer

```text
[
"",
"c",
"b",
"bc",
"a",
"ac",
"ab",
"abc"
]
```

---

# Recursion Tree

```text
                 abc
               /     \
             bc       bc
          Exclude A  Include A
```

Expand further:

```text
                     abc
                  /       \
                bc         bc
              /   \      /    \
             c     c    c      c
```

Each level:

```text
Include

OR

Exclude
```

---

# Number of Subsequences

For every character:

```text
2 choices
```

For n characters:

```text
2 × 2 × 2 × ...

n times
```

Result:

```text
2^n
```

---

# Complexity Analysis

## Time Complexity

Number of subsequences:

```text
2^n
```

Generating each one requires work.

Therefore:

```text
O(n × 2^n)
```

---

## Space Complexity

Recursion Depth:

```text
O(n)
```

Result Storage:

```text
O(2^n)
```

---

# Pattern Recognition

Whenever you see:

```text
Take

OR

Don't Take
```

Think:

```text
Include / Exclude
```

---

# Generic Template

```java
include current

exclude current
```

or

```java
left subtree

right subtree
```

---

# Connection to LeetCode

This exact pattern appears in:

### LeetCode 78

```text
Subsets
```

---

### LeetCode 90

```text
Subsets II
```

---

### Combination Sum

```text
Take

Skip
```

---

### Target Sum

```text
+num

-num
```

---

### Partition Problems

```text
Group A

Group B
```

---

# Common Mistakes

## Mistake 1

Wrong Base Case

Wrong:

```java
return [];
```

Correct:

```java
[""]
```

---

## Mistake 2

Modifying Recursive Result Directly

Always create:

```java
myres
```

instead of changing:

```java
rres
```

---

## Mistake 3

Not Understanding Include/Exclude

Every character creates:

```text
Two branches
```

No exceptions.

---

# Interview Insight

Whenever a problem says:

```text
Generate All

Find All

List All Possibilities
```

Think:

```text
Decision Tree
```

and usually:

```text
Include / Exclude
```

---

# Key Takeaways

- GSS is the foundation of backtracking.
- Every character has two choices.
- Include / Exclude creates a binary recursion tree.
- Total subsequences = 2^n.
- Directly maps to Subsets and Combination Sum.
- One of the most important recursion patterns.

---

# Practice Problems

## Easy

- Generate Subsequences
- Generate Binary Strings

## Medium

- LeetCode 78 (Subsets)
- LeetCode 90 (Subsets II)

## Advanced

- Combination Sum
- Target Sum
- N Queens

---

# Next Topic

17-Get-KPC.md

Question:

```text
Given digits:

"23"

Generate all keypad combinations.
```

Example:

```text
2 -> abc
3 -> def
```

Output:

```text
ad
ae
af
bd
be
bf
cd
ce
cf
```

This chapter introduces:

```text
Faith + Cartesian Product Pattern
```

which is another major recursion pattern.
