# Comparable vs Comparator in Java

Both **Comparable** and **Comparator** are used to **sort objects in Java**.

They define **how objects should be ordered**.

Example:

```
Sort by name
Sort by age
Sort by salary
Sort by ID
```

---

# 1. Comparable Interface

`Comparable` is used when a class **defines its natural sorting order**.

The class itself implements the `Comparable` interface.

Package:

```
java.lang
```

Method:

```java
int compareTo(T obj)
```

---

# Example: Sorting Students by Age

```java
import java.util.*;

class Student implements Comparable<Student>{

    int age;
    String name;

    Student(int age,String name){
        this.age=age;
        this.name=name;
    }

    public int compareTo(Student s){

        return this.age - s.age;
    }
}

public class Main {

    public static void main(String[] args){

        List<Student> list = new ArrayList<>();

        list.add(new Student(22,"A"));
        list.add(new Student(18,"B"));
        list.add(new Student(25,"C"));

        Collections.sort(list);

        for(Student s:list){
            System.out.println(s.age+" "+s.name);
        }
    }
}
```

Output

```
18 B
22 A
25 C
```

Sorted by **age**.

---

# How Comparable Works

Step-by-step:

```
Collections.sort(list)
        ↓
compareTo() method called
        ↓
Objects compared
        ↓
Sorted order generated
```

---

# compareTo() Return Values

| Return Value | Meaning |
|------|------|
0 | Objects are equal |
Negative | Current object smaller |
Positive | Current object greater |

Example:

```
return this.age - s.age
```

---

# Limitations of Comparable

A class can have **only one natural sorting logic**.

Example:

```
Student sorted by age
```

But what if we want:

```
Sort by name
Sort by marks
Sort by salary
```

Then we use **Comparator**.

---

# 2. Comparator Interface

`Comparator` is used when we want **multiple sorting strategies**.

It is implemented **outside the class**.

Package:

```
java.util
```

Method:

```java
int compare(T o1, T o2)
```

---

# Example: Sort Student by Name

```java
import java.util.*;

class Student{

    int age;
    String name;

    Student(int age,String name){
        this.age=age;
        this.name=name;
    }
}

class NameComparator implements Comparator<Student>{

    public int compare(Student s1, Student s2){

        return s1.name.compareTo(s2.name);
    }
}

public class Main {

    public static void main(String[] args){

        List<Student> list = new ArrayList<>();

        list.add(new Student(22,"Ravi"));
        list.add(new Student(18,"Amit"));
        list.add(new Student(25,"Zoya"));

        Collections.sort(list,new NameComparator());

        for(Student s:list){
            System.out.println(s.name+" "+s.age);
        }
    }
}
```

Output

```
Amit 18
Ravi 22
Zoya 25
```

Sorted by **name**.

---

# Using Lambda (Modern Java)

Comparator with lambda:

```java
Collections.sort(list,(s1,s2)->s1.age-s2.age);
```

Or:

```java
list.sort((a,b)->a.age-b.age);
```

---

# Comparator with TreeMap / TreeSet

Comparator can also control sorting inside:

```
TreeMap
TreeSet
PriorityQueue
```

Example:

```java
TreeMap<Integer,String> map =
        new TreeMap<>((a,b)->b-a);
```

This sorts keys in **descending order**.

---

# Visual Example

Student list:

```
[ 25,A ]
[ 18,B ]
[ 22,C ]
```

Comparable sorting:

```
18 → 22 → 25
```

Comparator sorting (name):

```
A → B → C
```

---

# Comparable vs Comparator

| Feature | Comparable | Comparator |
|------|------|------|
Package | java.lang | java.util |
Method | compareTo() | compare() |
Defined | Inside class | Outside class |
Sorting Logic | Single | Multiple |
Modification | Requires class change | No change needed |
Flexibility | Less | More |

---

# Real Example

Class:

```
Employee
```

Sorting requirements:

```
Sort by salary
Sort by age
Sort by name
Sort by joining date
```

Solution:

```
Comparable → one default sorting
Comparator → multiple sorting strategies
```

---

# Interview Summary

```
Comparable
    |
    |-- Natural sorting
    |-- compareTo()
    |-- Implemented inside class
```

```
Comparator
    |
    |-- Custom sorting
    |-- compare()
    |-- Implemented outside class
```

---

# One-Line Interview Answer

```
Comparable defines the natural sorting order of objects,
while Comparator provides custom sorting logic for objects.
```
