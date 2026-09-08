# Get Keypad Combinations (KPC)

## Introduction

In the previous chapter (GSS), every character had:

```text
Include

OR

Exclude
```

which created:

```text
2 choices per character
```

Now we learn a new recursion pattern.

Instead of:

```text
2 choices
```

a digit can have:

```text
3 choices
4 choices
5 choices
...
```

depending on the keypad mapping.

This chapter introduces:

```text
Faith + Cartesian Product Pattern
```

This pattern appears in:

- LeetCode 17 (Letter Combinations of a Phone Number)
- String Generation Problems
- DFS Enumeration
- Combination Builders

---

# Problem Statement

Given:

```text
"23"
```

Keypad Mapping:

```text
2 -> abc
3 -> def
```

Generate all combinations.

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

---

# Keypad Mapping

Pepcoding mapping:

```java
String[] codes = {
    ".;",
    "abc",
    "def",
    "ghi",
    "jkl",
    "mno",
    "pqrs",
    "tu",
    "vwx",
    "yz"
};
```

---

# Understanding the Problem

Input:

```text
23
```

Digit:

```text
2 -> abc
```

Digit:

```text
3 -> def
```

Possible combinations:

```text
a + d
a + e
a + f

b + d
b + e
b + f

c + d
c + e
c + f
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

---

# Faith and Expectation

Suppose:

```java
getKPC("23")
```

Current Digit:

```text
2
```

Remaining Digits:

```text
3
```

Faith:

```java
getKPC("3")
```

returns:

```text
[d,e,f]
```

Now my work:

```text
Attach

a
b
c

to every answer.
```

---

# Recursive Relation

```text
Current Digit Characters

×

Combinations From Remaining Digits
```

This is called:

```text
Cartesian Product
```

---

# Recursive Solution

```java
public static ArrayList<String> getKPC(String str){

    if(str.length() == 0){

        ArrayList<String> base =
                new ArrayList<>();

        base.add("");

        return base;
    }

    char ch = str.charAt(0);

    String ros = str.substring(1);

    ArrayList<String> rres =
            getKPC(ros);

    ArrayList<String> myres =
            new ArrayList<>();

    String code =
            codes[ch - '0'];

    for(int i = 0; i < code.length(); i++){

        char option = code.charAt(i);

        for(String s : rres){

            myres.add(option + s);
        }
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
No digits left.

One empty combination exists.
```

---

# Dry Run

Input:

```java
getKPC("23")
```

---

# Step 1

Current Digit:

```text
2
```

Characters:

```text
abc
```

Need:

```java
getKPC("3")
```

---

# Step 2

Current Digit:

```text
3
```

Characters:

```text
def
```

Need:

```java
getKPC("")
```

---

# Base Case

Returns:

```text
[""]
```

---

# Processing Digit 3

Characters:

```text
d
e
f
```

Result:

```text
[d,e,f]
```

Return.

---

# Processing Digit 2

Current Characters:

```text
a
b
c
```

Recursive Result:

```text
[d,e,f]
```

---

## For a

```text
ad
ae
af
```

---

## For b

```text
bd
be
bf
```

---

## For c

```text
cd
ce
cf
```

---

# Final Answer

```text
[
ad,
ae,
af,
bd,
be,
bf,
cd,
ce,
cf
]
```

---

# Visualization

Input:

```text
23
```

Decision Tree:

```text
                ""
          /      |      \
         a       b       c
       / | \   / | \   / | \
      d  e f  d  e f  d  e f
```

Leaves:

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

---

# Why Called Cartesian Product?

Suppose:

```text
Set A

[a,b,c]
```

and

```text
Set B

[d,e,f]
```

Result:

```text
A × B
```

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

Every element combines with every element.

---

# Complexity Analysis

Suppose:

```text
n digits
```

Average choices:

```text
k
```

Total combinations:

```text
k^n
```

---

## Time Complexity

```text
O(k^n)
```

---

## Space Complexity

Recursion Depth:

```text
O(n)
```

Result Storage:

```text
O(k^n)
```

---

# Difference Between GSS and KPC

## GSS

Each character:

```text
Include

OR

Exclude
```

Choices:

```text
2
```

Total:

```text
2^n
```

---

## KPC

Each digit:

```text
Multiple Choices
```

Example:

```text
abc
```

Choices:

```text
3
```

Total:

```text
k^n
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

Appending in Wrong Order

Wrong:

```java
s + option
```

Correct:

```java
option + s
```

---

## Mistake 3

Forgetting Nested Loop

Need:

```java
for(each option)
{
    for(each recursive answer)
    {
        combine
    }
}
```

---

# Pattern Recognition

Whenever you see:

```text
Current Choices

×

Remaining Choices
```

Think:

```text
Cartesian Product
```

---

# Interview Insight

This exact pattern appears in:

### LeetCode 17

```text
Letter Combinations of Phone Number
```

### Password Generation

### Combination Builders

### DFS Enumeration

### String Generation Problems

---

# Generic Template

```java
for(each current choice){

    for(each recursive answer){

        combine
    }
}
```

---

# Key Takeaways

- KPC uses Cartesian Product.
- Recursive call generates combinations for remaining digits.
- Current digit expands all recursive answers.
- Time Complexity is exponential.
- Directly maps to LeetCode 17.

---

# Practice Problems

## Easy

- Phone Keypad Combinations

## Medium

- LeetCode 17
- Password Generation

## Advanced

- DFS String Enumeration
- Word Generation Problems

---

# Next Topic

18-Print-Subsequence.md

Question:

```text
Instead of returning subsequences,

print them directly.
```

This introduces:

```text
Answer So Far (ASF)
```

which is the foundation of Backtracking.
